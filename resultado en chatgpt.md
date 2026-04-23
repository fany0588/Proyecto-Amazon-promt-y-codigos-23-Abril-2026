
# 🧠 1. Metodología (trabajando con “antigravity” como enfoque)

Vamos a estructurarlo como si fuera un equipo de desarrollo:

### 🔹 Agentes (roles del proyecto)

* **Frontend Agent (Flutter UI)** → Diseña pantallas bonitas
* **Backend Agent (Firebase)** → Maneja Firestore
* **Logic Agent (CRUD)** → Controla lógica de datos
* **Config Agent** → Configuración de dependencias

---

### 🔹 Skills (habilidades necesarias)

* Flutter básico (widgets, navegación)
* Firebase (Firestore)
* CRUD (Create, Read, Update, Delete)
* Manejo de estados simple (`setState`)

---

### 🔹 Flujo de trabajo

1. Crear proyecto Flutter
2. Conectar Firebase
3. Crear base de datos (Firestore)
4. Configurar dependencias
5. Crear modelo Usuario
6. Crear servicios (CRUD)
7. Crear UI bonita
8. Integrar todo

---

# 📁 2. Estructura de carpetas

Primero crea:

```
Xflutterestefania0588/
 └── crudamazon/
      └── amazoncrud/
```

Luego dentro del proyecto Flutter:

```
lib/
 ├── main.dart
 ├── models/
 │    └── usuario.dart
 ├── services/
 │    └── firestore_service.dart
 ├── screens/
 │    └── home_screen.dart
 ├── widgets/
 │    └── usuario_form.dart
```

---

# ⚙️ 3. Crear proyecto Flutter

En terminal:

```bash
flutter create amazoncrud
cd amazoncrud
```

---

#  4. Configurar Firebase

1. Ir a Firebase Console
2. Crear proyecto
3. Agregar app Android
4. Descargar `google-services.json`
5. Colocarlo en:

```
android/app/
```

---

# 📦 5. Librerías necesarias

### 🔹 pubspec.yaml

Agrega:

```yaml
dependencies:
  flutter:
    sdk: flutter

  firebase_core: ^2.25.4
  cloud_firestore: ^4.15.4
```

Luego:

```bash
flutter pub get
```

---

# 🔌 6. Inicializar Firebase

### main.dart

```dart
import 'package:flutter/material.dart';
import 'package:firebase_core/firebase_core.dart';
import 'screens/home_screen.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Firebase.initializeApp();
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Amazon CRUD',
      theme: ThemeData(
        primarySwatch: Colors.purple,
      ),
      home: HomeScreen(),
    );
  }
}
```

---

# 🗄️ 7. Crear base de datos en Firestore

En Firebase:

1. Ir a **Firestore Database**
2. Crear base de datos
3. Modo prueba
4. Crear colección:

```
usuarios
```

Campos:

* nombre
* apellido
* correo

---

# 🧩 8. Modelo Usuario

### models/usuario.dart

```dart
class Usuario {
  String id;
  String nombre;
  String apellido;
  String correo;

  Usuario({
    required this.id,
    required this.nombre,
    required this.apellido,
    required this.correo,
  });

  Map<String, dynamic> toMap() {
    return {
      'nombre': nombre,
      'apellido': apellido,
      'correo': correo,
    };
  }

  factory Usuario.fromMap(String id, Map<String, dynamic> data) {
    return Usuario(
      id: id,
      nombre: data['nombre'],
      apellido: data['apellido'],
      correo: data['correo'],
    );
  }
}
```

---

# 🔧 9. Servicio Firestore (CRUD)

### services/firestore_service.dart

```dart
import 'package:cloud_firestore/cloud_firestore.dart';
import '../models/usuario.dart';

class FirestoreService {
  final CollectionReference usuarios =
      FirebaseFirestore.instance.collection('usuarios');

  // CREATE
  Future<void> agregarUsuario(Usuario usuario) async {
    await usuarios.add(usuario.toMap());
  }

  // READ
  Stream<List<Usuario>> obtenerUsuarios() {
    return usuarios.snapshots().map((snapshot) {
      return snapshot.docs.map((doc) {
        return Usuario.fromMap(
          doc.id,
          doc.data() as Map<String, dynamic>,
        );
      }).toList();
    });
  }

  // UPDATE
  Future<void> actualizarUsuario(Usuario usuario) async {
    await usuarios.doc(usuario.id).update(usuario.toMap());
  }

  // DELETE
  Future<void> eliminarUsuario(String id) async {
    await usuarios.doc(id).delete();
  }
}
```

