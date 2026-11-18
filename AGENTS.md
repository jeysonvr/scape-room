# AGENTS.md — Archivo Faro (especificación para agentes de IA)

## Propósito
Escape room web de una sola página para anunciar un embarazo a familiares y amigos ("vas a ser tío/tía"). Estilo: misión secreta + HUD de videojuego. Idioma: español. Sin dependencias ni build.

## Archivos
- `index.html`: juego completo (HTML + CSS + JS en un solo archivo, ~17 KB).
- `generador.html`: genera links personalizados `index.html?d=<payload>`.
- `README.md`: guía para humanos y despliegue. `LICENSE`: MIT.

## Parámetro de personalización `?d=`
Payload = JSON UTF-8 codificado en base64url (sin relleno). Claves, todas opcionales (máx. 40 caracteres, se muestran con `textContent`):
- `n` nombre; `a` apodo (por defecto = `n`); `r` rol: `"o"` tío (por defecto) o `"a"` tía; `c` chiste interno; `f` fecha estimada.
Sin `d`, o con `d` inválido, el juego usa textos genéricos ("Agente"). No hay datos personales en el repo.

## Flujo del juego (estado: `step` 0–5, luego `fin()`)
Cada nivel 1–5 resuelto desbloquea 2 letras (`got += 2`) de la frase `VASASERTI` + `O|A`, mostradas barajadas en la bandeja HUD (`order`).
1. `reto1` Candado de 4 dígitos. Código **3907**, dígitos escondidos en objetos de una oficina 4x4 (foto de familia=3, biberón=9, peluche=0, calendario=7). Al tocar un objeto se expande en pantalla completa (`inspect`) y su texto se descifra con efecto; el dígito clave aparece resaltado. Otros objetos son decoración con pistas sutiles.
2. `reto2` Cifrado César +3: `EUXMXOD` → **BRUJULA** (se normaliza sin tildes).
3. `reto3` Memoria tipo Simon en 3 rondas progresivas (3, 5 y 7 pulsos, cada una más rápida), 4 canales de color con sonido.
4. `reto4` Ordenar 4 pasos: sobre → revisar → guardar en secreto → abrir.
5. `reto5` Elegir el candidato correcto: la opción con el apodo del jugador; cada opción incorrecta tiene su propio mensaje de error.
6. `reto6` Ordenar las 10 letras en casillas con forma de frase (3,1,3,3) = `VAS A SER TIO/TIA`.
Final (`fin`): revelación animada, mensaje, fecha y chiste del link, contador de pistas, botón de copiar respuesta, confeti.

## Arquitectura
- Funciones `retoN()` renderizan con `screen(title, intro, body, hint)`; `win()`/`bad()` gestionan éxito/error; `next()` despacha por `step`.
- Efectos: fondo canvas de partículas reactivo al puntero, inclinación 3D de la tarjeta, barrido de transición entre niveles, destello verde/rojo en acierto/error, barra de progreso, sonido WebAudio (`beep`, silenciable), texto tipo máquina de escribir (`say`), partículas (`burst`), radio inferior con pistas (`radio[]`).
- Accesibilidad: `prefers-reduced-motion` desactiva animaciones; botones con `aria-label`; foco visible.
- Seguridad: nunca usar `innerHTML` con datos del link.

## Cómo modificar
- Cambiar código del candado: valor `'3907'` y textos de `items`.
- Cambiar clave César: texto cifrado y respuesta en `reto2`.
- Cambiar frase final: constante `L` y longitudes de palabra `wl` en `reto6`.
- Cambiar tono: textos en `items`, `radio`, `reto*` y `fin`.

## Despliegue y pruebas
Despliegue: GitHub Pages desde `main`/raíz. Pruebas manuales: sin `?d`, con `?d` de tío y de tía, con `?d` corrupto; móvil (~380 px); recorrido completo con y sin sonido.

## Privacidad
Repo público: no incluir fotos, fechas ni nombres reales. Base64 no es cifrado.
