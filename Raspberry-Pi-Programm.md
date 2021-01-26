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

![image.png](/.attachments/image-b0040d62-03c8-463b-bcf3-2a9677435e2c.png)

![image.png](/.attachments/image-b6cb6b74-d250-435c-a491-7b8471b85141.png)