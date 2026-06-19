# Liga de Pádel — cómo publicarla (gratis)

Esto es un único archivo (`index.html`) que ya tiene toda la liga armada.
Para que **todos tus compañeros vean y editen la misma tabla**, necesitás conectarlo
a una base de datos gratuita (JSONBin) y después subirlo a GitHub Pages. Son ~10 minutos.

---

## Paso 1 — Crear tu API Key gratis en JSONBin

1. Entrá a https://jsonbin.io y registrate gratis (podés usar Google/GitHub).
2. Una vez adentro, andá a **API Keys** (en el menú de tu cuenta).
3. Copiá tu **X-Master-Key** (es un string largo tipo `$2a$10$...`).

## Paso 2 — Crear el "bin" (donde se guardan los resultados)

Tenés dos formas, elegí la que te resulte más cómoda:

### Opción A — Con curl (si tenés terminal a mano)

```bash
curl -X POST https://api.jsonbin.io/v3/b \
  -H "Content-Type: application/json" \
  -H "X-Master-Key: TU_API_KEY_ACA" \
  -H "X-Bin-Name: liga-padel" \
  -d '{
    "matches": [
      {"id":"f1","label":"Viernes 19/6","teamA":["Roman","Nahue"],"teamB":["Gonza","Luis"],"winner":null},
      {"id":"f2","label":"Lunes 22/6","teamA":["Gonza","Luis"],"teamB":["Roman","Nahue"],"winner":null},
      {"id":"f3","label":"Viernes 26/6","teamA":["Gonza","Roman"],"teamB":["Luis","Nahue"],"winner":null},
      {"id":"f4","label":"Lunes 29/6","teamA":["Roman","Luis"],"teamB":["Gonza","Nahue"],"winner":null}
    ]
  }'
```

La respuesta trae un campo `"id"` — ese es tu **BIN_ID**. Copialo.

### Opción B — Sin terminal, con Postman/web

Si no tenés terminal, decime y te doy una alternativa sin curl (por ejemplo,
pegando el mismo JSON en el "Quick Create" de jsonbin.io desde su web).

## Paso 3 — Completar el archivo

Abrí `index.html` con cualquier editor de texto y buscá esta sección cerca del final:

```js
const JSONBIN_API_KEY = "PEGA_TU_API_KEY_ACA";
let BIN_ID = "PEGA_TU_BIN_ID_ACA";
const EDIT_PIN = "2580";
```

Reemplazá:
- `PEGA_TU_API_KEY_ACA` → tu X-Master-Key del paso 1
- `PEGA_TU_BIN_ID_ACA` → el `id` que te devolvió el paso 2
- `2580` → el PIN que quieras usar para habilitar la edición (podés dejarlo)

Guardá el archivo.

## Paso 4 — Subir a GitHub Pages (gratis)

1. Andá a https://github.com y creá una cuenta si no tenés.
2. Click en **New repository** (botón verde). Nombre sugerido: `liga-padel`.
   Marcalo como **Public**. Creá el repo.
3. Dentro del repo, click en **Add file → Upload files**.
4. Arrastrá el `index.html` ya editado (con tu API key y bin id puestos).
5. Click en **Commit changes**.
6. Andá a **Settings → Pages** (menú de la izquierda).
7. En "Branch", elegí `main` y la carpeta `/ (root)`. Guardá.
8. Esperá 1-2 minutos. GitHub te va a mostrar la URL pública, algo como:

   `https://TU_USUARIO.github.io/liga-padel/`

Esa es la URL que le compartís a Gonza, Roman, Luis y Nahue. Cualquiera puede
**ver** la tabla sin PIN; para **marcar un ganador** hace falta tocar
"Desbloquear" e ingresar el PIN que configuraste.

---

## Notas importantes

- **Es gratis** tanto JSONBin (free tier) como GitHub Pages, sin tarjeta de crédito.
- El PIN es una protección simple, no es seguridad real — cualquiera que lo
  tenga puede editar desde cualquier dispositivo. Está pensado solo para
  evitar toques accidentales o de gente ajena al grupo.
- Si en algún momento querés resetear la liga, solo hace falta volver a hacer
  el PUT al bin con los partidos en `"winner": null`.
- La página revisa cambios cada 8 segundos automáticamente, así que si alguien
  carga un resultado, los demás lo ven solos sin recargar.
