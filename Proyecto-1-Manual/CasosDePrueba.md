##  Módulo: Autenticación (Login)

###  TC-LOGIN-001: Inicio de sesión exitoso con usuario estándar (Happy Path)
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

###  TC-LOGIN-002: Intento de inicio de sesión con contraseña incorrecta (Caso Negativo)
* **Descripción:** Validar que el sistema impida el acceso y muestre un mensaje de error adecuado cuando se introduce una contraseña inválida.
* **Precondiciones:**
  1. El navegador debe estar abierto en la página [https://www.saucedemo.com/](https://www.saucedemo.com/).
* **Pasos a ejecutar:**
  1. Ingresar en el campo "Username": `standard_user`.
  2. Ingresar en el campo "Password" una contraseña errónea: `clave_incorrecta_123`.
  3. Hacer clic en el botón "Login".
* **Resultado Esperado:** El sistema no debe permitir el ingreso. Debe mantenerse en la página de login y mostrar un mensaje de error en color rojo que diga exactamente: *"Epic sadface: Username and password do not match any user in this service"*.

---

###  TC-LOGIN-003: Intento de inicio de sesión con campos vacíos (Caso de Borde)
* **Descripción:** Verificar el comportamiento del sistema cuando el usuario intenta ingresar sin rellenar ningún campo de texto.
* **Precondiciones:**
  1. El navegador debe estar abierto en la página [https://www.saucedemo.com/](https://www.saucedemo.com/).
  2. Los campos "Username" y "Password" deben estar completamente vacíos.
* **Pasos a ejecutar:**
  1. Hacer clic directamente en el botón "Login".
* **Resultado Esperado:** El sistema debe bloquear la acción, resaltar los campos vacíos con un icono de advertencia y mostrar el mensaje de error: *"Epic sadface: Username is required"*.

## Módulo: Gestión del Carrito de Compras

### TC-CART-001: Agregar un producto al carrito exitosamente (Happy Path)
* **Descripción:** Validar que al hacer clic en el botón "Add to cart" de un producto, este se agregue correctamente y el contador del carrito se actualice.
* **Precondiciones:**
  1. El usuario debe haber iniciado sesión y encontrarse en el catálogo de productos (`/inventory.html`).
  2. El icono del carrito de compras (en la esquina superior derecha) debe estar vacío (sin números).
* **Pasos a ejecutar:**
  1. Localizar el primer producto de la lista (ej: "Sauce Labs Backpack").
  2. Hacer clic en el botón "Add to cart" de ese producto.
* **Resultado Esperado:**
  * El botón del producto debe cambiar su texto a "Remove" y su estilo visual debe modificarse.
  * El icono del carrito en la esquina superior derecha debe mostrar ahora el número "1" en color rojo, indicando que hay un artículo dentro.

---

### TC-CART-002: Remover un producto desde el catálogo (Caso Funcional)
* **Descripción:** Verificar que el usuario pueda arrepentirse y quitar un producto del carrito directamente desde la lista de productos.
* **Precondiciones:**
  1. El usuario está en el catálogo (`/inventory.html`).
  2. El usuario ya agregó previamente un producto, por lo que el botón dice "Remove" y el contador del carrito muestra "1".
* **Pasos a ejecutar:**
  1. Localizar el producto previamente agregado ("Sauce Labs Backpack").
  2. Hacer clic en el botón "Remove".
* **Resultado Esperado:**
  * El botón debe volver a cambiar su texto a "Add to cart".
  * El número "1" debe desaparecer por completo del icono del carrito, quedando este vacío nuevamente.

---

### TC-CART-003: Persistencia del carrito al navegar por la app (Caso de Borde / Flujo)
* **Descripción:** Garantizar que los productos agregados al carrito no se pierdan si el usuario navega a otra sección de la página o refresca el sitio.
* **Precondiciones:**
  1. El usuario está en el catálogo (`/inventory.html`).
  2. El usuario ha agregado dos productos al carrito (el contador muestra "2").
* **Pasos a ejecutar:**
  1. Hacer clic en el nombre de cualquier producto para entrar a su vista de detalle (ej: `/inventory-item.html?id=4`).
  2. Verificar el icono del carrito en esa nueva pantalla.
  3. Presionar el botón de "Atrás" del navegador para volver al catálogo principal.
  4. Refrescar la página web (F5 o Ctrl+R).
  5. Volver a verificar el icono del carrito.
* **Resultado Esperado:** En todo momento (en la vista de detalle, al volver atrás y después de refrescar la página), el contador del carrito debe mantener el número "2". Los datos de la sesión de compra deben persistir y no resetearse.

## Módulo: Proceso de Checkout (Pago y Confirmación)

### TC-CHECK-001: Flujo de compra exitoso (Happy Path)
* **Descripción:** Validar que el usuario pueda completar una compra exitosamente ingresando datos válidos en el formulario de envío.
* **Precondiciones:**
  1. El usuario está en la pantalla del carrito (`/cart.html`) con al menos un producto agregado.
* **Pasos a ejecutar:**
  1. Hacer clic en el botón "Checkout".
  2. En la pantalla "Checkout: Your Information" (`/checkout-step-one.html`), ingresar los siguientes datos:
     * **First Name:** Juan
     * **Last Name:** Pérez
     * **Zip/Postal Code:** 12345
  3. Hacer clic en el botón "Continue".
  4. En la pantalla de revisión (`/checkout-step-two.html`), verificar que aparezca el total y hacer clic en el botón "Finish".
* **Resultado Esperado:** El sistema debe procesar la orden correctamente y redirigir a la pantalla de éxito (`/checkout-complete.html`), mostrando el icono verde de verificación y el mensaje: *"Thank you for your order!"*. El icono del carrito debe quedar vacío (en "0").

---

### TC-CHECK-002: Validación de campos obligatorios en el formulario (Caso Negativo)
* **Descripción:** Verificar que el sistema impida avanzar con el proceso de pago si el usuario deja campos requeridos vacíos en el formulario de información.
* **Precondiciones:**
  1. El usuario está en la pantalla de ingreso de datos (`/checkout-step-one.html`).
* **Pasos a ejecutar:**
  1. Ingresar en "First Name": Juan.
  2. Ingresar en "Last Name": Pérez.
  3. Dejar el campo "Zip/Postal Code" completamente vacío.
  4. Hacer clic en el botón "Continue".
* **Resultado Esperado:** El sistema no debe permitir avanzar a la siguiente pantalla. Debe resaltar el campo vacío y mostrar un mensaje de error en la parte inferior que diga exactamente: *"Error: Postal Code is required"*.

---

### TC-CHECK-003: Cancelación del proceso de checkout (Flujo Alternativo)
* **Descripción:** Garantizar que si el usuario decide cancelar la compra a mitad del proceso, el sistema lo devuelva a la sección correcta y conserve los artículos en el carrito.
* **Precondiciones:**
  1. El usuario está en la pantalla de revisión final antes de pagar (`/checkout-step-two.html`).
  2. El carrito mantiene los productos seleccionados.
* **Pasos a ejecutar:**
  1. Localizar y hacer clic en el botón "Cancel".
* **Resultado Esperado:** El sistema debe abortar el proceso de pago de forma segura, redirigir al usuario de vuelta al catálogo principal (`/inventory.html`) y el icono del carrito debe seguir mostrando la misma cantidad de productos que tenía antes de iniciar el checkout (no se debe vaciar).
