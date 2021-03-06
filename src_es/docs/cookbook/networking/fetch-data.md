---
title: Obtener datos desde internet
prev:
  title: Enviar datos a una nueva pantalla
  path: /docs/cookbook/navigation/passing-data
next:
  title: Creando peticiones autentificadas
  path: /docs/cookbook/networking/authenticated-requests
---

Obtener datos desde internet es necesario para la mayoría de las apps. Afortunadamente, Dart y 
Flutter provéen de herramientas para este tipo de trabajo.
  
## Instrucciones

  1. Añade el paquete `http`
  2. Realiza una petición de red usando el paquete `http`
  3. Convierte la respuesta en un objeto personalizado en Dart
  4. Obtiene y muestra los datos con Flutter
  
## 1. Añade el paquete `http`

El paquete [`http`]({{site.pub-pkg}}/http) proporciona la más 
simple manera de obtener datos desde internet.

Para instalar el paquete `http`, necesitas añadir este a la sección de dependencias 
en el fichero `pubspec.yaml`. Puedes [encontrar la última versión del paquete http en 
el sitio web de pub]({{site.pub-pkg}}/http#-installing-tab-).

```yaml
dependencies:
  http: <latest_version>
```
  
## 2. Realiza una petición de red

En este ejemplo, obtendrás un Post de muestra de 
[JSONPlaceholder REST API](https://jsonplaceholder.typicode.com/) 
usando el método 
[http.get()]({{site.pub-api}}/http/latest/http/get.html) .

<!-- skip -->
```dart
Future<http.Response> fetchPost() {
  return http.get('https://jsonplaceholder.typicode.com/posts/1');
}
```

El método `http.get()` devuelve un `Future` que contiene un `Response`. 

  * [`Future`]({{site.api}}/flutter/dart-async/Future-class.html) es 
  una clase del core de Dart para trabajar con operaciones asíncronas. Es usado para representar un 
  valor potencial o un error que estará disponible en algún momento en el futuro.
  * La clase `http.Response` contiene los datos recibidos en una llamada http 
  satisfactoria.  

## 3. Convierte la respuesta en un objeto personalizado de Dart

Mientras que es fácil realizar una petición de red, trabajar con un 
`Future<http.Response>` crudo no es muy conveniente. Para hacer tu vida más sencilla, 
convierte la `http.Response` en nuestro propio objeto Dart.

### Crea una clase `Post`

Primero, necesitaremos crear una clase `Post` que contiene los datos de la 
petición de red. También incluirá un constructor factory que 
creaa un `Post` desde un json.

Convertir JSON a mano es solo una opción. Para más información, por favor ve el 
artículo completo en [JSON y 
serialización](/docs/development/data-and-backend/json). 

<!-- skip -->
```dart
class Post {
  final int userId;
  final int id;
  final String title;
  final String body;

  Post({this.userId, this.id, this.title, this.body});

  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(
      userId: json['userId'],
      id: json['id'],
      title: json['title'],
      body: json['body'],
    );
  }
}
```

### Convierte la `http.Response` en un `Post`

Ahora, actualiza la función `fetchPost` para devolver un `Future<Post>`. Para hacerlo,
necesitas:

  1. Convertir el body de la respuesta en un `Map` json con el paquete `dart:convert`.

  2. Si el servidor devuelve una respuesta "OK" con un status code de 200, convierte 
  el `Map` json en un `Post` usando el método `fromJson` de tipo factory.
  3. Si el servidor devuelve una respuesta inesperada, lanza un error

<!-- skip -->
```dart
Future<Post> fetchPost() async {
  final response =
      await http.get('https://jsonplaceholder.typicode.com/posts/1');

  if (response.statusCode == 200) {
    // Si el servidor devuelve una repuesta OK, parseamos el JSON
    return Post.fromJson(json.decode(response.body));
  } else {
    // Si esta respuesta no fue OK, lanza un error.
    throw Exception('Failed to load post');
  }
}
```

¡Hurra! Ahora tienes una función a la que podemos llamar para obtener un Post 
desde internet.

## 4. Obtén y muestra los datos con Flutter

Para obtener los datos y mostrarlos en la pantalla, puedes usar el widget 
[`FutureBuilder`]({{site.api}}/flutter/widgets/FutureBuilder-class.html)! 
El widget `FutureBuilder` viene con Flutter y hace que sea fácil trabajar 
con fuentes de datos asíncronas.

Debes proporcionar dos parámetros:

  1. El `Future` con el que quieres trabajar. En nuestro caso, llamaremos a nuestra 
  función `fetchPost()`.
  2. Una función `builder` que dice a Flutter que reproducir, dependiendo del 
  estado del `Future`: loading, success, o error.

<!-- skip -->
```dart
FutureBuilder<Post>(
  future: fetchPost(),
  builder: (context, snapshot) {
    if (snapshot.hasData) {
      return Text(snapshot.data.title);
    } else if (snapshot.hasError) {
      return Text("${snapshot.error}");
    }

    // Por defecto, muestra un loading spinner
    return CircularProgressIndicator();
  },
);
```

## 5. Mueve la llamada al método fetch fuera del método `build()`

Aunque es conveniente, no se recomienda poner una llamada a una API en un
método `build()`.

Flutter llama al método `build()` cada vez que quiere cambiar algo en la vista, 
y esto ocurre sorprendentemente a menudo. Si dejas la llamada a fetch en tu método
`build()`, saturaras la API con llamadas innecesarias y relantizarás la 
app.

Aquí hay algunas opciones mejores para llamar a la API solo cuando la pantalla 
cargue inicialmente.

### Pásalo dentro de un `StatelessWidget`

Con esta estrategia, el widget padre es reponsable de llamar al método 
fetch, guarda su resultado, y entonces los pasa a tu widget.

<!-- skip -->
```dart
class MyApp extends StatelessWidget {
  final Future<Post> post;

  MyApp({Key key, this.post}) : super(key: key);
```

Puedes ver un ejemplo funcional de esto en el ejemplo completo siguiente.

### Llámalo en el ciclo de vida del estado de un `StatefulWidget`

Si tu widget es stateful, puedes llamar al método fetch en cualquiera de los 
métodos 
[`initState`]({{site.api}}/flutter/widgets/State/initState.html) o
[`didChangeDependencies`]({{site.api}}/flutter/widgets/State/didChangeDependencies.html).

`initState` es llamado exactamente una vez y nunca de más. Si quieres tener la opción 
de recargar la API en respuesta a un cambio en un 
[`InheritedWidget`]({{site.api}}/flutter/widgets/InheritedWidget-class.html), 
haz la llamada en el método `didChangeDependencies`. Mira
[`State`]({{site.api}}/flutter/widgets/State-class.html) para más
detalles.

<!-- skip -->
```dart
class _MyAppState extends State<MyApp> {
  Future<Post> post;

  @override
  void initState() {
    super.initState();
    post = fetchPost();
  }
```

## Pruebas

Para más información sobre como probar esta funcionalidad, por favor vea las 
siguientes receta:

  * [Introducción a las pruebas unitarias](/docs/cookbook/testing/unit/introduction)
  * [Simular dependencias usando Mockito](/docs/cookbook/testing/mocking/) 

## Ejemplo completo

```dart
import 'dart:async';
import 'dart:convert';

import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

Future<Post> fetchPost() async {
  final response =
      await http.get('https://jsonplaceholder.typicode.com/posts/1');

  if (response.statusCode == 200) {
    // Si la llamada al servidor fue exitosa, analiza el JSON
    return Post.fromJson(json.decode(response.body));
  } else {
    // Si la llamada no fue exitosa, lanza un error.
    throw Exception('Failed to load post');
  }
}

class Post {
  final int userId;
  final int id;
  final String title;
  final String body;

  Post({this.userId, this.id, this.title, this.body});

  factory Post.fromJson(Map<String, dynamic> json) {
    return Post(
      userId: json['userId'],
      id: json['id'],
      title: json['title'],
      body: json['body'],
    );
  }
}

void main() => runApp(MyApp(post: fetchPost()));

class MyApp extends StatelessWidget {
  final Future<Post> post;

  MyApp({Key key, this.post}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Fetch Data Example',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: Scaffold(
        appBar: AppBar(
          title: Text('Fetch Data Example'),
        ),
        body: Center(
          child: FutureBuilder<Post>(
            future: post,
            builder: (context, snapshot) {
              if (snapshot.hasData) {
                return Text(snapshot.data.title);
              } else if (snapshot.hasError) {
                return Text("${snapshot.error}");
              }

              // Por defecto, muestra un loading spinner
              return CircularProgressIndicator();
            },
          ),
        ),
      ),
    );
  }
}
```
