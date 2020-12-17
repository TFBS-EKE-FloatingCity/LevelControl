# Visualisierung <=> Raspberry Pi
Die Kommunikation zwischen dem Raspberry Pi und der Visualisierung erfolgt über einen SocketIO Websocket. Der Raspberry Pi sendet die Daten derzeit alle 5 Sekunden an die Visualisierung (kann eingestellt werden). Nach dem Erhalt sendet die Visualisierung im Body die aktuellen Simulationsdaten (Wind, Sun, Consumption Delta) zurück an den Raspberry Pi.

# Arduino <=> Raspberry Pi
Der Raspberry Pi kommuniziert über SPI im Interval von 100ms mit dem Arduino. Bei der SPI Kommunikation ist der Raspberry Pi der Master und der Arduino der Slave. Bei der Kommunikation sendet der Raspberry Pi falls vorhanden die aktuellen Simulations Daten mit. 

# Simulationsdaten
| ID | Bezeichnung | Datenformat |
|--|--|--|
| 1 | Wind | int (0 bis 100) |
| 2 | Sun | int (0 bis 100) |
| 3 | Consumption Delta | int (-100 bis 100) |

# Sensordaten: Visualisierung <=> Raspberry Pi
Die Daten werden wie folgt in einem JSON Array übertragen:
* SensorInside: int (0 bis 300)
* SensorOutside: int (0 bis 300)
* PumpLevel: int (-100 bis 100)

Diese Daten gibt es 3x (Für jede Sektion einen Datenblock). Sie werden aber in einem JSON Array gesammelt verschickt.
Weiters gibt es einen zusätzlichen Parameter "" der vom Datenformat ein TimeStamp ist. Der Parameter wird von der REST API erzeugt. Dieser beinhaltet die Zeit, bei dem die REST API die Daten vom Raspberry Pi bekommen hat. Dieser Parameter wird nur für Auswertungen (z.B. Excelauswertungen) gebraucht, wann und wie oft es zu Unterbrechungen zwischen Raspberry Pi und REST API gekommen ist.