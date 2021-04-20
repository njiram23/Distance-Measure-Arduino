//Distance-Measure-Arduino
//A code for arduino to measure the distance to a target in unit cm using a ultrasonic sensor and a LCD. 
//Afstand is a translation for Distance in Dutch

#include <Wire.h>
#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd(0x27, 2, 1, 0, 4, 5, 6, 7, 3, POSITIVE); 
int trigPin = 12;
int echoPin = 11;
int pingTravelTime;
float pingDistance;
float DistancetoTarget; 
float Afstand;
int dt(50); 
float A, B;
void setup()
{
  A = 10;
  B = 25;

  lcd.begin(16, 2);
  pinMode(6, OUTPUT);
  pinMode(5, OUTPUT);
  pinMode(3, OUTPUT);
  
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT); 
  Serial.begin(9600);
    
    
  }

  void loop()
  { 
   
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    
    digitalWrite(trigPin, LOW);
    delayMicroseconds(10);

    
    
    

     pingTravelTime = pulseIn(echoPin, HIGH);
     delay(20);
     
     pingDistance = (pingTravelTime*765.*5280.*12)/(3600.*1000000);
     
     DistancetoTarget = pingDistance/2;
     
     Afstand = (DistancetoTarget*2.54);
     
     Serial.print("Afstand tot het object is:  ");
     Serial.println(Afstand);
     Serial.println(" centimeters");
     lcd.setCursor(0, 0);
     lcd.print ("Afstand doel: ");
     lcd.setCursor(0,1);
     lcd.print(Afstand);
     lcd.print(" cm");
     delay(dt);
     lcd.clear();
   
   if ( Afstand <= A)
   {
    digitalWrite (6, HIGH);
    delay(25);
    digitalWrite(6, LOW);
    delay(25);
    }else if (Afstand > A)
    {
      digitalWrite(6, LOW);
    }

   if (Afstand <= B & Afstand > A)
   {
    digitalWrite(5, HIGH);
    delay(75);
    digitalWrite(5, LOW);
    delay(75);
   }else if(Afstand > 20)
   {
    digitalWrite(5, LOW);
    digitalWrite(3, HIGH);
    delay(100);
    digitalWrite(3, LOW);
    delay(100);
    
   }   
   

      }
     
