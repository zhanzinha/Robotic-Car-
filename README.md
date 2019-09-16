# Robotic-Car

// Arduino 

////Analogue Read

// Naming of Pins
int Led = 5;
int Signal= 0;      // A0 pin is named as signal variable
int SignalValue = 0; // The digitzed value of signal will be assigned to this variable

// the setup routine runs once when you press reset:
void setup() {
  // initialize the digital pin as an output.
  pinMode(Led, OUTPUT);
  Serial.begin(9600); 
 
}

// the loop routine runs over and over again forever:
void loop() {
 
  SignalValue = analogRead(Signal);    // read the input pin
  Serial.println(SignalValue);         // debug value on Seral Monitor
  if(SignalValue < 512)
  {
  digitalWrite(Led, LOW);    // turn on the LED 1
  }
  else if(SignalValue >= 512)
  {
  digitalWrite(Led, HIGH);    // turn on the LED 1
  }

}



/////Digital input output

// Naming of Pins
int led1=7;
int led2=6;
int switch1=5;
int switch2=4;

// the setup routine runs once when you press reset:
void setup() {
  // initialize the digital pin as an output.
  pinMode(switch1, INPUT);
  pinMode(switch2, INPUT); 
  pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT); 
 
}

// the loop routine runs over and over again forever:
void loop() {
 // read the switch1 input:
   if (digitalRead(switch1) == HIGH) {
     // if the switch is closed:
     digitalWrite(led1, HIGH);    // turn on the LED 1
   }
   else {
     // if the switch is open:
     digitalWrite(led1, LOW);    // turn off the LED 1
   }   
  
  
  // read the switch2 input:
   if (digitalRead(switch2) == HIGH) {
     // if the switch is closed:
     digitalWrite(led2, HIGH);    // turn on the LED 2
   }
   else {
     // if the switch is open:
     digitalWrite(led2, LOW);    // turn off the LED 2
   }              
  
  
}


/////LCD_HEllo World


/********************************/
// include the library code
#include <Wire.h> 
#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x27,20,4);  // set the LCD address to 0x27 for a 16 chars and 2 line display
/*********************************************************/
void setup()
{
  lcd.init();  //initialize the lcd
  lcd.backlight();  //open the backlight 
   
  
  lcd.setCursor ( 0, 0 );            // go to the top left corner
  lcd.print("    Hello,world!    "); // write this string on the top row
  lcd.setCursor ( 0, 1 );            // go to the 2nd row
  lcd.print("   IIC/I2C LCD2004  "); // pad string with spaces for centering
  lcd.setCursor ( 0, 2 );            // go to the third row
  lcd.print("  20 cols, 4 rows   "); // pad with spaces for centering
  lcd.setCursor ( 0, 3 );            // go to the fourth row
  lcd.print(" LCD-LAB1 EXAMPLE ");
}
/*********************************************************/
void loop() 
{

}
/************************************************************/

/////ServoMotor

// Include the Servo library 
#include <Servo.h> 
// Declare the Servo pin 
int servoPin = 3; 
// Create a servo object 
Servo Servo1; 
void setup() { 
   // We need to attach the servo to the used pin number 
   Servo1.attach(servoPin); 
}
void loop(){ 
   // Make servo go to 0 degrees 
   Servo1.write(0); 
   delay(1000); 
   // Make servo go to 90 degrees 
   Servo1.write(90); 
   delay(1000); 
   // Make servo go to 180 degrees 
   Servo1.write(180); 
   delay(1000); 
}


/////Ultrasonic Distance


#define trigPin 13
#define echoPin 12
#define led1 11
#define led2 10

void setup() {
  Serial.begin (9600);      // Set the baudrate for serial communication
  pinMode(trigPin, OUTPUT); 
  pinMode(echoPin, INPUT);
  pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT);
}

void loop() {
  long duration, distance;
  digitalWrite(trigPin, LOW);  // Added this line
  delayMicroseconds(2);        // 2usec delay between pulses
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);       // Trig pin High for 10usec
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH); //Gives the duration of HIGH state a echoPin 
  distance = (duration/2) / 29.1;
  
  // The speed of sound is 340 m/s or 29.1 microseconds per centimeter.
  // The ping travels out and back, so to find the distance of the
  // object we take half of the distance travelled.
  
  if (distance < 4) {  // This is where the LED On/Off happens
    digitalWrite(led1,HIGH); // When the Red condition is met, the Green LED should turn off
  digitalWrite(led2,LOW);
}
  else {
    digitalWrite(led1,LOW);
    digitalWrite(led2,HIGH);
  }
  if (distance >= 200 || distance <= 0){
    Serial.println("Out of range");
    digitalWrite(led1,HIGH);
    digitalWrite(led2,HIGH);
  }
  else {
    Serial.print(distance);
    Serial.println(" cm");
  }
  delay(500);
}



