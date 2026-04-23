
# 🧩 1. ¿Qué necesitas antes de Firebase CLI?

Para usar Firebase CLI necesitas:

✔ Node.js
✔ npm (se instala junto con Node)
✔ Cuenta de Google

---

# 💻 2. Instalar Node.js y npm en Windows

### 🔹 Paso 1: Descargar Node.js

1. Ve a la página oficial:
   👉 [https://nodejs.org](https://nodejs.org)
2. Descarga la versión **LTS (recomendada)**

---

### 🔹 Paso 2: Instalar

* Abre el archivo descargado (.msi)
* Sigue estos pasos:

  * Next
  * Accept terms
  * Next
  * ✔ **Asegúrate que esté marcada la opción "Add to PATH"**
  * Install

---

### 🔹 Paso 3: Verificar instalación

Abre la terminal (CMD o PowerShell) y escribe:

```bash
node -v
```

Ejemplo de salida:

```bash
v20.10.0
```

Luego:

```bash
npm -v
```

Ejemplo:

```bash
10.2.3
```

---

### ❗ Si NO está instalado

Si te sale error tipo:

```
'node' no se reconoce como un comando
```

Haz esto:

1. Reinstala Node.js
2. Verifica que esté en el PATH:

   * Panel de control → Sistema → Variables de entorno
   * Busca en “Path” algo como:

     ```
     C:\Program Files\nodejs\
     ```

---

# 🌍 3. Instalación global de Firebase CLI

Ahora sí, instala Firebase CLI:

```bash
npm install -g firebase-tools
```

✔ `-g` significa instalación global (para usarlo en cualquier proyecto)

---

### 🔹 Verificar instalación

```bash
firebase --version
```

Ejemplo:

```bash
12.5.0
```

---

# 🔥 4. Cómo usar firebase-tools

Firebase CLI te permite:

* Crear proyectos
* Conectar apps Flutter
* Subir funciones
* Manejar Firestore

---

### 🔹 Comando básico

```bash
firebase
```

Te muestra todos los comandos disponibles

---

# 🔐 5. Iniciar sesión con tu cuenta de Google

### 🔹 Paso clave

Ejecuta:

```bash
firebase login
```

---

### 🔹 Qué pasará:

1. Se abre el navegador 🌐
2. Inicias sesión con tu cuenta de Google
3. Aceptas permisos
4. Regresas a la terminal

---

### ✔ Verificación

Si todo salió bien verás:

```
✔ Success! Logged in as tu_correo@gmail.com
```

---

# 📂 6. Inicializar Firebase en tu proyecto Flutter

Ubícate dentro del proyecto:

```bash
cd amazoncrud
```

Luego:

```bash
firebase init
```

---

### 🔹 Selecciona:

* Firestore
* Hosting (opcional)

Usa espacio para seleccionar ✔

---

### 🔹 Elegir proyecto

* Puedes crear uno nuevo
* O usar uno existente de Firebase Console

---

# 🔗 7. Conectar Flutter con Firebase (IMPORTANTE)

Instala FlutterFire CLI:

```bash
dart pub global activate flutterfire_cli
```

Luego:

```bash
flutterfire configure
```

Esto:

✔ Detecta tu app
✔ Genera archivo automático de configuración
✔ Conecta Firebase con Flutter

---

# 🧠 8. Resumen rápido (tipo examen)

✔ Node.js → permite usar npm
✔ npm → instala paquetes
✔ firebase-tools → CLI de Firebase
✔ `npm install -g firebase-tools` → instalación global
✔ `firebase login` → iniciar sesión
✔ `firebase init` → iniciar proyecto
✔ `flutterfire configure` → conectar Flutter

---

# ⚠️ Problemas comunes

### ❌ npm no funciona

→ Reinstalar Node.js

### ❌ firebase no reconocido

→ Cerrar y abrir terminal

### ❌ login no abre navegador

→ Usa:

```bash
firebase login --no-localhost
```

