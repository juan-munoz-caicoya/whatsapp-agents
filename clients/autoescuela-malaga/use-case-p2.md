# Use Case P2: Recordatorio y confirmación de clases prácticas

**Fecha:** 2026-03-19
**Estado:** v1 — aprobado para construir
**Cliente:** Build partner — autoescuela en Málaga

---

## Problema

Los alumnos no se presentan a clases prácticas sin avisar. El instructor
pierde el slot (45 min), la autoescuela pierde 25-40€ por clase, y otro
alumno que podría haber usado ese hueco se queda sin practicar.

---

## Solución

Bot de WhatsApp que el día anterior envía un recordatorio automático
al alumno con opción de confirmar o reagendar. Si no responde, insiste
una vez. Si sigue sin responder, avisa a la autoescuela.

---

## v1 — MVP (lo que construimos ahora)

### Actores

| Actor | Rol en el flujo |
|-------|----------------|
| **Alumno** | Recibe recordatorio, confirma o reagenda |
| **Secretaría/Dueño** | Recibe notificaciones de cambios y no-respuestas |
| **Bot** | Envía mensajes, procesa respuestas, actualiza agenda |

### Flujo principal

```
[09:00 día anterior]
Bot lee agenda → encuentra clases de mañana → envía recordatorio a cada alumno

RECORDATORIO:
┌─────────────────────────────────────────────┐
│ Hola María 👋                                │
│                                              │
│ Recordatorio: mañana tienes clase práctica.  │
│                                              │
│ 📅 Martes 25 de marzo, 10:00h               │
│ 🚗 Instructor: Antonio                      │
│ 📍 Recogida: Av. de Andalucía 15            │
│ 📋 Trae: DNI y gafas (si las necesitas)     │
│                                              │
│ ┌──────────────┐  ┌────────────────┐        │
│ │ ✅ Confirmo   │  │ 🔄 Reagendar  │        │
│ └──────────────┘  └────────────────┘        │
└─────────────────────────────────────────────┘
```

### Rama 1: Confirma

```
Alumno toca "✅ Confirmo"
  │
  └─ Bot responde:
     "Perfecto, te esperamos mañana a las 10:00. ¡Buena clase! 🚗"
  │
  └─ Actualiza agenda: estado = "confirmado"
  │
  └─ FIN
```

### Rama 2: Reagenda

```
Alumno toca "🔄 Reagendar"
  │
  └─ Bot responde con horarios disponibles:
     ┌──────────────────────────────────────────┐
     │ Sin problema. Estos son los próximos      │
     │ horarios disponibles con Antonio:         │
     │                                           │
     │ ┌─────────────────────────────┐           │
     │ │ 1️⃣ Miércoles 26, 10:00h    │           │
     │ └─────────────────────────────┘           │
     │ ┌─────────────────────────────┐           │
     │ │ 2️⃣ Jueves 27, 11:00h       │           │
     │ └─────────────────────────────┘           │
     │ ┌─────────────────────────────┐           │
     │ │ 3️⃣ Viernes 28, 09:00h      │           │
     │ └─────────────────────────────┘           │
     └──────────────────────────────────────────┘
  │
  └─ Alumno elige opción (ej: toca "2️⃣ Jueves 27, 11:00h")
  │
  └─ Bot confirma:
     "Listo ✅ Tu clase queda para el jueves 27 a las 11:00h
     con Antonio en Av. de Andalucía 15. ¡Nos vemos!"
  │
  └─ Actualiza agenda:
     - Clase original: estado = "reagendado"
     - Nueva clase: crea fila con fecha/hora nueva
  │
  └─ Notifica a secretaría/dueño:
     "📋 María López reagendó su clase:
     ❌ Martes 25, 10:00h → ✅ Jueves 27, 11:00h
     Instructor: Antonio"
  │
  └─ FIN
```

### Rama 3: No responde

```
[4 horas después del primer mensaje — 13:00h]
Bot detecta: alumno no respondió
  │
  └─ Bot envía segundo aviso:
     "Hola María, ¿nos confirmas tu clase de mañana a las 10:00h?
     Necesitamos saber para organizar la agenda 🙏"
     [✅ Confirmo] [🔄 Reagendar]
  │
  ├─ Si responde → mismas ramas 1 o 2
  │
  └─ Si no responde en 3h más (16:00h):
     │
     └─ Notifica a secretaría/dueño:
        "⚠️ María López no confirmó su clase de mañana
        Martes 25, 10:00h con Antonio.
        Puede que no se presente."
     │
     └─ Marca en agenda: estado = "sin_confirmar"
     │
     └─ FIN (la secretaría decide si llama o no)
```

### Datos que maneja

| Dato | Dónde | Quién lo llena |
|------|-------|---------------|
| Agenda de clases (alumno, fecha, hora, instructor, punto) | Google Sheets — hoja "agenda" | Autoescuela (manual por ahora) |
| Horarios disponibles para reagendar | Google Sheets — hoja "disponibilidad" | Autoescuela (manual por ahora) |
| Log de confirmaciones/reagendas | Google Sheets — hoja "log" | Bot (automático) |
| Número de WhatsApp de secretaría | Config de n8n | Juan (una vez) |

