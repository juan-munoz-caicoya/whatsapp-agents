# Problem Definition: Autoescuela — WhatsApp Automation

**Fecha:** 2026-03-19
**Estado:** Pre-discovery (sin entrevista con dueño todavía)
**Agente:** product
**Cliente:** Build partner — autoescuela en Málaga

> **Nota:** Este documento se basa en research online (reseñas, foros, artículos
> del sector, software existente, estudios CNMC). Cada problema debe validarse
> en la discovery call con el dueño. Lo marcado como [A VALIDAR] es hipótesis.

---

## Contexto del sector

| Dato | Valor | Fuente |
|------|-------|--------|
| Autoescuelas en España | ~8.500 | CNMC 2025 |
| Cerradas desde 2018 | 1.350 (-14%) | Autónomos y Emprendedor |
| Facturación media/autoescuela | ~110.000€/año | Código Ganador |
| Empleados promedio | 2 personas (dueño + 1) | Sector data |
| Coste permiso B para alumno | 700-1.600€ | Mercado |
| Usan WhatsApp Business | Solo 18% | Alzado.org |
| Espera examen práctico | 3-7 meses según zona | COPE, Xataka |

**Implicación:** Negocio pequeño, márgenes bajos (5-15%), gestión anticuada,
sector en contracción. Cada alumno perdido duele. Cada hora de secretaria cuenta.

---

## Customer journey del alumno

```
1. Interés       → Busca online, pregunta amigos, pasa por delante
2. Contacto      → Llama, entra al local, o escribe por WhatsApp
3. Info/Precio   → Recibe presupuesto, condiciones, opciones de pago
4. Matrícula     → Entrega docs (DNI, foto, psicotécnico), paga, firma
5. Expediente    → Autoescuela abre expediente en DGT
6. Teórico       → Estudia tests (1-3 meses)
7. Examen teórico
8. Prácticas     → Reserva y hace clases (20-30 clases, 2-4 meses)
9. Lista espera  → Espera fecha examen práctico (3-7 meses)
10. Examen práctico
11. Permiso      → Trámite final
```

**Dónde se pierde valor:**
- Pasos 2-3: leads que no reciben respuesta rápida → se van a otra autoescuela
- Paso 4: fricción documental → abandono antes de empezar
- Paso 6: desmotivación → abandono antes de examinar
- Paso 8: no-shows → dinero perdido por clase vacía
- Paso 9: ansiedad → llamadas repetitivas que saturan secretaría

---

## Los 5 problemas priorizados

### Criterios de priorización

| Criterio | Peso | Por qué |
|----------|------|---------|
| **Impacto en revenue** | 30% | ¿Cuánto dinero pierde o deja de ganar la autoescuela? |
| **Frecuencia** | 25% | ¿Cada cuánto ocurre? Diario > semanal > mensual |
| **Facilidad de automatizar con WA** | 25% | ¿Se puede resolver con un chatbot sin integración compleja? |
| **Visibilidad del resultado** | 20% | ¿El dueño va a notar el cambio rápido? Importa para retención |

### Scoring

| Problema | Revenue (30%) | Frecuencia (25%) | Automatizable (25%) | Visibilidad (20%) | **Total** |
|----------|:---:|:---:|:---:|:---:|:---:|
| P1. Responder FAQs automáticamente | 3 | 5 | 5 | 4 | **4.15** |
| P2. Capturar leads 24/7 | 5 | 4 | 5 | 5 | **4.75** |
| P3. Reducir no-shows en prácticas | 4 | 4 | 5 | 5 | **4.45** |
| P4. Reactivar alumnos inactivos | 4 | 2 | 4 | 3 | **3.35** |
| P5. Informar estado de lista de espera | 2 | 5 | 3 | 3 | **3.15** |

---

### P1. Capturar leads que contactan fuera de horario — Score: 4.75

**El problema:**
Un potencial alumno escribe por WhatsApp a las 21:00 preguntando cuánto cuesta
el carnet. La autoescuela está cerrada. Si no recibe respuesta en minutos, busca
otra autoescuela. La tasa de conversión del primer contacto es solo 20-30%.
El 70% de los leads se pierde sin seguimiento.

**Quién sufre:** Dueño (pierde ingresos), alumno (no recibe respuesta)

**Frecuencia:** Diaria. Picos en septiembre y enero (temporadas altas).

