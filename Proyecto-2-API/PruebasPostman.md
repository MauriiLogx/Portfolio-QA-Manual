# Proyecto 2: Pruebas de API con Postman - ReqRes API

## 1. Introducción y Objetivos
Este proyecto demuestra el diseño y ejecución de pruebas funcionales y de caja negra a nivel de Backend, interactuando directamente con la API REST de ReqRes (https://reqres.in/). 

El objetivo es validar que los endpoints encargados de la gestión de usuarios respondan correctamente bajo el protocolo HTTP, asegurando que el servidor maneje tanto las peticiones válidas (casos de éxito) como las inválidas (casos negativos) según los estándares de la industria.

### Herramientas utilizadas:
* **Postman:** Para la construcción de las peticiones, parametrización y automatización de validaciones (Assertions) en JavaScript.
* **JSON:** Como formato de intercambio de datos.

---

## Módulo: Gestión de Usuarios (API Requests)

### API-TC-001: Consultar un usuario por ID existente (GET - Happy Path)
* **Descripción:** Validar que al realizar una petición de consulta con un ID de usuario válido y existente, el servidor devuelva la información correcta con un código de estado adecuado.
* **Método HTTP:** GET
* **URL / Endpoint:** [https://reqres.in/api/users/2](https://reqres.in/api/users/2)
* **Cuerpo de la Petición (Request Body):** No requiere (Vacío).
* **Resultado Esperado (Response):**
  * **Código de Estado:** 200 OK
  * **Cuerpo de la Respuesta (JSON):** Debe retornar un objeto con los datos del usuario (id: 2, email: "janet.weaver@reqres.in", first_name: "Janet", last_name: "Weaver").
* **Script de Validación Automática (Postman Test en JS):**
```javascript
// Validar que el código de respuesta sea 200
pm.test("Status code es 200 OK", function () {
    pm.response.to.have.status(200);
});

// Validar que la respuesta contenga los datos esperados del usuario 2
pm.test("El usuario retornado es Janet Weaver", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.data.id).to.eql(2);
    pm.expect(jsonData.data.first_name).to.eql("Janet");
});
```
---

### API-TC-002: Consultar un usuario que NO existe (GET - Caso Negativo)
* **Descripción:** Validar que al intentar consultar un ID de usuario que no se encuentra en la base de datos, el servidor responda de forma segura con un error controlado y el código HTTP correspondiente.
* **Método HTTP:** GET
* **URL / Endpoint:** [https://reqres.in/api/users/23](https://reqres.in/api/users/23) (El usuario 23 no existe en este entorno).
* **Cuerpo de la Petición (Request Body):** No requiere (Vacío).
* **Resultado Esperado (Response):**
  * **Código de Estado:** 404 Not Found
  * **Cuerpo de la Respuesta (JSON):** Debe retornar un objeto vacío {}.
* **Script de Validación Automática (Postman Test en JS):**
```javascript
// Validar que el código de respuesta sea un 404 de recurso no encontrado
pm.test("Status code es 404 Not Found", function () {
    pm.response.to.have.status(404);
});
```
#  API-TC-003: Crear un nuevo usuario exitosamente

**Tipo de prueba:** Happy Path | **Método:** `POST`

---

##  Descripción

Verificar que el servidor permita registrar un nuevo usuario cuando se le envía un JSON válido con la información requerida, devolviendo los datos guardados y su fecha de creación.

---

##  Endpoint

| Campo       | Valor                          |
|-------------|--------------------------------|
| Método HTTP | `POST`                         |
| URL         | `https://reqres.in/api/users`  |

---

##  Cuerpo de la Petición (Request Body)

```json
{
    "name": "Andres",
    "job": "QA Engineer Junior"
}
```

---

##  Resultado Esperado (Response)

| Campo           | Valor esperado                                              |
|-----------------|-------------------------------------------------------------|
| Código de Estado | `201 Created`                                              |
| `name`          | `"Andres"`                                                  |
| `job`           | `"QA Engineer Junior"`                                      |
| `id`            | Valor único asignado por el servidor                        |
| `createdAt`     | Marca de tiempo del momento de creación                     |

> **Nota:** El cuerpo de la respuesta debe devolver los campos enviados, más un `id` único y el campo `createdAt` generado por el servidor.

---

##  Script de Validación Automática (Postman — JavaScript)

```javascript
// Validar el código de éxito de creación
pm.test("Status code es 201 Created", function () {
    pm.response.to.have.status(201);
});

// Validar que los datos guardados coincidan con lo enviado
pm.test("El usuario creado tiene el nombre y puesto correcto", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.name).to.eql("Andres");
    pm.expect(jsonData.job).to.eql("QA Engineer Junior");
    pm.expect(jsonData).to.have.property("id"); // Verifica que el servidor asignó una ID
});
```
# API-TC-004: Registrar un usuario de forma fallida

**Tipo de prueba:** Caso Negativo | **Método:** `POST`

---

## Descripción

Validar el comportamiento de seguridad de la API al intentar registrar un usuario en el sistema de autenticación sin enviar la contraseña requerida.

---

## Endpoint

| Campo        | Valor                             |
|--------------|-----------------------------------|
| Método HTTP  | `POST`                            |
| URL          | `https://reqres.in/api/register`  |

---

## Cuerpo de la Petición (Request Body)

```json
{
    "email": "sydney@fife"
}
```

---

## Resultado Esperado (Response)

| Campo            | Valor esperado                                                        |
|------------------|-----------------------------------------------------------------------|
| Código de Estado | `400 Bad Request`                                                     |
| `error`          | `"Missing password"`                                                  |

> **Nota:** El servidor debe rechazar la petición incompleta y retornar un mensaje explícito indicando el campo faltante.

---

## Script de Validación Automática (Postman — JavaScript)

```javascript
pm.test("Status code es 400 Bad Request", function () {
    pm.response.to.have.status(400);
});

pm.test("Muestra el mensaje de error correspondiente", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.error).to.eql("Missing password");
});
```

---

*Prueba ejecutada con [Reqres.in](https://reqres.in/) — API pública para pruebas y prototipos.*

---
---

# API-TC-005: Eliminar un usuario existente

**Tipo de prueba:** Happy Path | **Método:** `DELETE`

---

## Descripción

Validar que al enviar una petición de eliminación para un ID de usuario válido, el servidor procese la solicitud correctamente y responda con el código de estado estándar de éxito sin contenido.

---

## Endpoint

| Campo        | Valor                              |
|--------------|------------------------------------|
| Método HTTP  | `DELETE`                           |
| URL          | `https://reqres.in/api/users/2`    |

---

## Cuerpo de la Petición (Request Body)

No requiere (vacío).

---

## Resultado Esperado (Response)

| Campo            | Valor esperado                                                                                           |
|------------------|----------------------------------------------------------------------------------------------------------|
| Código de Estado | `204 No Content`                                                                                         |
| Cuerpo           | Vacío                                                                                                    |

> **Nota:** `204 No Content` es el código HTTP estándar que indica que la acción se completó con éxito, pero la respuesta no requiere cuerpo de texto.

---

## Script de Validación Automática (Postman — JavaScript)

```javascript
// Validar que el código de respuesta sea un 204 (Éxito sin contenido)
pm.test("Status code es 204 No Content", function () {
    pm.response.to.have.status(204);
});
```


