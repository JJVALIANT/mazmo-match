# Mazmo Match

Extensión para Firefox y Chrome/Edge que muestra, junto al nombre de cada usuario de [mazmo.net](https://mazmo.net), un porcentaje de compatibilidad entre su checklist y el tuyo.

> **Proyecto independiente.** No está afiliado ni respaldado por Mazmo.

## Qué hace

- Agrega un badge con el porcentaje en el feed, comentarios, temas de las comunidades, chat (mensajes, conversaciones privadas, participantes y panel lateral), notificaciones, listas de seguidores y reacciones, y cabeceras de perfil.
- Colores: verde (70 % o más), ámbar (40 % a 69 %) y gris (menos de 40 %). Al pasar el cursor se ve cuántas prácticas coinciden.
- El cálculo mide cuánto cubre el checklist de la otra persona los intereses del tuyo, ponderando «mucho», «algo» y «poco».
- Solo aparece el badge de quienes tienen un checklist accesible para vos. Hace falta haber iniciado sesión y tener tu checklist completo.
- La primera vez puede tardar unos segundos (o minutos en páginas con muchos usuarios): las consultas se hacen de a poco para no sobrecargar la API, y lo que está a la vista se pide primero.

## Privacidad

Todo ocurre en tu navegador y con tu propia sesión. No se envía nada al autor ni a terceros, y solo se guarda el porcentaje calculado por usuario, con vencimiento. Detalles en [PRIVACY.md](PRIVACY.md).

## Opciones

En la ventana del ícono:

- **Cargar mis conversaciones para identificar chats** (apagada por defecto). Pide `GET /chat/messengers` para reconocer a quién corresponde cada conversación. Según la documentación de la API, esa consulta marca los últimos mensajes como «recibidos».

## Permisos

| Permiso | Para qué |
|---|---|
| `storage` | Guardar los porcentajes ya calculados y las opciones. |
| `https://mazmo.net/*` | Agregar el badge a las páginas del sitio. |
| `https://api.mazmo.net/*` | Consultar checklists con tu sesión. |
| `webRequest` y `webRequestBlocking` (solo Firefox) | Leer, sin modificarlas, las respuestas de la API del sitio para saber qué usuarios se muestran. |

## Firefox y Chrome

| | Firefox (MV2) | Chrome / Edge (MV3) |
|---|---|---|
| Lectura de respuestas | interceptor inyectado + `background.js` (`webRequest`) | script de contenido en el mundo `MAIN`, antes del código del sitio |
| `background.js` | sí | no existe equivalente |
| Ícono en la barra | se coloca al instalar | lo fija cada persona (la ventana avisa cómo) |

## Estructura

```
src/content.js       lógica principal (script de contenido)
src/interceptor.js   lee respuestas JSON de api.mazmo.net (fetch y XHR)
src/background.js    solo Firefox: webRequest.filterResponseData
src/styles.css       estilos del badge
src/popup/           ventana del ícono
manifests/           firefox.json (MV2) y chrome.json (MV3)
icons/, assets/      íconos y arte original
build.sh             genera dist/firefox, dist/chrome y sus .zip
```

## Compilar y probar

```bash
bash build.sh
# dist/mazmo-match-firefox.zip   dist/mazmo-match-chrome.zip
```

- **Firefox:** `about:debugging` → Este Firefox → *Cargar complemento temporal* → `dist/firefox/manifest.json`.
- **Chrome / Edge:** `chrome://extensions` → *Modo desarrollador* → *Cargar descomprimida* → carpeta `dist/chrome`.
- Validar Firefox: `npx web-ext lint -s dist/firefox`.

## Modo debug

Para diagnosticar problemas, en la consola de mazmo.net:

```js
localStorage.mazmoMatchDebug = '1'   // y recargá la página
document.documentElement.dataset.mazmoMatch   // estado de la extensión
```

También se puede preguntar por qué una fila no tiene badge: `document.documentElement.dataset.mazmoExplain = 'NombreVisible'` y, un segundo después, leer `document.documentElement.dataset.mazmoExplainResult`.

El diagnóstico se escribe en un atributo de `<html>`, que puede leer cualquier script de la página. Desactivalo al terminar: `localStorage.removeItem('mazmoMatchDebug')`.

## Limitaciones

- Depende de la estructura de la web y de los datos que devuelve la API de Mazmo; si el sitio cambia, algunas secciones pueden dejar de mostrar badges.
- Los badges forman parte de la página, así que los scripts del propio sitio pueden leerlos.
- Si dos usuarios comparten el mismo avatar y el nombre de la fila no los distingue, no se muestra badge (se prefiere omitir antes que atribuirlo mal).

## Licencia

[MIT](LICENSE) © 2026 JJVALIANT

## Autor

Por [𝕁𝕁𝐕𝑨𝐋𝐈𝑨𝐍𝐓 👋](https://mazmo.net/@jjvaliant). Si te gustó y querés colaborar, [invitame un whiskicito 🥃](https://mpago.la/1JyUhk4).