**Impacto cuantificado:**
- Cada alumno perdido = 700-1.600€ de ingreso
- Si pierdes 2 leads/semana por respuesta lenta = **5.600-12.800€/año** [A VALIDAR]
- Una autoescuela en Soria aumentó conversión 40% automatizando envío de presupuesto
  (fuente: neomentor.es)

**Solución con WhatsApp chatbot:**
1. Respuesta instantánea 24/7 a consultas de precio y disponibilidad
2. Captura automática de datos del lead (nombre, edad, tipo de permiso)
3. Envío automático de presupuesto personalizado
4. Follow-up automatizado a las 24h y 72h si no responde

**Métrica de éxito:** Tasa de conversión lead → matrícula (medir antes/después)

**Riesgo:** [A VALIDAR] ¿Realmente reciben leads fuera de horario? ¿Cuántos/semana?

---

### P2. Reducir no-shows en clases prácticas — Score: 4.45

**El problema:**
El alumno tiene clase práctica el martes a las 10:00 y no aparece. No avisó.
El instructor espera 10 minutos, pierde el slot de 45 minutos, y ese hueco
no se puede rellenar porque nadie más estaba disponible.

**Quién sufre:** Instructor (tiempo muerto), dueño (dinero perdido), otros alumnos
(no acceden al slot)

**Frecuencia:** Varias veces por semana [A VALIDAR con dueño]

**Impacto cuantificado:**
- Cada no-show = 25-40€ perdidos (precio clase práctica)
- Con recordatorios automáticos, los no-shows se reducen **hasta 70%**
  (FasterCapital, driving school automation studies)
- Si evitas 3 no-shows/semana × 30€ × 48 semanas = **4.320€/año** recuperados [ESTIMACIÓN]

**Solución con WhatsApp chatbot:**
1. Recordatorio 48h antes: "Tienes clase el martes 10:00 con [instructor]
   en [punto de recogida]. Recuerda traer DNI y gafas si las necesitas."
2. Recordatorio 24h antes con botón: ✅ Confirmo / ❌ No puedo ir
3. Si cancela: oferta automática del slot a lista de espera
4. Si no confirma: alerta a secretaría para que llame

**Métrica de éxito:** Tasa de no-shows (medir antes/después)

**Riesgo:** Necesita que la autoescuela tenga un sistema de agenda (aunque sea Excel)
del que sacar las citas. [A VALIDAR] ¿Cómo gestionan agenda hoy?

---

### P3. Responder preguntas repetitivas automáticamente — Score: 4.15

**El problema:**
La secretaria pasa la mayor parte de su jornada respondiendo las mismas preguntas:
¿Cuánto cuesta el carnet? ¿Qué documentación necesito? ¿Cuántas clases prácticas
son? ¿Cuál es el horario de clases teóricas? ¿Puedo pagar a plazos?

Son siempre las mismas 8-12 preguntas. Por teléfono, en persona, y por WhatsApp.

**Quién sufre:** Secretaria (trabajo repetitivo), alumno (espera respuesta)

**Frecuencia:** Diaria, +10 veces/día [A VALIDAR]

**Impacto cuantificado:**
- Si dedica 2h/día a FAQs × 22 días = **44 horas/mes** de secretaría
- A 10€/hora = **440€/mes** en tiempo dedicado a repetir lo mismo [ESTIMACIÓN]
- El dueño que hace de secretario pierde ese tiempo de tareas de mayor valor

**Las FAQs más comunes** (verificado en webs de autoescuelas españolas):
1. ¿Cuánto cuesta el permiso B / A / AM?
2. ¿Qué documentación necesito para matricularme?
3. ¿Cuántas clases prácticas necesito?
4. ¿Cuál es el horario de clases teóricas?
5. ¿Puedo pagar a plazos?
6. ¿Cuánto tarda sacar el carnet?
7. ¿A partir de qué edad puedo sacarme el carnet de moto?
8. ¿Qué pasa si suspendo el examen?
9. ¿El psicotécnico lo hacéis vosotros?
10. ¿Dónde estáis / cómo llego?

**Solución con WhatsApp chatbot:**
1. Bot responde automáticamente las 10-12 preguntas más comunes
2. Si la pregunta no está cubierta, escala a humano
3. Puede enviar PDFs (lista de precios, checklist de documentación)
4. Responde en español (y opcionalmente en inglés para zonas turísticas)

