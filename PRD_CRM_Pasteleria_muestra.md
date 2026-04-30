# Product Requirements Document (PRD)

## CRM para Pastelería -- Gestión de Leads y Clientes

------------------------------------------------------------------------

## 1. Visión General

Este documento describe los requisitos para el desarrollo de un CRM
enfocado en la gestión de leads y clientes para una pastelería. El
objetivo es centralizar la información, optimizar el seguimiento
comercial y facilitar la conversión de leads en clientes.

------------------------------------------------------------------------

## 2. Objetivos del Producto

-   Implementar un CRM para gestión de leads y clientes.
-   Centralizar la información y evitar pérdida de leads.
-   Visualizar el pipeline de ventas de forma clara.
-   Priorizar tareas mediante próxima acción y fecha.
-   Permitir edición rápida (CRUD) de leads.
-   Incorporar búsqueda, filtros y paginación eficiente.

------------------------------------------------------------------------

## 3. Alcance (MVP)

Incluye: - Gestión de leads. - Visualización tipo Kanban. - CRUD de
leads. - Filtros y búsqueda. - Autenticación básica.

No incluye (futuras versiones): - Roles avanzados (admin/usuario). -
Integración con backend real (Supabase). - Despliegue en VPS.

------------------------------------------------------------------------

## 4. Usuarios y Roles

### 4.1 Usuarios

-   Usuarios internos de la pastelería.

### 4.2 Permisos

-   Todos los usuarios tienen los mismos permisos.
-   Acceso completo a funcionalidades del sistema.

------------------------------------------------------------------------

## 5. Modelo de Datos

### 5.1 Entidad Lead

-   id
-   nombre
-   contacto
-   origen
-   tipo_operacion
-   zona
-   presupuesto
-   estado
-   proxima_accion
-   fecha_proxima_accion
-   notas
-   created_at
-   updated_at

### 5.2 Entidad Cliente

-   id
-   nombre
-   contacto
-   historial
-   created_at

------------------------------------------------------------------------

## 6. Funcionalidades Principales

### 6.1 Pipeline (Kanban)

#### Columnas:

-   Nuevo
-   Contactado
-   Visita
-   Oferta
-   Cerrado
-   Perdido

#### Características:

-   Drag & Drop entre columnas.
-   Visualización rápida del estado de cada lead.
-   Tarjetas con:
    -   Nombre
    -   Zona
    -   Presupuesto
    -   Próxima acción
    -   Fecha

#### Extras:

-   Filtros dinámicos.
-   Búsqueda por texto.
-   Paginación (20--50 elementos).

------------------------------------------------------------------------

### 6.2 Ficha Lead (CRUD)

#### Funcionalidades:

-   Crear lead.
-   Editar lead.
-   Eliminar lead.
-   Visualizar detalles.

#### Campos editables:

-   Contacto
-   Origen
-   Operación
-   Zona
-   Presupuesto
-   Notas
-   Estado
-   Próxima acción
-   Fecha

#### Comportamiento:

-   Guardar cambios y volver al Kanban.
-   Reflejo inmediato en pipeline.

------------------------------------------------------------------------

## 7. Experiencia de Usuario (UX)

-   Interfaz limpia y rápida.
-   Navegación intuitiva.
-   Acciones en máximo 2--3 clics.
-   Feedback visual inmediato.

------------------------------------------------------------------------

## 8. Arquitectura Técnica

### 8.1 Frontend

-   Framework: Next.js
-   Estilos: Tailwind CSS

### 8.2 Backend (Temporal)

-   Lógica en frontend.
-   Base de datos SQLite local.

### 8.3 Futuro

-   Migración a Supabase:
    -   Base de datos
    -   Autenticación
    -   API backend

------------------------------------------------------------------------

## 9. Autenticación

-   Sistema básico de login.
-   Sin roles diferenciados.
-   Persistencia de sesión local.

------------------------------------------------------------------------

## 10. Búsqueda y Filtros

-   Búsqueda por nombre, zona o contacto.
-   Filtros por:
    -   Estado
    -   Presupuesto
    -   Zona
-   Actualización en tiempo real.

------------------------------------------------------------------------

## 11. Paginación

-   Límite configurable (20--50).
-   Navegación entre páginas.
-   Optimización de rendimiento.

------------------------------------------------------------------------

## 12. Manejo de Errores

-   Validación de formularios.
-   Mensajes claros al usuario.
-   Prevención de datos inválidos.

------------------------------------------------------------------------

## 13. Despliegue

### Fase actual:

-   Entorno local.

### Futuro:

-   GitHub (repositorio).
-   VPS + Dokploy.

------------------------------------------------------------------------

## 14. Roadmap Futuro

-   Roles y permisos avanzados.
-   Integración con Supabase.
-   Automatizaciones.
-   Notificaciones.
-   Integración con email/WhatsApp.

------------------------------------------------------------------------

## 15. Métricas de Éxito

-   Número de leads gestionados.
-   Tasa de conversión.
-   Tiempo de seguimiento.
-   Uso del sistema por usuarios.

------------------------------------------------------------------------

## 16. Riesgos

-   Escalabilidad limitada (SQLite).
-   Dependencia inicial del frontend.
-   Falta de control de roles.

------------------------------------------------------------------------

## 17. Conclusión

Este CRM MVP permitirá validar rápidamente el flujo de ventas y la
gestión de leads, sentando las bases para una solución más robusta y
escalable en futuras iteraciones.
