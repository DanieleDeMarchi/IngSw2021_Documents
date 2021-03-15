# Risoluzione errori riscontrati durante lo sviluppo

<!-- @import "[TOC]" {cmd="toc" depthFrom=2 depthTo=4 orderedList=false} -->

<!-- code_chunk_output -->

- [Flutter](#flutter)
  - [Verificare corretta installazione flutter e plugin per Android Studio](#verificare-corretta-installazione-flutter-e-plugin-per-android-studio)
  - [Errore dipendenze integration_test](#errore-dipendenze-integration_test)
- [Node JS server](#node-js-server)
  - [Corretta gestione errori express](#corretta-gestione-errori-express)
  - [Evitare attacchi XSS](#evitare-attacchi-xss)
- [Heroku](#heroku)
- [Firebase](#firebase)
  - [Errore social login con flutter (codice 16309)](#errore-social-login-con-flutter-codice-16309)

<!-- /code_chunk_output -->

## Flutter

### Verificare corretta installazione flutter e plugin per Android Studio

`> flutter doctor --verbose`
(la cartella flutter\bin dev'essere all'interno delle variabili di sistema (PATH) per poter eseguire questo comando)

### Errore dipendenze integration_test

(TO DO: VERIFICARE SE ERRORE ANCORA PRESENTE CON VERSIONE "STABLE 2.0.\*")

![integration_test Error](/images/ERROR_integration_test.jpg)

Errore dovuto alla mancanza di alcune dipendenze nella verisione stabile fi flutter 1.22.\*
Per risolvere errore passare alla versione "dev" dell'sdk flutter.

Per verificare la versione di flutter attualmente in uso:

`> flutter: "flutter channel"`

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
