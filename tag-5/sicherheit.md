# Sicherheit

## Mögliche Angriffsvektoren

* Datendiebstal 
* Man in the Middle \(verändern von Daten, Identiät übernehmen\)

## Schutzmöglichkeiten

### Man in the Middle

![](../.gitbook/assets/image.png)

==&gt; SSL-Pinning in der App implementieren

### Firebase Regeln

Firebase oder auch andere Backends müssen geschützt werden, Client-Seitig reicht dies nicht. Daher immer schön brav die Regeln erstellen:

{% embed url="https://firebase.google.com/docs/rules" %}

### Cordova Whitelisting von URL's

Schon mal die Datei `config.xml` von Cordova genau angeschaut? Dort gibt es ein access Tag, standartmässig sieht das so aus:

```markup
<access origin="*" />
```

Man kann dies jedoch einschränken:

```markup
<access origin="https://deinesicheredomain.ch" />
<access origin="https://api.deinedomain.ch" />
```

Weiter zu schützen sind die sogenannten Intends, hier kann man z.B. `mailto` entfernen oder auch gar keine `http://` Links mehr zulassen

```markup
<allow-intent href="http://*/*" />
<allow-intent href="https://*/*" />
<allow-intent href="tel:*" />
<allow-intent href="sms:*" />
<allow-intent href="mailto:*" />
<allow-intent href="geo:*" />
```

### Weitere nützliche Tipps

{% embed url="https://blog.jscrambler.com/securing-ionic-4-cordova-apps/" %}