/////Fading LED

int ledPin = 9;    // LED connected to digital pin 9

void setup() {
  // nothing happens in setup
}

void loop() {
  // fade in from min to max in increments of 5 points:
  for (int fadeValue = 0 ; fadeValue <= 255; fadeValue += 5) {
    // sets the value (range from 0 to 255):
    analogWrite(ledPin, fadeValue);
    // wait for 25 milliseconds to see the dimming effect
    delay(25);
  }

  // fade out from max to min in increments of 5 points:
  for (int fadeValue = 255 ; fadeValue >= 0; fadeValue -= 5) {
    // sets the value (range from 0 to 255):
    analogWrite(ledPin, fadeValue);
    // wait for 25 milliseconds to see the dimming effect
    delay(25);
  }
}


/////Motor Rotation Example


//Keyboard Controls:
//
// 1 -Motor 1 Left
// 2 -Motor 1 Stop
// 3 -Motor 1 Right

// Declare L298N Dual H-Bridge Motor Controller directly since there is not a library to load.

// Motor 1
int dir1PinA = 6;
int dir2PinA = 7;
int speedPinA = 5; // Needs to be a PWM pin to be able to control motor speed


void setup() {  // Setup runs once per reset
// initialize serial communication @ 9600 baud:
Serial.begin(9600);

//Define L298N Dual H-Bridge Motor Controller Pins

pinMode(dir1PinA,OUTPUT);
pinMode(dir2PinA,OUTPUT);
pinMode(speedPinA,OUTPUT);

}

void loop() {

// Initialize the Serial interface:

if (Serial.available() > 0) {
int inByte = Serial.read();
int speed; // Local variable

switch (inByte) {

//______________Motor 1______________

case '1': // Motor 1 Forward
analogWrite(speedPinA, 255);//Sets speed variable via PWM 
digitalWrite(dir1PinA, LOW);
digitalWrite(dir2PinA, HIGH);
Serial.println("Motor 1 Forward"); // Prints out “Motor 1 Forward” on the serial monitor
Serial.println("   "); // Creates a blank line printed on the serial monitor
break;

case '2': // Motor 1 Stop (Freespin)
analogWrite(speedPinA, 0);
digitalWrite(dir1PinA, LOW);
digitalWrite(dir2PinA, HIGH);
Serial.println("Motor 1 Stop");
Serial.println("   ");
break;

case '3': // Motor 1 Reverse
analogWrite(speedPinA, 255);
digitalWrite(dir1PinA, HIGH);
digitalWrite(dir2PinA, LOW);
Serial.println("Motor 1 Reverse");
Serial.println("   ");
break;

default:
// turn all the connections off if an unmapped key is pressed:
for (int thisPin = 2; thisPin < 11; thisPin++) {
digitalWrite(thisPin, LOW);
}
  }
    }
      }
      
     


/////Bluetooth Example
// Naming of Pins
int ledFirst=7;
int ledSecond=6;

void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  pinMode(ledFirst, OUTPUT);
  pinMode(ledSecond, OUTPUT); 
}

void loop() {
  // put your main code here, to run repeatedly:
  Serial.println("");
  Serial.println("Enter which LED to turn On!!");
  Serial.println("Type f for First LED..");
  Serial.println("Type s for Second LED..");
  Serial.println("Enter f or s: ");
    
  //Get the number of bytes (characters) available for reading from the serial port.
  while (Serial.available() == 0) {
    delay(200);  }
    
    if(Serial.available()){
    char inByte = Serial.read();
    Serial.println("");
    Serial.print("You have entered- ");
    Serial.println(inByte);
    Serial.println("");

  switch (inByte) {
  case 'f':
   // First LED is ON 
   digitalWrite(ledFirst, HIGH);    // turn on the LED 1
   digitalWrite(ledSecond, LOW);       
   Serial.println("-- First LED is turned ON --");
   Serial.println("");
   break;
    
   case 's':
   // Second LED is ON
   digitalWrite(ledFirst, LOW);    // turn on the LED 1
   digitalWrite(ledSecond, HIGH);       
   Serial.println("-- Second LED is turned ON --");
   Serial.println("");
   break;

   default:
   // both LED's are OFF:
   digitalWrite(ledFirst, LOW);    // turn off the LED 1
   digitalWrite(ledSecond, LOW);
   Serial.println("-- You should type only f or s !! --");
   }   
}}
