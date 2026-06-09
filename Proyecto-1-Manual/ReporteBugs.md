# Reporte de Defectos (Bug Reports) - SauceDemo

Los siguientes defectos fueron detectados utilizando el perfil de pruebas de sistema dinámico `problem_user` en la plataforma SauceDemo.

---

### BUG-2026-001: Error de carga de assets (Imágenes rotas en el catálogo)
* **ID del Defecto:** BUG-2026-001
* **Título:** Las imágenes de los productos en el catálogo de inventario se muestran rotas/por defecto al iniciar sesión con `problem_user`.
* **Severidad:** Media — No rompe la función de comprar, pero destruye la experiencia visual de la interfaz.
* **Prioridad:** Alta — Es la pantalla principal del negocio y da una pésima imagen al cliente.
* **Entorno de Pruebas:** Google Chrome (Versión actual), Sistema Operativo Windows 11.
* **Precondiciones:** El usuario no debe haber iniciado sesión previamente.
* **Pasos para reproducir:**
  1. Dirigirse a la URL [https://www.saucedemo.com/](https://www.saucedemo.com/).
  2. Ingresar el usuario: `problem_user`.
  3. Ingresar la contraseña válida: `secret_sauce`.
  4. Hacer clic en el botón "Login".
  5. Observar las imágenes de los productos en el catálogo (`/inventory.html`).
* **Resultado Obtenido:** Todas las imágenes de los productos (mochila, polera, etc.) muestran la misma imagen genérica de un perro con lentes (marcador de posición roto) en lugar de la imagen correspondiente al producto.
* **Resultado Esperado:** Cada producto debe cargar su imagen correspondiente e individualizada tal como se especifica en la base de datos de diseño.

---

### BUG-2026-002: Error funcional crítico en el botón "Add to cart"
* **ID del Defecto:** BUG-2026-002
* **Título:** El botón "Add to cart" del producto "Sauce Labs Fleece Jacket" no funciona y no añade el producto al carrito.
* **Severidad:** Alta — Bloquea una funcionalidad principal del negocio: comprar ese artículo específico.
* **Prioridad:** Alta — Directores de negocio quieren vender todos los productos del catálogo.
* **Entorno de Pruebas:** Mozilla Firefox, Android Mobile (Chrome App).
* **Precondiciones:** Haber iniciado sesión con el usuario `problem_user` y estar en `/inventory.html`.
* **Pasos para reproducir:**
  1. Localizar el producto "Sauce Labs Fleece Jacket" en el catálogo.
  2. Hacer clic en su respectivo botón "Add to cart".
  3. Verificar el contador del icono del carrito de compras en la esquina superior derecha.
* **Resultado Obtenido:** Al hacer clic, el botón no cambia su estado a "Remove", no ocurre ninguna acción en la interfaz y el contador del carrito se mantiene vacío (en "0"). El sistema ignora el evento click.
* **Resultado Esperado:** El botón debe cambiar a "Remove" y el contador del carrito debe incrementarse en "+1".

---

### BUG-2026-003: Error de Validación/Bloqueo en el formulario de Checkout
* **ID del Defecto:** BUG-2026-003
* **Título:** Error de sistema (Null Pointer / Bloqueo) al intentar ingresar el apellido (Last Name) en el formulario de Checkout.
* **Severidad:** Crítica — Bloquea por completo el flujo de ingresos de dinero; el usuario no puede pagar.
* **Prioridad:** Muy Alta / Crítica — Debe corregirse de inmediato por afectar el módulo de recaudación.
* **Entorno de Pruebas:** Google Chrome, Safari (macOS).
* **Precondiciones:** Haber iniciado sesión con `problem_user`, tener al menos un producto en el carrito y estar en la pantalla `/checkout-step-one.html`.
* **Pasos para reproducir:**
  1. En el campo "First Name" ingresar: `Andrés`.
  2. En el campo "Last Name" intentar ingresar: `Muñoz`.
  3. En el campo "Zip/Postal Code" ingresar: `12345`.
  4. Hacer clic en el botón "Continue".
* **Resultado Obtenido:** El sistema muestra un mensaje de error genérico que dice *"Error: Last Name is required"*, a pesar de que el campo fue rellenado por el usuario. El sistema está limpiando o ignorando el input del apellido, impidiendo avanzar al paso de pago.
* **Resultado Esperado:** El sistema debe aceptar el apellido ingresado, validar el formulario de manera correcta y permitir al usuario avanzar a la pantalla de revisión de costo (`/checkout-step-two.html`).
