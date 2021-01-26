# Webserver an Raspberry Pi
Die Kommunikation zwischen dem Raspberry Pi und dem Webserver erfolgt über einen H.Socket.IO Websocket. Der Raspberry Pi sendet die Daten an den Webserver. Nach dem Erhalt sendet der Webserver, im Body, die aktuellen Simulationsdaten (Wind, Sun, Consumption Delta) zurück an den Raspberry Pi.

Zusätzlich wird nach jedem Datensatz, den die REST API vom Raspberry Pi bekommt, eine Bestätigung, für Erhalt des Datensatzes, an den Raspberry Pi zurückgeschickt. Im Falle eines Fehlers oder wenn die REST API nicht antwortet, wird beim nächsten Mal wo Daten vom Raspberry Pi gemessen werden, der vorherige Datensatz nochmals versendet (Also zusätzlich zur neuen Messung) - Dies geht so lange, bis die REST API, die Bestätigung für Erhalt des Datensatzes, wieder zurück an den Raspberry Pi versendet und dieser die Bestätigung auch erhaltet.

# Arduino <=> Raspberry Pi
Der Raspberry Pi kommuniziert über SPI im Interval von 100ms mit dem Arduino. Bei der SPI Kommunikation ist der Raspberry Pi der Master und der Arduino der Slave. Bei der Kommunikation sendet der Raspberry Pi falls vorhanden die aktuellen Simulationsdaten mit. 

# Simulationsdaten
| ID | Bezeichnung | Datenformat |
|--|--|--|
| 1 | Wind | int (0 bis 100) |
| 2 | Sun | int (0 bis 100) |
| 3 | Consumption Delta | int (-100 bis 100) |

# Daten Raspberry Pi an Webserver
Die Daten werden wie folgt in einem JSON Array übertragen:
| ID | Bezeichnung | Datenformat |
|--|--|--|
| 1 | uuid | string |
| 2 | timestamp | timestamp |
| 3 | sector | string ("One" oder "Two" oder "Three") |
| 4 | sensorOutside | int (0 bis 300) |
| 5 | sensorInside | int (0 bis 300) |
| 6 | pumpLevel | int (-100 bis 100) |

Die Daten, mit der ID 3-6, gibt es 3x (Für jede Sektion einen Datenblock). Sie werden aber in einem JSON Array gesammelt verschickt. Weiters gibt es einen zusätzlichen Parameter _"timestamp"_ der vom Datenformat ein TimeStamp ist. Der Parameter wird von der REST API erzeugt. Dieser beinhaltet die Zeit, bei dem die REST API die Daten vom Raspberry Pi bekommen hat. Dieser Parameter wird nur für Auswertungen (z.B. Excelauswertungen) gebraucht, wann und wie oft es zu Unterbrechungen zwischen Raspberry Pi und REST API gekommen ist.

Dann werden die Simulationsdaten + Livedaten über den Websocket Server (Libary: Websocket.sharp) an die verbundenen Clients versendet.

# Daten Webserver an Visualisierung
Die Daten werden wie folgt in einem JSON Array übertragen:
| ID | Bezeichnung | Datenformat |
|--|--|--|
| 1 | CityDataID | int |
| 2 | USonicInner1 | short |
| 3 | USonicOuter1 | short |
| 4 | Pump1 | short |
| 5 | USonicInner2 | short |
| 6 | USonicOuter2 | short |
| 7 | Pump2 | short |
| 8 | USonicInner3 | short |
| 9 | USonicOuter3 | short |
| 10 | Pump3 | short |
| 11 | CreatedAt | DateTime |
| 12 | MesurementTime | DateTime |
| 13 | SimulationID | int? |
| 14 | WindMax | int? |
| 15 | WindCurrent | short? |
| 16 | SunMax | int? |
| 17 | SunCurrent | short? |
| 18 | ConsumptionMax | int? |
| 19 | ConsumptionCurrent | short? |
| 20 | SimulationActive | bool |
| 21 | Simulationtime | DateTime? |
| 22 | TimeFactor | decimal? |