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

<pre><code>
_Propiedad        Descripción                                                   ¿Obligatorio?_
* _urlREsponse_   URL donde enviará la respuesta                                Sí 
* _tokenData_
* branch          Sucursal para procesar token                                  Sí 
* company         Empresa para procesar token                                   Sí 
* country         País, debe contener el valor MEX                              Sí 
* user            Usuario centro de pagos para procesar token                   Sí 
* password        Password del usuario centro de pagos para autenticación       Sí
* merchant        Afiliación (ID) con la que procesará el token                 Sí 
* currency        Moneda de la transacción, debe contener el valor MXN          Sí 
* operationType   Forma de operar                                               Sí 
* reference       Referencia del cliente para identificar la petición de token  Sí 
* _3dsData_
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

<pre><code>
{
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

<pre><code>
 Propiedad           Descripción                                    ¿Obligatorio?
cdResponse          Código de respuesta de la petición              Sí 
nbResponse          Descripción de la respuesta                     Sí 
token               Token generado para el dispositivo autenticados No
</code></pre>

Donde cdRespone=”00” es Respuesta exitosa, cualquier otro valor es de error

Ejemplo:

<pre><code>
{
  "cdResponse": "00",
  "nbResponse": "success",
  "token": "8243078589705454"
}
</code></pre>


**Url de petición:**

Ambiente Calidad (test)       https://qa3.mitec.com.mx/pgs/eCommAuthService
Ambiente Producción           https://ssl.e-pago.com.mx/pgs/eCommAuthService




## Contacto Mesa de Ayuda MIT
Para la atención de los requerimientos de soporte, contamos con mesa de ayuda con los siguientes números en ciudad de México para resolver dudas y orientación para explotar eficientemente los recursos de nuestra plataforma.

Teléfono en la ciudad de México 
**(01.55) 1500.9000**

Correo electrónico 
**soporte.centrodepagos@mitec.com.mx**
