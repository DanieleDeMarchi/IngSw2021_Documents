# Risoluzione errori riscontrati durante lo sviluppo

<!-- @import "[TOC]" {cmd="toc" depthFrom=2 depthTo=4 orderedList=false} -->

<!-- code_chunk_output -->

- [Flutter](#flutter)
  - [Verificare corretta installazione flutter e plugin per Android Studio](#verificare-corretta-installazione-flutter-e-plugin-per-android-studio)
  - [Errore dipendenze integration_test](#errore-dipendenze-integration_test)
- [Android Studio](#android-studio)
- [Node JS server](#node-js-server)
  - [Corretta gestione errori express](#corretta-gestione-errori-express)
  - [Evitare attacchi XSS](#evitare-attacchi-xss)
- [Heroku](#heroku)
- [Firebase](#firebase)
  - [Errore social login con flutter (codice 16309)](#errore-social-login-con-flutter-codice-16309)

<!-- /code_chunk_output -->

## Flutter

### Verificare corretta installazione flutter e plugin per Android Studio

```
> flutter doctor --verbose

[✓] Flutter (Channel dev, 1.27.0-8.0.pre, on Microsoft Windows [Versione 10.0.18363.1440], locale it-IT)
• Flutter version 1.27.0-8.0.pre at C:\Development\flutter
• Framework revision b7d4806243 (3 weeks ago), 2021-02-19 09:22:45 -0800
• Engine revision 6993cb229b
• Dart version 2.13.0 (build 2.13.0-30.0.dev)

[✓] Android toolchain - develop for Android devices (Android SDK version 30.0.2)
• Android SDK at C:\Users\Daniele\AppData\Local\Android\sdk
• Platform android-30, build-tools 30.0.2
• Java binary at: C:\Program Files\Android\Android Studio\jre\bin\java
• Java version OpenJDK Runtime Environment (build 1.8.0_242-release-1644-b01)
• All Android licenses accepted.

[✓] Chrome - develop for the web
• Chrome at C:\Program Files (x86)\Google\Chrome\Application\chrome.exe

[✓] Android Studio (version 4.1.0)
• Android Studio at C:\Program Files\Android\Android Studio
• Flutter plugin can be installed from:
🔨 https://plugins.jetbrains.com/plugin/9212-flutter
• Dart plugin can be installed from:
🔨 https://plugins.jetbrains.com/plugin/6351-dart
• Java version OpenJDK Runtime Environment (build 1.8.0_242-release-1644-b01)

[✓] IntelliJ IDEA Community Edition (version 2020.2)
• IntelliJ at C:\Program Files\JetBrains\IntelliJ IDEA Community Edition 2020.2.3
• Flutter plugin can be installed from:
🔨 https://plugins.jetbrains.com/plugin/9212-flutter
• Dart plugin can be installed from:
🔨 https://plugins.jetbrains.com/plugin/6351-dart

[✓] VS Code (version 1.54.1)
• VS Code at C:\Users\Daniele\AppData\Local\Programs\Microsoft VS Code
• Flutter extension can be installed from:
🔨 https://marketplace.visualstudio.com/items?itemName=Dart-Code.flutter

[✓] Connected device (2 available)
• Chrome (web) • chrome • web-javascript • Google Chrome 89.0.4389.82
• Edge (web) • edge • web-javascript • Microsoft Edge 87.0.664.47

• No issues found!
```

(la cartella flutter\bin dev'essere all'interno delle variabili di sistema (PATH) per poter eseguire questo comando)

### Errore dipendenze integration_test

(TO DO: VERIFICARE SE ERRORE ANCORA PRESENTE CON VERSIONE "STABLE 2.0.\*")

![integration_test Error](/images/ERROR_integration_test.jpg)

Errore dovuto alla mancanza di alcune dipendenze nella verisione stabile di flutter 1.22.\*
Per risolvere errore passare alla versione "dev" dell'sdk flutter.

Per verificare la versione di flutter attualmente in uso:

`> flutter channel`

Per cambiare versione a "dev":

`> flutter channel dev`

Aggiornamento flutter dopo switch versione:

`> flutter upgrade`

## Android Studio

## Node JS server

### Corretta gestione errori express

Come gestire correttamente gli errori ed evantualmente 404.

Libreria npm per errori http : `http-errors`

##### Catturare 404

Dopo la specifica di tutti i routers, aggiungere il seguente:

```javascript
// catch 404 and forward to error handler
app.use(function (req, res, next) {
  next(createError(404));
});
```

##### Error handler

Dopo funzione per catturare 404, aggiungere il seguente error handler.
Se durante l'esecuzione della richiesta è stato generato un errore http specifico, con relativo status code e message, l'error handler risponderà con l'errore generato,
altrimenti risponderà con stato 500 Internal Server Error, se si è verificato un errore non previsto e gestito.

```javascript
// error handler
app.use(function (err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get("env") === "development" ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.send(err.message);
});
```

### Evitare attacchi XSS

##### Server side sanitazion

##### Client side sanitazion

## Heroku

## Firebase

### Errore social login con flutter (codice 16309)

Errore: `flutter (16309): [ERROR:flutter/lib/ui/ui_dart_state.cc(186)] Unhandled Exception: PlatformException(sign_in_failed, com.google.android.gms.common.api.ApiException: 10: , null, null)`

Errore causato dalla mancanza della chiave SHA-1 del computer in cui si sta sviluppando all'interno del progetto firebase.

##### Soluzione 1: una chiave per ogni computer in cui si sviluppa

1. Ottenere chiave sha-1 del computer:

   `> keytool -exportcert -list -v -alias androiddebugkey -keystore c:\Users\<USERNAME>\.android\debug.keystore`

2. Aggiungere la chiave alla console firebase:

   `Console Firebase > Impostazioni progetto > Generali > Le tue app (in fondo alla pagina) > selezionare app Android > Aggiungi impronta digitale`

##### Soluzione 2: chiave comune per ogni computer

(TO DO: Da verificare ed aggiungere)
