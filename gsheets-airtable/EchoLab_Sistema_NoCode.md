# EchoLab — Arquitectura No-Code Definitiva (Sheets + Airtable)

Este documento es el **plano maestro** del sistema. No da lugar a adivinanzas ni a código oculto: las operaciones matemáticas pesadas (promedios, percentiles y fórmulas de valoración) suceden estricta y automáticamente en Google Sheets. Airtable recibe los números pre-digeridos y la Inteligencia Artificial se usa puramente para deducción estratégica.

---

## 1. GLOSARIO DE CONCEPTOS METODOLÓGICOS

Antes de configurar las fórmulas, es vital entender de dónde salen estas reglas y qué significa cada cálculo:

### Conceptos Estadísticos (Métricas de Referencia)
*   **Media / Promedio (`AVERAGE`)**: El nivel normal de desempeño del artista en un mes específico. Todos los días o posts "normales" rondan este número.
*   **P10 / Piso (`PERCENTILE 0.1`)**: El 10% más bajo de rendimiento. Un día o post que cae por debajo del P10 es un día "Muerto" o "Bajo". Significa que el artista tuvo cero tracción u olvidó a su audiencia.
*   **P90 / Techo (`PERCENTILE 0.9`)**: El 10% más alto de rendimiento. Cruza la barrera de la rutina. Un día o post por encima del P90 es estadísticamente un **SPIKE (Pico)** o "Excepcional".

### Conceptos del Modelo (Spotify & Score)
*   **Save Rate**: El porcentaje de personas que escucharon la canción (Listeners) y además la guardaron (Saves). Es el indicador más agresivo de conexión a largo plazo. Un _Save Rate_ por encima del 5% es sobresaliente.
*   **Score EchoLab Mensual / Diario**: Una fórmula propietaria que le da pesos distintos a las métricas para penalizar los "views vacíos" y premiar la lealtad.
    *   *Saves* valen 8 puntos.
    *   *Playlist Adds* valen 5 puntos.
    *   *Streams* valen 1 punto.
*   **Modelo Invertido (Correlación)**: En vez de publicar algo en redes y sentarse a esperar qué pasa en Spotify (que suele frustrar porque el efecto no es inmediato), se observan los **Spikes (P90) en Spotify** y se mira 5-10 días hacia atrás en las redes sociales para encontrar a los culpables.

### Conceptos de Redes (Contenido Social)
*   **WScore (Weighted Score)**: No todos los views valen lo mismo. Un *Guardado* o un *Nuevo Seguidor* vale exponencialmente más para el negocio que un *View* pasivo. El WScore normaliza esto dándole un puntaje a cada interacción.
*   **Eficiencia (`WScore / Views`)**: Mide si un post fue "eficiente" convirtiendo ojos en acciones. Un post con 5K views y miles de comentarios puede tener mayor eficiencia que un post de 2M views que nadie guardó. Un post con eficiencia > 0.5 es un **Unicornio**.
*   **Share-to-Save Ratio (`Shares / Saves`)**: Si se comparte mucho pero no se guarda, es un meme, morbo o viralidad vacía. Si se guarda mucho pero no se comparte, es conexión íntima.

---

## 2. PANEL DIARIO: CONFIGURACIÓN EXPLÍCITA EN GOOGLE SHEETS

Esta es tu pestaña de datos principales. Asumimos que pegas el `rubimente_consolidado.csv` tal como sale del script. El orden exacto de las primeras columnas crudas será:

*   **A**: `fecha`
*   **B**: `spotify_streams`
*   **C**: `spotify_listeners`
*   **…**
*   **E**: `spotify_saves`
*   **F**: `spotify_playlist_adds`
*   **…**
*   **L**: `tiktok_video_views`
*   **…**
*   **R**: `ig_reach`
*   **S**: `ig_views`

A partir de la última columna ocupada de la data cruda (por lo general la `X` o posteriores), vas a crear las siguientes **Columnas de Inteligencia**. Solo debes pegar la fórmula en la fila 2 de cada columna. Gracias al `MAP(LAMBDA())`, la fórmula correrá hasta el infierno automáticamente.

### A. Referencia de Tiempo
**Nombre Columna (Ej. Col X): `ai_mes_id`**
```excel
=ARRAYFORMULA(IF(A2:A="", "", TEXT(A2:A, "YYYY-MM")))
```
> *Por qué:* Agrupa los días en "2026-01", asegurando que el promedio de Enero no se mezcle con Diciembre.

