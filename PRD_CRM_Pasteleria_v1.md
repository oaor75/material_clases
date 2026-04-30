# Product Requirements Document (PRD)
## CRM de Gestión de Leads y Clientes — Pastelería
**Versión:** 1.0 (MVP Local)
**Fecha:** Abril 2026
**Autor:** Arquitecto de Software
**Estado:** Borrador para revisión

---

## Índice

1. [Resumen Ejecutivo](#1-resumen-ejecutivo)
2. [Contexto y Problema](#2-contexto-y-problema)
3. [Objetivos del Producto](#3-objetivos-del-producto)
4. [Alcance de la Versión 1.0](#4-alcance-de-la-versión-10)
5. [Usuarios y Roles](#5-usuarios-y-roles)
6. [Modelo de Datos](#6-modelo-de-datos)
7. [Pantallas y Flujos](#7-pantallas-y-flujos)
8. [Requerimientos Funcionales](#8-requerimientos-funcionales)
9. [Requerimientos No Funcionales](#9-requerimientos-no-funcionales)
10. [Arquitectura Técnica](#10-arquitectura-técnica)
11. [Criterios de Aceptación](#11-criterios-de-aceptación)
12. [Roadmap y Fases Futuras](#12-roadmap-y-fases-futuras)
13. [Supuestos y Restricciones](#13-supuestos-y-restricciones)
14. [Glosario](#14-glosario)

---

## 1. Resumen Ejecutivo

Se requiere construir un CRM (Customer Relationship Manager) web ligero para una pastelería, orientado a la gestión eficiente de **leads** (clientes potenciales) y **clientes** activos. La aplicación permitirá al equipo visualizar el pipeline de ventas en un tablero Kanban, gestionar fichas individuales de leads con operaciones CRUD completas, y aplicar filtros y búsquedas en tiempo real.

Esta primera versión (MVP) operará **íntegramente en local**, con una base de datos SQLite y toda la lógica en el frontend. No requiere servidor externo ni despliegue en producción.

---

## 2. Contexto y Problema

Una pastelería que gestiona pedidos personalizados, eventos y encargos especiales recibe leads de múltiples canales (redes sociales, web, referidos, llamadas). Sin una herramienta centralizada:

- Los leads se pierden o quedan sin seguimiento.
- No existe visibilidad del estado de cada oportunidad comercial.
- Las próximas acciones se gestionan de memoria o en hojas sueltas.
- No hay criterio claro para priorizar qué lead atender primero.

El CRM resuelve estos problemas proporcionando una fuente única de verdad, accesible y operable sin fricción.

---

## 3. Objetivos del Producto

| # | Objetivo | Métrica de éxito |
|---|----------|-----------------|
| 1 | Centralizar todos los leads en un único lugar | 0 leads fuera del sistema |
| 2 | Visualizar el pipeline de ventas de un vistazo | Kanban cargado en < 1 segundo |
| 3 | Priorizar con próxima acción y fecha | Todos los leads tienen próxima acción asignada |
| 4 | Editar y gestionar leads de forma rápida | CRUD completo en < 3 clicks desde el Kanban |
| 5 | Buscar y filtrar leads en segundos | Resultados de búsqueda en < 500 ms |

---

## 4. Alcance de la Versión 1.0

### ✅ Incluido en v1.0
- Autenticación básica (usuario/contraseña, un único nivel de permisos)
- Vista Kanban del pipeline con drag & drop
- Ficha de lead con CRUD completo
- Búsqueda y filtros en tiempo real
- Paginación (20–50 registros por vista)
- Persistencia local con SQLite (vía biblioteca cliente para Next.js)
- Toda la lógica de negocio en el frontend

### ❌ Excluido de v1.0 (fases futuras)
- Diferenciación de roles (admin vs. usuario)
- Backend separado
- Base de datos en la nube (Supabase)
- Autenticación OAuth / SSO
- Despliegue en VPS
- Integración con repositorio GitHub (CI/CD)
- Notificaciones por email o WhatsApp
- Historial de cambios / auditoría
- Importación/exportación de datos (CSV, Excel)

---

## 5. Usuarios y Roles

### v1.0 — Un único rol: Usuario Autenticado

Todos los usuarios autenticados tienen los mismos permisos. No existe distinción entre administrador y usuario estándar en esta versión.

| Acción | Usuario Autenticado |
|--------|-------------------|
| Ver pipeline Kanban | ✅ |
| Crear lead | ✅ |
| Editar lead | ✅ |
| Eliminar lead | ✅ |
| Cambiar etapa (drag & drop) | ✅ |
| Buscar y filtrar | ✅ |
| Acceder a ficha de lead | ✅ |

### Sistema de Autenticación v1.0

- Login con **email + contraseña**.
- Sesión persistida en `localStorage` mediante un token simple (no JWT en v1.0).
- Sin recuperación de contraseña ni registro público — los usuarios se crean directamente en la base de datos SQLite durante el setup inicial.
- Ruta protegida: cualquier URL del CRM redirige a `/login` si no hay sesión activa.

> **Nota para v2.0:** Se expandirá a roles `admin` y `usuario`, con Supabase Auth como proveedor de autenticación.

---

## 6. Modelo de Datos

### 6.1 Entidad: `users`

```
users
├── id              TEXT PRIMARY KEY (UUID)
├── email           TEXT UNIQUE NOT NULL
├── password_hash   TEXT NOT NULL
├── name            TEXT NOT NULL
└── created_at      DATETIME DEFAULT CURRENT_TIMESTAMP
```

### 6.2 Entidad: `leads`

```
leads
├── id                  TEXT PRIMARY KEY (UUID)
├── name                TEXT NOT NULL           — Nombre completo del lead
├── email               TEXT                    — Email de contacto
├── phone               TEXT                    — Teléfono de contacto
├── origin              TEXT                    — Origen: instagram / web / referido / llamada / otro
├── zone                TEXT                    — Zona geográfica o barrio
├── budget              REAL                    — Presupuesto estimado (€)
├── operation_type      TEXT                    — Tipo de encargo: boda / bautizo / cumpleaños / corporativo / otro
├── stage               TEXT NOT NULL           — Etapa del pipeline (ver enum abajo)
├── next_action         TEXT                    — Descripción de la próxima acción
├── next_action_date    DATE                    — Fecha límite de la próxima acción
├── notes               TEXT                    — Notas libres
├── assigned_to         TEXT FK → users.id      — Usuario responsable
├── created_at          DATETIME DEFAULT CURRENT_TIMESTAMP
└── updated_at          DATETIME DEFAULT CURRENT_TIMESTAMP
```

**Enum `stage` (etapas del pipeline):**

| Valor interno | Etiqueta visible | Descripción |
|---|---|---|
| `nuevo` | Nuevo | Lead recién registrado, sin contacto |
| `contactado` | Contactado | Se realizó el primer contacto |
| `visita` | Visita | Se agendó o realizó una visita/reunión |
| `oferta` | Oferta | Se envió propuesta o presupuesto formal |
| `cerrado_ganado` | Cerrado ✓ | Se confirmó el encargo |
| `cerrado_perdido` | Perdido | No se concretó el encargo |

### 6.3 Entidad: `clients`

Los clientes son leads que alcanzaron la etapa `cerrado_ganado` y se promocionan a la tabla de clientes para gestión post-venta.

```
clients
├── id              TEXT PRIMARY KEY (UUID)
├── lead_id         TEXT FK → leads.id          — Referencia al lead original
├── name            TEXT NOT NULL
├── email           TEXT
├── phone           TEXT
├── zone            TEXT
├── total_spent     REAL DEFAULT 0              — Gasto acumulado
├── notes           TEXT
├── created_at      DATETIME DEFAULT CURRENT_TIMESTAMP
└── updated_at      DATETIME DEFAULT CURRENT_TIMESTAMP
```

### 6.4 Relaciones

```
users    ──< leads      (un usuario puede tener múltiples leads asignados)
leads    ──| clients    (un lead cerrado ganado origina un cliente — relación 1:1)
```

---

## 7. Pantallas y Flujos

### 7.1 Pantalla: Login (`/login`)

**Propósito:** Autenticar al usuario antes de acceder al CRM.

**Elementos:**
- Campo email
- Campo contraseña
- Botón "Entrar"
- Mensaje de error inline si las credenciales son incorrectas

**Flujo:**
```
Usuario introduce credenciales
    → Validación en SQLite
    → [OK] Redirigir a /pipeline
    → [Error] Mostrar mensaje: "Email o contraseña incorrectos"
```

**Restricción:** No existe pantalla de registro en v1.0. Los usuarios se crean manualmente en la base de datos.

---

### 7.2 Pantalla: Pipeline Kanban (`/pipeline`)

**Propósito:** Vista principal del CRM. Permite ver de un vistazo todos los leads, su estado y prioridades.

**Layout:**
- Barra superior: logo/nombre de la pastelería, buscador global, filtros, botón "Nuevo Lead", avatar + logout.
- Cuerpo: 6 columnas Kanban (una por etapa), con scroll horizontal en pantallas pequeñas.
- Cada columna muestra: nombre de etapa, contador de leads, y las tarjetas apiladas verticalmente.

**Tarjeta de Lead:**

```
┌─────────────────────────────┐
│ [Nombre del lead]           │
│ 📍 Zona · 💰 Presupuesto    │
│ 🎂 Tipo de operación        │
│ ─────────────────────────── │
│ ⏰ Próxima acción           │
│ 📅 Fecha límite             │
└─────────────────────────────┘
```

- Click en tarjeta → Navegar a `/leads/:id`
- Indicador visual si la fecha de próxima acción está vencida (color rojo)
- Indicador visual si la próxima acción vence hoy (color naranja)

**Drag & Drop:**
- Arrastrar tarjeta entre columnas actualiza el campo `stage` del lead en SQLite.
- No está permitido arrastrar a `cerrado_ganado` directamente sin confirmación (modal).
- Al mover a `cerrado_ganado`: modal de confirmación → si acepta, se crea el registro en `clients`.

**Filtros disponibles:**
- Por zona
- Por tipo de operación
- Por rango de presupuesto (mín / máx)
- Por usuario asignado
- Por fecha de próxima acción (vencida / hoy / esta semana)

**Búsqueda:**
- Búsqueda global por nombre, email o teléfono.
- Resultados en tiempo real (debounce de 300 ms).
- Resalta los términos encontrados en las tarjetas.

**Paginación:**
- Selector de densidad: 20 / 35 / 50 tarjetas por columna.
- Botón "Cargar más" dentro de cada columna si hay más registros.

---

### 7.3 Pantalla: Ficha de Lead (`/leads/:id`)

**Propósito:** Ver y editar todos los datos de un lead. Operaciones CRUD completas.

**Layout:**
- Breadcrumb: `Pipeline > [Nombre del lead]`
- Formulario de edición en dos columnas (datos de contacto izquierda, datos de operación derecha).
- Panel lateral derecho: estado actual, próxima acción, fecha, notas.
- Barra de acciones inferior: "Guardar cambios" · "Eliminar lead" · "Volver al pipeline".

**Campos editables:**

| Sección | Campo | Tipo | Obligatorio |
|---|---|---|---|
| Contacto | Nombre completo | Text | ✅ |
| Contacto | Email | Email | ❌ |
| Contacto | Teléfono | Text | ❌ |
| Origen | Canal de captación | Select | ❌ |
| Operación | Tipo de encargo | Select | ❌ |
| Operación | Zona | Text | ❌ |
| Operación | Presupuesto estimado | Number (€) | ❌ |
| Estado | Etapa del pipeline | Select | ✅ |
| Seguimiento | Próxima acción | Text | ❌ |
| Seguimiento | Fecha próxima acción | Date | ❌ |
| Notas | Notas libres | Textarea | ❌ |

**Comportamiento al guardar:**
- Validación inline antes de enviar.
- Toast de confirmación: "Lead actualizado correctamente".
- Redirige automáticamente al pipeline manteniendo los filtros activos.
- Los cambios se reflejan inmediatamente en la tarjeta Kanban.

**Eliminar lead:**
- Modal de confirmación: "¿Seguro que quieres eliminar este lead? Esta acción no se puede deshacer."
- Al confirmar: elimina de SQLite y redirige al pipeline.

---

### 7.4 Pantalla: Nuevo Lead (`/leads/nuevo`)

**Propósito:** Formulario de alta de un nuevo lead.

- Mismos campos que la Ficha de Lead.
- Campo `stage` prellenado con `nuevo`.
- Al guardar: redirige a `/pipeline` con el nuevo lead visible en la columna "Nuevo".

---

### 7.5 Flujo de Navegación

```
/login
    └── [autenticado] → /pipeline
            ├── [click tarjeta] → /leads/:id
            │       └── [guardar] → /pipeline (mismos filtros)
            ├── [botón nuevo lead] → /leads/nuevo
            │       └── [guardar] → /pipeline
            └── [logout] → /login
```

---

## 8. Requerimientos Funcionales

### RF-01 Autenticación
- RF-01.1 El sistema debe requerir login con email y contraseña para acceder a cualquier ruta.
- RF-01.2 Las rutas protegidas deben redirigir a `/login` si no hay sesión activa.
- RF-01.3 La sesión debe persistir entre recargas de página.
- RF-01.4 El usuario debe poder cerrar sesión desde cualquier pantalla.

### RF-02 Pipeline Kanban
- RF-02.1 El sistema debe mostrar las 6 etapas del pipeline como columnas.
- RF-02.2 Cada columna debe mostrar el número total de leads que contiene.
- RF-02.3 El usuario debe poder arrastrar tarjetas entre columnas para cambiar la etapa.
- RF-02.4 Al mover un lead a `cerrado_ganado`, el sistema debe solicitar confirmación y crear el registro en `clients`.
- RF-02.5 Las tarjetas con fecha de próxima acción vencida deben mostrarse con indicador visual rojo.
- RF-02.6 Las tarjetas con fecha de próxima acción para hoy deben mostrarse con indicador visual naranja.

### RF-03 Búsqueda y Filtros
- RF-03.1 El buscador global debe filtrar por nombre, email y teléfono en tiempo real.
- RF-03.2 Los filtros deben poder combinarse (zona + presupuesto + fecha, por ejemplo).
- RF-03.3 El estado de filtros activos debe persistir al volver del detalle al pipeline.
- RF-03.4 Debe existir un botón para limpiar todos los filtros activos.

### RF-04 Paginación
- RF-04.1 Cada columna debe soportar paginación con 20, 35 o 50 tarjetas por vista.
- RF-04.2 Debe existir un control "Cargar más" por columna si hay registros adicionales.

### RF-05 CRUD de Leads
- RF-05.1 El usuario puede crear un nuevo lead desde cualquier pantalla del CRM.
- RF-05.2 El usuario puede ver todos los datos de un lead en su ficha.
- RF-05.3 El usuario puede editar cualquier campo del lead y guardar los cambios.
- RF-05.4 El usuario puede eliminar un lead, previa confirmación.
- RF-05.5 Los cambios en la ficha deben reflejarse inmediatamente en el Kanban.

### RF-06 Gestión de Clientes
- RF-06.1 Al cerrar un lead como ganado, el sistema crea automáticamente un registro en `clients`.
- RF-06.2 El cliente hereda los datos de contacto del lead original.

---

## 9. Requerimientos No Funcionales

| ID | Categoría | Requerimiento |
|---|---|---|
| RNF-01 | Rendimiento | El Kanban debe cargar en menos de 1 segundo con hasta 500 leads en base de datos |
| RNF-02 | Rendimiento | Los resultados de búsqueda deben aparecer en menos de 500 ms |
| RNF-03 | Usabilidad | La interfaz debe ser operable sin formación previa en menos de 10 minutos |
| RNF-04 | Usabilidad | El flujo más frecuente (ver pipeline → editar lead → volver) debe completarse en menos de 3 clics |
| RNF-05 | Compatibilidad | Debe funcionar en Chrome, Firefox y Safari en sus versiones actuales |
| RNF-06 | Responsividad | Debe ser usable en pantallas desde 1024px de ancho (no se requiere mobile-first) |
| RNF-07 | Persistencia | Los datos deben sobrevivir recargas de página y reinicios del servidor de desarrollo |
| RNF-08 | Mantenibilidad | El código debe seguir las convenciones de Next.js App Router y estar organizado por módulos de feature |

---

## 10. Arquitectura Técnica

### 10.1 Stack v1.0

| Capa | Tecnología | Justificación |
|---|---|---|
| Framework | Next.js 14+ (App Router) | Renderizado híbrido, routing basado en carpetas, preparado para migración a Supabase |
| Estilos | Tailwind CSS | Velocidad de desarrollo, consistencia visual, sin overhead de CSS custom |
| Base de datos | SQLite (vía `better-sqlite3` o `@sqlite.org/sqlite-wasm`) | Sin servidor, persistencia local, fácil migración posterior a PostgreSQL/Supabase |
| Drag & Drop | `@dnd-kit/core` | Accesible, mantenido, compatible con React |
| Estado global | Zustand o Context API | Gestión ligera del estado del Kanban y filtros activos |
| Validación | Zod | Validación de esquemas en formularios, compatible con TypeScript |
| Autenticación | Custom (localStorage + hash bcrypt) | Sin dependencias externas en v1.0, reemplazable por Supabase Auth en v2.0 |

### 10.2 Estructura de Carpetas

```
/
├── app/
│   ├── (auth)/
│   │   └── login/
│   │       └── page.tsx
│   ├── (crm)/
│   │   ├── pipeline/
│   │   │   └── page.tsx
│   │   ├── leads/
│   │   │   ├── nuevo/
│   │   │   │   └── page.tsx
│   │   │   └── [id]/
│   │   │       └── page.tsx
│   │   └── layout.tsx            ← Layout con auth guard
│   └── layout.tsx
├── components/
│   ├── kanban/
│   │   ├── KanbanBoard.tsx
│   │   ├── KanbanColumn.tsx
│   │   └── LeadCard.tsx
│   ├── leads/
│   │   ├── LeadForm.tsx
│   │   └── LeadFilters.tsx
│   └── ui/                       ← Componentes base reutilizables
├── lib/
│   ├── db/
│   │   ├── client.ts             ← Conexión SQLite
│   │   ├── schema.ts             ← Definición de tablas
│   │   └── seed.ts               ← Datos de prueba iniciales
│   ├── actions/
│   │   ├── leads.ts              ← CRUD de leads
│   │   └── auth.ts               ← Login / logout
│   └── utils/
│       ├── scoring.ts
│       └── dates.ts
├── store/
│   └── useKanbanStore.ts         ← Estado global del Kanban
├── types/
│   └── index.ts                  ← Tipos TypeScript del dominio
└── middleware.ts                 ← Auth guard a nivel de ruta
```

### 10.3 Decisiones de Arquitectura

**¿Por qué toda la lógica en el frontend?**
En v1.0 se prioriza velocidad de desarrollo y simplicidad operativa (cero configuración de servidor). Next.js Server Actions permiten abstraer la capa de acceso a datos de forma que la migración a Supabase en v2.0 sea un cambio de implementación, no de arquitectura.

**¿Por qué SQLite y no localStorage o IndexedDB?**
SQLite garantiza consultas relacionales, integridad referencial y un esquema tipado. Facilita enormemente la migración a PostgreSQL/Supabase sin cambiar el modelo de datos.

**Preparación para Supabase (v2.0):**
- Los tipos TypeScript del dominio (`types/index.ts`) se definirán desde el inicio como agnósticos al proveedor de datos.
- Las funciones en `lib/actions/` seguirán la interfaz de Supabase Client desde el inicio, para que el cambio en v2.0 sea solo de driver.
- El esquema SQLite usará los mismos nombres de tabla y columna que se usarán en Supabase/PostgreSQL.

---

## 11. Criterios de Aceptación

### CA-01 — Login
```
DADO un usuario registrado en la base de datos
CUANDO introduce email y contraseña correctos
ENTONCES es redirigido al pipeline y ve sus leads

DADO un usuario no autenticado
CUANDO intenta acceder a /pipeline directamente
ENTONCES es redirigido a /login
```

### CA-02 — Kanban
```
DADO que existen leads en distintas etapas
CUANDO el usuario carga /pipeline
ENTONCES ve cada lead en la columna correspondiente a su etapa
Y cada columna muestra el número correcto de leads

DADO un lead en columna "Contactado"
CUANDO el usuario lo arrastra a "Visita"
ENTONCES el campo stage del lead se actualiza a "visita" en la base de datos
Y la tarjeta aparece en la columna "Visita"
```

### CA-03 — Filtros
```
DADO que el usuario tiene filtro de zona activo con valor "Centro"
CUANDO navega a la ficha de un lead y vuelve al pipeline
ENTONCES el filtro de zona sigue activo con el valor "Centro"
```

### CA-04 — CRUD de Lead
```
DADO que el usuario edita el presupuesto de un lead en su ficha
CUANDO guarda los cambios
ENTONCES la tarjeta del lead en el Kanban refleja el nuevo presupuesto
Y aparece un toast de confirmación

DADO que el usuario elimina un lead
CUANDO confirma la eliminación en el modal
ENTONCES el lead ya no aparece en el pipeline
Y no existe en la base de datos
```

### CA-05 — Cierre ganado
```
DADO un lead en cualquier etapa
CUANDO el usuario lo mueve a "Cerrado ganado" y confirma
ENTONCES se crea un registro en la tabla clients con los datos del lead
Y el lead permanece en el pipeline en etapa "cerrado_ganado"
```

---

## 12. Roadmap y Fases Futuras

### v1.0 — MVP Local *(este documento)*
- Kanban, CRUD, búsqueda, filtros, paginación, SQLite, autenticación básica.

### v2.0 — Backend en la nube
- Migración de SQLite → Supabase (PostgreSQL)
- Supabase Auth (email/contraseña + OAuth)
- Diferenciación de roles: `admin` y `usuario`
- Despliegue en VPS con Dokploy
- Repositorio GitHub con CI/CD básico

### v3.0 — Automatizaciones e integraciones
- Integración con N8N para workflows automatizados (notificaciones, seguimientos)
- Agente IA en Antigravity como primer punto de contacto
- Reportes automáticos (leads por semana, tasa de conversión por etapa)
- Importación/exportación CSV

### v4.0 — Funcionalidades avanzadas
- Historial de cambios y auditoría por lead
- Módulo de tareas y calendario
- Notificaciones por email / WhatsApp vía N8N
- Dashboard de métricas con gráficas

---

## 13. Supuestos y Restricciones

### Supuestos
- El entorno de desarrollo es macOS o Linux con Node.js 18+.
- La base de datos SQLite se almacena en el sistema de archivos local del proyecto.
- Los usuarios iniciales se crean manualmente mediante un script de seed.
- No existe requisito de acceso simultáneo desde múltiples dispositivos en v1.0.

### Restricciones
- v1.0 no soporta múltiples usuarios concurrentes editando el mismo lead.
- No hay recuperación de contraseña en v1.0.
- No hay copia de seguridad automática de la base de datos SQLite en v1.0.
- El rendimiento no está optimizado para bases de datos superiores a 10.000 leads en v1.0.

---

## 14. Glosario

| Término | Definición |
|---|---|
| **Lead** | Cliente potencial que ha mostrado interés pero aún no ha confirmado un encargo |
| **Cliente** | Lead que ha cerrado un trato y tiene un encargo confirmado |
| **Pipeline** | Representación visual del proceso comercial dividido en etapas |
| **Stage / Etapa** | Fase en la que se encuentra un lead dentro del pipeline |
| **Kanban** | Metodología de gestión visual mediante columnas y tarjetas |
| **CRUD** | Create, Read, Update, Delete — operaciones básicas sobre un registro |
| **Drag & Drop** | Interacción de arrastrar y soltar elementos en la interfaz |
| **Próxima acción** | Tarea concreta que el equipo debe realizar para avanzar con ese lead |
| **Supabase** | Plataforma BaaS (Backend as a Service) basada en PostgreSQL, prevista para v2.0 |
| **Dokploy** | Herramienta de despliegue en VPS, prevista para v2.0 |
| **N8N** | Plataforma de automatización de workflows, prevista para v3.0 |
| **Antigravity** | Plataforma de agentes IA, prevista para v3.0 |

---

*Documento preparado para revisión. Versión 1.0 sujeta a cambios tras validación con el equipo.*