### Límites del MVP

- La autoescuela carga la agenda manualmente en Google Sheets
- Los horarios disponibles para reagendar se cargan manualmente
- No hay integración con software de gestión de la autoescuela
- No hay IA conversacional — solo botones y flujos predefinidos
- Máximo 3 opciones de reagenda (límite de botones de WhatsApp)

---

## v2 — Después de validar con el build partner

| Feature | Descripción | Trigger para construirlo |
|---------|------------|--------------------------|
| **Ofrecer hueco a lista de espera** | Cuando un alumno cancela/reagenda, el hueco liberado se ofrece automáticamente al siguiente alumno en lista de espera | El dueño confirma que tiene alumnos esperando huecos |
| **Recordatorio día del examen** | Mismo flujo pero para exámenes teóricos/prácticos. Incluye checklist específica (DNI, documentación DGT) | El dueño dice que los no-shows en exámenes también son problema |
| **Respuestas con IA** | Si el alumno escribe texto libre en vez de tocar botón, Claude interpreta la intención y responde | Vemos que los alumnos escriben en vez de usar botones |
| **Dashboard de métricas** | Vista simple: tasa de confirmación, no-shows evitados, reagendas por semana | Tenemos 4+ semanas de datos |

---

## v3 — Escalar a otros problemas (P1, P3)

| Feature | Descripción | Trigger para construirlo |
|---------|------------|--------------------------|
| **FAQ bot** (P3) | Bot que responde preguntas frecuentes: precios, documentación, horarios, proceso de matrícula | Validamos que P2 funciona y el dueño quiere más |
| **Captura de leads** (P1) | Bot que responde fuera de horario a gente nueva que pregunta por el carnet. Captura nombre, edad, tipo de permiso. Envía presupuesto automático | El dueño confirma que pierde leads fuera de horario |
| **Follow-up a leads** | Mensaje automático a las 24h y 72h a leads que preguntaron pero no se matricularon | Tenemos datos de leads capturados |
| **Reagenda inteligente** | En vez de mostrar 3 opciones fijas, consultar disponibilidad real del instructor en tiempo real | La autoescuela adopta un sistema de agenda digital |

---

## v4 — Retención y ciclo completo

| Feature | Descripción | Trigger para construirlo |
|---------|------------|--------------------------|
| **Reactivar alumnos inactivos** (P4) | Mensaje automático si un alumno lleva 2+ semanas sin actividad: "¿Cómo vas? ¿Necesitas ayuda?" | Tenemos acceso a datos de progreso del alumno |
| **Estado de lista de espera** (P5) | Respuesta automática a "¿cuándo me toca examen?". Gestión de expectativas | El dueño confirma que es un dolor real |
| **Onboarding automático** | Guía paso a paso por WhatsApp para alumnos nuevos: documentación, psicotécnico, primer día | Tenemos el flujo de matrícula mapeado con datos reales |
| **Encuesta de satisfacción** | Mensaje automático post-examen: "¿Cómo fue tu experiencia?" + NPS | Tenemos suficientes alumnos pasando por el sistema |

---

## Métricas de éxito

### Para el MVP (v1)

| Métrica | Cómo la medimos | Objetivo |
|---------|----------------|----------|
| Tasa de confirmación | Confirmados / recordatorios enviados | >70% |
| No-shows | Alumnos que no se presentaron a pesar del recordatorio | <10% (vs ~15-20% sin sistema) |
| Reagendas exitosas | Alumnos que reagendaron / total cancelaciones | >50% |
| Tiempo de respuesta | Media de tiempo entre recordatorio y respuesta del alumno | <2h |

### Para el dueño (lo que le importa)

| Lo que ve | Cómo lo expresamos |
|-----------|-------------------|
| Menos huecos vacíos | "Esta semana se hubieran caído 4 clases. Con el bot, solo 1 fue no-show" |
| Menos tiempo al teléfono | "No tuviste que llamar a nadie para confirmar" |
| Agenda más predecible | "A las 13:00 ya sabés quién viene y quién no" |

---

## Decisiones tomadas

| Decisión | Alternativa descartada | Por qué |
|----------|----------------------|---------|
| Botones de WhatsApp (no texto libre) | Respuesta abierta con IA | Más simple, más rápido de construir, menor tasa de error |
| Google Sheets como agenda | Supabase, Airtable | El dueño probablemente usa Excel/papel. Sheets es lo más parecido |
| 3 opciones de reagenda fijas | Consulta dinámica de disponibilidad | Evita integración compleja. Las opciones las carga la autoescuela |
| Recordatorio a las 09:00 | Tarde anterior, mañana de la clase | 24h antes da tiempo a reagendar y ofrecer hueco |
| Segundo aviso a las 4h | 2h, 6h, no segundo aviso | 4h es suficiente sin ser agresivo |
| Notificación a secretaría por WhatsApp | Email, dashboard | El dueño ya vive en WhatsApp. No le vamos a pedir que abra otra cosa |