### B. Benchmarks de Streaming (Spotify)
**Columna: `ai_avg_streams` (Promedio normal del mes)**
```excel
=MAP(X2:X, LAMBDA(mes, IF(mes="", "", AVERAGE(FILTER(B2:B, X2:X=mes)))))
```

**Columna: `ai_p90_streams` (El TECHO para considerar algo "Excepcional" ese mes)**
```excel
=MAP(X2:X, LAMBDA(mes, IF(mes="", "", PERCENTILE(FILTER(B2:B, X2:X=mes), 0.9))))
```

**Columna: `ai_avg_saves` (Promedio de veces que guardan su música)**
```excel
=MAP(X2:X, LAMBDA(mes, IF(mes="", "", AVERAGE(FILTER(E2:E, X2:X=mes)))))
```

**Columna: `ai_p90_saves` (El TECHO de la métrica más valiosa)**
```excel
=MAP(X2:X, LAMBDA(mes, IF(mes="", "", PERCENTILE(FILTER(E2:E, X2:X=mes), 0.9))))
```

### C. Benchmarks Sociales (Alcance Viral)
**Columna: `ai_p90_ig_reach` (Pico de alcance en IG del mes)**
```excel
=MAP(X2:X, LAMBDA(mes, IF(mes="", "", PERCENTILE(FILTER(R2:R, X2:X=mes), 0.9))))
```

**Columna: `ai_p90_tiktok_views` (Pico viral en TikTok del mes)**
```excel
=MAP(X2:X, LAMBDA(mes, IF(mes="", "", PERCENTILE(FILTER(L2:L, X2:X=mes), 0.9))))
```

---

## 3. CONTENIDO SOCIAL: CONFIGURACIÓN EN GOOGLE SHEETS

*Nota para ti: Es fundamental hacer el cálculo en Google Sheets y no en Airtable ni en la IA, para eliminar el "hallucination" matemático de los modelos y que Airtable pueda filtrar views por eficiencia.*

El archivo `rubimente_social_content.csv` tiene esta estructura exacta según tu última captura de pantalla:
*   **A**: `plataforma` (Instagram o TikTok)
*   **C**: `fecha_publicacion`
*   **G**: `visualizaciones`
*   **H**: `alcance`
*   **I**: `likes`
*   **J**: `comentarios`
*   **K**: `compartidos`
*   **L**: `guardados`
*   **M**: `seguimientos`

Añade estas columnas matemáticas a la derecha, a partir de la primera columna libre (**Columna O** en tu imagen):

### A. Metadato de Tiempo Estricto (Columna O)
**Columna O: `ai_mes_id`** (Para agrupar por mes usando la columna C de Fecha)
```excel
=ARRAYFORMULA(IF(C2:C="", "", TEXT(C2:C, "YYYY-MM")))
```

### B. Matemáticas de Valor (Columnas P, Q, R)
Estas fórmulas crean interacción entre el peso de cada métrica y su engagement real.

**Columna P: `WScore`** (Valor Real Ponderado cruzando G, I, J, K, L, M)
```excel
=ARRAYFORMULA(IF(G2:G="", "", (G2:G * 0.05) + (I2:I * 1) + (J2:J * 4) + (K2:K * 6) + (L2:L * 5) + (M2:M * 10)))
```

**Columna Q: `Eficiencia`** (WScore dividido entre Views)
```excel
=ARRAYFORMULA(IF(G2:G="", "", IF(G2:G=0, 0, ROUND(P2:P / G2:G, 2))))
```

**Columna R: `SaveRate`** (Guardados divididos entre Views)
```excel
=ARRAYFORMULA(IF(G2:G="", "", IF(G2:G=0, 0, ROUND(L2:L / G2:G, 4))))
```

### C. Techo de Plataforma Estricto (Columna S)
El secreto de la receta. Calcula el Techo (P90) de Eficiencia **aislando por plataforma y por mes** simultáneamente (evitando que los views inflados naturales de TikTok arruinen la métrica de Instagram):

**Columna S: `ai_p90_eficiencia_local`**
```excel
=MAP(O2:O, A2:A, LAMBDA(mes, plat, IF(mes="", "", PERCENTILE(FILTER(Q2:Q, (O2:O=mes)*(A2:A=plat)), 0.9))))
```
*(Es decir, si el reel es de IG (leído en 'A'), busca el P90 de eficiencia solo de los reels de IG de ese mes (leído en 'O')).*

---

## 4. PROMPTS EN AIRTABLE (100% ESTÁTICOS)

Dado el nivel de exactitud provisto por Google Sheets, los Agentes en Airtable ahora son máquinas puras de estrategia de negocio, libres de cálculos.

### Panel Diario: Correlación Cross-Platform (El Modelo Invertido)

