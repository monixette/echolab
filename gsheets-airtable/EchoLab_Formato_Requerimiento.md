# EchoLab: Formato de Requerimiento de Datos

## Instrucciones

Cada vez que tengas una idea, un feature, o algo que quieras en el dashboard o en un reporte, llena **una copia de esta ficha** y entrégala.

No necesitas saber de fórmulas ni de código. Solo necesitas responder estas 7 preguntas con precisión. Si no es posible aún responder alguna, posiblemente requiere más información o clarificación.

**Reglas:**
- No es posible trabajar con información de screenshots para traducirla en datos de un periodo.
- Los datos deben entregarse en el formato homologado de Google Sheets o uno similar a nivel de formato: [Rubimente - Data Consolidada](https://docs.google.com/spreadsheets/d/1TU9Bz4ueHJESmwmjH-kwBNp2utqYykLSiFSHcsBnk-U/edit?gid=0#gid=0)
- La ficha debe estar completa sin campos vacíos.

---

## La Ficha

```
NOMBRE DEL FEATURE: [Nombre corto]

1. ¿QUÉ PREGUNTA RESPONDE?
   Escribe la pregunta exacta que el artista o manager se haría.

2. ¿QUÉ NÚMEROS SE NECESITAN?
   Lista los datos específicos con nombre propio.
   No "engagement". No "métricas". Números concretos.

3. ¿DE DÓNDE SALEN Y EN QUÉ FORMATO?
   Plataforma, sección, y formato de exportación (CSV, Excel).
   Los datos deben poder exportarse e integrarse al Google Sheets.

4. ¿CÓMO SE COMBINAN?
   Explica en español la operación lógica.
   No fórmulas, solo la lógica paso a paso.

5. ¿QUÉ RESULTADO SE MUESTRA?
   ¿Un número? ¿Un gráfico? ¿Un color? ¿Un texto?
   Sé específico sobre lo que ve el usuario final.

6. ¿CON QUÉ FRECUENCIA CAMBIA?
   ¿Diario? ¿Semanal? ¿Mensual?

7. EJEMPLO CONCRETO CON NÚMEROS REALES
   Un caso con números de Rubimente o de cualquier artista.
   Si no puedes inventar un ejemplo, no entiendes lo que estás pidiendo.
```

---

## Ejemplos resueltos

### Ejemplo 1: Velocidad de crecimiento mensual

```
NOMBRE: Velocidad de crecimiento mensual

1. ¿QUÉ PREGUNTA RESPONDE?
   "¿Estoy creciendo más rápido o más lento que el mes pasado?"

2. ¿QUÉ NÚMEROS SE NECESITAN?
   - Monthly Listeners del mes actual
   - Monthly Listeners del mes anterior
   - Streams totales del mes actual
   - Streams totales del mes anterior

3. ¿DE DÓNDE SALEN Y EN QUÉ FORMATO?
   - Spotify for Artists > Overview > exportar CSV
   - Ya están en el CSV consolidado que procesamos con el ETL
   - Entrego el CSV actualizado cada mes

4. ¿CÓMO SE COMBINAN?
   - Se resta el mes anterior al actual
   - Se divide entre el mes anterior
   - Se multiplica por 100 para sacar porcentaje
   - Se hace para Listeners y para Streams por separado

5. ¿QUÉ RESULTADO SE MUESTRA?
   - Dos porcentajes: uno para Listeners, otro para Streams
   - Flecha verde si subió, roja si bajó
   - Si los dos suben: "Crecimiento sano"
   - Si Listeners sube pero Streams baja: "Alcance sin retención"
   - Si Streams sube pero Listeners baja: "Base leal sin nuevos fans"

6. ¿FRECUENCIA?
   Mensual. Se calcula al cerrar cada mes.

7. EJEMPLO CONCRETO:
   Febrero: 2,800 ML / 11,200 Streams
   Marzo: 3,200 ML / 10,800 Streams
   Resultado: Listeners +14.3% | Streams -3.6%
   Diagnóstico: "Alcance sin retención. Llegan oídos nuevos
   pero no se quedan."
```

### Ejemplo 2: Contexto de eventos

```
NOMBRE: Registro de eventos mensuales

1. ¿QUÉ PREGUNTA RESPONDE?
   "¿Qué pasó este mes que podría explicar los movimientos
   en los números?"

2. ¿QUÉ NÚMEROS SE NECESITAN?
   No son números. Son datos cualitativos:
   - Fecha del evento
   - Tipo: lanzamiento / colaboración / show / campaña pagada /
     aparición en medios / playlist editorial
   - Descripción corta (1 línea)
   - Alcance estimado (si lo hay)

3. ¿DE DÓNDE SALEN Y EN QUÉ FORMATO?
   - Del artista o manager.
   - Se diseña un Google Form con preguntas cerradas tipo checklist.
   - Se exporta automáticamente a una hoja de Google Sheets.
   - Se envía al artista o manager una vez al mes y se verifica
     que esté lleno.

4. ¿CÓMO SE COMBINAN?
   - Cada evento se asocia a una fecha.
   - Se cruza esa fecha con los datos de streaming ese día
     y los 5-10 días siguientes.
   - Si hay un spike en streaming dentro de esa ventana,
     el evento se marca como "probable detonador".

5. ¿QUÉ RESULTADO SE MUESTRA?
   - Una tarjeta que diga: "Evento: Lanzamiento Nena Brava
     (15 marzo). Spike de +280 streams detectado el 18 marzo."

6. ¿FRECUENCIA?
   Mensual. El formulario se envía al final de cada mes.

7. EJEMPLO CONCRETO:
   Evento: "Colaboración con Tequila Cabrito anunciada en IG"
   Fecha: 15 de marzo
   Streams promedio esa semana: 350
   Streams 18-20 de marzo: 520, 580, 490
   Resultado: "El anuncio generó un spike de +48% en ventana
   de 3-5 días."
```

---

## Regla de oro

> Si una idea no se puede convertir en esta ficha, es posible que requiera más clarificación antes de llegar a implementación. La diferencia entre "estaría cool tener un mapa de calor" y "necesito las ciudades top 10 de IG Reach cruzadas con las ciudades top 10 de Spotify Listeners, entregadas en CSV, para calcular un índice de temperatura territorial" es que lo segundo se puede construir. Lo primero es una conversación.
