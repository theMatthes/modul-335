# Übung: Chat

![](../.gitbook/assets/ralph_uebung.png)

1. Wir werden nun einen kleinen Chat schreiben. Er soll am Ende dieser Übung in etwa so aussehen: ![](../.gitbook/assets/chat.png)
2. Du nimmst wiederum deine Übung von Tag 1 "GX\_NachnameVorname\_Übung" und erstellst dort eine neue Seite "Chat".
3. Lass uns zuerst das Styling etwas vorantreiben. Hier ist unser SCSS-Code:

   ```css
   // chat.page.scss
   .chat-nachrichten {
       height: calc(100% - 60px);
       overflow: scroll;
       background-color: var(--ion-color-light-shade);
       span {
           background-color: var(--ion-color-light-shade) !important;
           display: inline-block !important;
           color: var(--ion-color-dark) !important;
           padding: 10px !important;
           border-radius: 10px !important;
           text-align: left !important;
           max-width: 240px !important;
       }
   }
   .chat-eingabe {
       position: absolute;
       bottom: 0px;
       display: block;
       width: 100%;
       background-color: white;
       -webkit-box-shadow: 0px -1px 5px 0px rgba(204,204,204,1);
       -moz-box-shadow: 0px -1px 5px 0px rgba(204,204,204,1);
       box-shadow: 0px -1px 5px 0px rgba(204,204,204,1);
       button {
           margin: 0px !important;
       }
       ion-spinner {
           width: 12px;
           height: 12px;
       }
       ion-spinner * {
           stroke: var(--ion-color-light);
           fill: white;
           margin: 0px !important;
       }
   }
   .messages {
       display: -webkit-box !important;
       display: -moz-box !important;
       display: -ms-flexbox !important;
       display: -webkit-flex !important;
       display: flex !important;
       -webkit-align-content: center !important;
       -ms-flex-line-pack: center !important;
       align-content: center !important;
       -webkit-box-align: center !important;
       -moz-box-align: center !important;
       -webkit-align-items: center !important;
       -ms-flex-align: center !important;
       align-items: center !important;
       margin-bottom: 5px !important;
       h3 {
           font-size: 12px;
           margin: 0px;
           padding-bottom: 10px;
       }
       p {
           margin: 0px;
       }
       .time {
           font-size: 10px;
           color: rgb(180, 179, 179);
       }
       .message {
           -webkit-box-flex: 1 !important;
           -moz-box-flex: 1 !important;
           -webkit-flex: 1 1 auto !important;
           -ms-flex: 1 1 auto !important;
           flex: 1 1 auto !important;
           padding: 5px !important;
           -webkit-transition: all 250ms ease-in-out !important;
           transition: all 250ms ease-in-out !important;
           overflow: hidden !important;
           text-align: left !important;
           -webkit-transform: translate3d(0, 0, 0) !important;
           -moz-transform: translate3d(0, 0, 0) !important;
           transform: translate3d(0, 0, 0) !important;
       }
   }
   .messages.other {
       .message {
           -webkit-transform: translate3d(0, 0, 0) !important;
           -moz-transform: translate3d(0, 0, 0) !important;
           transform: translate3d(0, 0, 0) !important;
           text-align: right !important;
       }
       span {
           color: var(--ion-color-light) !important;
           background: var(--ion-color-primary) !important;
       }
   }
   ```

4. Setze im HTML den `ion-content` so, dass er nicht scrollt und kein padding hat.
5. Ändere die Navigationsleiste so ab, dass sie eine Rote Farbe erhält.
6. Wir helfen dir nochmals, füge folgenden Code direkt unterhalb von `ion-content` ein. Versuch dabei den Code zu verstehen.

   ```markup
    <div #scrollMe class="chat-nachrichten" (swipe)="swipeEvent($event)">
           <ion-list>
               <div class="messages" [ngClass]="{other: chat.username == this.currentUser}">
                   <div class="message">
                       <span>
                           <h3 *ngIf="chat.username" >{{chat.username}} </h3>                                                    
                           <p *ngIf="chat.text">{{chat.text}}</p>
                        </span>
                       <div class="time" *ngIf="showDates">{{chat.date}}</div>
                   </div>
               </div>
           </ion-list>
       </div>
   ```

7. Unterhalb dieses divs fügst du ein `form` mit der CSS-Klasse `chat-eingabe` darunter ein. Dieses Form wird unsere Eingabe sein.
8. Im `form` möchten wir mit einem `ion-grid` den Input und Button nebeneinander platzieren \(Tipp: `size="10"` / `size="2"` sehen nicht schlecht aus\).
9. Den Button möchten wir mit einem Icon lösen, dazu kannst du folgenden Code verwenden:

   ```markup
   <ion-icon *ngIf="!showSpinnerIcon" name="send"></ion-icon>
   <ion-spinner *ngIf="showSpinnerIcon" name="bubbles"></ion-spinner>
   ```

10. Nun musst du deinem Projekt Angularfire2 und Firebase hinzufügen
11. ```bash
    npm install angularfire2 firebase --save
    ```
