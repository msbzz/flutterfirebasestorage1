# flutterfirebasestorage1


Projeto baseado na video aula 'Enviar Fotos para o Firebase no Flutter (Firebase Storage)' que se encontra no YOUTUBE 'https://www.youtube.com/watch?v=D7OGzzc6jj8&t=1436s' que tem como intuito estudar o uso do recurso 'storage' no firebase que é uma plataforma de desenvolvimento de aplicativos móveis e web oferecida pelo Google.

obs: neste projeto foi considerado apenas o android e precisa dos arquivos 'google-services.json' e '.env' que conterá informações de sua conta firebase

### 1. Criar o arquivo `.env`

Crie um arquivo chamado `.env` na pasta `assets` do seu projeto com o seguinte conteúdo:

```env
ANDROID_API_KEY=YOUR_ANDROID_API_KEY
ANDROID_APP_ID=YOUR_ANDROID_APP_ID
ANDROID_MESSAGING_SENDER_ID=YOUR_ANDROID_MESSAGING_SENDER_ID
ANDROID_PROJECT_ID=YOUR_ANDROID_PROJECT_ID
ANDROID_STORAGE_BUCKET=YOUR_ANDROID_STORAGE_BUCKET

IOS_API_KEY=YOUR_IOS_API_KEY
IOS_APP_ID=YOUR_IOS_APP_ID
IOS_MESSAGING_SENDER_ID=YOUR_IOS_MESSAGING_SENDER_ID
IOS_PROJECT_ID=YOUR_IOS_PROJECT_ID
IOS_STORAGE_BUCKET=YOUR_IOS_STORAGE_BUCKET
IOS_CLIENT_ID=YOUR_IOS_CLIENT_ID
IOS_BUNDLE_ID=YOUR_IOS_BUNDLE_ID

WEB_API_KEY=YOUR_WEB_API_KEY
WEB_APP_ID=YOUR_WEB_APP_ID
WEB_MESSAGING_SENDER_ID=YOUR_WEB_MESSAGING_SENDER_ID
WEB_PROJECT_ID=YOUR_WEB_PROJECT_ID
WEB_AUTH_DOMAIN=YOUR_WEB_AUTH_DOMAIN
WEB_STORAGE_BUCKET=YOUR_WEB_STORAGE_BUCKET
WEB_MEASUREMENT_ID=YOUR_WEB_MEASUREMENT_ID
```

### 2. Configurar o `pubspec.yaml`

Adicione a configuração de assets no seu arquivo `pubspec.yaml`:

```yaml
flutter:
  assets:
    - assets/.env
```

### 3. Modificar o `main.dart`

Certifique-se de que o seu `main.dart` está configurado para carregar o arquivo `.env`:

```dart
import 'package:firebase_core/firebase_core.dart';
import 'firebase_options.dart';
import 'package:flutter/material.dart';
import 'package:flutter_dotenv/flutter_dotenv.dart';
import 'package:flutterfirebasestorage1/pages/error_page.dart';
import 'package:flutterfirebasestorage1/pages/loading_page.dart';
import 'package:flutterfirebasestorage1/pages/storage_page.dart';

Future<void> main() async {
  WidgetsFlutterBinding.ensureInitialized();

  await dotenv.load(fileName: "assets/.env");

  await Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );

  runApp(App());
}

class App extends StatelessWidget {
  final Future<FirebaseApp> _inicializacao = Firebase.initializeApp(
    options: DefaultFirebaseOptions.currentPlatform,
  );

  App({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Firebase Storage',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        primarySwatch: Colors.yellow,
        brightness: Brightness.dark,
      ),
      home: FutureBuilder(
        future: _inicializacao,
        builder: (context, app) {
          if (app.connectionState == ConnectionState.done) {
            return const StoragePage();
          }

          if (app.hasError) return const ErrorPage();

          return const LoadingPage();
        },
      ),
    );
  }
}
```

### 4. Adicionar ao `.gitignore`

Certifique-se de que o arquivo `.env` está incluído no seu `.gitignore` para que suas credenciais não sejam expostas:

```plaintext
.env
assets/.env
```

### Estrutura do Projeto

A estrutura do seu projeto deve ser semelhante a esta:

```
my_flutter_project/
├── android/
├── assets/
│   └── .env
├── build/
├── ios/
├── lib/
│   ├── firebase_options.dart
│   └── main.dart
├── test/
├── .gitignore
├── pubspec.yaml
└── README.md
``` 