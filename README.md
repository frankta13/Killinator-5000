## Killinator-5000

Automated Car which drives anti-clockwise around a block!

![]({{BASE_PATH}}/images/robot.PNG)
<img src = "https://frankta13.github.io/Team-Asians-Jye/images/robot3final.png">

Price: AU $74.99

**BUY IT NOW**

Free Postage | 30-day Returns | New Condition
-------------|----------------|--------------



### Item Specifics
Condition:  New: A brand-new, unused, unopened, undamaged item.  
Country:    Australia  
Dimensions: 245mm x 167mm x 104mm  
Brand: Unbranded/Generic  
Colour: Multi-Colour  
Quantity: 1  
UPC: Does not apply  
Model: Robot Car Chassis Kit  
MPN: Does Not Apply  

### Vehicle Parts
1 x Arduino Uno R3  
1 x L298N Dual Motor Controller H-Bridge Module  
2 x DC Gear Motor  
2 x HC-SR04 Ultrasonic sensor  
6 x 1.5V AA Battery (9V)  
4 x Small Breadboard  
1 x Switch  

*Note: Purchase does not include batteries and USB to Arduiuno Uno connector sold separately.


### Software Implementation Code

```#include <NewPing.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x38, 2, 1, 0, 4, 5, 6, 7, 3, POSITIVE); 

//Sensor A Back Left
#define trigger_pinA 2
#define echo_pinA 3

//Sensor B  FRONT
#define trigger_pinB 4
#define echo_pinB 5

//Sensor C Front Left
#define trigger_pinC 6
#define echo_pinC 7

//Initialise Sensors
NewPing backLeftSensor(trigger_pinA,echo_pinA,100);
NewPing frontSensor(trigger_pinB,echo_pinB,100);
NewPing leftSensor(trigger_pinC,echo_pinC,100);

int distanceLeft;
int distanceFront;
int distanceBackLeft;

//Define directions
#define LEFT 0
#define BACKLEFT 1
#define FRONT 2

//Motors

//Right
int enA = 10;
int in1 = 9;
int in2= 8;
//Left

int enB = 11;
int in3 = 13;
int in4 = 12;

void setLeft(int direction, int speed){
  if(direction == -1){
    digitalWrite(in1,HIGH);
    digitalWrite(in2,LOW);
  }
    else if (direction==1){
      digitalWrite(in2,HIGH);
      digitalWrite(in1,LOW);
    }
    else if (direction ==0){
      digitalWrite(in2,LOW);
      digitalWrite(in1,LOW);
    }
  analogWrite(enA,speed);
}

void setRight(int direction,int speed){
  
   if(direction == -1){
    digitalWrite(in3,HIGH);
    digitalWrite(in4,LOW);
   }
    else if (direction== 1){
      digitalWrite(in4,HIGH);
      digitalWrite(in3,LOW);
  }
  
  else if (direction == 0){
    digitalWrite(in3,LOW);
    digitalWrite(in4,LOW);
  }
  analogWrite(enB,speed);
}
int PSDGet(int direction){
  switch(direction){
    case LEFT:
      return leftSensor.ping_cm();
    case BACKLEFT:
      return backLeftSensor.ping_cm();
     case FRONT:
      return frontSensor.ping_cm();
  }
}
void print(String str){
  lcd.clear();
  lcd.print(str);
  
}
//
//void reallignWall(){
//  distanceLeft=PSDGet(LEFT);
//  distanceBackLeft=PSDGet(BACKLEFT);
//  int distance = distanceLeft;
//  while(
//}

void turnRight(){
  Serial.print(“Turn Right”);
  setLeft(0,0);
  setRight(1,200);
  delay(200);
  setRight(1,100);
  setLeft(1,100);
  delay(900);
  setLeft(0,0);
  setRight(1,120);
  delay(150);
  setRight(0,0);
  setLeft(0,0);
  delay(1000);
  setRight(1,100);
  setLeft(1,100);
  delay(250);
//  distanceLeft = PSDGet(LEFT);
//  distanceBackLeft = PSDGet(BACKLEFT);
//  int dist = distanceLeft;
//  while(distanceBackLeft == 0){
//    distanceLeft = PSDGet(LEFT);
//    if (distanceLeft > dist){
//      setRight(1,100);
//      setLeft(1,60);
//      
//    }
//    else if(distanceLeft < dist){
//      setRight(1,60);
//      setLeft(1,100);
//    }
//    else{
//      setRight(1,100);
//      setLeft(1,100);
//    }
//    distanceLeft = PSDGet(LEFT);
//    distanceBackLeft = PSDGet(BACKLEFT);
//  }
}

void setup() {
  lcd.begin(16,2);
  lcd.setCursor(0,0);
  
  pinMode(enA,OUTPUT);
  pinMode(enB,OUTPUT);
  pinMode(in1,OUTPUT);
  pinMode(in2,OUTPUT);
  pinMode(in3,OUTPUT);
  pinMode(in4,OUTPUT);
  
  Serial.begin(9600);
  distanceLeft = leftSensor.ping_cm();
  distanceFront = frontSensor.ping_cm();
  distanceBackLeft = backLeftSensor.ping_cm();
}

void loop() {
//  int pingLeft = leftSensor.ping_median(1);
//  int pingFront = frontSensor.ping_median(1);
//  int pingRight = leftSensor.ping_median(1);
//  int distanceLeft = leftSensor.convert_cm(pingLeft);
//  int distanceFront = frontSensor.convert_cm(pingFront);
//  int distanceBackLeft = backLeftSensor.convert_cm(pingRight);
 
 distanceLeft = PSDGet(LEFT);
 distanceFront = PSDGet(FRONT);
 distanceBackLeft = PSDGet(BACKLEFT);
 
// if (distanceFront < 10 && distanceFront != 0){
//  setLeft(-1,100);
//  setRight(-1,100);
//  delay(750);
// }
 
 if(distanceLeft == 0){
  setLeft(0,0);
  setRight(0,0);
  delay(1000);
  turnRight();
 }
 
 else{
  Serial.print(“Wall Follow”);
  Serial.print(“\n”);
 if(distanceBackLeft < 10 && distanceLeft < 10){
  setLeft(1,100);
  setRight(1,60);
//  delay(50);
//  setLeft(1,100);
//  setRight(1,100);
 }
 else if (distanceBackLeft>15 && distanceLeft>15){
  setLeft(1,60);
  setRight(1,100);
//  delay(50);
//  setLeft(1,100);
//  setRight(1,100);
 }
 else{
  if(distanceBackLeft == distanceLeft){
    setLeft(1,100);
    setRight(1,100);
  }
  else if(distanceBackLeft > distanceLeft && distanceLeft!=0){
    setLeft(1,100);
    setRight(1,60);
  }
  else if(distanceBackLeft < distanceLeft && distanceBackLeft != 0){
    setLeft(1,60);
    setRight(1,100);
  }
 }
 }
}




