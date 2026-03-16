# CLAUDE.md

## Quién soy
Juan, Product Manager con experiencia en fintech y pagos.
No soy técnico pero quiero aprender mientras construyo.
Trabajo rápido y tomo decisiones con contexto mínimo suficiente.

## Qué es este proyecto
Servicio de automatización de WhatsApp con IA para pequeñas empresas locales.
Automatizo comunicación con clientes, operaciones repetitivas y seguimiento
de leads. Se vende como servicio a negocios locales en la Costa del Sol, España.

## Estado actual
- 0 clientes, 0 código construido
- 1 build partner en proceso: autoescuela en Málaga (gratis, para aprender)
- Primera reunión de discovery pendiente
- Todo está en validación: stack, pricing, ICP

## Modelo de negocio
- **Build partner**: gratis a cambio de caso de uso real
- **Clientes pagos**: setup fee (customización + implementación) + retainer
  mensual (soporte, mantenimiento, mejoras)
- **Pricing**: por definir — atado al valor que genera al cliente
  (ahorro de tiempo, reducción de no-shows, leads cualificados)
- **Estructura**: suscripción con caps de uso. Si el cliente los supera,
  se le notifica para subir de plan
- Todo el pricing está en validación

## ICP (Ideal Customer Profile)
- Negocio con local físico, Costa del Sol / Málaga
- Máximo 10 empleados, idealmente negocio familiar
- Dueño involucrado en la operación del día a día
- Alejado de la tecnología — eso es una ventaja, no un obstáculo
- Verticales target: autoescuelas, clínicas, peluquerías,
  academias, talleres, inmobiliarias
- **Assumption a validar**: negocios físicos son más fáciles de captar
  que negocios digitales

## Distribución y ventas
Es la prioridad número uno. Construir es fácil, conseguir clientes no.
- Canal inicial: red personal y proximidad geográfica
- Diferencial: contacto presencial, cercanía, hablar su idioma
- Estrategia: un cliente a la vez, caso de uso documentado,
  referencias para el siguiente

## Cómo trabajamos
- Respuestas cortas y directas, sin introducción ni cierre
- Tradeoffs en formato simple: opción A vs B con pros y contras
- Una recomendación clara si la pregunta lo permite
- No me expliques lo que no pregunté
- Si falta contexto para decidir, pedímelo
- Idioma: español. Si algo funciona mejor en inglés, avisame

## Stack (en definición)
- Automatizaciones: n8n
- IA: Claude API
- WhatsApp: BSP por definir (360dialog, WANotifier u otro)
- Base de datos: por definir (Supabase o Google Sheets)
- Repositorio: GitHub

## Agentes disponibles
- `/cto` — arquitectura, stack, infraestructura, decisiones técnicas
- `/product` — priorización, definición de problema, validación
- `/researcher` — análisis de entrevistas, insights de clientes
- `/comercial` — ICP, pricing, adquisición de clientes, distribución

## Estructura del repo
- `core/` — flujos, prompts y docs reutilizables entre clientes
- `clients/` — carpeta por cliente con su research y customizaciones
- `agents/` — definición de cada agente
- `research/` — research general del mercado y verticales
