## 🔐 Módulo: Autenticación (Login)

### 📝 TC-LOGIN-001: Inicio de sesión exitoso con usuario estándar (Happy Path)
* **Descripción:** Validar que un usuario registrado pueda iniciar sesión correctamente introduciendo credenciales válidas.
* **Precondiciones:**
  1. El usuario debe tener acceso a internet.
  2. El navegador debe estar abierto en la página [https://www.saucedemo.com/](https://www.saucedemo.com/).
* **Pasos a ejecutar:**
  1. Localizar el campo de texto "Username".
  2. Ingresar el usuario válido: `standard_user`.
  3. Localizar el campo de texto "Password".
  4. Ingresar la contraseña válida: `secret_sauce`.
  5. Hacer clic en el botón "Login".
* **Resultado Esperado:** El sistema debe autenticar al usuario correctamente, redirigirlo a la página de inventario ([https://www.saucedemo.com/inventory.html](https://www.saucedemo.com/inventory.html)) y mostrar el catálogo de productos con el menú de navegación.

---

### 📝 TC-LOGIN-002: Intento de inicio de sesión con contraseña incorrecta (Caso Negativo)
* **Descripción:** Validar que el sistema impida el acceso y muestre un mensaje de error adecuado cuando se introduce una contraseña inválida.
* **Precondiciones:**
  1. El navegador debe estar abierto en la página [https://www.saucedemo.com/](https://www.saucedemo.com/).
* **Pasos a ejecutar:**
  1. Ingresar en el campo "Username": `standard_user`.
  2. Ingresar en el campo "Password" una contraseña errónea: `clave_incorrecta_123`.
  3. Hacer clic en el botón "Login".
* **Resultado Esperado:** El sistema no debe permitir el ingreso. Debe mantenerse en la página de login y mostrar un mensaje de error en color rojo que diga exactamente: *"Epic sadface: Username and password do not match any user in this service"*.

---

### 📝 TC-LOGIN-003: Intento de inicio de sesión con campos vacíos (Caso de Borde)
* **Descripción:** Verificar el comportamiento del sistema cuando el usuario intenta ingresar sin rellenar ningún campo de texto.
* **Precondiciones:**
  1. El navegador debe estar abierto en la página [https://www.saucedemo.com/](https://www.saucedemo.com/).
  2. Los campos "Username" y "Password" deben estar completamente vacíos.
* **Pasos a ejecutar:**
  1. Hacer clic directamente en el botón "Login".
* **Resultado Esperado:** El sistema debe bloquear la acción, resaltar los campos vacíos con un icono de advertencia y mostrar el mensaje de error: *"Epic sadface: Username is required"*.
