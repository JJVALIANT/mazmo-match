# Política de privacidad — Mazmo Match

*Última actualización: 21 de noviembre de 2026*

Mazmo Match es una extensión independiente (no afiliada a Mazmo). Funciona únicamente en `mazmo.net`.

## Qué hace con tus datos

- Usa tu sesión de Mazmo para consultar la API del sitio (`api.mazmo.net`) y leer los checklists que tu cuenta ya tiene permiso de ver. Con ellos calcula un porcentaje de compatibilidad con tu propio checklist.
- Lee las respuestas JSON de esa API para saber qué usuarios aparecen en pantalla. No las modifica.
- El token de sesión del sitio se envía únicamente a `api.mazmo.net`, el mismo servidor que ya lo recibe desde la página.

## Qué guarda

- Solo el nombre de usuario y el porcentaje calculado, en el almacenamiento local de tu navegador, con vencimiento (hasta 6 horas; 1 hora cuando no hay datos).
- La opción «Cargar mis conversaciones» de la ventana del ícono (activada o no).
- **No** guarda los checklists de otras personas.

## Qué no hace

- No envía datos al autor ni a terceros.
- No incluye analíticas, anuncios, rastreadores ni código remoto.
- No vende ni comparte información.

## Opción «Cargar mis conversaciones»

Está **apagada por defecto**. Si la activás, la extensión pide a Mazmo la lista de tus conversaciones (`GET /chat/messengers`) para reconocer a quién corresponde cada chat. Según la documentación de la API, esa consulta marca los últimos mensajes de cada conversación como «recibidos», igual que cuando la página la hace al cargar.

## Qué ven los scripts de la página

Los badges forman parte de la página, así que los scripts del propio sitio pueden leerlos. Si activás el modo debug, la extensión además escribe un diagnóstico en un atributo de `<html>`; por eso está desactivado salvo que lo pidas.

## Borrar los datos

Desinstalar la extensión elimina todo lo que guardó.

## Contacto

Abrí una consulta en <https://github.com/JJVALIANT/mazmo-match/issues>.