```text
Eres un analista de marketing musical que aplica el "Modelo Invertido" de EchoLab. Analizas los SPIKES (Picos por encima del Percentil 90 del mes) y correlacionas hacia atrás temporalmente.

DATOS DEL DÍA ({fecha}):

STREAMING: 
- Spotify Streams de hoy: {spotify_streams} (TECHO DEL MES P90: {ai_p90_streams})
- Spotify Saves de hoy: {spotify_saves} (TECHO DEL MES P90: {ai_p90_saves})

REDES (El frente de adquisición):
- TikTok Views: {tiktok_video_views} (TECHO DEL MES P90: {ai_p90_tiktok_views})
- IG Reach: {ig_reach} (TECHO DEL MES P90: {ai_p90_ig_reach})

REGLAS DEL MODELO INVERTIDO ECHOLAB:
1. El efecto puente entre Redes y Spotify tarda 5-10 días. Nunca es inmediato.
2. Si los Streams o Saves de HOY superaron el TECHO P90 Mensual estipulado arriba, hay un SPIKE CONFIRMADO en Spotify. La detonación sucedió 5 a 10 días en el pasado.
3. Si los IG Reach o TikTok Views de HOY superaron su TECHO P90 Mensual, hay un SPIKE SOCIAL CONFIRMADO. El efecto en Spotify se sentirá a futuro, dentro de 5 a 10 días.

INSTRUCCIONES ESTRATÉGICAS:
Toma el lugar del director del proyecto.
1. Evalúa si cruzamos techos. Dictamina el "Estado" del ecosistema el día de hoy.
2. Basado en la regla de 5-10 días, construye el retrovisor (Lookback) si hubo éxito en Spotify, o el pronóstico (Forecast) si el puente de redes está lleno.
3. Proporciona una explicación para el Mánager en texto humano simple.

OUTPUT TEMPLATE EXACTO:

---
🔗 ESTADO DE SISTEMA: [SPIKE STREAMING / SPIKE SOCIAL / SPIKE DUAL / DÍA BASE]

DIAGNÓSTICO VASCULAR:
• Retención Spotify (Saves): [X] (vs P90 Mensual de {ai_p90_saves}) -> [Excepcional/Normal/Bajo]
• Volumen Spotify (Streams): [X] (vs P90 Mensual de {ai_p90_streams}) -> [Excepcional/Normal/Bajo]
• Tracción IG (Reach): [X] (vs P90 Mensual de {ai_p90_ig_reach}) -> [Pico/Normal]

EL MODELO INVERTIDO:
⏪ Lookback: [SI SPIKE STREAMING: "Se confirma Spike de retención masiva. El equipo debe revisar el contenido social publicado estrictamente entre {fecha - 10} y {fecha - 5} para identificar qué provocó la conexión profunda." / SI NO: "A la espera de tracción."]
⏩ Forecast: [SI SPIKE SOCIAL: "Se confirma Spike de adquisición en redes hoy. El eco natural en los streams debería verse aterrizar entre {fecha + 5} y {fecha + 10}." / SI NO: "A la espera de tracción."]

🧠 NOTA DEL ANALISTA A MANAGER:
[1 Párrafo. Diagnóstico limpio de la salud holística. Sin mencionar qué números son más altos que otros, solo hablando del negocio humano ("La retención en plataformas está sana pero requerimos un detonador de volumen nuevo desde Tiktok...")]
---
```

### Contenido Social: Predictor Estratégico (WScore y Conversión a Spotify)

