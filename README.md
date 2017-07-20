/* Pinbelegung
   Arduino        TMC-2660-EVAL     Sketch   
	 Pin  Bez.      Pin Bez.          Bez.
   GND            2   GND   				
	 5V      	 			5  	+5V_USB   
	 11   DIO       23  CLK16         CLOCKOUT   
	 17   DIO       8   DIO ENN       enable   
	 24   DIO       24  SPI2_CSN0     CS   
	 50   DIO MISO  28  SPI2_MISO   
	 51   DIO MOSI  29  SPI2_MOSI   
	 52   DIO SCK   27  SPI_CLK
*/

#include <SPI.h>

const byte CLOCKOUT = 11;const int  enable   = 17;const int  CS       = 24;
const int STEP = 6;const int DIR  = 7;
const int motor_steps = 200;

void setup() {
	pinMode(CS, OUTPUT);  
	pinMode(CLOCKOUT, OUTPUT);  
	pinMode(enable, OUTPUT);  
	pinMode(STEP, OUTPUT);  
	pinMode(DIR, OUTPUT);
	
  digitalWrite(CS, HIGH);  
	digitalWrite(enable, LOW);  
	digitalWrite(DIR,  LOW);  
	digitalWrite(STEP, LOW);    
	
	TCCR1A = bit (COM1A0);                //toggle OC1A on Compare Match  
	TCCR1B = bit (WGM12) | bit (CS10);    //CTC, no prescaling  
	OCR1A = 0;                            //output every cycle    
	
	SPI.setBitOrder(MSBFIRST);  
	SPI.setClockDivider(SPI_CLOCK_DIV8);  
	SPI.setDataMode(SPI_MODE3);  
	SPI.begin();
	
  Serial.begin(9600);  
	while(!Serial){
	;
	}  
	Serial.println("Serial läuft!");
	
  uint8_t one = SPI.transfer(0x901B4);  
	uint8_t two = SPI.transfer(0xD001F);  
	uint8_t three = SPI.transfer(0xE0010);  
	uint8_t four  = SPI.transfer(0x00000);
	
  Serial.println(one);  
	Serial.println(two);  
	Serial.println(three);  
	Serial.println(four);}   

void loop() {
	for (int i = 0; i < (motor_steps/2); i++){      
		digitalWrite(STEP, HIGH);      
		delayMicroseconds(500);     
		digitalWrite(STEP, LOW);     
		delayMicroseconds(500);  
	}  
	Serial.println("Rotation ausgeführt?");  
	delay(5000);    
}
