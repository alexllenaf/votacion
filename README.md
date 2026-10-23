# Votación en directo: página de voto

Página para el móvil de la votación en directo de la presentación AI Organisational Impact (diapositiva «Votación en directo», justo antes de la de retos: la votación se abre sola al llegar a ella). El QR de la presentación trae aquí con la sesión en la dirección (`#s=<id>`). Cada voto se publica en ntfy.sh, en el tema `aiimpact-<id>`, y la presentación lo recibe al momento.

Es un solo fichero, `index.html`, sin dependencias ni servidor propio.

## Publicarla en GitHub Pages

La presentación espera la página en `https://alexllenaf.github.io/votacion/` (la dirección está en `src/nucleo/votacion.js` del deck). Para dejarla ahí:

1. En GitHub, con la cuenta `alexllenaf`, crear un repositorio **público** llamado `votacion`. El nombre da la ruta `/votacion/`, y con el plan gratuito Pages solo publica repositorios públicos.
2. Subir `index.html` y este README, desde la web (*Add file → Upload files → Commit changes*) o desde esta carpeta:
   ```powershell
   git init
   git add index.html README.md
   git commit -m "Página de voto"
   git branch -M main
   git remote add origin https://github.com/alexllenaf/votacion.git
   git push -u origin main
   ```
3. En *Settings → Pages → Build and deployment*, elegir *Deploy from a branch*, rama `main` y carpeta `/ (root)`, y pulsar *Save*.
4. Esperar uno o dos minutos y abrir en el móvil `https://alexllenaf.github.io/votacion/#s=prueba12345`. Tiene que salir la pregunta con las cuatro opciones. Votar ahí publica un mensaje de prueba en un tema que no escucha nadie.

Para cambiarla después basta con subir el `index.html` nuevo; Pages lo publica solo en uno o dos minutos.

## Probarla

- En el deck: con `?votacion=demo` se ensaya sin red (al llegar a la diapositiva de la votación llegan 14 votos simulados en 8 s); sin parámetros, la votación es real.
- En local, sin publicar: abrir `index.html#s=<id>` en el navegador, con el id que lleva el QR del deck. Los votos llegan igual, porque los envía el navegador a ntfy.sh.
- Las pruebas del deck (`node scripts/test-votacion.mjs`) votan con esta página en local.

## Cómo funciona y privacidad

- Al tocar una opción, el móvil envía a `https://ntfy.sh/aiimpact-<id>` el cuerpo `{"v": 1-4, "u": "<id>"}`. `u` es un identificador al azar que se guarda en el navegador del móvil para que, si alguien cambia de opinión, cuente solo su último voto.
- No se pide nombre ni correo y el voto no lleva ningún dato personal. ntfy.sh es un servicio público y gratuito, sin cuentas. Guarda los mensajes unas 12 horas y, como cualquier web, ve la dirección IP de quien vota.
- El tema cambia en cada sesión (12 caracteres al azar), así que solo quien tiene el QR puede votar o leer los votos.
- Si no hay red o el envío falla, la página lo dice y deja reintentar.

## Límites

- ntfy.sh no garantiza el servicio. Por dirección IP admite unas 60 peticiones seguidas y después una cada 5 segundos, de sobra para una reunión aunque toda la sala vote desde la misma wifi.
- Si la presentación se queda sin red, lo avisa y los votos se pueden contar a mano con las teclas 1-4 (Mayús+1-4 los quitan).
- La página no lleva marcas ni contenido de clientes, solo la pregunta y las cuatro opciones.

## Si cambia la dirección

Si se publica en otra dirección, hay que cambiarla también en el deck: `PAGINA_VOTO` en `src/nucleo/votacion.js` y su excepción en `comprobarDist` (`vite.config.js`).
