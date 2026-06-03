# Workflows n8n — Vida Natural

Workflows para `https://n8n.eaglemarketing.tech`.

## `whatsapp-pedidos.workflow.json`

Recepción de pedidos por WhatsApp con **validación de tienda** antes de procesar.

### Flujo

```
Webhook WhatsApp
   → Config (links, teléfonos, URLs)
   → Extraer mensaje (from_phone + message_text)
   → Buscar tienda (Postgres/Supabase)
   → IF status = 'activa'
        ├─ true  → IA extrae pedido → POST /api/n8n/orders → Responder confirmación
        └─ false → IF status = 'bloqueada'
                      ├─ true  → Responder: suspendida
                      └─ false → Responder: no registrada
```

### Nodo de validación (clave)

```sql
SELECT id, name, status
FROM stores
WHERE regexp_replace(coalesce(whatsapp_phone, ''), '\D', '', 'g')
    = regexp_replace($1, '\D', '', 'g')
LIMIT 1;
```

**Por qué difiere del enunciado:** el requisito original incluía `AND status = 'activa'`
en la query. Pero con ese filtro una tienda **bloqueada** devolvería 0 filas, igual que
una **no registrada**, y sería imposible distinguir ambos casos para enviar el mensaje
correcto. Por eso se consulta **solo por teléfono** y la ramificación por `status` se hace
en n8n (nodos IF). Resultado idéntico al pedido, con los tres mensajes diferenciados:

| Resultado | Condición | Respuesta |
|-----------|-----------|-----------|
| No registrada | sin fila / status `pendiente` | Mensaje de registro + teléfono admin |
| Suspendida | status `bloqueada` | Mensaje de cuenta suspendida |
| Continuar | status `activa` | Procesa el pedido |

**Normalización de teléfono:** `regexp_replace(..., '\D', '', 'g')` elimina espacios, `+`
y guiones en ambos lados, de modo que `+34 600 111 222` y `34600111222` hacen match.

### Importar en n8n

1. n8n → *Workflows* → *Import from File* → seleccionar `whatsapp-pedidos.workflow.json`.
2. Configurar credenciales y variables (abajo).
3. Activar el workflow y registrar la URL del webhook en Meta.

### Credenciales necesarias

| Nodo | Credencial | Notas |
|------|-----------|-------|
| Buscar tienda | **Postgres** (Supabase) | Reemplazar `REEMPLAZAR_CREDENCIAL_SUPABASE`. Host/DB/usuario del entorno Supabase self-hosted. |

### Variables de entorno en n8n

Definir en n8n (Settings → Variables, o variables de entorno del contenedor):

| Variable | Uso |
|----------|-----|
| `WHATSAPP_PHONE_NUMBER_ID` | ID del número de WhatsApp Cloud API (Meta) |
| `WHATSAPP_ACCESS_TOKEN` | Token de la WhatsApp Business API |
| `N8N_WEBHOOK_SECRET` | Header `X-N8N-Secret` para llamar a `/api/n8n/orders` de la app |

> Estas mismas variables existen en `.env.example` de la app. El secreto
> `N8N_WEBHOOK_SECRET` debe ser **idéntico** en ambos lados.

### Editar antes de producción (nodo `Config`)

- `registroLink` → `https://app8.eaglemarketing.tech/registro-tienda`
- `adminPhone` → teléfono real del administrador
- `ordersWhatsapp` → número público de pedidos
- `appApiBase` → `https://app8.eaglemarketing.tech`

### Pendiente de completar (continuación tienda activa)

Los nodos posteriores a `IF status = activa` son un **scaffold**:

1. **IA: Extraer pedido** — sustituir el placeholder por un nodo de OpenAI/Antigravity que
   convierta `message_text` en `items: [{ product_name, quantity }]`.
2. **POST /api/n8n/orders** — el endpoint de la app aún no existe (ver checklist en
   `UPDATES.md`). Debe aceptar `{ store_id, source, notes, items }` y crear el pedido.
3. **Responder: confirmación** — ya listo.
