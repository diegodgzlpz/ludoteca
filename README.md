# Ludoteca de Diego y Alba

Inventario de juegos de mesa con filtro por número de jugadores, tiempo disponible y edad.
Sitio estático: `index.html` lee `games.json` y muestra las portadas de `portadas-juegos/`.

## Estructura

```
index.html      La página (HTML, CSS y JS en un solo archivo, sin dependencias)
games.json      La colección: la única fuente de datos
portadas-juegos/ Una imagen por juego: <id>.jpg o <id>.png
cuentos.html    La página de cuentos infantiles (misma estructura, acento verde azulado)
cuentos.json    La colección de cuentos
portadas-cuentos/  Una imagen por cuento: <id>.jpg o <id>.png
.nojekyll       Evita que GitHub Pages procese el sitio con Jekyll
```

## Publicar en GitHub Pages

1. Copia tus portadas descargadas dentro de `portadas-juegos/` (los nombres ya coinciden con `games.json`).
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

Añade una entrada a `games.json` y, si tienes portada, su imagen en `portadas-juegos/`:

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
  "img": "portadas-juegos/azul-pabellon-de-verano.jpg",
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
"Cómo jugar" con los vídeos, expansiones, enlaces a BoardGameGeek y a YouTube y botón
para compartir por WhatsApp.
Se cierra con ✕, Esc, clic fuera o el botón "atrás".

- **Pasar de ficha** sin cerrarla: flechas ‹ › a los lados (en el móvil, abajo), teclas ← →
  o deslizando el dedo. Recorre la lista que se está viendo, con sus filtros y su orden, y del
  último vuelve al primero; el contador «12 / 48» indica la posición. Si la ficha se abrió
  desde un enlace, recorre toda la colección por orden alfabético. Cuentos funciona igual.

- Los vídeos se muestran como miniaturas (`i.ytimg.com`); el reproductor de
  `youtube-nocookie.com` solo se carga al pulsar play, y solo uno a la vez.
- Un juego sin vídeos muestra un enlace a la búsqueda "<nombre> cómo se juega" en YouTube.

### Enlaces directos

Cada ficha tiene su dirección: `https://<usuario>.github.io/ludoteca/#<id>`, por ejemplo
`#catan`. Abrir esa dirección abre la ficha; el enlace de una expansión
(`#catan-navegantes`) abre la ficha de su juego base.

El botón **Compartir por WhatsApp** de la ficha abre WhatsApp con el nombre del juego y
ese enlace; quien lo pulse entra con la ficha ya abierta. La vista previa del enlace en
WhatsApp muestra el título general de la ludoteca, no el del juego (WhatsApp ignora el `#`).
Las fichas de cuentos no llevan este botón.

## Ordenar, "jugado" y valoraciones

- **Ordenar**: botones Nombre, Dificultad, Duración, Año, Nota BGG y Mi nota. Pulsar otra
  vez el que está activo invierte el orden (la flecha ↑/↓ lo indica). Los juegos sin ese
  dato van siempre al final.
- **Lo he jugado y Mi nota**: en la ficha, cada persona marca si ha jugado el juego y le
  pone de 1 a 5 estrellas (valorar lo marca como jugado; pulsar la misma estrella la quita).
  La tarjeta muestra «✓ Jugado · ★★★★».
- **Yo**: filtro Todos / Jugados / Sin jugar.
- **🎲 Sorpréndeme**: abre al azar un juego sin jugar que encaje con los filtros puestos.
- **En el móvil** los filtros y el orden se pliegan tras el botón «Filtros y orden», que
  muestra cuántos filtros hay activos; la búsqueda y Sorpréndeme quedan siempre a la vista.

Las marcas se guardan **solo en el navegador de cada dispositivo** (`localStorage`, clave
`ludoteca:mis-juegos`): no hay cuentas, no se sincronizan entre móvil y ordenador y se
pierden si se borran los datos del sitio. Cuentos no tiene estas opciones.

## Diseño

Estilo editorial cálido: fondo crema, títulos en Source Serif 4, texto en Inter y cifras en
JetBrains Mono (Google Fonts). Juegos usa acento coral con el panel de filtros oscuro; cuentos,
acento verde azulado con el panel claro. Los colores están como variables al principio del
`<style>` de cada página, con su versión para modo oscuro (se activa según el sistema).

## Cuentos

`cuentos.html` («Cuentos de Óliver») es la misma estantería para cuentos infantiles, en
`https://<usuario>.github.io/ludoteca/cuentos.html`. Cada página enlaza a la otra
desde la cabecera. Filtra por edad del niño o niña («Tiene»: Bebé, 2…8, 10+), por tema y
por texto (título, autor, ilustrador o tema). Los cuentos no llevan vídeos. Los datos salen de
[Open Library](https://openlibrary.org) y [Casa del Libro](https://www.casadellibro.com), que da
la edad recomendada y las portadas; para añadir uno, usa `/anadir-cuento <título o ISBN>`.

```json
{
  "id": "el-monstruo-de-colores",
  "name": "El monstruo de colores",
  "author": "Anna Llenas",
  "illustrator": "Anna Llenas",
  "publisher": "Flamboyant",
  "year": 2014,
  "isbn": "9788494157820",
  "age": 3,
  "pages": 40,
  "format": "Álbum ilustrado",
  "temas": ["Emociones"],
  "img": "portadas-cuentos/el-monstruo-de-colores.jpg"
}
```

| Campo | Obligatorio | Significado |
|---|---|---|
| `id` | sí | Identificador único, en minúsculas y con guiones. Nombre del archivo de portada. |
| `name` | sí | Título que se muestra. |
| `author`, `illustrator`, `publisher` | no | Autor, ilustrador y editorial de la edición. |
| `year`, `pages`, `isbn` | no | Año de la edición, páginas e ISBN-13 (enlaza a Open Library). |
| `age` | no | Edad mínima recomendada (0 para bebés). Sin ella, el cuento no aparece al filtrar por edad. |
| `format` | no | Cartoné, Tapa blanda, Álbum ilustrado, Pop-up… |
| `temas` | no | Se muestran los tres primeros; alimentan el desplegable «Tema» y la búsqueda. |
| `img`, `summary`, `hue` | no | Portada (de Casa del Libro), resumen breve y tono de la portada generada. |

Valida el archivo con `python -m json.tool cuentos.json > /dev/null`.

Datos y portadas de [BoardGameGeek](https://boardgamegeek.com), [Open Library](https://openlibrary.org) y [Casa del Libro](https://www.casadellibro.com).
