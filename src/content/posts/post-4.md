---
title: Integración paso a paso PayPal con Node SDK
author: Kaisher
description: "Antes de comenzar la integración, debe configurar su entorno de desarrollo."
image:
  url: "https://images.unsplash.com/photo-1556740714-a8395b3bf30f?w=500&auto=format&fit=crop&q=60&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTR8fHBhc2FyZWxhJTIwZGUlMjBwYWdvfGVufDB8fDB8fHww"
  alt: "La palabra 'astro' contra una ilustración de planetas y estrellas."
pubDate: 2022-08-08
tags: ["éxitos","paypal","django"]
---


# **Integrar pago Paypal**

Antes de comenzar la integración, debe configurar su entorno de desarrollo. Puede consultar este diagrama de flujo y ver un vídeo en el que se muestra cómo integrar el Pago estándar de PayPal.

Comience su integración tomando el código de ejemplo del repositorio de GitHub de PayPal o visitando el espacio de código de GitHub de PayPal. Lea la guía Codespaces para obtener más información. También puede utilizar Postman para explorar y probar las API de PayPal. Lea la Guía de Postman para obtener más información.

🅰️ Integrar `front end` `CLIENTE`

Configure su `front-end` para integrar pagos estándar en caja.

## **Proceso `Front-end`**

1️⃣ Su aplicación muestra los botones de pago de PayPal.

2️⃣ Tu aplicación llama a puntos finales del servidor para crear el pedido y capturar el pago.

## **Proceso `Front-end`**

Este ejemplo utiliza un archivo `checkout.html` para mostrar cómo configurar el front-end para integrar pagos estándar.

Los archivos `/client/checkout.html` y `/client/app.js` gestionan la lógica del lado del cliente y definen cómo se conectan los componentes del front-end de PayPal con el back-end. Utilice estos archivos para configurar el proceso de pago de PayPal mediante el SDK de JavaScript y gestionar las interacciones del pagador con el botón de pago de PayPal.

__Necesitará:__

✅ Guardar el archivo `checkout.html` en una carpeta llamada `/client.`

✅ Guardar el archivo `app.js` en una carpeta llamada `/client.`

__Paso 1.__ Añadir la etiqueta script

Incluya la etiqueta `<script>`en cualquier página que muestre los botones de PayPal. Esta secuencia de comandos obtendrá todo el JavaScript necesario para acceder a los botones en el objeto `window`.

__Paso 2.__ Configure los parámetros de su script

El fragmento del Paso 1. Añadir la etiqueta de script muestra que necesita pasar un `client-id` y especificar qué `components` desea utilizar. El SDK ofrece botones, marcas, campos de tarjeta y otros componentes. Este ejemplo se centra en el componente de `buttons`.

Además de pasar el `client-id` y especificar los `components` que desea utilizar, también puede pasar la moneda que desea utilizar para la fijación de precios. Para este ejercicio, usaremos `USD`.

`Buyer Country` y `Currency` son sólo para uso en pruebas sandbox. No deben utilizarse en producción.

> Iniciar sesión para ver las credenciales Inicie sesión con su cuenta PayPal para utilizar las credenciales de su cuenta del entorno de pruebas en ejemplos de código.

__Paso 3.__ Renderizar los botones de PayPal
Después de configurar el SDK para su sitio Web, debe renderizar los botones.

El espacio de nombres `paypal` tiene una función `Buttons` que inicia los callbacks necesarios para configurar un pago.

La llamada de retorno `createOrder` se lanza cuando el cliente pulsa el botón de pago. La devolución de llamada inicia el pedido y devuelve un Id. de pedido. Después de que el cliente realice el pago mediante la ventana emergente de PayPal, este Id. de pedido le ayudará a confirmar cuándo se ha completado el pago.

Completing the payment launches an `onApprove` callback. Use the `onApprove` response to update business logic, show a celebration page, or handle error responses.

If your website handles shipping physical items, this documentation includes details about our shipping callbacks.

__Paso 4.__ Configure el diseño del componente Botones `OPCIONAL`

Dependiendo de dónde quieras que aparezcan estos botones en tu sitio web, puedes colocarlos en horizontal o en vertical. También puede personalizar los botones con diferentes colores y formas.

