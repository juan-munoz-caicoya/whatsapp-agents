# Propuesta técnica: Demo P2 — Reducir no-shows en prácticas

**Fecha:** 2026-03-19
**Agente:** cto
**Objetivo:** Construir un demo funcional con datos ficticios que el dueño
de la autoescuela pueda ver y usar desde su propio WhatsApp.

---

## Qué vamos a construir

Un sistema que envía recordatorios automáticos de clases prácticas por WhatsApp
y permite al alumno confirmar o cancelar con un toque. Si cancela, ofrece el
hueco a otro alumno automáticamente.

**Lo que el dueño va a ver en la demo:**
1. Le llega un WhatsApp como si fuera un alumno: recordatorio de clase práctica
2. Puede tocar "Confirmo" o "No puedo ir"
3. Si toca "No puedo ir", ve cómo otro alumno (ficticio) recibe la oferta del hueco
4. Puede ver un resumen: quién confirmó, quién canceló, quién no respondió

El dueño lo experimenta en primera persona. No le explicamos nada — lo usa y entiende.

---

## Arquitectura

```
Google Sheets          n8n                    WhatsApp
(agenda fake)  ──────► (3 workflows) ──────► (número de prueba)
                           │
                           ▼
                    Google Sheets
                    (log de respuestas)
```

**4 piezas, nada más:**

| Pieza | Qué es | Costo |
|-------|--------|-------|
| **Google Sheets** | La "base de datos". Una hoja con la agenda de prácticas fake y otra con la lista de espera | €0 |
| **n8n Cloud Starter** | El cerebro. 3 workflows que leen la agenda, envían mensajes y procesan respuestas | €24/mes (ya en el plan) |
| **WhatsApp Business API** | El canal. Para enviar mensajes con botones interactivos | Sandbox gratis / o BSP de prueba |
| **Tu teléfono** | Para recibir los mensajes como si fueras el alumno y el dueño | €0 |

**Costo total del demo: €0-24/mes** (si ya tenés n8n Starter).

---

## Los 3 workflows de n8n

### Workflow 1: "Recordatorio 24h antes"

**Trigger:** Cron job, se ejecuta 1 vez al día a las 09:00.

**Lógica:**
1. Lee Google Sheets → busca clases de mañana
2. Para cada clase, envía WhatsApp al alumno:

```
Hola [nombre] 👋

Recordatorio: mañana tienes clase práctica.

📅 Martes 25 de marzo, 10:00h
🚗 Instructor: Antonio
📍 Punto de recogida: Av. de Andalucía 15
📋 Recuerda traer: DNI y gafas (si las necesitas)

¿Puedes asistir?
```

Con 2 botones interactivos (WhatsApp interactive message):
- ✅ **Confirmo**
- ❌ **No puedo ir**

3. Marca en la hoja: "recordatorio_enviado = sí"

---

### Workflow 2: "Procesar respuesta del alumno"

**Trigger:** Webhook — se activa cuando el alumno toca un botón.

**Si toca ✅ Confirmo:**
1. Responde: "Perfecto, te esperamos mañana a las 10:00. ¡Buena clase!"
2. Actualiza hoja: estado = "confirmado"

**Si toca ❌ No puedo ir:**
1. Responde: "Entendido, cancelamos tu clase. ¿Quieres reservar otro día?"
2. Actualiza hoja: estado = "cancelado"
3. **Dispara Workflow 3** (reasignar hueco)

**Si no responde en 4 horas:**
1. Segundo recordatorio más corto: "¿Nos confirmas la clase de mañana a las 10:00?"
2. Si sigue sin responder → marca como "sin_confirmar" y notifica a secretaría

---

### Workflow 3: "Ofrecer hueco a lista de espera"

**Trigger:** Llamado por Workflow 2 cuando hay cancelación.

**Lógica:**
1. Lee hoja "lista_de_espera" → busca alumnos que pidieron horario disponible
2. Envía al primer alumno de la lista:

```
¡Buenas noticias, [nombre]!

Se ha liberado un hueco de práctica:
📅 Martes 25 de marzo, 10:00h
🚗 Instructor: Antonio

¿Lo quieres?
```

Con botones:
- ✅ **Sí, lo reservo**
- ❌ **No, gracias**

3. Si acepta → asigna hueco, actualiza agenda
4. Si rechaza → pasa al siguiente de la lista

---

## Estructura de Google Sheets

### Hoja 1: "agenda_practicas"

| alumno_nombre | alumno_telefono | fecha | hora | instructor | punto_recogida | recordatorio_enviado | estado |
|---------------|-----------------|-------|------|------------|----------------|---------------------|--------|
| María López | +34600111222 | 2026-03-25 | 10:00 | Antonio | Av. Andalucía 15 | no | pendiente |
| Carlos Ruiz | +34600333444 | 2026-03-25 | 11:00 | Antonio | Plaza Mayor 3 | no | pendiente |
| Laura García | +34600555666 | 2026-03-25 | 12:00 | Marta | Av. Andalucía 15 | no | pendiente |
| Pedro Sánchez | +34600777888 | 2026-03-26 | 09:00 | Antonio | C/ Larios 8 | no | pendiente |

