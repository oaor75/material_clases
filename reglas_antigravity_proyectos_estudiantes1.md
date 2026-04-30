# 📐 Reglas del Proyecto en Antigravity
### Curso: Inteligencia Artificial Práctica para Crear Soluciones de Negocio

> **¿Para qué sirven estas reglas?**
> Estas reglas son tu brújula al construir proyectos reales en Antigravity. No son restricciones: son los hábitos de trabajo de los profesionales que crean soluciones con IA. Seguirlas te ahorrará horas de frustraciones, errores innecesarios y trabajo repetido.

---

## 🧭 Regla 1 — Planifica antes de construir

> *"Un buen plan evita el 80% de los problemas."*

### Lo que debes hacer:
- **Antes de crear o modificar cualquier flujo, agente o automatización**, escribe un plan paso a paso y valídalo con tu docente o compañero de equipo.
- Trabaja en **pasos pequeños y verificables**. No intentes construir toda la solución de un solo intento.
- Al terminar cada tarea, genera una evidencia (captura de pantalla, video corto, lista de verificación) que demuestre que funciona correctamente.

### ❌ Evita:
- Lanzarte a construir sin entender bien qué necesitas lograr.
- Hacer cambios masivos a un flujo que ya funciona parcialmente.
- Dar una tarea por terminada sin haberla probado.

### ✅ Ejemplo práctico:
> Antes de automatizar la recolección de datos de clientes, escribe: *"Paso 1: Definir qué datos necesito. Paso 2: Conectar el formulario. Paso 3: Probar con un dato falso. Paso 4: Validar que el dato llegue a la base de datos."*

---

## 🧹 Regla 2 — Simplicidad y orden en tu solución

> *"Si no lo entiendes en 30 segundos, es demasiado complejo."*

### Lo que debes hacer:
- **Construye soluciones simples, claras y fáciles de entender**. No uses funciones complicadas si existe una forma sencilla de lograr lo mismo.
- **Divide tu solución en partes pequeñas**, cada una con un único propósito claro. Por ejemplo: un agente que solo responde preguntas, otro que solo guarda datos.
- **Nombra todo con claridad**: tus agentes, flujos, variables y nodos deben tener nombres que expliquen para qué sirven, no nombres genéricos como "flujo1" o "agente_nuevo".

### ❌ Evita:
- Crear un único flujo gigante que haga todo a la vez.
- Copiar y pegar la misma lógica en varios lugares (mejor crea un módulo reutilizable).
- Usar nombres crípticos o números como identificadores.

### ✅ Ejemplo práctico:
> En lugar de un flujo llamado `proceso_principal`, crea tres flujos separados: `recibir_solicitud_cliente`, `validar_datos_formulario` y `enviar_respuesta_email`.

---

## 💬 Regla 3 — Comunica tu trabajo con claridad

> *"Quien no puede explicar lo que hace, no lo entiende del todo."*

### Lo que debes hacer:
- **Sé directo y concreto** al presentar avances: muestra el plan, el estado actual de las tareas o el resultado, sin rodeos innecesarios.
- **Si algo no te convence de una tarea**, detente y consulta antes de continuar. Es mejor preguntar dos minutos que rehacer dos horas.
- **Cuando modifiques un flujo existente**, anota exactamente qué cambiaste y por qué, para que tú mismo o tu equipo puedan entenderlo después.

### ❌ Evita:
- Entregar una solución sin explicar cómo funciona.
- Ejecutar una instrucción que no comprendes bien esperando que "salga bien".
- Hacer cambios silenciosos en flujos compartidos con tu equipo.

### ✅ Ejemplo práctico:
> Al presentar tu avance: *"Modifiqué el agente de respuestas porque estaba fallando cuando el usuario escribía en mayúsculas. Ahora convierto el texto a minúsculas antes de procesarlo."*

---

## 🛡️ Regla 4 — Programa pensando en los errores

> *"Los sistemas reales fallan. Un buen desarrollador lo anticipa."*

### Lo que debes hacer:
- **Nunca asumas que el "camino feliz" siempre ocurrirá.** ¿Qué pasa si el usuario no llena un campo? ¿Si la API no responde? ¿Si los datos llegan en un formato distinto?
- **Incluye siempre un manejo de errores** en tus flujos: mensajes amigables cuando algo falla, flujos alternativos, o al menos una alerta para el administrador.
- **Ante un error que no se resuelve fácilmente**, analiza el problema antes de cambiar cosas al azar. Describe tu hipótesis, revisa los logs y aplica una solución basada en lo que encontraste.

### ❌ Evita:
- Dejar flujos sin manejo de errores esperando que nunca fallen.
- Cambiar configuraciones aleatoriamente esperando que el problema se "arregle solo".
- Ignorar mensajes de error pensando que no son importantes.

### ✅ Ejemplo práctico:
> Si tu agente recibe un formulario vacío, debe responder: *"Por favor completa todos los campos requeridos"*, no simplemente fallar en silencio o mostrar un error técnico al usuario.

---

## 🔒 Regla 5 — Seguridad y buenas prácticas desde el inicio

> *"Un error de seguridad puede echar a perder toda una solución."*

### Lo que debes hacer:
- **Nunca expongas datos sensibles** (contraseñas, claves de API, datos personales de usuarios) en flujos visibles o en mensajes de prueba.
- **Antes de ejecutar una automatización sobre datos reales**, pruébala con datos ficticios o de prueba.
- Si detectas que una instrucción o configuración puede representar un riesgo (para los datos, para los usuarios o para el proyecto), **detente y consulta con tu docente** antes de continuar.

### ❌ Evita:
- Usar datos reales de clientes para hacer pruebas.
- Compartir credenciales de acceso por mensajes o chats del curso.
- Omitir permisos o validaciones pensando que "solo es una prueba".

### ✅ Ejemplo práctico:
> Para probar tu flujo de registro de usuarios, usa datos inventados: *nombre: "Ana Prueba", email: "prueba@test.com"*, nunca información real de personas.

---

## 📋 Lista de Verificación del Proyecto

Antes de entregar cualquier avance o versión final de tu proyecto, responde estas preguntas:

| # | Pregunta de verificación | ✅ / ❌ |
|---|--------------------------|--------|
| 1 | ¿Tienes un plan documentado de lo que construiste? | |
| 2 | ¿Cada parte de tu solución tiene un nombre claro y descriptivo? | |
| 3 | ¿Probaste tu solución con datos de prueba (no reales)? | |
| 4 | ¿Tu solución maneja al menos el error más probable? | |
| 5 | ¿Puedes explicar en 2 minutos qué hace tu solución y cómo? | |
| 6 | ¿Documentaste los cambios importantes que hiciste? | |
| 7 | ¿No hay datos sensibles expuestos en tu flujo? | |

---

## 🚀 Recuerda

> Estas reglas no son obstáculos — son la diferencia entre un proyecto que funciona una vez y una solución profesional que puede escalar, mantenerse y crecer.
>
> No importa si es tu primer proyecto con IA: los mejores desarrolladores del mundo siguen estas mismas reglas cada día.

---

*Curso: IA Práctica para Crear Soluciones de Negocio con Antigravity · Versión 1.0*
