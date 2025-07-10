
---

## 🧾 Guía Súper Fácil: Cómo Crear tus Credenciales de Google Sheets (API) 🤖🔑

¡Hola! En esta guía vamos a crear unas "llaves especiales" (credenciales) para que un programa o aplicación pueda leer y escribir en tus hojas de cálculo de Google Sheets de forma automática.

Piénsalo como si estuvieras creando un **asistente robot** 🤖 al que le darás permiso para trabajar en tus documentos por ti. ¡Vamos a ello!

---

### PASO 1️⃣: Construir la "Oficina" de nuestro Robot 🏗️
*🎯 **Objetivo:** Crear un "Proyecto" en la nube de Google. Es como la oficina central donde vivirá nuestro robot.*

1.  **Abre la Consola de Google Cloud** haciendo clic en este enlace:
    [https://console.cloud.google.com/](https://console.cloud.google.com/)
2.  **Inicia sesión** con tu cuenta de Google (la misma que usas para Gmail o Google Drive).
3.  Arriba a la izquierda, haz clic en el **menú de hamburguesa (☰)**.
4.  Ve a `IAM y Administración` y luego a `Crear un proyecto`.
     *(Nota para el PDF: aquí podrías poner una captura de pantalla)*
5.  **Dale un nombre** a tu proyecto. ¡Sé creativo!
    *   *Ejemplo:* `Mi Asistente para Sheets`
6.  Haz clic en el botón azul **CREAR**. Espera unos segundos a que se complete.

---

### PASO 2️⃣: Darle la Habilidad de Leer Hojas de Cálculo 📈
*🎯 **Objetivo:** Activar la "API" de Google Sheets. Esto es como enseñarle a nuestro robot a entender el lenguaje de las hojas de cálculo.*

1.  **Asegúrate de que tu nuevo proyecto está seleccionado** en la parte superior de la pantalla. Deberías ver su nombre allí.
     *(Nota para el PDF: aquí podrías poner una captura de pantalla)*
2.  Ahora, ve a la **Biblioteca de APIs** con este enlace:
    [https://console.cloud.google.com/apis/library](https://console.cloud.google.com/apis/library)
3.  En la barra de búsqueda, escribe `Google Sheets API`.
4.  Haz clic en el resultado que aparece y luego presiona el botón azul **HABILITAR**.

---

### PASO 3️⃣ (Recomendado): Darle un "GPS" para Encontrar Archivos 📂
*🎯 **Objetivo:** Activar la "API" de Google Drive. Esto le da a tu robot el superpoder de encontrar tus hojas de cálculo por su nombre, no solo por su ID.*

1.  Vuelve a la **Biblioteca de APIs**:
    [https://console.cloud.google.com/apis/library](https://console.cloud.google.com/apis/library)
2.  Busca `Google Drive API`.
3.  Haz clic en el resultado y presiona **HABILITAR**.

💡 **¿Por qué es esto útil?** Sin este paso, solo podrías acceder a una hoja de cálculo si sabes su ID exacto (la cadena larga de letras y números en la URL). Con esto, puedes buscarla por su nombre, ¡mucho más fácil!

---

### PASO 4️⃣: Crear la "Llave Secreta" de nuestro Robot 🔑
*🎯 **Objetivo:** Crear la "Cuenta de Servicio" (nuestro robot) y descargar su archivo de credenciales (su llave secreta).*

1.  Ve al apartado de **Credenciales** con este enlace:
    [https://console.cloud.google.com/apis/credentials](https://console.cloud.google.com/apis/credentials)
2.  Haz clic en el botón `➕ CREAR CREDENCIALES` en la parte superior y selecciona **Cuenta de servicio**.

#### **Parte A: Crear el Asistente Robot**
1.  **Nombre de la cuenta de servicio:** Ponle un nombre descriptivo.
    *   *Ejemplo:* `robot-lector-sheets`
2.  **ID de la cuenta de servicio:** Se generará automáticamente. ¡Déjalo como está!
3.  **Descripción:** Escribe algo para recordar para qué sirve.
    *   *Ejemplo:* `Accede a mis Google Sheets para actualizar datos.`
4.  Haz clic en **CREAR Y CONTINUAR**.
5.  **Rol:** ¡Este paso es importante! Haz clic en el campo y busca **Editor**. Selecciónalo. Esto le da permiso a tu robot para leer *y* escribir en los archivos que compartas con él.
     *(Nota para el PDF: aquí podrías poner una captura de pantalla)*
6.  Haz clic en **CONTINUAR** y luego en **LISTO**.

#### **Parte B: Descargar la Llave Secreta (Archivo JSON)**
1.  Ahora verás tu nueva cuenta de servicio en una lista. Busca la que acabas de crear y, a la derecha, haz clic en los **tres puntos verticales (⋮)** y selecciona **Administrar claves**.
2.  Haz clic en `AGREGAR CLAVE` → `Crear clave nueva`.
3.  Elige el tipo **JSON** y haz clic en **CREAR**.
4.  ¡Automáticamente se descargará un archivo en tu computadora! Este archivo termina en `.json`.

⚠️ **¡MUY IMPORTANTE!** Este archivo `.json` es tu llave secreta. Guárdalo en un lugar seguro y **nunca lo compartas públicamente** (por ejemplo, en un repositorio de GitHub público).

---

### PASO 5️⃣: Invitar a nuestro Robot a la Hoja de Cálculo 🤝
*🎯 **Objetivo:** Compartir tu Google Sheet con la dirección de correo electrónico de tu robot para que pueda acceder.*

1.  **Abre el archivo `.json`** que acabas de descargar con un editor de texto (como el Bloc de Notas en Windows o TextEdit en Mac).
2.  Busca el campo que dice `"client_email"`. Copia la dirección de correo electrónico que aparece allí. Se verá algo así:
    ```json
    {
      "type": "service_account",
      "project_id": "mi-asistente-para-sheets",
      ...
      "client_email": "robot-lector-sheets@mi-asistente-para-sheets.iam.gserviceaccount.com",
      ...
    }
    ```
3.  Ve a tu hoja de cálculo de **Google Sheets**.
4.  Haz clic en el botón verde de **Compartir** en la esquina superior derecha.
5.  **Pega la dirección de correo electrónico** del robot en el campo "Añadir personas y grupos".
6.  Asegúrate de que tenga el permiso de **Editor**.
7.  Haz clic en **Enviar**. No te preocupes por la notificación, ¡es un robot!

---

### ¡Y listo! 🎉🚀