Para anular la configuración de estilo predeterminada de su página, utilice un objeto de `style` dentro del componente `buttons`. Obtenga más información sobre cómo personalizar los botones de pago en la sección de estilo de la página de referencia del SDK de JavaScript.

__Paso 5.__ Admita varias opciones de envío OPCIONAL
Integre opciones de envío para ofrecer a sus compradores la flexibilidad de elegir su método de entrega preferido. Los compradores pueden actualizar su dirección de envío y seleccionar entre sus opciones de envío.

✅ La devolución de llamada `onShippingAddressChange` se activa cuando el comprador selecciona una nueva dirección de envío. Utilice los datos de esta devolución de llamada para indicar al comprador si admite la nueva dirección de envío, actualizar los gastos de envío y actualizar los artículos del carro.

✅ La devolución de llamada `onShippingOptionsChange` se activa cuando el comprador selecciona una nueva opción de envío. Utilice los datos de esta devolución de llamada para indicar al comprador si admite el nuevo método de envío, actualizar los gastos de envío y actualizar los artículos del carrito.

Visite la página de referencia del SDK de JavaScript para obtener más información sobre las retrollamadas onShippingAddressChange y onShippingOptionsChange.

🅱️ Integrar back end `SERVIDOR`

En esta sección se explica cómo configurar su back-end para integrar los pagos de la caja estándar.

**Proceso de back-end**

1️⃣ Su aplicación crea un pedido en el back-end llamando al punto final de la API Crear Pedidos V2.
2️⃣ Tu app llama al punto final Capturar Pago de la API Pedidos V2 en el back end para mover el dinero cuando el pagador confirma el pedido.

**Código back-end**

Este ejemplo de integración utiliza un archivo server.js para configurar el back-end e integrarlo con pagos estándar.

Declarar importaciones y funciones adicionales

El archivo server.js configura el puerto para ejecutar tu servidor e inicia el framework de aplicaciones web express Node.js.

✅ Declare un `process.env` para que la biblioteca DotEnv importe y declare el Id. de cliente y el secreto de cliente de su archivo `.env`.
✅ Configure el archivo server.js para recuperar variables y establecer la URL base para la API de sandbox de PayPal.
✅ Haga declaraciones `app.use` para definir un directorio de archivos para archivos estáticos y llame a la función `express.json` para analizar cuerpos de respuesta JSON.

__Etapa 1__ Generar token de acceso

Necesitas un token de acceso OAuth 2.0 para autenticar todas las solicitudes de la `API REST`.

✅ Declare una función `generateAccessToken()` que realice una llamada POST al endpoint `/v1/oauth2/` token para crear un token de acceso.
✅ Establezca un parámetro `auth` que combine `PAYPAL_CLIENT_ID` y `PAYPAL_CLIENT_SECRET` como un par clave-valor.
✅ Establezca un objeto de datos que capture los datos `response.json` de la solicitud y devuelva el `access_token`.

__Paso 2.__ Crear pedido Crear orden
Necesita una función createOrder para iniciar un pago entre un ordenante y un vendedor.

✅ Configure la función `createOrder` para que realice una solicitud `POST` a `/v2/checkout/orders` y pase datos del objeto cart para calcular las unidades de compra del pedido.
✅ Establezca un objeto `accessToken` para capturar la salida de la función `generateAccessToken` más adelante en la llamada a la API.

__Paso 3.__ Capturar el pago
Necesita una función `captureOrder` para mover el dinero del pagador al vendedor.

✅ Configure la función `captureOrder` para realizar una llamada `POST` al endpoint `/v2/checkout/orders/ORDER-ID/capture` utilizando el `orderID` generado desde el endpoint Create Order.
✅ Establezca un objeto `accessToken` para capturar la salida de la función `generateAccessToken` más adelante en la llamada a la API.
✅ Configure una respuesta que realice una llamada `POST` al punto final `/v2/checkout/orders/ORDER-ID/capture` para capturar el pedido, pasando el `accessToken` en la cabecera Authorization.

Visite el ___Capture payment endpoint of the PayPal Orders v2 API___ para ver ejemplos de respuestas y otros detalles.

__Paso 4.__ Handle responses
You need a `handleResponse` function to set up a listener that returns an HTTP status code from the API response.

Set up `handleResponse` to make a `POST` call to the `/api/orders` endpoint and return an HTTP status code response.
Declare an `errorMessage` object to show an error message when handleResponse returns an error code.