```text
Eres el director de A&R y Contenido. Tienes métricas ponderadas que el equipo de Datos (Google Sheets) ya preparó. Debes juzgar si esta pieza de contenido funciona y predecir cuándo va a estallar en la música.

IDENTIDAD DEL ACTIVO:
- Publicado: {fecha_publicacion}
- Plataforma: {plataforma} | Formato: {tipo_contenido}
- Contexto Creativo (Descripción): {descripcion}

CALIFICACIÓN MATEMÁTICA ECHOLAB (Data Real):
- WScore de Valor Ponderado: {WScore_Calc}
- Nivel de Eficiencia Base: {Eficiencia_Calc}
- Tasa de Guardados (SaveRate): {SaveRate_Calc}%

CÓMO INTERPRETAR LA CIENCIA:
1. UMBRAL DE CLASIFICACIÓN (Eficiencia y Formato):
   • Eficiencia >0.5: UNICORNIO (Contenido Maestro)
   • Eficiencia 0.3 a 0.5: PIEDRA ANGULAR (Alta Calidad)
   • Eficiencia 0.15 a 0.3: RELLENO (CUMPLE)
   • Eficiencia <0.15: RUIDO ESPACIAL (Baja Calidad)
2. PREDICTOR SPOTIFY (Basado en el SaveRate %):
   • Si SaveRate > 2%: Disparará un Spike de retención fuerte en Spotify.
   • Si SaveRate 1-2%: Conversión moderada y sana en la audiencia core.
   • Las historias NO convierten a Spotify (exclúyelas).
3. MATRIZ DE TONO ECHOLAB:
   • Temas de desamor (extraño, doliendo, ruptura): Generan JACKPOT (todas métricas suben).
   • Nostalgia (antes, recuerdos): Genera alta profundidad pero no viralidad.
   • CTAs vacíos o Promos de concierto vacías: Generan RUIDO pero 0 conversión.

OUTPUT TEMPLATE EXACTO (Si es Reel / TikTok / Post):

---
🏷️ EVALUACIÓN DEL ACTIVO CREADO

CALIFICACIONES:
• Categoría Analística: [UNICORNIO / PIEDRA ANGULAR / RELLENO / RUIDO] (Eficiencia: {Eficiencia_Calc})
• Matriz de Tono: Detectado tema de [Desamor / Nostalgia / Humor / Promocional] → Veredicto [JACKPOT / ALTO VALOR / BAJO VALOR]

PREDICCIÓN DE EFECTO EN SPOTIFY (Modelo 5-10):
• Termómetro (SaveRate): {SaveRate_Calc}% → Área [ROJA de detonación / AMARILLA sana / VERDE pasiva]
• Horizonte de Impacto: El golpe de gracia de estético contenido en Spotify se espera manifestarse de {fecha_publicacion + 5 días} a {fecha_publicacion + 10 días}. Monitoricen esas fechas.

📍 DIRECTRIZ TÁCTICA:
[El equipo debe / No debe... replicar esto urgentemente enfocado en este nicho por X motivo.]
---
```

---

## 5. SINCRONIZACIÓN: DATA FETCHER (Sheets → Airtable)

Data Fetcher es una extensión gratuita del marketplace de Airtable que jala datos de Google Sheets a tu base. Sustituye la importación manual y evita duplicados.

### Instalación (Primera vez, una sola vez)
1. En Airtable, abre tu base **Echolab – Rubimente**.
2. Abajo a la izquierda, haz click en el ícono de **puzzle** (Extensions).
3. Click en **"+ Add an extension"**.
4. Busca **"Data Fetcher"** en el marketplace.
5. Instálala. Se abrirá un panel lateral.

### Request 1: Panel Diario (Streaming + Métricas Generales)
1. En Data Fetcher, click en **"New Request"** (o el botón "+").
2. Selecciona **Google Sheets** como fuente.
3. Conecta tu cuenta de Google (la de `monica@lifestrategics.com`).
4. **Spreadsheet:** Selecciona `Rubimente — Data Consolidada`.
5. **Sheet:** Selecciona `Panel Diario`.
6. **Major dimension:** `Rows`.
7. **First row of Google Sheet is headers:** Activado (ON) ✅.
8. **Destination Table & View:** `Panel Diario` / `Grid view`.
9. Click en **"Edit field mapping"** para verificar que TODAS las columnas de Google Sheets estén mapeadas:
    - Las columnas originales (fecha, spotify_streams, etc.) se mapean automáticamente.
    - Las columnas de fórmulas (`ai_mes_id`, `ai_avg_streams`, `ai_p90_streams`, etc.) pueden aparecer como "unmapped". **Actívalas** y selecciona **"Create new field"** para cada una.
10. Click en **"Save & Run"**.
11. Cuando aparezca el popup **"Keep records in sync on future runs"**:
    - **Match records using:** Selecciona `Fecha` (es el campo único — un registro por día).
    - **Create new records if no match:** Activado ✅.
    - **Delete records not in response:** Desactivado ❌ (para no perder datos si un sheet se corta accidentalmente).
12. Click en **Save**.