**Métrica de éxito:** Mensajes resueltos sin intervención humana (% automatización)

**Riesgo:** Bajo. Las FAQs son estáticas y fáciles de programar. Es el quick win
más claro.

---

### P4. Reactivar alumnos inactivos antes de que abandonen — Score: 3.35

**El problema:**
Un alumno se matricula, hace algunas clases teóricas, y desaparece. Nadie lo
contacta proactivamente. Después de 2-3 meses sin actividad, el alumno se
desmotiva y abandona. O peor: se cambia de autoescuela.

Es "cada vez más común que alumnos abandonen el proceso antes de presentarse
al examen teórico" (PracticaVial).

**Quién sufre:** Dueño (pierde ingreso de un alumno ya captado)

**Frecuencia:** Continua. Especialmente meses 2-4 del proceso. [A VALIDAR]

**Impacto cuantificado:**
- Coste de captación ya pagado (marketing, tiempo de secretaria)
- Ingreso restante perdido: si un alumno abandonó tras pagar matrícula (200-300€)
  pero antes de contratar prácticas, se pierden 400-1.000€ de ingreso adicional
  [ESTIMACIÓN]
- Un cambio de autoescuela le cuesta al alumno 50-100€ + burocracia DGT, pero
  aún así lo hacen → señal de que la experiencia fue mala

**Solución con WhatsApp chatbot:**
1. Mensaje automático si no hay actividad en 2 semanas: "Hola [nombre], ¿cómo
   vas con los tests? ¿Necesitas ayuda?"
2. A las 4 semanas: "Llevas un mes sin hacer prácticas. ¿Quieres reservar una
   clase esta semana?"
3. Mensaje motivacional con datos: "Ya aprobaste el teórico, estás a un paso.
   El 80% de los alumnos que llegan a este punto aprueban a la primera."
4. Alerta a secretaría si tras 2 intentos no responde

**Métrica de éxito:** Tasa de abandono (alumnos que no terminan / alumnos matriculados)

**Riesgo:** Necesita saber en qué etapa está cada alumno. [A VALIDAR] ¿Tienen
un sistema que registre progreso, o es todo manual/en la cabeza del dueño?

---

### P5. Informar estado de examen y lista de espera — Score: 3.15

**El problema:**
Después de completar las prácticas mínimas, el alumno entra en lista de espera
para examen práctico. La media nacional es 3 meses. En algunas zonas, 4-7 meses.
Durante toda esa espera, el alumno llama o escribe cada semana: "¿Ya tengo fecha?",
"¿Cuánta gente hay delante de mí?", "¿Salió mi nota?"

**Quién sufre:** Secretaria (pregunta sin respuesta clara), alumno (ansiedad)

**Frecuencia:** Diaria en periodos de espera. Multiplica por número de alumnos
en lista de espera (10-30 alumnos simultáneos). [A VALIDAR]

**Impacto cuantificado:**
- Tiempo: Si 15 alumnos preguntan 1 vez/semana × 5 min = **75 min/semana** [ESTIMACIÓN]
- Frustración: "Se frustran y lo dejan" (Xataka) — genera abandono y reseñas negativas
- No hay impacto directo en revenue, pero sí en satisfacción y referidos

**Solución con WhatsApp chatbot:**
1. Mensaje proactivo cuando hay actualización: "Tu fecha de examen es el [fecha]"
2. Respuesta automática a "¿cuándo me toca?": "Estamos gestionando tu fecha.
   Te avisamos en cuanto la DGT confirme. Estimado: [rango]."
3. Notificación inmediata de notas cuando están disponibles
4. Mensajes de ánimo durante la espera: "Mientras esperas, ¿quieres repasar
   maniobras con un vídeo?"

**Métrica de éxito:** Reducción de llamadas/mensajes sobre estado de examen

**Riesgo:** La autoescuela no controla los tiempos de la DGT. El chatbot puede
gestionar expectativas pero no resolver el problema de fondo. [A VALIDAR]
¿Cuántas veces/semana les preguntan esto?

---

## Recomendación de MVP

Para el build partner, recomiendo empezar con **P1 + P2 + P3** en un solo bot:

```
MVP = Chatbot que:
  1. Responde FAQs automáticamente (P3) — quick win, demuestra valor día 1
  2. Captura leads fuera de horario (P1) — impacto en revenue, el dueño lo nota
  3. Envía recordatorios de prácticas (P2) — reduce no-shows, resultado medible
```

