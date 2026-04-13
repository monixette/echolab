

**ECHOLAB**

*Documento interno de visión y metodología*

Abril 2026

*Confidencial — uso interno*

# **¿Por qué existe este documento?**

Mi Yoyo, este documento nace de una introspección profunda que tuve sobre EchoLab: lo que es, hacia dónde va, y qué necesitamos construir para que sea lo que tenemos en mente.

No es un reporte de avance. Es un documento de alineación. Quiero que entiendas la visión completa, los gaps que identifiqué en lo que llevamos construido, y la propuesta de arquitectura que creo que sí cumple con lo que buscamos.

Léelo como una conversación, no como un brief.

# **1\. La visión: qué es EchoLab**

EchoLab nace de una observación simple pero poderosa:

| Todos los artistas tienen acceso a sus datos. Casi ninguno los analiza. Y los que los analizan, rara vez los cruzan entre plataformas para entender qué está moviendo realmente la aguja. |
| :---- |

La idea es convertir EchoLab en una plataforma de suscripción mensual que ayude a artistas en desarrollo a entender cómo su actividad en redes sociales tiene un impacto directo en plataformas de streaming, y entregarles esa inteligencia de manera digerible, sin que tengan que ser analistas de datos para entenderla.

El posicionamiento es claro: imagina un Chartmetric, pero hecho mucho más sencillo. Donde el artista o su manager entran, ven qué movió la aguja este mes, y toman decisiones con esa información. Sin tener que interpretar tablas, sin perderse en métricas que no entienden, sin publicar por publicar.Ven un dashboard que les ayuda visualmente a ver qué se movió y pueden descargar el reporte más compelto (de lo que ven en el dahsboard) en PDF.

## **¿A quién va dirigido?**

EchoLab no es para artistas nuevos en ceros. Necesitamos 'carnita' para analizar: un catálogo razonable de canciones, tiempo suficiente en plataformas para que los datos tengan comportamiento, y actividad en redes que se pueda cruzar con los movimientos en streaming.

El cliente ideal en esta fase es el artista en desarrollo con carrera establecida (mínimo 1-2 años activo, con catálogo en plataformas) y su círculo: manager, sello independiente, agencia de booking.

## 

## **El diferenciador más poderoso: la ruta de shows**

Más allá del análisis de contenido, EchoLab tiene un diferenciador que ninguna herramienta tiene bien empaquetado: el cruce demográfico y geográfico entre consumo de contenido en redes sociales y consumo de música en plataformas de streaming, para identificar en qué ciudades un artista tiene mayor probabilidad de éxito en un show.

Los booking agents hoy trabajan con feeling, con datos de una sola fuente (o rrss, o streaming, no ambos), y con intuición de mercado. El problema es que eso genera decisiones costosas. Veamos un ejemplo real:

| Caso real: La Garfield en el Pepsi Center WTC La Garfield es una banda con números sólidos: 73.5k seguidores en Instagram, 405k monthly listeners en Spotify, y canciones con más de 15-20 millones de reproducciones. El 13 de febrero de 2026 tocaron en el Pepsi Center WTC, con capacidad de entre 6,500 y 8,000 personas en formato concierto. El resultado: entre 50% y 60% de ocupación. Ticketmaster los puso al 2x1 y ni así superaron ese porcentaje. ¿Qué pudo haber pasado diferente con información cruzada? Si hubieran visto que ciudades como (solo un ejemplo) Puebla, Querétaro, Pachuca o Cuernavaca tenían mayor tracción combinada (consumo de contenido \+ consumo de música), podrían haber armado una ruta de shows más pequeños con sold outs sostenidos, superado en boletaje total al Pepsi Center, y llegado a CDMX con una historia de éxito que generara FOMO. En cambio, llegaron a CDMX sin esa historia, con un venue que los rebasó, y con una venta de entradas que los obligó a ofrecer 2x1. Eso es exactamente lo que EchoLab puede prevenir. |
| :---- |

