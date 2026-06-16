# Skill: Clima Local

Obtén el clima actual para una ubicación. Si el usuario no especifica una ciudad, usa **Ciudad de México** como predeterminado.

## Instrucciones

1. Determina la ubicación:
   - Si el usuario pasó un argumento (p. ej. `/clima Guadalajara`), usa esa ciudad.
   - Si no hay argumento, usa **Yautepec, Morelos, México**.

2. Consulta el clima usando `curl` con la API pública de wttr.in (no requiere API key):

```bash
curl -s "wttr.in/{CIUDAD}?format=j1&lang=es"
```

Reemplaza `{CIUDAD}` con el nombre de la ciudad (sustituye espacios por `+`).

3. Parsea el JSON de respuesta y presenta la información con este formato:

```
📍 Ciudad: <nombre completo>
🌡️  Temperatura actual: <temp_C>°C (sensación térmica: <FeelsLikeC>°C)
🌤️  Condición: <descripción en español>
💧 Humedad: <humidity>%
💨 Viento: <windspeedKmph> km/h dirección <winddir16Point>
👁️  Visibilidad: <visibility> km

🌅 Pronóstico de hoy:
   Máxima: <maxtempC>°C  |  Mínima: <mintempC>°C
   Amanecer: <sunrise>  |  Atardecer: <sunset>
```

4. Si el campo `weatherDesc` viene en inglés, tradúcelo al español antes de mostrarlo.

5. Si `curl` falla o la ciudad no se encuentra, informa al usuario y sugiere verificar el nombre de la ciudad.

## Argumentos

`$ARGUMENTS` — nombre de la ciudad (opcional). Ejemplos: `Monterrey`, `Nueva York`, `London`.
