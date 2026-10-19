# Johan Rivera · Desarrollador Python (APIs, HTTP y automatización)

Arreglo bugs, construyo integraciones con APIs y automatizo tareas en Python. Cada entrega incluye una **prueba automatizada que falla antes del arreglo y pasa después**.

📩 **Contacto:** johanriveraprojects@gmail.com · [LinkedIn](https://www.linkedin.com/in/johanxavierriverabarrientos) · [WhatsApp: +503 6041 4674](https://wa.me/50360414674)
🌎 Trabajo en español e inglés · ⏱ Respuesta en menos de 24 h · 💵 Precio fijo acordado antes de empezar

---

## Qué puedo hacer por ti

| Servicio | Entrega | Desde |
|---|---|---|
| Arreglo de un bug en Python | 24-48 h | USD 30 |
| Script de automatización (archivos, datos, reportes) | 2-4 días | USD 60 |
| Integración con una API (autenticación, errores, reintentos) | 3-5 días | USD 100 |
| Pruebas automatizadas para tu código existente | 2-4 días | USD 80 |
| Sesión 1:1 de ayuda en Python (1 h) | mismo día | USD 15 |

---

## Portafolio: correcciones reales en `requests`

[`requests`](https://github.com/psf/requests) es la librería HTTP más usada de Python (cientos de millones de descargas semanales). En este fork encontré y corregí los fallos que se listan abajo. Cada uno sigue el mismo método: reproducir el fallo con un caso realista, corregir la causa raíz con el cambio más pequeño posible y dejar una prueba que lo cubra.

Las correcciones viven en este fork; no afirmo que estén aceptadas en el proyecto original. Las desarrollé con apoyo de herramientas de IA y las revisé y validé con la suite de pruebas y CI.

### Autenticación
- **Autenticación Digest ignoraba los algoritmos `-sess` de la RFC 7616** (SHA-256-sess, SHA-512-sess, SHA-sess). *Síntoma:* la librería no enviaba ninguna cabecera `Authorization` y el login fallaba en silencio. *Resultado:* ahora soporta todas las variantes, verificadas recalculando la respuesta con la fórmula de la RFC.
- **Digest fallaba si el servidor ofrecía `qop="auth-int, auth"`** (con espacio). *Síntoma:* tampoco se enviaba la autenticación. *Resultado:* se interpreta la lista correctamente.
- **Credenciales `.netrc` ignoradas si el nombre de la máquina tenía mayúsculas** (`API.Example.com`). *Síntoma:* se usaban las credenciales de `default`, es decir, las equivocadas. *Resultado:* la comparación ya no distingue mayúsculas, como hace curl.

### Proxies
- **Proxies sin esquema, como `localhost:3128` o `user:pass@proxy:3128`, se rompían** y la petición fallaba, mientras `127.0.0.1:3128` sí funcionaba. *Resultado:* ahora se interpretan bien y las URLs mal formadas se siguen rechazando con el error correcto.
- **`NO_PROXY=10.0.0.1:8080` no se respetaba:** el tráfico seguía saliendo por el proxy. *Resultado:* se comparan IP y puerto.

### Timeouts y reintentos
- **Al agotarse los reintentos de lectura se lanzaba `ConnectionError` en vez de `ReadTimeout`.** *Síntoma:* el código que capturaba `requests.Timeout` no atrapaba el error. *Resultado:* se lanza la excepción correcta.
- **Cabecera `Retry-After` hostil:** un valor con cientos de dígitos provocaba `OverflowError`, y valores enormes bloqueaban el programa indefinidamente. *Resultado:* los valores no válidos se tratan como ausentes y la documentación explica cómo limitar la espera.
- **Funciones nuevas para clientes resilientes:** `Session.timeout` (timeout por defecto), `Session.retries` (reintentos con espera exponencial que respetan `Retry-After`) y `Response.retry_after`, con ejemplo en la documentación.

### Respuestas y streaming
- **`iter_lines()` devolvía líneas vacías de más o partía un `\r\n` a la mitad,** según cómo llegaran los bloques de datos. *Síntoma:* resultados distintos para la misma respuesta. *Resultado:* la salida ya no depende de los límites de los bloques.
- **Charset desconocido o vacío rompía el streaming** (`LookupError`) aunque `Response.text` sí funcionaba. *Resultado:* ambos usan el mismo respaldo a UTF-8.
- **El parser de cabeceras `Link` (paginación de APIs) se rompía** con `;` o `=` dentro de URLs y valores entre comillas, y se perdían enlaces como `rel="next"`. *Resultado:* se divide solo fuera de `<...>` y de las comillas.

### Cookies
- **`morsel_to_cookie` lanzaba `ValueError` con fechas de expiración en formato RFC 1123,** el que envían los servidores y que genera la propia librería estándar. *Resultado:* se aceptan ambos formatos.
- **`RequestsCookieJar.multiple_domains()` devolvía `False` con dos dominios distintos.** *Resultado:* cuenta dominios distintos, como dice su documentación.

---

## Cómo trabajo

1. **Reproduzco el problema** antes de tocar nada.
2. **Arreglo la causa**, no el síntoma, con el cambio más pequeño posible.
3. **Dejo una prueba automatizada** que demuestra que quedó resuelto y evita que vuelva.
4. **Te explico qué pasó** en lenguaje claro, en español.

## Herramientas

Python · pytest · APIs REST · HTTP/HTTPS · autenticación (Basic, Digest, tokens) · cookies, proxies y reintentos · Git y GitHub · CI · ruff · pyright

---

¿Tienes un bug o una tarea pendiente? Escríbeme con una captura del error o una descripción breve y te respondo con precio y plazo.