**Por qué estos 3:**
- Son los más fáciles de implementar (no requieren integración con sistemas complejos)
- Dan resultado visible en la primera semana
- Cubren todo el journey: captación → atención → retención
- No necesitan datos del alumno en tiempo real (P4 y P5 sí)

**P4 y P5 quedan para fase 2**, cuando tengamos acceso a datos de progreso del alumno
y entendamos cómo gestiona la autoescuela sus expedientes.

---

## Preguntas para la discovery call

Cada problema tiene hipótesis que validar. Estas son las preguntas clave:

### Sobre el negocio
1. ¿Cuántos alumnos activos tenés ahora?
2. ¿Cuántos alumnos nuevos se matriculan por mes?
3. ¿Quién atiende teléfono y WhatsApp? ¿El dueño, secretaria, instructor?
4. ¿Cuántas horas al día le dedican a responder mensajes/llamadas?

### Sobre leads (P1)
5. ¿Recibís consultas fuera de horario? ¿Por qué canal?
6. ¿Cuántas consultas de precio recibís por semana?
7. ¿Cuántas se convierten en matrícula? ¿Cuántas se pierden?
8. ¿Tenés idea de por qué se pierden? ¿Precio, tiempo de respuesta, otra cosa?

### Sobre no-shows (P2)
9. ¿Cuántas veces por semana un alumno no se presenta a la práctica?
10. ¿Cómo avisás al alumno de su próxima clase? ¿Llamada, WhatsApp, nada?
11. ¿Tenés política de cancelación? ¿Cobrás si no avisa?
12. ¿Usás alguna app o sistema para gestionar la agenda de prácticas?

### Sobre FAQs (P3)
13. ¿Cuáles son las 5 preguntas que más te hacen?
14. ¿Ya tenés respuestas escritas o las improvisás cada vez?
15. ¿Usás WhatsApp Business con respuestas rápidas?

### Sobre retención (P4)
16. ¿Se te van alumnos antes de terminar? ¿Cuántos por año?
17. ¿Hacés algo para contactar a un alumno que dejó de venir?

### Sobre lista de espera (P5)
18. ¿Cuánto tardan tus alumnos en conseguir fecha de examen práctico?
19. ¿Cuántas veces por semana te preguntan "cuándo me toca"?

### Sobre herramientas
20. ¿Qué software usás hoy? ¿Excel, papel, alguna app de autoescuela?
21. ¿Usás WhatsApp para comunicarte con alumnos? ¿Desde el personal o Business?
22. ¿Tenés grupos de WhatsApp con alumnos?

---

## Fuentes

- CNMC — Estudio autoescuelas 2025: https://www.cnmc.es/prensa/estudio-autoescuelas-20250109
- Autónomos y Emprendedor — 1.350 cierres: https://www.autonomosyemprendedor.es
- WeGen-AI — WhatsApp AI driving schools: https://www.wegen-ai.com/en/blog/smart-support-driving-school-whatsapp-ai/
- Alzado.org — WhatsApp Business autoescuelas: https://alzado.org/2022/05/01/whatsapp-business-para-autoescuelas-con-tochat-be/
- PracticaVial — Retención alumnos: https://practicavial.com/blog/actualidadpracticavial/mejorar-la-retencion-de-alumnos-es-posible-y-rentable
- DriveScout — Lead conversion 20-30%: https://drivescout.com/blog/generate-driving-school-leads-convert-customers/
- Neomentor — Autoescuela Soria +40% conversión: https://neomentor.es/transformacion-digital-en-autoescuelas-y-centros-de-formacion-guia-paso-a-paso/
- FasterCapital — No-shows -70% con recordatorios: https://fastercapital.com/content/Driving-school-automation-Revolutionizing-the-Driving-School-Industry--The-Power-of-Automation.html
- COPE — Espera 4-6 meses examen práctico: https://www.cope.es/trafico/
- Xataka — Frustración listas de espera: https://www.xataka.com/movilidad/se-frustran-dejan-autoescuelas
- OCU — Reclamaciones autoescuelas: https://www.ocu.org/reclamar/
- Autopractik — +2M prácticas gestionadas: https://autopractik.es/
- Control-L — Software gestión: https://www.control-l.com/
