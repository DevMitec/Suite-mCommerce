# Suite MCommerceMIT Web Versión 1.0

## Prefacio

### ¿Cuál es el proposito de este documento?

Esta guía contiene la información necesaria para que los clientes puedan integrar de manera exitosa la implementación de los servicios Suite _mCommcerceMIT_.

## Servicios POST

Los servicios POST que proporciona suite mCommerce son los siguientes:
* **Autenticación**	
* **Cobro con Token**	


## Autenticación


Servicio POST que registra el dispositivo desde el que se invoca la petición y desde el cual se solicita tokenizar una tarjeta; para consumir el servicio se envía una cadena cifrada en el parámetro POST “xml”, la cadena en claro es una cadena JSON con los siguientes atributos:

<pre><code>Propiedad         Descripción                                                   ¿Obligatorio?
* urlREsponse     URL donde enviará la respuesta                                Sí 

* tokenData
* branch          Sucursal para procesar token                                  Sí 
* company         Empresa para procesar token                                   Sí 
* country         País, debe contener el valor MEX                              Sí 
* user            Usuario centro de pagos para procesar token                   Sí 
* password        Password del usuario centro de pagos para autenticación       Sí
* merchant        Afiliación (ID) con la que procesará el token                 Sí 
* currency        Moneda de la transacción, debe contener el valor MXN          Sí 
* operationType   Forma de operar                                               Sí 
* reference       Referencia del cliente para identificar la petición de token  Sí 
* 3dsData
* branch          Sucursal para procesar petición 3DS                           No 
* country         País, debe contener el valor MEX                              No 
* user            Usuario centro de pagos para procesar 3DS                     No 
* password        Password del usuario centro de pagos para autenticación       No
* currency        Moneda de la transacción, debe contener el valor MXN          No
* authKey         Identificador del cliente para identificar la petición        No 
* merchant        Afiliación (ID) con la que procesará 3DS                      Sí 
* reference       Referencia del cliente para identificar la petición de 3DS    Sí 
</code></pre>

_* Si los valores opcionales se omiten tomará los del objeto “tokenData”_.

Ejemplo:

<pre><code>{
  "urlResponse": "https://dev.mitec.com.mx/pgs/jsp/demoEcommRespuesta.jsp",
  "tokenData": {
  "branch": "004",
  "company": "1015",
  "country": "MEX",
  "user": "1015JUAN0",
  "password": "TEMPORAL1",
  "merchant": "06330",
  "currency": "MXN",
  "operationType": "X",
  "reference": "COBROMOTO01"
  },
  "3dsData": {
  "branch": "004",
  "country": "MEX",
  "user": "1015JUAN0",
  "password": "TEMPORAL1",
  "currency": "MXN",
  "authKey": "123",
  "merchant": "06330",
  "reference": "PRUEBA3DSDEV003"
  }
}
</code></pre>

La respuesta será enviada a la URL indicada en el atributo urlResponse, se enviará dentro de un parámetro denominado strResponse el cual contendrá una cadena JSON cifrada en AES, la estructura de la misma es la siguiente:

<pre><code>Propiedad           Descripción                                    ¿Obligatorio?
cdResponse          Código de respuesta de la petición              Sí 
nbResponse          Descripción de la respuesta                     Sí 
token               Token generado para el dispositivo autenticados No
</code></pre>

Donde cdRespone=”00” es Respuesta exitosa, cualquier otro valor es de error

Ejemplo:

<pre><code>{
  "cdResponse": "00",
  "nbResponse": "success",
  "token": "8243078589705454"
}
</code></pre>


**Url de petición:**

*Ambiente Calidad (test)       https://qa3.mitec.com.mx/pgs/eCommAuthService

*Ambiente Producción           https://ssl.e-pago.com.mx/pgs/eCommAuthService


## Cobro con Token

Servicio POST que permite invocar un cobro usando el token registrado previamente desde el dispositivo, para consumir este servicio se envía una cadena cifrada en el parámetro POST “xml”, la cadena en claro es una cadena JSON con los siguientes atributos:

<pre><code>Propiedad        Descripción                                                   ¿Obligatorio?
* urlREsponse     URL donde enviará la respuesta                                Sí 

