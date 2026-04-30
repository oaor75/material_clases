# 📐 Reglas del Proyecto CRM — Guía para Estudiantes
### Stack: Antigravity · Make.com · N8N · Base de datos externa
### Nivel: Avanzado — Agente IA + Integraciones + Reportes

> Estas reglas definen cómo trabajar en equipo, cómo construir con criterio técnico y cómo entregar un CRM que funcione en el mundo real. No son sugerencias: son los estándares mínimos del proyecto.

---

## 🧭 REGLA 1 — Planifica antes de tocar cualquier herramienta

Antes de crear un flujo en Make, un nodo en N8N, un agente en Antigravity o una tabla en tu base de datos, escribe un plan y valídalo con tu docente.

**Tu plan debe responder:**
- ¿Qué problema concreto resuelve este componente del CRM?
- ¿Qué herramienta del stack es la más adecuada para esta tarea?
- ¿Qué datos entran, qué datos salen y dónde se almacenan?
- ¿Cómo se conecta este componente con el resto del sistema?

**Distribución de responsabilidades en el stack:**

| Capa | Herramienta | Qué resuelve en el CRM |
|------|-------------|------------------------|
| Inteligencia y conversación | Antigravity | Agente que atiende, califica y responde a leads |
| Automatización de procesos | Make.com | Flujos automáticos: notificaciones, asignación de contactos, seguimientos |
| Pipelines de datos complejos | N8N | Transformación de datos, lógica condicional avanzada, integraciones técnicas |
| Persistencia y reportes | Airtable / Notion / Google Sheets | Registro de contactos, historial, pipeline de ventas, dashboards |

> **Regla de oro:** Si puedes hacerlo con Make, no lo hagas en N8N. Si puedes hacerlo en N8N, no lo hagas en Antigravity. Cada herramienta tiene su lugar.

---

## 🏗️ REGLA 2 — Construye en iteraciones, no todo de una vez

El CRM se construye en capas. No intentes conectar todo el stack en una sola sesión.

**Orden recomendado de construcción:**

```
1. Base de datos → Define la estructura de tus tablas primero
2. Flujos básicos → Crea y verifica entrada/salida de datos (Make o N8N)
3. Agente IA → Conéctalo una vez que los flujos base funcionan
4. Integraciones adicionales → Solo después de que el núcleo esté estable
5. Reportes y dashboards → Al final, cuando los datos son confiables
```

**Antes de dar por terminado cualquier componente**, genera una evidencia:
- ✅ Captura del flujo funcionando con datos de prueba
- ✅ Registro en la base de datos con datos correctos
- ✅ Log del agente respondiendo correctamente

---

## 🧹 REGLA 3 — Nomenclatura y organización obligatoria

Todo elemento del proyecto debe tener un nombre que explique qué hace, sin necesidad de abrirlo.

**Convención de nombres para el CRM:**

```
[herramienta]_[módulo_crm]_[acción]

Ejemplos correctos:
  make_contactos_registrar_nuevo_lead
  n8n_pipeline_calificar_lead_score
  antigravity_agente_atencion_inicial
  sheets_tabla_contactos_principal
  airtable_vista_pipeline_ventas

Ejemplos incorrectos:
  flujo1
  agente_nuevo
  automatizacion_crm
  test_final_v3
```

**Estructura mínima de tu base de datos del CRM:**

| Tabla / Hoja | Campos obligatorios |
|---|---|
| Contactos | nombre, email, teléfono, fuente, fecha_registro, estado, propietario |
| Pipeline | contacto_id, etapa, fecha_cambio_etapa, valor_estimado, próxima_acción |
| Historial de interacciones | contacto_id, fecha, canal, resumen, agente_responsable |
| Reportes | métricas semanales, tasa de conversión por etapa, tiempo promedio en pipeline |

---

## 🛡️ REGLA 4 — Manejo de errores en cada capa del stack

Un CRM que falla en silencio es peor que un CRM que no existe.

**Escenarios de error que debes cubrir obligatoriamente:**

### En Make.com:
- Ruta de error activa en cada módulo crítico (no dejes módulos sin el path de error)
- Notificación por email o Slack si un flujo falla en producción
- Filtros que validen que los datos tienen el formato esperado antes de procesarlos

