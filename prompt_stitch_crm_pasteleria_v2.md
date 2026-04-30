# Prompt de Diseño — Stitch
## CRM para [Pastelería] · Estética [Bento Grid] · Con imagen de referencia

---

### Instrucción principal

Diseña una interfaz de usuario completa para un CRM de gestión de leads y clientes orientado a una **[pastelería]**, tomando como base visual la **imagen de referencia adjunta**. Extrae de ella la paleta de colores, el estilo tipográfico, los acabados visuales y la atmósfera general, y aplícalos de forma coherente a todos los componentes de la interfaz.

La estética estructural debe ser **[Bento Grid]**: tarjetas modulares con bordes redondeados, jerarquía clara por tamaño de bloque y espaciado generoso. La imagen de referencia define el *tono visual* (calidez, materialidad, carácter de marca); el Bento Grid define la *estructura compositiva*.

La interfaz debe transmitir **[calidez, profesionalidad artesanal y orden visual]**, diferenciándose de un CRM corporativo frío.

> 💡 *En corchete: sustituye el tipo de negocio, la estética estructural y el tono visual según el cliente. La instrucción sobre la imagen de referencia es fija para todas las versiones.*

---

### Uso de la imagen de referencia

Al procesar la imagen adjunta, extrae y aplica los siguientes elementos a la interfaz:

- **Paleta de color:** identifica los 3–5 colores dominantes y úsalos como sistema de color de la UI (fondo, tarjetas, acento primario, acento secundario, texto).
- **Tipografía por analogía:** si la imagen contiene texto o logotipo, toma su estilo (serif / sans-serif / script, peso, espaciado) como referencia para elegir las fuentes de la interfaz.
- **Textura y acabado:** si la imagen tiene texturas (madera, papel, tela, concreto, mármol), incorpóralas sutilmente como fondos de sección, patrones de tarjeta o separadores visuales.
- **Nivel de contraste y luminosidad:** respeta si la imagen es de tono claro (light UI) u oscuro (dark UI) y mantén esa decisión en toda la interfaz.
- **Estilo fotográfico o ilustrativo:** si la imagen sugiere un mundo visual específico (artesanal, minimalista, lujoso, orgánico), ese mundo debe reflejarse en la elección de íconos, ilustraciones de estado vacío y microdetalles decorativos.

> 💡 *Esta sección es fija y no necesita modificarse entre proyectos. La imagen de referencia hace el trabajo de definición visual; el prompt solo necesita el contexto del negocio y la estructura de pantallas.*

---

### Pantalla 1 — Pipeline Kanban (`/pipeline`)

**Descripción:** Vista principal del CRM. Tablero Kanban con las etapas del proceso comercial de la **[pastelería]**.

**Columnas del pipeline:**
`[Nuevo]` → `[Contactado]` → `[Visita / Degustación]` → `[Oferta enviada]` → `[Cerrado ✓]` / `[Perdido]`

> 💡 *Renombra las etapas según el sector. Ejemplos — inmobiliaria: Captación / Visita / Oferta / Firma; clínica: Consulta / Presupuesto / Tratamiento / Alta.*

**Tarjeta de lead — campos visibles:**
- **[Nombre del cliente o empresa]**
- **[Zona o barrio]** *(pastelería: zona de entrega o referencia geográfica)*
- **[Presupuesto estimado]** *(pastelería: valor del encargo en €)*
- **[Tipo de encargo]**: `[Boda / Bautizo / Cumpleaños / Corporativo / Otro]`
- Próxima acción + fecha límite
- Indicador visual de urgencia: 🔴 vencida · 🟠 hoy · 🟢 próxima

> 💡 *Sustituye "Tipo de encargo" y sus opciones por el equivalente del sector. Ejemplos — inmobiliaria: Tipo de operación (Venta / Alquiler / Traspaso); clínica: Tratamiento de interés.*

**Interacciones a mostrar:**
- Drag & drop entre columnas con feedback visual (sombra al arrastrar, columna destino resaltada con el acento primario extraído de la imagen)
- Hover sobre tarjeta: revelar acceso rápido a la ficha
- Badge con contador de leads por columna

**Barra superior:**
- Logo / nombre de la **[pastelería]** *(usa el estilo tipográfico de la imagen de referencia)*
- Buscador global (por **[nombre, email o teléfono]**)
- Filtros activos como chips: **[zona]**, **[tipo de encargo]**, **[rango de presupuesto]**, **[fecha de próxima acción]**
- Botón prominente: `+ Nuevo [Lead]`
- Avatar de usuario + logout

