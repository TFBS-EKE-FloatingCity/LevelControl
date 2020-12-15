# Visualisierung <=> Raspberry Pi
Die Kommunikation zwischen dem Raspberry Pi und der Visualisierung erfolgt über einen SocketIO Websocket. Der Raspberry Pi sendet die Daten derzeit alle 5 Sekunden an die Visualisierung (kann eingestellt werden). Nach dem erhalt sendet die Visualisierung im Body die aktuellen Simulationsdaten (Wind, Sun, Consumption) zurück an den Raspberry Pi.

# Arduino <=> Raspberry Pi
Der Raspberry Pi kommuniziert über SPI im Interval von 100ms mit dem Arduino. Bei der SPI Kommunikation ist der Raspberry Pi der Master und der Arduino der Slave. Bei der Kommunikation sendet der Raspberry Pi falls vorhanden die aktuellen Simulations Daten mit. 


# Simulationsdaten

| ID | Bezeichung | Datenformat |
|--|--|--|
| 1 | Wind | int (0 bis 100) |
| 2 | Sun | int (0 bis 100) |
| 3 | Consumption Delta | int (-100 bis 100) |

# Sensordaten: Arduino <=> Raspberry Pi

Der Aufbau ist dort wie folgt:
* 1 Byte - SensorID 
* 2 Bytes - Daten die Struktur muss je Sensor aber noch festgelegt werden - die Werte werden in der Visualisierung umgerechnet

# Sensordaten: Visualisierung <=> Raspberry Pi

Die Daten werden wie folgt in einem JSON Array übertragen:
* SensorInside: int (0 bis 300)
* SensorOutside: int (0 bis 300)
* PumpLevel: int (-100 bis 100)
