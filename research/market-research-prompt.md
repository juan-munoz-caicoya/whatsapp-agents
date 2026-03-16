# Prompt: Market Research Estratégico

## CONTEXTO — leer antes de hacer cualquier cosa

Soy Juan, Product Manager argentino viviendo en Benalmádena, Costa del Sol.
Estoy en etapa 0 de un emprendimiento: vender servicios de automatización
de WhatsApp con IA a PYMEs locales con local físico en la Costa del Sol.

Stack: n8n + Claude API + WhatsApp Business API
Modelo de negocio: setup fee único + retainer mensual
Primer cliente: build partner gratis (autoescuela en Málaga)
Trabajo solo, sin equipo, sin inversión externa.

### Decisiones concretas que voy a tomar con este research

1. A qué vertical ir primero — necesito saber cuál tiene más demanda,
   es más fácil de penetrar, y tiene menos competencia directa local.
   Quiero entender cuáles son blue ocean vs red ocean en la Costa del Sol.

2. Si el negocio tiene sentido financiero — cuántos clientes necesito
   para vivir de esto, cuánto podría facturar, cuándo llegaría al breakeven.

3. Cómo pensar el pricing — qué cobrar por setup y por retainer,
   basado en benchmarks reales y en el valor que genero al cliente.

4. Qué problemas resuelvo en cada vertical — necesito esto para cuando
   hable con un potencial cliente, tener claro cuál es su dolor y cómo
   se lo resuelvo. Esto es mi munición de ventas.

### Situación personal relevante para el análisis

- Necesito que el negocio sea rentable en 6-12 meses
- No puedo permitirme ciclos de venta largos ni clientes enterprise
- La cercanía geográfica y el contacto presencial son mi ventaja competitiva
- No soy técnico — mi valor es entender el negocio del cliente y
  traducirlo a automatizaciones que funcionen
- Cada cliente que consiga me tiene que dar referidos para el siguiente

---

## PASO 1 — Researcher Strategy

Lee agents/researcher-strategy.md y actuá como ese agente.

Primero presentame un PLAN de investigación con:
- Qué vas a investigar y en qué orden
- Qué búsquedas específicas vas a hacer
- Qué fuentes vas a priorizar para cada sección
- Cómo vas a estructurar el documento

Esperá mi aprobación del plan antes de arrancar.

Una vez aprobado, investigá en profundidad:

### 1. TAM / SAM / SOM

- TAM: mercado global y europeo de conversational AI y WhatsApp
  automation para PYMEs. Fuentes: Grand View Research, MarketsandMarkets,
  Statista, Gartner, IDC. Incluir CAGR 2024-2030.
- SAM: mercado español de automatización para PYMEs con menos de
  10 empleados por vertical. Fuentes: INE DIRCE, Eurostat,
  Cámara de Comercio de Málaga, Junta de Andalucía.
- SOM: negocios capturables en Costa del Sol en 24 meses.
  Estimar número de negocios por vertical en la zona.
  Calcular escenarios de revenue: conservador, base y optimista.

### 2. Modelo financiero básico

- ¿Cuántos clientes necesito para facturar 2.000€/mes? ¿3.000€? ¿5.000€?
- ¿Cuánto tiempo me lleva conseguir cada cliente (ciclo de venta)?
- ¿Cuánto tiempo me lleva implementar cada cliente?
- ¿Cuál es el churn estimado para este tipo de servicio?
- Benchmarks de pricing en España para servicios similares:
  setup fee y retainer mensual. Fuentes: G2, Capterra, webs de agencias.

### 3. Competitive Landscape

- SaaS globales: WATI, ManyChat, Landbot, Respond.io, Trengo, 360dialog
- Plataformas españolas: Clientify, Hiwabot, Arkibot
- Agencias de automatización en Andalucía y Costa del Sol
- Para cada competidor: pricing, ICP, fortalezas, debilidades
- Buscar reseñas en G2, Capterra, Trustpilot — qué se quejan los usuarios
- Whitespace: qué no cubre nadie, especialmente en mercado local

### 4. Priorización de verticales

Analizar: autoescuelas, clínicas dentales, peluquerías y estética,
talleres mecánicos, academias, inmobiliarias.

Para cada vertical hacer un scoring en:
- Volumen de negocios en Costa del Sol
- Intensidad del dolor operativo
- Willingness to pay estimado
- Facilidad de venta (ciclo corto, decisión simple)
- Velocidad de referidos (¿hablan entre ellos?)
- Blue ocean vs red ocean localmente

Output: ranking claro con recomendación de cuál atacar primero,
segundo y tercero, con justificación.

### 5. Problemas y propuesta de valor por vertical

Para cada vertical, esto es munición de ventas:
- Los 3 dolores operativos principales con datos que los respalden
- Cómo los resuelve un bot de WhatsApp específicamente
- Qué le ahorra al dueño en tiempo y dinero (cuantificado)
- Frase de una línea que resume el valor:
  "Te ahorro X horas por semana / X euros al mes"
- Objeciones típicas que va a poner el dueño y cómo responderlas

### 6. Riesgos del negocio

- GDPR y regulación española de datos y comunicaciones
- Dependencia de Meta / WhatsApp API (qué pasa si cambian las reglas)
- Riesgo de commoditización a 12-24 meses
- Cómo mitigar cada riesgo

### Reglas de investigación

- Mínimo 20 búsquedas, en inglés Y español
- Marcar [VERIFICADO], [ESTIMACIÓN] o [SUPOSICIÓN] en cada cifra
- Citar URL y fecha en cada fuente
- Si un dato no existe, decirlo — nunca inventar
- Preferir rangos conservadores con fuente que números exactos sin respaldo

Guardá el borrador en research/market-research-draft.md
Cuando termines avisame para pasar al PASO 2.

---

## PASO 2 — Reviewer

Lee agents/reviewer.md y actuá como ese agente.

Revisá research/market-research-draft.md con todos tus criterios.

Prestá especial atención a:
- Cifras de mercado sin URL verificable
- Recomendaciones de verticales que no estén respaldadas por datos
- Modelo financiero con números inventados
- Benchmarks de pricing sin fuente real

Output en los tres bloques:
✅ Sólido
⚠️ Débil o cuestionable
❌ Falta o está mal

Score de confiabilidad al final.

- Si es 🟢 SÓLIDO → renombrá a research/market-research.md
- Si es 🟡 ACEPTABLE → guardá revisión en research/market-research-review.md y avisame
- Si es 🔴 RECHAZADO → listá exactamente qué corregir y esperá instrucciones