* paymentData
* branch          Sucursal para procesar pago con token                         Sí 
* company         Empresa para procesar pago con token                          Sí 
* country         País, debe contener el valor MEX                              Sí 
* user            Usuario centro de pagos para procesar pago con token          Sí 
* password        Password del usuario centro de pagos para autenticación       Sí
* merchant        Afiliación (ID) con la que procesará el pago con token        Sí 
* currency        Moneda de la transacción, debe contener el valor MXN          Sí 
* operationType   Forma de operar                                               Sí 
* reference       Referencia del cliente para identificar la petición de pago   Sí 
* amount          Monto del cobro                                               Sí 
* token           Token de la tarjeta de cobro                                  Sí 

</code></pre>

Ejemplo:

<pre><code>{
  "urlResponse": "https://dev3.mitec.com.mx/pgs/jsp/demoEcommRespuesta.jsp",
  "paymentData": {
    "branch": "004",
    "company": "1015",
    "country": "MEX",
    "user": "1015JUAN0",
    "password": "TEMPORAL1",
    "merchant": "06330",
    "currency": "MXN",
    "operationType": "6",
    "reference": "COBROTDC002",
    "amount": "15.00",
    "token": "3716355370080597"
    }
}
</code></pre>

La respuesta será enviada a la URL indicada en el atributo urlResponse, se enviará dentro de un parámetro denominado strResponse el cual contendrá una cadena JSON cifrada en AES, la estructura de la misma es la siguiente:

<pre><code>Propiedad        Descripción                                                             ¿Obligatorio?

* cdResponse      Código de respuesta de la peticiónSucursal para procesar pago con token Sí 
* nbResponse      Descripción de la respuesta                                             Sí 
* response        Descripción de respuesta en cobro                                       Sí 
* foliocpagos     Folio de pago (pgs)                                                     No 
* auth            Número de autorización (pgs)                                            No
* reference       Referencia (pgs)                                                        No 
* time            Hora de la transacción (pgs)                                            No 
* date            Fecha de la transacción (pgs)                                           No 
* nb_company      Nombre de la empresa que realizó el cobro (pgs)                         No 
* nb_merchant     Nombre de la afiliación (pgs)                                           No 
* nb_street       Domicilio de la empresa (pgs)                                           No 
* cc_type         Tipo de tarjeta (pgs)                                                   No 
* tp_operation    Tipo de operación (pgs)                                                 No 
* cc_name         Nombre (pgs)                                                            No 
* cc_number       Últimos 4 dígitos de TDC (pgs)                                          No 
* cc_expmonth     Mes de expiración (pgs)                                                 No 
* cc_expyear      Año de expiración (pgs)                                                 No 
* amount          Monto de la transacción (pgs)                                           No 

</code></pre>

Ejemplo:

<pre><code>{
  "cdResponse": "00",
  "nbResponse": "success",
  "response": "approved",
  "foliocpagos": "000550280",
  "auth": "357538",
  "reference": "REF115536",
  "time": "13:31:42",
  "date": "22/06/2016",
  "nb_company": "EXPOPAY DESA",
  "nb_merchant": "7628597 WEBPAY",
  "nb_street": "CORREGIDORA 92 COL. MIGUEL HIDALGO 1A SECCION, DISTRITO FEDERAL",
  "cc_type": "CREDITO/BANCO EXTRANJERO/MasterCard",
  "tp_operation": "VENTA",
  "cc_name": "",
  "cc_number": "5454",
  "cc_expmonth": "07",
  "cc_expyear": "16",
  "amount": "15.00"
}
</code></pre>

Nota: si durante este proceso se detectara que la autenticación de contexto no existe previamente o si esta ya expiró se hará un redireccionamiento a ese servicio con el fin de renovar las credenciales y posteriormente continuar con el cobro con token.

Url de petición:

*Ambiente Calidad (test) https://qa3.mitec.com.mx/pgs/eCommPayService 

*Ambiente Producción https://ssl.e-pago.com.mx/pgs/eCommPayService 




## Contacto Mesa de Ayuda MIT
Para la atención de los requerimientos de soporte, contamos con mesa de ayuda con los siguientes números en ciudad de México para resolver dudas y orientación para explotar eficientemente los recursos de nuestra plataforma.

Teléfono en la ciudad de México 
**(01.55) 1500.9000**

Correo electrónico 
**soporte.centrodepagos@mitec.com.mx**
