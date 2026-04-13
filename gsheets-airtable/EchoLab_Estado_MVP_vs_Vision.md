# EchoLab: Estado actual vs. tu documento de visión

Yoyo, revisé tu documento contra lo que ya está construido y operativo en el dashboard que te compartí. Aquí va el mapeo para que tengamos la misma foto.

## Ya considerado en el MVP actual

| Lo que propones en tu doc | Estado | Dónde está |
|---|---|---|
| Capa 1: Análisis de IG y TikTok | ✅ Operativo | Datos de IG y TikTok ya están integrados en el pipeline de Google Sheets y se visualizan en el dashboard |
| Capa 2: Análisis de Spotify | ✅ Operativo | Streams, Listeners, Saves, Playlist Adds, Followers. Todo con benchmarks mensuales (P90, promedio) |
| Capa 3: Cruce redes vs. streaming | ✅ Operativo | Echo Insight ya analiza la correlación entre actividad social y spikes de streaming con ventana de 5-10 días |
| Comparativa mes a mes | ✅ Operativo | Los benchmarks P90 y promedios ya se calculan por mes de forma dinámica |
| Cruce de TikTok | ✅ Operativo | TikTok Views, Likes, Comments, Shares ya están en el pipeline |
| Puntuación de contenido | ✅ Operativo | WScore, Eficiencia, Save Rate y categorias (Unicornio / Piedra Angular / Cumple / Ruido) ya se calculan por post |
| Diagnóstico diario | ✅ Operativo | Echo Pulse genera diagnóstico automático comparando métricas del día contra el promedio |
| Dashboard visual | ✅ Operativo | https://monixette.github.io/echolab/gsheets-airtable/dashboard.html con dos vistas y glosario |

## Lo que falta

| Lo que propones | Qué se necesita para implementarlo |
|---|---|
| Integrar Amazon Music | Datos de Amazon Music en CSV con el mismo formato que los de Spotify. Necesito saber qué campos exporta Amazon y recibirlos |
| Velocidad de crecimiento | Es una formula adicional en Sheets. Necesito que llenes la ficha de requerimiento para definir exactamente qué comparar y cómo mostrarlo |
| Contexto de eventos (Capa 4) | Un Google Form para que el artista/manager registre eventos del mes. Necesito que llenes la ficha con los tipos de evento, los campos, y un ejemplo |
| Mapa de calor territorial (Capa 5) | Los datos de ciudades de Spotify for Artists y de Meta Business Suite, exportados en CSV o en formato tabular. Necesito que investigues cómo exportarlos y me los entregues en el formato de Sheets |
| Reporte PDF | Definir qué secciones incluye, en qué orden, y con qué datos. Cada sección es una ficha |

## Lo que necesito de ti

Para cada item de la columna "Lo que falta", necesito una ficha de requerimiento completa. Te la comparto por separado.

Sin la ficha llena, no es posible avanzar en la implementación.
