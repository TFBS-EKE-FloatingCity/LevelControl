# Visualisation.js
Kurz das File grob erklären.........
Globals = json + cubeRotationZ, cubeRotationX, EnergyConsumptionVal.. wsDate????
## Websocket
Mittels einem `onmessage` Event werden Daten in Form eines `JSON` files abgefangen und in globale Variablen gespeichert.
###CityData.json
| Bezeichnung| Datenformat|
|--|--|
| heightA | Decimal  |
| heightB | Decimal  |
| heightC | Decimal  |
| heightHY | Decimal  |

###CityDataHead.json
| Bezeichnung | Datenformat |
|--|--|
| cityDateHeadID | int (mind. 0) |
| simulationID | 0 (mind. 0) |
| simulationStartTime | Datetime |
| simulationEndTime | DateTime |

## Progressbar
Die `Progressbar` zeigt an, wie lange eine Simulation schon läuft.
Mittels einer Funktion, welche alle `1000 Millisekunden` ausgeführt wird, wird die `Progressbar` mithilfe einer `Kalkulation` aktualisiert.