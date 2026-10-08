# Ludoteca de Diego

Inventario de juegos de mesa con filtro por número de jugadores, tiempo disponible y edad.
Sitio estático: `index.html` lee `games.json` y muestra las portadas de `portadas/`.

## Estructura

```
index.html      La página (HTML, CSS y JS en un solo archivo, sin dependencias)
games.json      La colección: la única fuente de datos
portadas/       Una imagen por juego: <id>.jpg o <id>.png
.nojekyll       Evita que GitHub Pages procese el sitio con Jekyll
```

## Publicar en GitHub Pages

1. Copia tus portadas descargadas dentro de `portadas/` (los nombres ya coinciden con `games.json`).
2. Crea un repositorio **público** en GitHub, por ejemplo `ludoteca`, y sube el contenido:
   ```
   git init
   git add .
   git commit -m "Ludoteca inicial"
   git branch -M main
   git remote add origin https://github.com/<tu-usuario>/ludoteca.git
   git push -u origin main
   ```
3. En el repositorio: **Settings → Pages → Build and deployment → Source: Deploy from a branch**,
   rama `main`, carpeta `/ (root)`. Guarda.
4. En uno o dos minutos estará en `https://<tu-usuario>.github.io/ludoteca/`.

## Probar en local

La página carga `games.json` con `fetch`, que no funciona abriendo el archivo con doble clic.
Lanza un servidor en la carpeta y abre http://localhost:8000:

```
python -m http.server 8000
```

## Añadir un juego

Añade una entrada a `games.json` y, si tienes portada, su imagen en `portadas/`:

```json
{
  "id": "azul-pabellon-de-verano",
  "name": "Azul: Pabellón de Verano",
  "bgg": "287954",
  "year": 2019,
  "pmin": 2, "pmax": 4,
  "tmin": 30, "tmax": 45,
  "weight": 2.3,
  "age": 8,
  "rating": 7.6,
  "best": 2,
  "tags": ["Patrones", "Colección de sets"],
  "img": "portadas/azul-pabellon-de-verano.jpg",
  "videos": [
    {"yt": "WOoKrjIkMxU", "t": "Conociendo Azul Pabellón de Verano"}
  ]
}
```

| Campo | Obligatorio | Significado |
|---|---|---|
| `id` | sí | Identificador único, en minúsculas y con guiones. Nombre del archivo de portada. |
| `name` | sí | Nombre que se muestra. |
| `bgg` | no | ID de BoardGameGeek: el número de `boardgamegeek.com/boardgame/<id>/...` |
| `pmin`, `pmax` | no | Jugadores mínimo y máximo. Sin ellos, el juego no aparece al filtrar por jugadores. |
| `tmin`, `tmax` | no | Duración en minutos. Sin ellos, no aparece al filtrar por tiempo. |
| `weight` | no | Dificultad de 1 a 5 (el *weight* de BGG). |
| `age` | no | Edad mínima. Sin ella, el juego no aparece al filtrar por edad. |
| `year`, `rating`, `best` | no | Año, nota de BGG y mejor número de jugadores. |
| `tags` | no | Mecánicas; se muestran las tres primeras y sirven para la búsqueda. |
| `img` | no | Ruta de la portada. Sin ella se genera una portada con el nombre. |
| `parent` | no | `id` del juego base si es una expansión. Se muestra dentro de su tarjeta. |
| `videos` | no | Vídeos de "Cómo jugar": lista de `{"yt": "<id>", "t": "<título>"}`. Ver abajo. |
| `notes`, `hue` | no | Notas internas (no se muestran en la página, pero cualquiera puede leerlas en `games.json`) y tono de la portada generada. |

Si el JSON queda mal formado, la página muestra "No se pudo cargar la colección".
Valídalo antes del push con `python -m json.tool games.json > /dev/null`.

### Vídeos (`videos`)

- `yt`: el id de 11 caracteres del vídeo, el de `youtube.com/watch?v=<id>`.
- `t`: el título que se muestra bajo la miniatura.
- Dos o tres como mucho, en orden: primero la serie "Conociendo / Abriendo / Ampliando /
  Reeditando" de Zacatrus, luego tutoriales de la editorial o en español.
- Las expansiones llevan sus propios vídeos; se muestran en la ficha del juego base
  con la etiqueta "Expansión: <nombre>".

## Ficha del juego

Al pulsar cualquier parte de una tarjeta se abre su ficha: datos del juego, carrusel
"Cómo jugar" con los vídeos, expansiones y enlaces a BoardGameGeek y a YouTube.
Se cierra con ✕, Esc, clic fuera o el botón "atrás".

- Los vídeos se muestran como miniaturas (`i.ytimg.com`); el reproductor de
  `youtube-nocookie.com` solo se carga al pulsar play, y solo uno a la vez.
- Un juego sin vídeos muestra un enlace a la búsqueda "<nombre> cómo se juega" en YouTube.

### Enlaces directos

Cada ficha tiene su dirección: `https://<usuario>.github.io/ludoteca/#<id>`, por ejemplo
`#catan`. Abrir esa dirección abre la ficha; el enlace de una expansión
(`#catan-navegantes`) abre la ficha de su juego base.

Datos y portadas de [BoardGameGeek](https://boardgamegeek.com).
