Das Programm für den Raspberry Pi wurde in NodeJS geschrieben und ist relativ übersichtlich. Für das Debuggen wurde eine Faker "Klasse" geschrieben damit auch ein Betrieb ohne Arduino möglich ist.

Um den Faker zu verwenden muss in der `arduino.js` in der Zeile 4 die `'./comm/Faker'` required werden statt `'./comm/Arduino'`.

Mehr Informationen, zur Kommunikation, stehen im "Kommunikation" Wiki Eintrag.

Zusätzlich gibt es noch eine kleine Config-Datei:

```json
{
    "endpoint": "http://localhost:1234",
    "sendInterval": 5000
}
```

Der Endpoint ist die Schnittstelle bei der Visualisierung (`/api/sensors`) und der `sendInterval` ist der Interval, in welchem die Daten an die Visualisierung gesendet werden.