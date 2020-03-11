http://rpc.gehennom.org/2013/09/atmega2560-as-an-spi-slave/
https://github.com/gregd72002/AvrMiniCopter-archive/blob/master/arduino/SPIdev.h#L82


Ultrasonic
Neigungssensor
Pumpen
Generatoren
Windräder

U01
1 Byte = Identifier -> ersten 3 Bit Type, 5 Bit Nummer (maximal 32 Geräte pro Typ)
2 Bytes = Daten

Die Übertragung vom Arduino sieht dann folgend aus:
![1 Sensor übertragung.PNG](/.attachments/1%20Sensor%20übertragung-0aa4238c-de5b-4540-a13e-7ae2d773f74a.PNG)
1 Sensor


![15 Sensoren Übertragung.PNG](/.attachments/15%20Sensoren%20Übertragung-e593c741-3c8c-4ab8-9bc8-394d4176e9c3.PNG)
15 Sensoren

Der zugehörige Code um diese Daten zu erzeugen:



```
#define F_CPU	16000000UL //define Clockrate

#include <avr/io.h>
#include <util/delay.h>

#define DDR_SPI         DDRB //define SPI Port
#define SPI_PORT	PORTB
#define DD_MOSI		PB2  //define Pinouts
#define DD_SCK		PB1
#define DD_SS		PB0

        //Init Controller as Master and set Clockrate to 1MHz
	void SPI_MasterInit(void){
		/* Set MOSI and SCK output, all others input */
		DDR_SPI = (1<<DD_MOSI)|(1<<DD_SCK)|(1<<DD_SS);
		/* Enable SPI, Master, set clock rate fck/16 */
		SPCR = (1<<SPE)|(1<<MSTR)|(1<<SPR0);
		SPI_PORT |= ( 1 << DD_SS);		
	}
	
        //send Transmission
	uint8_t SPI_MasterTransmit(char cData){
		SPI_PORT &= ~( 1 << DD_SS);
		/* Start transmission */
		SPDR = cData;
		/* Wait for transmission complete */
		while(!(SPSR & (1<<SPIF)));
		SPI_PORT |= ( 1 << DD_SS);
		return SPDR;
	}
int main(void)
{	
	SPI_MasterInit();

        //define variables and set for first Time
	uint16_t s = 1;
	uint8_t sH = 0; //Higher 8 Bit
	uint8_t sL = 0; //Lower 8 Bit
	sL = (uint8_t)s;
	sH = (uint8_t)(s >> 8);
	
	uint8_t i = 0;
	while(1){ //Send endless Data
	    while (i<15) //Data for each of 15 Sensors
	    {
                        //Send Data
			SPI_MasterTransmit(i);
			SPI_MasterTransmit(sL);
			SPI_MasterTransmit(sH);
			
			//define new Data
			i = i+1;
			s = s + 66;
			if (i==14){ //If last Sensor send big Value
			     s = 1000;
			}
			sL = (uint8_t)s;
			sH = (uint8_t)(s >> 8);

	    }
                //After 15 Sesors -> again
		s = 1;
		sL = (uint8_t)s;
		sH = (uint8_t)(s >> 8);
		i = 0;
		_delay_ms(1000); //Delay one second before send Data again
	}
}
```