### En N8N:
- Nodo `Error Trigger` conectado a los flujos principales
- Validación de campos obligatorios antes de escribir en la base de datos
- Logs activados en nodos de transformación de datos

### En Antigravity:
- Respuesta definida para cuando el agente no entiende la intención del usuario
- Límite de reintentos antes de escalar a un humano
- Fallback si la conexión con la base de datos falla

### En la base de datos:
- Campos obligatorios marcados como requeridos
- Validaciones de formato en email y teléfono
- Vista de registros con errores o datos incompletos

---

## 🔗 REGLA 5 — Arquitectura de integración documentada

Antes de conectar dos herramientas, dibuja (o describe en texto) exactamente cómo fluyen los datos entre ellas.

**Flujo de datos del CRM completo:**

```
[Fuente de lead] 
    → Formulario web / WhatsApp / Email
    ↓
[Antigravity — Agente IA]
    → Recibe el contacto inicial
    → Califica al lead (preguntas básicas)
    → Registra intención y datos clave
    ↓
[Make.com / N8N — Automatización]
    → Toma los datos del agente
    → Los normaliza y valida
    → Los escribe en la base de datos
    → Asigna propietario según reglas del negocio
    → Dispara notificación al equipo de ventas
    ↓
[Base de datos — Airtable / Notion / Sheets]
    → Registra contacto en tabla principal
    → Crea entrada en pipeline en etapa inicial
    → Registra primera interacción en historial
    ↓
[Reportes automáticos]
    → Métricas actualizadas en tiempo real o diariamente
```

> Cada vez que modifiques este flujo, actualiza el diagrama. Un flujo no documentado no existe para el resto del equipo.

---

## 🔒 REGLA 6 — Seguridad y datos de prueba

**Obligatorio durante todo el desarrollo:**

- Usa **siempre datos ficticios** para probar: nunca datos reales de clientes.
- Las credenciales (API keys de Airtable, tokens de N8N, conexiones de Make) se guardan **únicamente en las variables de entorno** de cada herramienta, nunca en el cuerpo de un flujo o en un mensaje.
- Antes de mostrar el proyecto en clase o presentación, verifica que no haya datos personales reales visibles en ninguna vista.
- Si tu base de datos tiene datos de prueba que parecen reales (nombres, emails), etiquétalos claramente como `[TEST]`.

**Datos de prueba recomendados para el CRM:**
```
Nombre: Carlos Prueba / Ana Demo / Test Usuario
Email: test01@crm-demo.com / prueba@test.com
Teléfono: +34 000 000 001
Empresa: Empresa Demo S.L.
Fuente: formulario_web_test
```

---

## 💬 REGLA 7 — Comunicación y trabajo en equipo

- Antes de modificar un flujo o agente que otro miembro del equipo construyó, **avísale**.
- Si detectas que una decisión tomada antes genera un problema ahora, **no la corrijas en silencio**: documenta el problema y la solución aplicada.
- En cada sesión de trabajo, el equipo debe poder responder: *¿qué funciona hoy? ¿qué falta? ¿qué está bloqueado?*
- Si una instrucción del docente o del proyecto no tiene sentido técnico para el stack elegido, **detente y consulta** antes de improvisar una solución.

---

## 📋 Checklist de entrega por componente

### Base de datos ✅
- [ ] Tablas creadas con todos los campos obligatorios
- [ ] Campos requeridos marcados como obligatorios
- [ ] Datos de prueba cargados (etiquetados como `[TEST]`)
- [ ] Vista de pipeline visible y funcional

### Automatizaciones (Make / N8N) ✅
- [ ] Flujo de registro de nuevo lead funcionando end-to-end
- [ ] Manejo de errores activo en todos los módulos críticos
- [ ] Notificación de asignación de lead funcionando
- [ ] Probado con al menos 3 casos de prueba distintos

### Agente IA (Antigravity) ✅
- [ ] Agente responde correctamente la atención inicial
- [ ] Califica leads con al menos 3 criterios definidos
- [ ] Conectado al flujo de automatización
- [ ] Fallback definido para errores o intenciones no reconocidas

### Reportes ✅
- [ ] Dashboard con métricas principales visible
- [ ] Se actualiza automáticamente con nuevos registros
- [ ] Muestra al menos: total de leads, conversión por etapa, leads nuevos esta semana

---

*Proyecto CRM — Curso IA Práctica con Antigravity · Versión 1.0*