No se trata de garantizar el éxito de un show. Se trata de reducir el riesgo y darle al artista y al manager información suficientemente clara para tomar una mejor decisión.

# **2\. Lo que hemos construido hasta ahora**

Para desarrollar la metodología de EchoLab, estamos usando a Rubimente como caso piloto. Rubi tiene más de 16 canciones en plataformas desde 2020, es muy activa en redes sociales, y tiene en puerta el lanzamiento de 'Nena Brava', patrocinado por Tequila Cabrito. Es el conejillo de indias ideal.

Hasta ahora hemos producido dos reportes principales: un reporte de performance con análisis de Instagram y Spotify, y un documento de metodología EchoLab. Ambos tienen cosas muy buenas que vale la pena reconocer:

| Lo que sí logramos • Metodología Spotify-first: tratamos Spotify como la capa central de datos y usamos la actividad en redes para explicar los picos observados en streaming. Esto resuelve el problema de atribución. • EchoLab Score: un índice propietario de 10 puntos que pondera las métricas de Spotify por su valor estratégico (Save 3.5, MAL 2.0, Playlist Add 1.5, Follower 1.5, Listener 1.0, Stream 0.5). • Distinción clean days / noisy days: días con una sola publicación permiten atribución directa de tipo de contenido; días con múltiples publicaciones solo permiten conclusiones de volumen. • Hallazgos accionables: identificamos que el contenido emocional/desamor en Instagram genera saves y streams en Spotify en ventanas de 24-48 horas, y hasta días 3-4 post-publicación. • Cruce demográfico: detectamos una discrepancia importante entre la audiencia de Instagram (\~59% femenina) y la audiencia de Spotify (\~57% masculina). |
| :---- |

## **El gap: lo que los reportes actuales no responden**

Con toda esa base sólida, hay una pregunta que los reportes actuales no responden, y creo que es la pregunta más importante para EchoLab:

| ¿Qué información le falta a este artista para tomar una mejor decisión de negocio? |
| :---- |

Los reportes actuales describen bien qué pasó y razonablemente bien por qué pasó. Pero se detienen justo antes de la capa de mayor valor: la inteligencia de mercado que le permite al artista o al manager entender su posición territorial, la velocidad real de su crecimiento, y hacia dónde debería dirigir su energía.

Específicamente, lo que no está resuelto hoy es:

* No hay cruce de datos de TikTok y Amazon Music con las otras plataformas, el análisis es Instagram \+ Spotify, pero no el ecosistema completo. Recién estamos integrando TikTok y Amazon.

* No hay análisis de velocidad de crecimiento, solo de volumen. Saber que subiste 500 listeners es distinto a saber si llevas 4 meses desacelerando.

* No hay contexto de eventos registrados, si Rubimente lanzó una colaboración y los saves subieron 40%, eso es un hallazgo poderoso. Pero sin registrar qué pasó ese mes, el número queda huérfano.

* No hay mapa de calor territorial (que es el que tú estás armando), no sabemos en qué ciudades Rubi está 'más caliente' considerando el cruce de consumo de contenido y consumo de música.

* No hay comparativa mes a mes estructurada, cada reporte es una foto, no una película.

# **3\. La propuesta: arquitectura de análisis en capas**

Para que EchoLab cumpla con su visión, el reporte necesita estar construido en capas que respondan preguntas distintas y que juntas den una fotografía completa. Aquí está la arquitectura propuesta:

## **Capa 1 — Redes Sociales**

Análisis independiente por plataforma: qué pasó en Instagram, qué pasó en TikTok. Métricas de alcance, engagement, crecimiento de comunidad, demografía y geografía de audiencia. Al final de esta capa: un análisis general que cruza ambas plataformas de rrss entre sí.

## **Capa 2 — Plataformas de Streaming**

