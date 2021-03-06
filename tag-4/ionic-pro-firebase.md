# Ionic Appflow / Firebase

## Ionic: Cloud

**\(Im Januar 2019 heisst es bye bye Wolke\)**

Ionic Cloud war ein hybrid-fokussiertes Mobiles Backend inkl. Services von Ionic. Es erlaubt einem Entwickler sehr einfach eine performante, produktive App aufzusetzen und zu skalieren.

Die [Dokumentation für Ionic Cloud](http://docs.ionic.io/) war jeweils mit beiden Versionen Ionic 1 und 2 kompatibel und geschrieben .

### Services

Folgende Services waren darin enthalten und sind jetzt nicht mehr im neuen Produkt drin:

* [Auth](http://docs.ionic.io/services/auth/) eine Benutzerverwaltung inkl. Social Logins
* [Ionic DB](http://docs.ionic.io/services/database/) eine einfache Echzeit-Datenbank
* [Push](http://docs.ionic.io/services/push/)-Notifikationen über die Plattform an Benutzer senden

Ionic möchte sich auf die Entwicklung fokussieren und nicht anderen Cloud-Grössen wie Amazon, Google, Heroku, usw. Konkurrenz machen.

## Ionic Appflow

**\(Ihr neues Flagschiff, seit Herbst 2018 nicht mehr Ionic PRO\)**

Ionic Appflow ist eine powervolle Sammlung von Services und Features, welche das Ionic Framework erweitert. Es bringt die App Entwicklung für Entwicklungsteams auf total neues Level.

Ionic Appflow hat diverse Services, welche dich durch den gesamten App Lifecyle beschäftigen:

* \*\*\*\*[**Builds**](https://ionicframework.com/docs/appflow/builds/#builds): Zusammen mit einem Git-Workflow können Builds deployed oder packetiert werden
* \*\*\*\*[**Deploy**](https://ionicframework.com/docs/appflow/deploy/)**:** Spiele in Echzeit Remote-Updates deiner App ein, ohne über die Verzögerungen im App-Store.
* \*\*\*\*[**Package**](https://ionicframework.com/docs/appflow/package/)**:** Builde native App-Binaries in der Cloud für iOS und Android.
* \*\*\*\*[**Automation**](https://ionicframework.com/docs/appflow/automation/): Automatisierte Build mit Möglichkeiten für Webhooks

Die meistens Services basieren auf einem einfachen GIT-Workflow, welcher dir als Entwickler bekannt sein sollte.

### Du möchtest, resp. hast Ionic Appflow bereits verwendet

Zu Beginn dieses Kurses habe wir dich darum gebeten einen Account auf folgender Website zu machen: [https://dashboard.ionicjs.com/apps](https://dashboard.ionicjs.com/apps) Ja genau, bereits auf Ionic Appflow. Zudem hast du sicher die DevApp auf deinem Gerät installiert, auch diese gehört bereits zur Ionic Appflow Produktepalette.

## Google Firebase

Wir werden die kommenden Übungen mit [Google's Firebase](https://firebase.google.com/) als Backend as a Service absolvieren, schau dir dazu folgendes Video an:

{% embed url="https://www.youtube.com/watch?v=O17OWyx08Cg" %}

### Wie füge ich Firebase zu meinem Projekt hinzu?

Um Firebase zu installieren, brauchen wir AngularFire2. Öffne dein Terminal, geh in den Projektordner und führe folgenden Befehl aus:

```bash
npm install @angular/fire firebase --save
```

Jetzt können wir Firebase initialisieren, gehe dazu in die Datei `src/app/app.module.ts` und importiere alles was du von Firebase benötigst.

```javascript
// AngularFire2 importieren
import { AngularFireModule } from '@angular/fire';
import { AngularFirestoreModule } from '@angular/fire/firestore';
import { AngularFireDatabaseModule } from '@angular/fire/database';
import { AngularFireAuthModule } from '@angular/fire/auth';

// Firebase Einstellungen 
export const firebaseConfig = {
    apiKey: 'AIzaSyBM_MflQcqElJum8Mc6IGDBr5ruBeDSVKI',
    authDomain: 'm335-auth.firebaseapp.com',
    databaseURL: 'https://m335-auth.firebaseio.com',
    projectId: 'm335-auth',
    storageBucket: 'm335-auth.appspot.com',
    messagingSenderId: '535601451759',
    appId: '1:535601451759:web:78213803d4d40b5d'
};
```

{% hint style="info" %}
Die Verbindungsdaten der `firebaseConfig` sind für diesen Kurs hier gegeben. Du brauchst also nicht extra eine eigene App zu erfassen. Möchtest du es trotzdem tun, gehe in das [Firebase Console Dashboard](https://console.firebase.google.com). Du solltest dort direkt nach dem Erstellen einer App ein grossen violetten Knopf sehen mit _Firebase zu meiner Web-App hinzufügen_. That's it, so weiter im Kontext...
{% endhint %}

Füge noch folgende beiden Zeilen im `@NgModule`-Teil im `imports`-Array hinzu:

```javascript
    AngularFireModule.initializeApp(firebaseConfig),
    AngularFireDatabaseModule,
    AngularFireAuthModule
```

Deine `app.module.ts`-Datei sollte etwa so aussehen:

{% code title="app.module.ts" %}
```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { RouteReuseStrategy } from '@angular/router';

import { IonicModule, IonicRouteStrategy } from '@ionic/angular';
import { SplashScreen } from '@ionic-native/splash-screen/ngx';
import { StatusBar } from '@ionic-native/status-bar/ngx';

import { AppComponent } from './app.component';
import { AppRoutingModule } from './app-routing.module';
import { IonicStorageModule } from '@ionic/storage';

// AngularFire2 importieren
import { AngularFireModule } from '@angular/fire';
import { AngularFirestoreModule } from '@angular/fire/firestore';
import { AngularFireDatabaseModule } from '@angular/fire/database';
import { AngularFireAuthModule } from '@angular/fire/auth';


// Firebase Einstellungen 
export const firebaseConfig = {
     apiKey: 'AIzaSyBM_MflQcqElJum8Mc6IGDBr5ruBeDSVKI',
    authDomain: 'm335-auth.firebaseapp.com',
    databaseURL: 'https://m335-auth.firebaseio.com',
    projectId: 'm335-auth',
    storageBucket: 'm335-auth.appspot.com',
    messagingSenderId: '535601451759',
    appId: '1:535601451759:web:78213803d4d40b5d'
};
@NgModule({
  declarations: [AppComponent],
  entryComponents: [],
  imports: [
    BrowserModule,
    IonicModule.forRoot(),
    AppRoutingModule,
    IonicStorageModule.forRoot(),
    AngularFireModule.initializeApp(firebaseConfig),
    AngularFirestoreModule,
    AngularFireDatabaseModule,
    AngularFireAuthModule
  ],
  providers: [
    StatusBar,
    SplashScreen,
    { provide: RouteReuseStrategy, useClass: IonicRouteStrategy }
  ],
  bootstrap: [AppComponent]
})
export class AppModule {}

```
{% endcode %}

### 

