# Conversor de Monedas

## Descripción
Este proyecto implementa un **Conversor de Monedas** en Java, utilizando la API de tasas de cambio **Exchange Rate API** para obtener las tasas de cambio actualizadas. El objetivo es permitir al usuario convertir valores entre distintas monedas, con una interfaz textual en la consola.

## Características Principales
1. **Configuración del Ambiente Java**
2. **Creación del Proyecto**
3. **Consumo de la API**
4. **Análisis de la Respuesta JSON**
5. **Filtro de Monedas**
6. **Exhibición de Resultados a los Usuarios**

---

## Configuración del Ambiente Java

Para desarrollar este proyecto, es necesario:

1. **Instalar Java**: Asegúrate de tener instalado Java Development Kit (JDK) versión 11 o superior.
   - Descarga desde: [Oracle Java](https://www.oracle.com/java/technologies/javase-downloads.html).

2. **Configurar IntelliJ IDEA**: Configura tu entorno de desarrollo integrado (IDE) para compilar y ejecutar proyectos en Java.
   - Agrega el SDK de Java a IntelliJ IDEA mediante `File > Project Structure > SDKs`.

3. **Bibliotecas Necesarias**:
   - Agrega la biblioteca Gson para manipular datos JSON:
     ```xml
     <dependency>
         <groupId>com.google.code.gson</groupId>
         <artifactId>gson</artifactId>
         <version>2.11.0</version>
     </dependency>
     ```

---

## Creación del Proyecto

1. **Estructura del Proyecto:**
   - Crea un nuevo proyecto en IntelliJ IDEA.
   - Estructura básica del proyecto:
     ```
     src/
     ├── Main.java
     ├── ApiClient.java
     ├── ExchangeRateResponse.java
     └── Utils.java
     ```

2. **Clases Principales:**
   - `ApiClient`: Gestiona las solicitudes HTTP a la API.
   - `ExchangeRateResponse`: Contiene el método `getExchangeRate` para realizar conversiones.
   - `Utils`: Maneja operaciones auxiliares como análisis JSON.

---

## Consumo de la API

Utilizamos la clase `HttpClient` para realizar solicitudes HTTP a la **Exchange Rate API**.

1. **URL Base de la API:**
   ```plaintext
   https://v6.exchangerate-api.com/v6/6bb322b1ed0c3135197ed859/latest/USD
   ```

2. **Ejemplo de Solicitud:**
   ```java
   HttpClient client = HttpClient.newHttpClient();
   HttpRequest request = HttpRequest.newBuilder()
       .uri(URI.create("https://v6.exchangerate-api.com/v6/6bb322b1ed0c3135197ed859/latest/USD"))
       .GET()
       .build();
   HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
   ```

3. **Gestión de la Respuesta:**
   Utilizamos `HttpResponse` para manejar la respuesta recibida de la API.

---

## Análisis de la Respuesta JSON

Para analizar el JSON devuelto por la API:

1. **Herramientas Recomendadas:**
   - Utiliza Postman para probar la API y comprender la estructura del JSON.

2. **Ejemplo de Respuesta JSON:**
   ```json
   {
       "base_code": "USD",
       "conversion_rates": {
           "ARS": 350.00,
           "BOB": 6.90,
           "BRL": 5.20,
           "CLP": 880.00
           
       }
   }
   ```

3. **Uso de Gson:**
   - Analiza el JSON utilizando `JsonParser` y `JsonObject`:
     ```java
     JsonObject jsonObject = JsonParser.parseString(response.body()).getAsJsonObject();
     JsonObject conversionRates = jsonObject.getAsJsonObject("conversion_rates");
     ```

---

## Filtro de Monedas

Filtramos las monedas de interés utilizando sus códigos:

- **Códigos de Monedas Incluidos:**
   - `ARS` - Peso argentino
   - `BOB` - Boliviano
   - `USD` - Dólar estadounidense

- **Acceso a los Datos Específicos:**
  ```java
  double tasaCambio = conversionRates.get(Div2).getAsDouble();
  ```

---

## Exhibición de Resultados a los Usuarios

1. **Interfaz de Usuario:**
   - Presentamos un menú interactivo para seleccionar las monedas a convertir y capturamos la entrada del usuario utilizando la clase `Scanner`.

2. **Interacción Principal:**
   ```java
   boolean verifier = false;
   do {
       Scanner escribir = new Scanner(System.in);
       System.out.println("******************");
       System.out.println("Conversor de monedas");
       System.out.println("*****************");
       System.out.println("1 - ARS - BOB");
       System.out.println("2 - BOB - ARS");
       System.out.println("3 - ARS - USD");
       System.out.println("4 - USD - ARS");
       System.out.println("5 - BOB - USD");
       System.out.println("6 - USD - BOB");
       System.out.println("*****************");
       System.out.println("Escoja una opcion para la moneda que desea convertir");
       String Div1 = escribir.nextLine();
       String Div2 = "";

       if (Div1.equals("1")) {
           Div1 = "ARS";
           Div2 = "BOB";
           verifier = true;
       } else if (Div1.equals("2")) {
           Div1 = "BOB";
           Div2 = "ARS";
           verifier = true;
       } else if (Div1.equals("3")) {
           Div1 = "ARS";
           Div2 = "USD";
           verifier = true;
       } else if (Div1.equals("4")) {
           Div1 = "USD";
           Div2 = "ARS";
           verifier = true;
       } else if (Div1.equals("5")) {
           Div1 = "BOB";
           Div2 = "USD";
           verifier = true;
       } else if (Div1.equals("6")) {
           Div1 = "USD";
           Div2 = "BOB";
           verifier = true;
       } else {
           System.out.println("Ingresaste un valor incorrecto");
       }

       System.out.println("Escriba el monto");
       int Monto = Integer.parseInt(escribir.nextLine());
       ExchangeRateResponse.getExchangeRate(Div1, Div2, Monto);

   } while (!verifier);
   ```

3. **Cálculo de Conversión:**
   - Realiza la conversión dentro de `ExchangeRateResponse.getExchangeRate` y muestra el resultado al usuario.

---

## Pruebas

Realiza pruebas exhaustivas con diferentes valores de entrada y monedas para garantizar la precisión de las conversiones y la estabilidad del programa.

---

## Conclusión

Este proyecto reúne las funcionalidades esenciales para realizar conversiones de moneda de manera eficiente y precisa. Con el uso de herramientas modernas de Java, como `HttpClient` y Gson, logramos una implementación robusta y modular.
