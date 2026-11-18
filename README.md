# Operación Cigüeña

Escape room web (HTML/CSS/JS, sin dependencias) para anunciar un bebé de forma divertida.

## Cómo jugar
Abre `index.html` o el link generado. Son 4 retos que desbloquean la frase final.

## Link personalizado
Abre `generador.html`, completa los datos de cada persona y copia el link. Los datos van en el parámetro `?d=` (JSON en base64url con las claves `n` nombre, `a` apodo, `r` rol `o`/`a`, `c` chiste, `f` fecha). Sin parámetro, el juego usa textos genéricos.

Privacidad: el repo no contiene datos personales. Base64 no es cifrado: quien tenga el link puede leer sus datos.

## Publicar con GitHub Pages
1. Crea un repositorio público y sube estos archivos.
2. Settings > Pages > Deploy from a branch > `main` / root.
3. Tu juego queda en `https://TU-USUARIO.github.io/NOMBRE-REPO/` y el generador en `/generador.html`.

## Licencia
MIT.
