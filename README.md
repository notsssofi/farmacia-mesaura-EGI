<!-- <p align="center">
<img src="/src/frontend/static/icons/Hipster_HeroLogoMaroon.svg" width="300" alt="Online Boutique" />
</p> -->

**¿Qué es Farmacia Mesaura?** Farmacia Mesaura es una aplicación de demostración de microservicios.

Se trata de una tienda e-commerce donde los usuarios pueden navegar productos, agregarlos al carrito y realizar compras.
Este repositorio está basado en la plantilla oficial de Google Cloud Platform (GCP), y fue adaptado en el contexto de la actividad integradora académica.

## Arquitectura de Infraestructura
Actualmente este repositorio se encuentra desplegado en GCP, con GKE Autopilot. Contamos con dos clusters, uno es un cluster compartido para el Test y el Desarrollo (un namespace para cada uno), mientras que el otro cluster es dedicado para Produccion, con su respectivo namespace.

[![Arquitectura 
infra](/docs/img/MarcohorizontaldeGCP.png)](/docs/img/MarcohorizontaldeGCP.png)

## Arquitectura del Software

**Farmacia Mesaura** está compuesta por 11 microservicios escritos en distintos lenguajes de programación, que se comunican entre sí mediante gRPC.

[![Architecture of
microservices](/docs/img/architecture-diagram.png)](/docs/img/architecture-diagram.png)


| Servicio                                              | Lenguaje      | Descripcion                                                                                                                       |
| ---------------------------------------------------- | ------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [frontend](/src/frontend)                           | Go            | Expone un servidor HTTP que muestra el sitio web. No requiere registro/inicio de sesión y genera IDs de sesión automáticamente. |
| [cartservice](/src/cartservice)                     | C#            | Guarda los productos del carrito de compras del usuario en Redis y los recupera.                                                           |
| [productcatalogservice](/src/productcatalogservice) | Go            | Proporciona una lista de productos desde un archivo JSON. Permite buscar productos y obtener su detalle.                        |
| [currencyservice](/src/currencyservice)             | Node.js       | Convierte montos de dinero entre distintas monedas. Usa valores reales del Banco Central Europeo. Es el servicio con más QPS. |
| [paymentservice](/src/paymentservice)               | Node.js       | Simula el cobro con tarjeta de crédito y devuelve un ID de transacción.                                     |
| [shippingservice](/src/shippingservice)             | Go            | Calcula estimaciones de costos de envío según el carrito y simula el envío a una dirección.                                 |
| [emailservice](/src/emailservice)                   | Python        | Envía por correo electrónico una confirmación del pedido al usuario (simulado).                                                                                   |
| [checkoutservice](/src/checkoutservice)             | Go            | Recupera el carrito del usuario, arma la orden y coordina el pago, el envío y el correo de confirmación.                            |
| [recommendationservice](/src/recommendationservice) | Python        | Recomienda otros productos en base a los que hay en el carrito.                                                                      |
| [adservice](/src/adservice)                         | Java          | Proporciona anuncios de texto según palabras clave del contexto.                                                                                   |
| [loadgenerator](/src/loadgenerator)                 | Python/Locust | Genera carga automáticamente enviando peticiones que simulan flujos reales de usuarios en la tienda.

## Screenshots

| Home Page                                                                                                         | Checkout Screen                                                                                                    |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| [![Caotura Home](/docs/img/frontend-farmacia.png)](/docs/img/frontend-farmacia.png) | [![Captura Carrito](/docs/img/carrito-farmacia.png)](/docs/img/carrito-farmacia.png) |

