# Setup Guide: Demo P2 — Recordatorios de prácticas

**Estado:** Listo para ejecutar
**Tiempo estimado:** 8-10 horas
**Costo:** €0 (Cloud API sandbox) + €24/mes si no tenés n8n Starter

---

## Paso 0: Cuentas necesarias

Antes de empezar, asegurate de tener estas 3 cuentas:

- [ ] **Google Account** — para Google Sheets (probablemente ya tenés)
- [ ] **Meta Business Account** — para WhatsApp Cloud API
- [ ] **n8n Cloud** — para los workflows

---

## Paso 1: Crear cuenta Meta Business + WhatsApp Cloud API (30 min)

### 1.1 Meta Business Account

1. Ir a https://business.facebook.com
2. Crear cuenta de empresa (o usar una existente)
3. No hace falta verificar la empresa para el sandbox

### 1.2 App de Meta con WhatsApp

1. Ir a https://developers.facebook.com
2. Click "My Apps" → "Create App"
3. Tipo: "Business"
4. Nombre: "Autoescuela Demo" (o lo que quieras)
5. En el dashboard de la app → "Add Product" → **WhatsApp** → "Set Up"

### 1.3 Configurar WhatsApp sandbox

1. En la sección WhatsApp → "API Setup" vas a ver:
   - Un **número de prueba** que Meta te asigna (el que "envía")
   - Un **token temporal** (dura 24h, se regenera)
   - Un campo para agregar **números de test** (los que "reciben")
2. Agregar tu número de teléfono como test recipient
3. Agregar un segundo número (otro móvil tuyo, o pedile a alguien) para simular el segundo alumno
4. Meta te envía un código de verificación a cada número — confirmalo

### 1.4 Generar token permanente

El token temporal caduca. Para el demo conviene uno permanente:

1. En Meta Business → System Users → crear un System User
2. Asignarle permisos de WhatsApp
3. Generar token permanente

> **Si esto te parece mucho:** usá el temporal y regeneralo antes de cada
> sesión de trabajo. Para la demo de 1 día alcanza.

### 1.5 Datos que vas a necesitar después

Anotá estos 3 valores, los vas a usar en n8n:

```
WHATSAPP_PHONE_NUMBER_ID = 1052519621273741
WHATSAPP_SENDER_NUMBER = +15551776545
WHATSAPP_ACCESS_TOKEN = (token temporal — regenerar antes de cada sesión)
TEST_RECIPIENT_1 = +34XXXXXXXXX (tu número)
TEST_RECIPIENT_2 = +34XXXXXXXXX (segundo número para simular segundo alumno)
```

> **Nota:** El portal de Meta Developers cambia de layout frecuentemente.
> Los pasos 1.1-1.3 de esta guía pueden no coincidir con la navegación actual.
> Lo importante es llegar a la sección de API Setup de WhatsApp dentro de tu app.

---

## Paso 2: Armar Google Sheets (20 min)

### 2.1 Crear spreadsheet

1. Ir a https://sheets.google.com → nuevo spreadsheet
2. Nombre: "Autoescuela Demo — Agenda Prácticas"

### 2.2 Hoja 1: "agenda"

Crear estas columnas en fila 1:

| A | B | C | D | E | F | G | H |
|---|---|---|---|---|---|---|---|
| alumno_nombre | alumno_telefono | fecha | hora | instructor | punto_recogida | recordatorio_enviado | estado |

Datos fake (fila 2 en adelante):

```
María López    | +34600111222 | 2026-03-25 | 10:00 | Antonio | Av. Andalucía 15  | no | pendiente
Carlos Ruiz    | +34600333444 | 2026-03-25 | 11:00 | Antonio | Plaza Mayor 3      | no | pendiente
Laura García   | +34600555666 | 2026-03-25 | 12:00 | Marta   | Av. Andalucía 15   | no | pendiente
Pedro Sánchez  | +34600777888 | 2026-03-26 | 09:00 | Antonio | C/ Larios 8        | no | pendiente
```

> **Para la demo:** reemplazá el teléfono de María López con TU número real
> y el de Carlos Ruiz con tu SEGUNDO número. El resto no importa.

### 2.3 Hoja 2: "lista_espera"

| A | B | C | D |
|---|---|---|---|
| alumno_nombre | alumno_telefono | horario_preferido | prioridad |

```
Ana Martín       | +34600999000 | mañana    | 1
Diego Fernández  | +34600111333 | cualquiera | 2
```

> Reemplazá el teléfono de Ana Martín con tu SEGUNDO número.

### 2.4 Hoja 3: "log"

| A | B | C | D | E |
|---|---|---|---|---|
| timestamp | alumno | accion | clase_original | reasignado_a |

Dejar vacía — se llena automáticamente.

### 2.5 Conectar Sheets con n8n

