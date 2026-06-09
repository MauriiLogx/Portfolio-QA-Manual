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