---

# 🎨 10. UI bonita (colores pastel + sombras)

### screens/home_screen.dart

```dart
import 'package:flutter/material.dart';
import '../services/firestore_service.dart';
import '../models/usuario.dart';
import '../widgets/usuario_form.dart';

class HomeScreen extends StatelessWidget {
  final FirestoreService service = FirestoreService();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("CRUD Usuarios"),
        centerTitle: true,
        flexibleSpace: Container(
          decoration: BoxDecoration(
            gradient: LinearGradient(
              colors: [Colors.purple.shade200, Colors.blue.shade200],
            ),
          ),
        ),
      ),
      body: StreamBuilder<List<Usuario>>(
        stream: service.obtenerUsuarios(),
        builder: (context, snapshot) {
          if (!snapshot.hasData) return CircularProgressIndicator();

          final usuarios = snapshot.data!;

          return ListView.builder(
            itemCount: usuarios.length,
            itemBuilder: (context, index) {
              final user = usuarios[index];

              return Card(
                margin: EdgeInsets.all(10),
                elevation: 6,
                shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(15),
                ),
                child: ListTile(
                  title: Text("${user.nombre} ${user.apellido}"),
                  subtitle: Text(user.correo),
                  trailing: Row(
                    mainAxisSize: MainAxisSize.min,
                    children: [
                      IconButton(
                        icon: Icon(Icons.edit, color: Colors.blue),
                        onPressed: () {
                          showDialog(
                            context: context,
                            builder: (_) => UsuarioForm(usuario: user),
                          );
                        },
                      ),
                      IconButton(
                        icon: Icon(Icons.delete, color: Colors.red),
                        onPressed: () {
                          service.eliminarUsuario(user.id);
                        },
                      ),
                    ],
                  ),
                ),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        backgroundColor: Colors.purple.shade200,
        child: Icon(Icons.add),
        onPressed: () {
          showDialog(
            context: context,
            builder: (_) => UsuarioForm(),
          );
        },
      ),
    );
  }
}
```

---

# 🧾 11. Formulario (Crear / Editar)

### widgets/usuario_form.dart

```dart
import 'package:flutter/material.dart';
import '../models/usuario.dart';
import '../services/firestore_service.dart';

class UsuarioForm extends StatefulWidget {
  final Usuario? usuario;

  UsuarioForm({this.usuario});

  @override
  _UsuarioFormState createState() => _UsuarioFormState();
}

class _UsuarioFormState extends State<UsuarioForm> {
  final _formKey = GlobalKey<FormState>();
  final service = FirestoreService();

  late String nombre;
  late String apellido;
  late String correo;

  @override
  void initState() {
    super.initState();
    nombre = widget.usuario?.nombre ?? "";
    apellido = widget.usuario?.apellido ?? "";
    correo = widget.usuario?.correo ?? "";
  }

  @override
  Widget build(BuildContext context) {
    return AlertDialog(
      title: Text(widget.usuario == null ? "Agregar Usuario" : "Editar Usuario"),
      content: Form(
        key: _formKey,
        child: Column(
          mainAxisSize: MainAxisSize.min,
          children: [
            _input(nombre, "Nombre", (v) => nombre = v),
            _input(apellido, "Apellido", (v) => apellido = v),
            _input(correo, "Correo", (v) => correo = v),
          ],
        ),
      ),
      actions: [
        ElevatedButton(
          child: Text("Guardar"),
          onPressed: () async {
            if (_formKey.currentState!.validate()) {
              _formKey.currentState!.save();

              if (widget.usuario == null) {
                await service.agregarUsuario(
                  Usuario(
                    id: '',
                    nombre: nombre,
                    apellido: apellido,
                    correo: correo,
                  ),
                );
              } else {
                await service.actualizarUsuario(
                  Usuario(
                    id: widget.usuario!.id,
                    nombre: nombre,
                    apellido: apellido,
                    correo: correo,
                  ),
                );
              }

              Navigator.pop(context);
            }
          },
        )
      ],
    );
  }

  Widget _input(String initial, String label, Function(String) onSaved) {
    return TextFormField(
      initialValue: initial,
      decoration: InputDecoration(
        labelText: label,
        filled: true,
        fillColor: Colors.purple.shade50,
        border: OutlineInputBorder(
          borderRadius: BorderRadius.circular(10),
        ),
      ),
      validator: (value) =>
          value!.isEmpty ? "Campo requerido" : null,
      onSaved: (value) => onSaved(value!),
    );
  }
}
```

---