### Hoja 2: "lista_espera"

| alumno_nombre | alumno_telefono | horario_preferido | prioridad |
|---------------|-----------------|-------------------|-----------|
| Ana Martín | +34600999000 | mañana | 1 |
| Diego Fernández | +34600111333 | cualquiera | 2 |

### Hoja 3: "log_respuestas"

| timestamp | alumno | accion | clase_original | reasignado_a |
|-----------|--------|--------|----------------|-------------|
| 2026-03-24 09:15 | María López | confirmado | 25/03 10:00 | — |
| 2026-03-24 10:30 | Carlos Ruiz | cancelado | 25/03 11:00 | Ana Martín |

---

## WhatsApp: cómo lo resolvemos para el demo

Hay 2 opciones:

**Opción A: WhatsApp Cloud API sandbox (gratis)**
- Meta te da un número de prueba y 5 números de test
- Puedes enviar mensajes con botones interactivos
- Limitación: solo funciona con números que registres como testers
- Costo: €0
- Setup: 30 minutos (necesitas cuenta Meta Business)

**Opción B: BSP con trial gratuito (360dialog, WATI, o WANotifier)**
- Número real de WhatsApp Business
- Trial de 7-14 días gratis
- Sin limitación de números destinatarios
- Costo: €0 durante trial
- Setup: 1-2 horas (verificación de número)

**Recomendación:** Opción A para construir y probar. Opción B solo para el día
de la demo con el dueño, si queremos que le llegue a su número real.

---

## Datos fake para la demo

Para la demo usamos 4 alumnos ficticios. En la reunión con el dueño:
- **Su número es "María López"** → recibe el recordatorio
- Le pedimos que toque "No puedo ir"
- Le mostramos cómo "Ana Martín" (otro número nuestro) recibe la oferta del hueco
- Le mostramos la hoja de Google Sheets actualizada en tiempo real

**Script de la demo (3 minutos):**

> "Mirá tu WhatsApp." [le llega el recordatorio]
>
> "Tocá 'No puedo ir'." [lo toca]
>
> [le mostramos el otro teléfono] "Esto es lo que le llega automáticamente
> al siguiente alumno de la lista de espera."
>
> [abrimos Google Sheets] "Y acá ves todo: quién confirmó, quién canceló,
> y a quién se le ofreció el hueco. Sin que nadie haga nada."
>
> "¿Cuántas clases se te caen por semana?"

---

## Timeline de construcción

| Paso | Qué | Tiempo | Quién |
|------|-----|--------|-------|
| 1 | Crear cuenta Meta Business + Cloud API sandbox | 30 min | Juan |
| 2 | Armar Google Sheets con datos fake | 20 min | Juan |
| 3 | Construir Workflow 1 (recordatorio) en n8n | 2-3h | Juan (con guía) |
| 4 | Construir Workflow 2 (procesar respuesta) en n8n | 2-3h | Juan (con guía) |
| 5 | Construir Workflow 3 (reasignar hueco) en n8n | 1-2h | Juan (con guía) |
| 6 | Probar end-to-end con números de test | 1h | Juan |
| 7 | Preparar script de demo | 30 min | Juan |
| **Total** | | **~8-10 horas** | **1-2 días de trabajo** |

---

## De demo a producción: qué cambia

Cuando el dueño diga "lo quiero", esto es lo que hay que hacer para pasarlo a real:

| Demo | Producción | Esfuerzo |
|------|-----------|----------|
| Google Sheets como agenda | Conectar a su sistema de agenda real (o seguir con Sheets si no tiene otro) | Bajo si usa Sheets/Excel |
| 4 alumnos fake | Sus alumnos reales | Solo cambiar datos |
| WhatsApp sandbox | Número real de WhatsApp Business de la autoescuela | 1-2h setup con BSP |
| Cron a las 09:00 | Mismo cron, ajustar hora si quiere | 5 min |
| Sin Claude API | Agregar IA para respuestas libres (fase 2, no necesario para recordatorios) | Medio |

**Lo importante:** La demo ES el producto. No hay que reconstruir nada.
Solo se cambian los datos y el número de WhatsApp.

---

## Riesgos y mitigaciones

| Riesgo | Mitigación |
|--------|-----------|
| Meta rechaza templates de mensaje | Usar templates genéricos pre-aprobados (appointment_reminder es estándar) |
| El dueño no tiene agenda digital | Ofrecerle Google Sheets como solución incluida en el servicio |
| Alumnos no interactúan con botones | Agregar opción de respuesta libre ("escribe SÍ o NO") como fallback |
| Límite de mensajes en sandbox | Para la demo solo necesitamos 5-10 mensajes, sobra |

---

## Decisión que necesito de vos, Juan

¿Querés construir esto con la Cloud API de Meta (gratis, más técnico)
o preferís usar un BSP con trial gratuito (más fácil, menos control)?

Con la Cloud API aprendés más del stack real. Con el BSP llegas antes a la demo.
**Mi recomendación:** Cloud API. Son 30 minutos más de setup y aprendés cómo
funciona WhatsApp por dentro. Eso te da ventaja para todos los clientes futuros.
