# Backlog: Autoescuela Demo

## Pendiente

### WF1-001: Fusionar los dos nodos IF en uno solo
**Prioridad:** Baja
**Esfuerzo:** 5 min
**Descripcion:** Actualmente hay dos nodos IF separados: uno filtra por fecha = manana y otro por recordatorio_enviado = no. n8n soporta condiciones combinadas en un solo IF. Fusionarlos reduce un nodo y simplifica el flujo visual.

### WF1-002: Renombrar nodos con nombres descriptivos
**Prioridad:** Baja
**Esfuerzo:** 5 min
**Descripcion:** Los nodos se llaman "If", "If1", "Code", "Code in JavaScript1". Renombrarlos a nombres descriptivos: "Filtro: clase manana", "Filtro: no enviado", "Armar mensaje WA", "Prep update sheet". Mejora mantenibilidad a largo plazo.

### WF1-003: Agregar columna `id` a la hoja agenda
**Prioridad:** Media
**Esfuerzo:** 10 min
**Descripcion:** No hay ID unico por fila. El matching actual usa `alumno_nombre`, que se rompe si hay dos alumnos con el mismo nombre. Agregar columna `id` (numero secuencial) como primera columna y usarla para el matching en el Update. Actualizar ambos Code nodes y el nodo Google Sheets Update.

### WF1-004: Cambiar `$('Code').all()` por `$input.all()` en Code1
**Prioridad:** Media
**Esfuerzo:** 2 min
**Descripcion:** El segundo Code node referencia el primero por nombre exacto (`$('Code')`). Si se renombra el primer nodo, se rompe silenciosamente. Cambiar a `$input.all()` para que tome datos del nodo anterior sin depender del nombre.

### WF1-005: Usar variables de entorno para IDs hardcodeados
**Prioridad:** Media (requerido para produccion)
**Esfuerzo:** 15 min
**Descripcion:** El Phone Number ID (`1052519621273741`) esta hardcodeado en la URL del HTTP Request. El Document ID de Google Sheets esta hardcodeado en dos nodos. Mover a variables de entorno de n8n para no tener que editar nodos al cambiar de numero o spreadsheet.

### WF1-006: Agregar Error Trigger global
**Prioridad:** Media (requerido para produccion)
**Esfuerzo:** 15 min
**Descripcion:** Agregar un nodo Error Trigger al workflow que notifique (por WhatsApp o email) si cualquier nodo falla. Actualmente los errores pasan desapercibidos si el workflow corre por Schedule Trigger.

### WF2-001: Implementar idempotencia en webhook
**Prioridad:** Media (requerido para produccion)
**Esfuerzo:** 30 min
**Descripcion:** El webhook de WhatsApp puede dispararse dos veces por el mismo mensaje. Guardar el `message_id` en la hoja log y chequear antes de procesar. Si ya existe, ignorar el mensaje duplicado.

### WF3-001: Agregar timeout para oferta de hueco
**Prioridad:** Baja
**Esfuerzo:** 1h
**Descripcion:** Cuando se ofrece un hueco a un alumno de lista de espera y no responde en 30 min, ofrecer al siguiente. Implementar como Schedule Trigger separado (WF3b) que chequee ofertas sin respuesta.

## Completado

(vacio)