12. Um mit unserer Firebase-Chat-API zu kommunizieren, benötigt es einige Änderungen im `app.module.ts`:   


    1. Wir müssen die Angularfire2Module importieren:  


       ```javascript
       // AngularFire2 importieren
       import { AngularFireModule } from 'angularfire2';
       import { AngularFireDatabaseModule } from 'angularfire2/database';
       import { AngularFireAuthModule } from 'angularfire2/auth';
       ```

    2. Wir setzen die Firebase-Konfiguration für diese Übung  


       ```javascript
       // Firebase Einstellungen 
       export const firebaseConfig = {
         apiKey: "AIzaSyCiaDccUKo4hwUW3m3n3qmEACOODb71-dc",
         authDomain: "m335-chat.firebaseapp.com",
         databaseURL: "https://m335-chat.firebaseio.com",
         projectId: "m335-chat",
         storageBucket: "m335-chat.appspot.com",
         messagingSenderId: "477777194250"
       };
       ```

    3. Wir fügen die Angularfire in den `imports` hinzu:  


       ```javascript
        AngularFireModule.initializeApp(firebaseConfig),
        AngularFireDatabaseModule,
        AngularFireAuthModule
       ```

    Hier die komplette Datei:

    {% code title="app.module.ts" %}
    ```javascript

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
    import { AngularFireModule } from 'angularfire2';
    import { AngularFireDatabaseModule } from 'angularfire2/database';
    import { AngularFireAuthModule } from 'angularfire2/auth';
    ​
    ​
    // Firebase Einstellungen 
    export const firebaseConfig = {
      apiKey: "AIzaSyCiaDccUKo4hwUW3m3n3qmEACOODb71-dc",
      authDomain: "m335-chat.firebaseapp.com",
      databaseURL: "https://m335-chat.firebaseio.com",
      projectId: "m335-chat",
      storageBucket: "m335-chat.appspot.com",
      messagingSenderId: "477777194250"
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

13. Um nun die Daten zu laden müssen wir die _ChatPage_ anpassen. Hier eine Vorlage mit TODO's für dich:  


    {% code title="chat.page.ts" %}
    ```typescript
    import { Component, OnInit, ViewChild, ElementRef } from '@angular/core';
    import { AlertController } from '@ionic/angular';
    import { AngularFireDatabase, AngularFireList } from 'angularfire2/database';
    import { Observable } from 'rxjs';

    @Component({
      selector: 'app-chat',
      templateUrl: './chat.page.html',
      styleUrls: ['./chat.page.scss'],
    })
    export class ChatPage implements OnInit {
      // TODO: In der Angular Doku nachlesen, was ViewChild macht und basierend auf deinem HTML XXXXX ersetzen 
      @ViewChild('XXXXX') private myScrollContainer: ElementRef;

      message: string;
      showSpinnerIcon: boolean = false;
      showDates: boolean = false;
      chatList: Observable<ChatMessage[]>;
      chatListRef: AngularFireList<ChatMessage>;
      // TODO: Passe deine Gruppennummer und deinen Namen an
      groupNumber: string =  "G0"; // Bsp. G1
      currentUser: string =  "Roomies Ralph"; // Bsp. Ralph
  
      constructor(private alertCtrl: AlertController, afDb: AngularFireDatabase) {
        this.chatListRef = afDb.list('/chats/'+this.groupNumber);
        this.chatList = this.chatListRef.valueChanges();
      }
  
      ngOnInit() { 
        // TODO: An das Ende scrollen
      }

      ngAfterViewChecked() {        
        // TODO: An das Ende scrollen
      } 

      scrollToBottom(): void {
          try {
              this.myScrollContainer.nativeElement.scrollTop = this.myScrollContainer.nativeElement.scrollHeight;
          } catch(err) { }                 
      }
      swipeEvent(swipe) {
        // 2  = Right to left swipe
        // 4  = Left to right swipe
        if(swipe.direction == 2 || swipe.direction == 4) {
          // TODO: Datum ein resp. ausblenden
        }
      }
      sendMessage(e) {
        if(this.message != '') {
        // TODO: Spinner anzeigen
        let formattedDate = new Date().toLocaleString();  
    
        // TODO: Mittels push()) die Nachricht an Firebase senden 
        // gesendet muss werden: { username: <DEIN-USERNAME> , text: <NACHRICHT>, date: formattedDate }

        // TODO: Cleanup: Nachricht löschen und Spinner ausblenden
        }
      }
    }
    interface ChatMessage {
      username: String;
      text: String;    
      date: any;
    }

    ```
    {% endcode %}

14. Spätestens jetzt möchten wir die Chatnachrichten noch ausgeben, studier den Code oben genau und gebe mittel `*ngFor` die Nachrichten in deinem Template aus.



## Zusatz

1. Zusatz: Füge eine Funktion hinzu, dass wie bei Whatsapp die Namen der anderen Benutzern in einer anderen Farbe erscheinen. Tipp:  Schau dir ngStyle in der Angular Doku an.
2. Zusatz: Passe das Styling so an, dass das Datum rechts neben der Nachricht und nicht mehr unterhalb eingeblendet wird
3. Zusatz: Erweitere deine Chat-App um Nachrichten weiterleiten zu können