**Paginación:**
- Selector por columna: `[20]` / `[35]` / `[50]` tarjetas
- Botón "Ver más" al pie de columna cuando hay registros adicionales

---

### Pantalla 2 — Ficha de Lead / CRUD (`/leads/:id`)

**Descripción:** Vista de detalle y edición completa de un lead. Layout de dos columnas con panel lateral de seguimiento.

**Sección izquierda — Datos de contacto:**
- **[Nombre completo]**
- **[Email]**
- **[Teléfono]**
- **[Canal de captación]**: `[Instagram / Web / Referido / Llamada / Feria / Otro]`

> 💡 *Los canales de captación varían por sector y estrategia de marketing del negocio.*

**Sección derecha — Datos del encargo:**
- **[Tipo de encargo]**: `[Boda / Bautizo / Cumpleaños / Corporativo / Otro]`
- **[Zona o dirección de entrega]**
- **[Presupuesto estimado]** (numérico en €)
- **[Fecha del evento o entrega]** *(pastelería: cuándo se necesita el pedido)*

> 💡 *"Fecha del evento o entrega" es específica de pastelería. En inmobiliaria sería "Fecha de disponibilidad"; en clínica, "Fecha preferida de inicio de tratamiento".*

**Panel lateral — Gestión y seguimiento:**
- Selector de etapa del pipeline
- Próxima acción (texto libre)
- Fecha límite de la próxima acción
- Área de notas *(el fondo y textura de este campo deben inspirarse en la imagen de referencia)*

**Barra de acciones:**
- `Guardar cambios` (botón primario — color acento de la imagen)
- `Eliminar [lead]` (botón destructivo con modal de confirmación)
- `← Volver al pipeline` (mantiene filtros activos)

**Breadcrumb:** `Pipeline > [Nombre del lead]`

**Microinteracciones:**
- Toast al guardar: `"[Lead] actualizado correctamente ✓"` *(color y estilo del toast derivado de la imagen)*
- Modal de confirmación al eliminar
- Validación inline en email, teléfono y presupuesto

---

### Guía de Estilo Visual

**La imagen de referencia define la paleta.** Como orientación general para una **[pastelería]**, los rangos tonales esperados son:

| Rol | Descripción orientativa |
|-----|------------------------|
| Fondo principal | Tono cálido claro (crema, marfil, blanco roto) |
| Tarjetas | Blanco puro o ligeramente más claro que el fondo |
| Acento primario | Tono saturado del color dominante de la imagen |
| Acento secundario | Versión pastel o desaturada del acento primario |
| Texto principal | Oscuro cálido (marrón, carbón cálido) |
| Estados de urgencia | Rojo / naranja / verde — siempre coherentes con la paleta extraída |

> 💡 *Esta tabla es solo una guía. Stitch debe priorizar los colores reales de la imagen de referencia sobre cualquier sugerencia de este documento.*

**Tipografía para [pastelería]:**
- Títulos y nombre de marca: `[fuente serif elegante o script suave — Playfair Display, Cormorant, o equivalente al estilo de la imagen]`
- Cuerpo, etiquetas y campos: `[fuente sans-serif legible — DM Sans, Nunito, o la que mejor complemente el titular]`

> 💡 *Ajusta la familia tipográfica al carácter del sector: inmobiliaria premium → sans-serif geométrica fría; startup → sans-serif moderna; clínica → neutra y limpia.*

**Estética Bento Grid (fija para todas las versiones):**
- Bloques de tamaño variable con `border-radius` generoso: 16–24 px
- Sombras suaves y difusas (`box-shadow` sin dureza)
- Padding interior amplio en tarjetas: 20–24 px
- Sin bordes duros ni líneas divisorias agresivas
- Íconos de línea fina coherentes con el tono visual de la imagen

---

### Elementos fijos (no modificar entre versiones)

Los siguientes elementos son estructurales del CRM y no deben cambiar independientemente del sector o imagen de referencia:

- Estructura de dos pantallas: Pipeline Kanban + Ficha de Lead
- Columnas del Kanban: mínimo 5 etapas con estado final doble (ganado / perdido)
- Campos obligatorios en tarjeta: nombre, presupuesto, próxima acción, fecha
- Barra superior con buscador, filtros y botón de creación
- Panel lateral en ficha con gestión de estado y seguimiento
- Paginación configurable por columna
- Sistema de indicadores de urgencia en tarjetas (vencida / hoy / próxima)

---

*Prompt de diseño Stitch — CRM adaptable por sector · Versión 1.1 con imagen de referencia*