1. En Google Cloud Console (https://console.cloud.google.com):
   - Crear proyecto "autoescuela-demo"
   - Habilitar Google Sheets API
   - Crear credenciales tipo "Service Account"
   - Descargar JSON de credenciales
2. En el spreadsheet → Compartir con el email del Service Account (permiso Editor)

> **Alternativa más rápida:** n8n tiene autenticación OAuth con Google integrada.
> Cuando configures el nodo Google Sheets en n8n, te va a pedir conectar tu cuenta
> Google directamente. Es más fácil que Service Account.

---

## Paso 3: Workflow 1 — Recordatorio 24h antes (2-3h)

### Qué hace
Cada día a las 09:00, lee la agenda, busca clases de mañana, y envía recordatorio.

### Nodos en n8n (en orden)

```
[Cron Trigger] → [Google Sheets: leer agenda] → [Filter: clases de mañana]
    → [Filter: recordatorio_enviado = no] → [WhatsApp: enviar mensaje]
    → [Google Sheets: marcar recordatorio_enviado = sí]
```

### Detalle de cada nodo

**1. Cron Trigger**
- Se ejecuta todos los días a las 09:00
- Para testing: poné un trigger manual también (botón "Execute Workflow")

**2. Google Sheets — Read Rows**
- Spreadsheet: "Autoescuela Demo — Agenda Prácticas"
- Sheet: "agenda"
- Lee todas las filas

**3. IF — Filtrar clases de mañana**
- Condición: `fecha == mañana` (usar nodo Date/Time para calcular fecha de mañana)
- Solo pasan las clases del día siguiente

**4. IF — Filtrar no enviados**
- Condición: `recordatorio_enviado == "no"`

**5. HTTP Request — Enviar WhatsApp**
- Method: POST
- URL: `https://graph.facebook.com/v21.0/{{PHONE_NUMBER_ID}}/messages`
- Headers: `Authorization: Bearer {{ACCESS_TOKEN}}`
- Body (JSON):

```json
{
  "messaging_product": "whatsapp",
  "to": "{{alumno_telefono}}",
  "type": "interactive",
  "interactive": {
    "type": "button",
    "body": {
      "text": "Hola {{alumno_nombre}} 👋\n\nRecordatorio: mañana tienes clase práctica.\n\n📅 {{fecha}}, {{hora}}h\n🚗 Instructor: {{instructor}}\n📍 Recogida: {{punto_recogida}}\n📋 Trae: DNI y gafas (si las necesitas)\n\n¿Puedes asistir?"
    },
    "action": {
      "buttons": [
        {
          "type": "reply",
          "reply": {
            "id": "confirmar",
            "title": "✅ Confirmo"
          }
        },
        {
          "type": "reply",
          "reply": {
            "id": "cancelar",
            "title": "❌ No puedo ir"
          }
        }
      ]
    }
  }
}
```

> **Nota:** En el sandbox, los mensajes interactivos con botones funcionan.
> No necesitás templates pre-aprobados para mensajes tipo `interactive`.
> Pero el destinatario tiene que haber enviado un mensaje primero en las últimas 24h
> (ventana de servicio). Para la demo, asegurate de escribir algo al número
> de prueba antes de lanzar el workflow.

**6. Google Sheets — Update Row**
- Actualizar la fila del alumno: `recordatorio_enviado = "sí"`

---

## Paso 4: Workflow 2 — Procesar respuesta (2-3h)

### Qué hace
Cuando un alumno toca un botón, procesa la respuesta y actúa.

### Nodos en n8n

```
[Webhook Trigger] → [Switch: confirmar/cancelar]
    ├── confirmar → [WhatsApp: "Perfecto, te esperamos"] → [Sheets: estado=confirmado] → [Sheets: log]
    └── cancelar  → [WhatsApp: "Entendido, cancelamos"] → [Sheets: estado=cancelado] → [Sheets: log] → [Trigger Workflow 3]
```

### Configurar Webhook en Meta

1. En Meta Developers → WhatsApp → Configuration
2. Webhook URL: la URL que te da n8n al crear el nodo Webhook
3. Verify Token: poné lo que quieras (ej: "autoescuela-demo-2026")
4. Suscribir al campo: `messages`

> **Problema común:** n8n Cloud te da una URL tipo
> `https://tu-instancia.app.n8n.cloud/webhook/xxxx`. Meta necesita que
> responda al challenge de verificación. El nodo Webhook de n8n lo maneja
> automático. Si no funciona, buscá "n8n whatsapp webhook" en el foro de n8n.

### Detalle del Switch

El payload de WhatsApp cuando tocan un botón viene así:

```json
{
  "entry": [{
    "changes": [{
      "value": {
        "messages": [{
          "from": "34600111222",
          "type": "interactive",
          "interactive": {
            "type": "button_reply",
            "button_reply": {
              "id": "confirmar",    ← esto es lo que chequeás
              "title": "✅ Confirmo"
            }
          }
        }]
      }
    }]
  }]
}
```

- Si `button_reply.id == "confirmar"` → rama confirmar
- Si `button_reply.id == "cancelar"` → rama cancelar

---

## Paso 5: Workflow 3 — Ofrecer hueco a lista de espera (1-2h)

### Qué hace
Cuando un alumno cancela, busca al siguiente en lista de espera y le ofrece el hueco.

### Nodos en n8n

```
[Trigger: llamado por WF2] → [Sheets: leer lista_espera]
    → [Sort: por prioridad] → [Limit: 1] → [WhatsApp: ofrecer hueco]
```

### Mensaje

```json
{
  "messaging_product": "whatsapp",
  "to": "{{alumno_telefono}}",
  "type": "interactive",
  "interactive": {
    "type": "button",
    "body": {
      "text": "¡Buenas noticias, {{alumno_nombre}}! 🎉\n\nSe ha liberado un hueco de práctica:\n\n📅 {{fecha}}, {{hora}}h\n🚗 Instructor: {{instructor}}\n\n¿Lo quieres?"
    },
    "action": {
      "buttons": [
        {
          "type": "reply",
          "reply": {
            "id": "aceptar_hueco",
            "title": "✅ Sí, lo reservo"
          }
        },
        {
          "type": "reply",
          "reply": {
            "id": "rechazar_hueco",
            "title": "❌ No, gracias"
          }
        }
      ]
    }
  }
}
```

> La respuesta a este mensaje la procesa el Workflow 2 (mismo webhook).
> Agregá dos ramas más al Switch: `aceptar_hueco` y `rechazar_hueco`.

---

## Paso 6: Test end-to-end (1h)

### Checklist de prueba

- [ ] Ejecutar WF1 manualmente → ¿llega el recordatorio a tu WhatsApp?
- [ ] Tocar "Confirmo" → ¿llega respuesta? ¿se actualiza Sheets?
- [ ] Ejecutar WF1 de nuevo con otro alumno → tocar "No puedo ir"
- [ ] ¿Le llega oferta de hueco al segundo número (lista de espera)?
- [ ] ¿El log en Sheets registra todo?
- [ ] Abrir Sheets → ¿los estados son correctos?

### Troubleshooting común

| Problema | Causa probable | Solución |
|----------|---------------|----------|
| No llega el mensaje | Token expirado | Regenerar token en Meta Developers |
| No llega el mensaje | Número no registrado como tester | Agregar en API Setup |
| Botones no aparecen | Formato JSON incorrecto | Verificar que `type: "interactive"` está bien |
| Webhook no dispara WF2 | URL incorrecta en Meta | Copiar URL exacta del nodo Webhook de n8n |
| Sheets no se actualiza | Permisos de Service Account | Compartir sheet con el email del SA |

---

## Paso 7: Preparar demo para el dueño (30 min)

### Antes de la reunión

1. Cambiar fecha de las clases fake a "mañana" respecto al día de la demo
2. Poner el teléfono del dueño como "María López"
3. Poner tu teléfono como "Ana Martín" (lista de espera)
4. Verificar que todo funciona con un dry run
5. Tener Google Sheets abierto en tu laptop para mostrarlo en tiempo real

### Script de la demo

**Minuto 0-1: Contexto**
> "Te voy a mostrar algo funcionando. No es una presentación, es real.
> Mirá tu WhatsApp."

**Minuto 1-2: Recordatorio**
> [Ejecutar WF1] → le llega el recordatorio
> "Esto le llegaría automáticamente a cada alumno 24h antes de su práctica.
> Tocá 'No puedo ir'."

**Minuto 2-3: Reasignación**
> [Le muestras tu teléfono] "Esto es lo que recibe automáticamente
> el siguiente alumno de la lista de espera."
> [Abrís Sheets] "Y acá ves todo: quién confirmó, quién canceló,
> a quién se le ofreció el hueco."

**Minuto 3: Cierre**
> "¿Cuántas clases se te caen por semana por no-shows?"
> [Escuchar. No vender todavía. Solo escuchar.]

---

## Archivos en el repo

```
clients/autoescuela-malaga/
├── problem-definition.md        ← ya existe
├── p2-demo-proposal.md          ← ya existe
├── setup-guide.md               ← este documento
└── workflows/                   ← crear cuando construyas los workflows
    ├── wf1-recordatorio.json    ← export de n8n
    ├── wf2-procesar-respuesta.json
    └── wf3-reasignar-hueco.json
```

> Cuando construyas cada workflow en n8n, exportalo como JSON y guardalo acá.
> Así tenés backup y podés reutilizarlo para el próximo cliente.