### Request 2: Contenido Social (Posts individuales de IG + TikTok)
1. En Data Fetcher, click en **"< Home"** para volver al menú principal.
2. Click en **"New Request"** (o el botón "+").
3. Selecciona **Google Sheets** como fuente.
4. **Spreadsheet:** `Rubimente — Data Consolidada` (el mismo archivo).
5. **Sheet:** Selecciona `Contenido Social`.
6. **Major dimension:** `Rows`.
7. **First row of Google Sheet is headers:** Activado (ON) ✅.
8. **Destination Table & View:** `Contenido Social` / `Grid view`.
9. **⚠️ PASO CRÍTICO — Click en "Edit field mapping":**
    - Verifica que las columnas de datos originales estén mapeadas (plataforma, tipo_contenido, fecha_publicacion, descripcion, enlace, duracion_seg, visualizaciones, alcance, likes, comentarios, compartidos, guardados, seguimientos, visitas_perfil).
    - Busca las columnas de fórmulas que añadiste (`ai_mes_id`, `WScore`, `Eficiencia`, `SaveRate`, `ai_p90_eficiencia_local`). Estarán como "unmapped" o con toggle apagado.
    - **Activa cada una** y selecciona **"Create new field"** como destino. Esto crea las columnas automáticamente en Airtable.
10. Click en **"Save & Run"**.
11. Cuando aparezca el popup de sync:
    - **Match records using:** Selecciona `enlace` (la URL del post es única e irrepetible).
    - **Create new records if no match:** Activado ✅.
    - **Delete records not in response:** Desactivado ❌.
12. Click en **Save**.

### ¿Y los AI Field Agents?
Los AI Field Agents NO se sincronizan con Data Fetcher. Son columnas internas de Airtable que se re-generan automáticamente cada vez que los datos de su fila cambian. Por tanto:
- Si Data Fetcher actualiza el valor de `spotify_streams` de un día, el AI Field Agent de esa fila se recalcula solo.
- Si agregas un nuevo registro (un día nuevo), Airtable le crea su AI Field Agent automáticamente.

---

## 6. FLUJO OPERATIVO DIARIO (El Ritual)

Este es el proceso completo que seguirá tu equipo cada vez que haya datos nuevos:

### Quién hace qué:

| Paso | Quién | Qué hace | Dónde |
|------|-------|----------|-------|
| 1 | Ed / Equipo | Descarga los CSVs crudos de Spotify, Amazon, TikTok, IG | Plataformas |
| 2 | Tú (Mónica) | Ejecuta `python3 etl_consolidate.py` y `python3 etl_social_content.py` en tu terminal | Tu Mac |
| 3 | Tú (Mónica) | Abre los CSVs de `output/` y pega los datos en Google Sheets (`Rubimente — Data Consolidada`) | Google Sheets |
| 4 | Nadie | Las fórmulas de Google Sheets (`ai_avg_streams`, `ai_p90_saves`, `WScore`, etc.) se recalculan solas al instante | Automático |
| 5 | Tú (Mónica) | Abre Airtable → Extensions → Data Fetcher → **Run** en Request 1 (Panel Diario) y Request 2 (Contenido Social) | Airtable |
| 6 | Nadie | Los AI Field Agents se re-generan automáticamente con los datos frescos y los benchmarks dinámicos del mes | Automático |
| 7 | Tú / Ed | Leen los insights generados por los AI Agents en Airtable | Airtable |

### Tiempo estimado del ritual completo: ~10 minutos.

---

## 7. TROUBLESHOOTING (Problemas Comunes)

| Problema | Causa | Solución |
|----------|-------|----------|
| Data Fetcher crea columnas duplicadas con `(2)` | Ya existían columnas con el mismo nombre de una importación anterior | Borra las columnas viejas (sin el "(2)"), renombra las nuevas quitándoles el "(2)" |
| Las fórmulas de Google Sheets muestran `#N/A` | El mes tiene muy pocos datos (menos de 2 días) para calcular un percentil | Normal al inicio de un mes nuevo. Se corrige solo cuando haya 3+ días de datos |
| El AI Field Agent dice cosas genéricas | El prompt no referencia las columnas correctas | Verifica que los nombres de campo en el prompt `{campo}` coincidan exactamente con los nombres de columnas en Airtable |
| Data Fetcher dice "Success" pero las columnas nuevas no aparecen | Las columnas nuevas de Google Sheets NO están mapeadas en el field mapping | Ve a Data Fetcher → click en el Request → **"Edit field mapping"** → busca las columnas sin mapear → actívalas → selecciona "Create new field" → Save → Run de nuevo |
| Data Fetcher no jala las columnas nuevas de fórmulas | Las columnas nuevas no están mapeadas | En Data Fetcher → Edit field mapping → asegúrate de que las columnas nuevas (`WScore`, `Eficiencia`, etc.) estén mapeadas a campos de Airtable |

### Regla de oro de Data Fetcher:
> **Cada vez que añadas una columna nueva en Google Sheets, tienes que ir a Data Fetcher → Edit field mapping → activar esa columna → asignarle un destino en Airtable.** Data Fetcher NO detecta columnas nuevas automáticamente. Si no la mapeas, no la jala, aunque diga "Success".