Análisis independiente por plataforma: qué pasó en Spotify, qué pasó en Amazon Music. Streams, saves, listeners, followers, playlist adds, MAL. Desglose geográfico y demográfico. Al final de esta capa: un análisis general que cruza ambas plataformas de streaming entre sí.

## **Capa 3 — Cruce de datos: Redes Sociales vs. Streaming**

Esta es la capa más poderosa y la que define a EchoLab. Responde preguntas que ninguna plataforma responde por separado:

* ¿Qué tipo de contenido en rrss generó movimiento en streaming, y en qué ventana de tiempo?

* ¿Quién consume contenido en redes versus quién consume música? ¿Coinciden demográficamente o hay discrepancias?

* ¿Hay ciudades donde hay consumo fuerte de contenido pero débil en streaming, o viceversa?

* ¿Qué está acelerando y qué está desacelerando? No solo cuánto creció, sino a qué ritmo.

## **Capa 4 — Contexto de eventos**

Los datos sin contexto son números huérfanos. Esta capa registra qué ocurrió en el período analizado: lanzamientos, colaboraciones, shows, campañas pagadas, apariciones en medios, sincronizaciones.

¿Cómo se alimenta esta capa? Por el momento no contamos con una herramientos confiable de Social Listening. La solución, un formulario mensual enviado al artista o manager. Diseñado para llenarse en 10-15 minutos, con preguntas principalmente cerradas. Si no se llena, las otras capas siguen siendo válidas, solo pierden el contexto explicativo.

Para los primeros reportes de artistas con quienes no tenemos relación de cliente (como los reportes de intercambio de valor que estamos planeando), se complementa con información pública disponible.

## **Capa 5 — Mapa de calor territorial**

Esta es la capa que diferencia a EchoLab de todo lo que existe en el mercado. Un mapa de calor que cruza el consumo de contenido en rrss por ciudad con el consumo de música en streaming por ciudad, para mostrar:

* Ciudades con alta tracción combinada: mayor probabilidad de éxito en un show.

* Ciudades con consumo fuerte en rrss pero débil en streaming: conectan con el artista como persona, pero aún no consumen su música. Necesitan trabajo antes de un show.

* Ciudades con consumo fuerte en streaming pero débil en rrss: escuchan la música, pero no tienen relación con el artista en digital. Oportunidad de contenido dirigido.

Esta sección aparece desde el primer reporte, no cuando el artista 'esté listo'. La idea es que el artista y el manager vean cómo evoluciona el mapa mes a mes. No es una ruta de shows, es una fotografía territorial de dónde está el artista hoy.

| Capa | ¿Qué responde? | Fuente de datos |
| :---- | :---- | :---- |
| **1 — Redes Sociales** | ¿Qué pasó en IG y TikTok? | CSVs de Instagram y TikTok |
| **2 — Streaming** | ¿Qué pasó en Spotify y Amazon? | CSVs de Spotify y Amazon Music |
| **3 — Cruce de datos** | ¿Qué contenido movió qué? ¿Hay desfase demográfico o geográfico? | Cruce de Capas 1 y 2 |
| **4 — Contexto de eventos** | ¿Qué ocurrió ese mes que explica los datos? | Formulario mensual \+ fuentes públicas |
| **5 — Mapa de calor** | ¿Dónde está más caliente el artista? ¿Dónde hay oportunidad? | Cruce geográfico Capas 1 y 2 |

# 

# 

# **4\. Estructura del reporte EchoLab**

Con esta arquitectura de capas, la estructura del reporte queda así:

