# Visualisation.js
Im `Visualisation.js` file wird das `Dashboard.csthml` gehandeld. Hier werden Daten vom `Websocket` abgefangen und gespeichert.

## Globals
| Bezeichnung| Datenformat|
|--|--|
| wsData| ?  |
| cubeRotationZ | Decimal  |
| cubeRotationX | Decimal  |
| heightA | Decimal  |
| heightB | Decimal  |
| heightC | Decimal  |
| heightHY | Decimal  |
| cityDataHeadID | int (mind. 0) |
| simulationID | int (mind. 0)  |
| simulationStartTime | Datetime  |
| simulationEndTime | DateTime  |
| EnergyConsumptionComparisonVal | int  |

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

Die `Progressbar` wird als `Partial View` in `Dashboard.cshtml` geladen.

## Simulation Title
Der `Simulation Title` hat außerhalb einer `Simulation` den Wert `No Simulation started`. Wird eine `Simulation` gestartet, wird der `Titel` mithilfe der `SimScenarioID` in der `Datenbank` abgefragt und im `Dashboard.csthml` in den `Tag <h3>` geladen.

## Aktualisieren des 3D-Modells
Um das 3D-Modell entsprechend zu neigen und in Höhe anzupassen brauchen wir Formeln um die Neigungen in Radianten anzugeben. Dabei ist zu beachten, dass wir von einem gleichschenkligem Dreieck ausgehen, dass auf das Objekt gelegt wird. Die Sensoren sind jeweils an den 3 Ecken angebracht. Die 3 Sensoren/Ecken werden folgend mit A, B und C bezeichnet.

**Neigung**
1. B > A: $y_{z}=(-1)*tan^{-1} (\frac{(A_{B} - A_{A})}{a})$
1. A > B: B > A:$y_{z}=tan^{-1} (\frac{(A_{A} - A_{B})}{a})$
1. A > C: $y_{x}=(-1)*tan^{-1} (\frac{(A_{A} - A_{C})}{a})$
1. C > A: B > A:$y_{x}=tan^{-1} (\frac{(A_{C} - A_{A})}{a})$