| Sección | Contenido |
| :---- | :---- |
| **0\. Portada** | Nombre del artista, período analizado, fecha de entrega. |
| **1\. Resumen ejecutivo** | Una página. Para el manager o sello que no tiene tiempo. ¿Qué pasó, qué movió la aguja, qué es lo más importante saber? |
| **2\. Contexto del período** | Eventos relevantes del mes. Sin esto, los números no tienen historia. (Capa 4\) |
| **3\. Redes Sociales** | Instagram \+ TikTok. Análisis por plataforma \+ comparativa \+ tendencia vs. mes anterior. |
| **4\. Plataformas de Streaming** | Spotify \+ Amazon Music. Análisis por plataforma \+ comparativa \+ tendencia vs. mes anterior. |
| **5\. Cruce de datos** | Contenido vs. streaming. Demografía comparada. Geografía comparada. Velocidad de crecimiento. |
| **6\. Hallazgos y findings** | Los insights más importantes del período, ordenados por relevancia. Sin relleno. |
| **7\. Recomendaciones** | Basadas en los findings. No decisiones de negocio — información suficientemente clara para que la decisión sea obvia. |
| **8\. Mapa de calor territorial** | Fotografía de dónde está el artista hoy en términos de presencia territorial. |
| **9\. Contexto histórico** | Solo en el reporte inicial: análisis completo desde el inicio del proyecto. Es la línea base. En reportes subsecuentes se convierte en tendencia acumulada. |

# **5\. El roadmap de EchoLab**

Para que tengas visibilidad completa de hacia dónde vamos, aquí está el roadmap que tenemos en mente:

## **Fase 1 — Piloto con Rubimente (ahora)**

Estamos construyendo y validando la metodología usando a Rubimente como caso de prueba. El objetivo es encontrar la receta perfecta del reporte antes de escalar. El reporte inicial de Rubimente será la línea base contra la que se medirá el impacto del lanzamiento de 'Nena Brava' (canción hecha en conjunto con Tequila Cabrito).

## **Fase 2 — Casos de éxito (próximos meses)**

Una vez que tengamos el reporte de Rubimente bien construido, lo usaremos como moneda de intercambio para abrir relaciones con artistas medianos. La estrategia: ofrecer el primer reporte sin costo a cambio de colaboración o servicios. Los primeros objetivos son Playa Limbo y Renee Mooi.

En esta fase, los CSVs los envían los artistas/managers manualmente. Hacemos videos de onboarding e instructivos de 'dónde picarle' para que sepan exactamente qué descargar y en qué rango de fechas. Es tedioso, pero es solo para generar casos de éxito.

## **Fase 3 — Apertura por invitación**

Con casos de éxito en mano y una página que explique qué hace EchoLab, abrimos la plataforma por invitación. Solo aceptamos los proyectos que podamos manejar de manera manual. Comenzamos a generar ingresos recurrentes.

En esta fase EchoLab funciona como agencia: acceso como colaboradores en Meta Business Suite para descarga de datos de rrss, formato de agencia como lo platicaste, y acceso como analistas en las plataformas de streaming de cada artista ([eduardo@echolab.mx](mailto:eduardo@echolab.mx) / [monica@echolab.mx](mailto:monica@echolab.mx)).

## **Fase 4 — Automatización y escala**

Una vez que estemos facturando algo mensual que nos permita vivir, levantamos capital para desarrollar la automatización. En ese momento se abren las APIs y la plataforma se abre para cualquier suscriptor.

# 

# **6\. Cierre**

My Yoyo, la información que necesitan los artistas ya existe. Está en sus paneles de Instagram, en su Spotify for Artists, en su TikTok Analytics. El problema no es acceso a datos, es que nadie les ha dado una forma clara de leerlos juntos. OJO: actualmente pueden usar Chartmetric, pero se sigue necesitando un cierto ojo de analista de datos para entender lo que está pasando. 

Eso es lo que EchoLab resuelve. No somos una herramienta de redes sociales, no somos un tracker de streaming. Somos la capa que cruza ambos mundos y convierte datos dispersos en inteligencia de mercado, **bien masticadita y lista para llevarse a la boca**,  que cualquier artista o manager puede leer y usar.

| El siguiente paso es construir el reporte completo de Rubimente bajo esta nueva arquitectura. Ese reporte es el molde del que van a salir todos los que vienen después. |
| :---- |

**Yoyo Ed**