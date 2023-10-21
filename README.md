- 👋 Hi, I’m @Dahman99abdo
- 👀 I’m interested in robotics
- 🌱 I’m currently learning C++
- 💞️ I’m looking to collaborate on a project of agricultural robot
- 📫 How to reach me( a.dahman@univ-boumerdes.dz )

<!---
Dahman99abdo/Dahman99abdo is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
//scenario with button 
#include <QMC5883LCompass.h>
#include <Wire.h>
#define trigPin 11
#define echoPin 12
QMC5883LCompass compass;
#define HMC5883L_ADDRESS              (0x1E)
// Motor A connections
int enA = 9;
int in1 = 8;
int in2 = 7;
// Motor B connections
int enB = 3;
int in3 = 5;
int in4 = 4;
int distance;
long duration, cm;
int startButtonPin = 2;
bool isRobotRunning = false;

void setup() {
Serial.begin(9600);
Wire.begin();
Wire.beginTransmission(HMC5883L_ADDRESS);
Wire.write(0x02);
pinMode(trigPin, OUTPUT);
pinMode(echoPin, INPUT);
compass.init();
pinMode(enA, OUTPUT);
pinMode(in1, OUTPUT);
pinMode(in2, OUTPUT);
pinMode(enB, OUTPUT);
pinMode(in3, OUTPUT);
pinMode(in4, OUTPUT);
pinMode(startButtonPin, INPUT_PULLUP);
}
void marcheavant(int v){
analogWrite(enA, v);
digitalWrite(in1, LOW);
digitalWrite(in2, HIGH);
analogWrite(enB, v);
digitalWrite(in3, LOW);
digitalWrite(in4, HIGH);}
void loop() {
int x, y, z, a, b;
int duration, distance;
bool S;
digitalWrite(trigPin, HIGH);
delayMicroseconds(1000);
digitalWrite(trigPin, LOW);
duration = pulseIn(echoPin, HIGH);
distance = (duration / 2) / 29.1;
char myArray[3];
compass.read();
a = compass.getAzimuth();
Serial.print(" Azimuth: ");
Serial.print(a);
Serial.print(" Dist: ");
Serial.println(distance);
delay(250);
S=digitalRead(startButtonPin);
if (S== LOW) {
isRobotRunning = true;
}
if (isRobotRunning && distance > 50) {
marcheavant(255);
Serial.println("robot is running");
} else {
freinage();
Serial.println("STOP");
}
}
void freinage() {
analogWrite(enA, 0);
digitalWrite(in1, LOW);
digitalWrite(in2, LOW);
analogWrite(enB, 0);
digitalWrite(in4, HIGH);}
void marcheaarierre(int v) {
analogWrite(enA, v);
digitalWrite(in1, HIGH);
digitalWrite(in2, LOW);
analogWrite(enB, v);
digitalWrite(in3, HIGH);
digitalWrite(in4, LOW);}
void tournerd(int v) {
analogWrite(enA, v);
digitalWrite(in1, HIGH);
digitalWrite(in2, LOW);
analogWrite(enB, v);
digitalWrite(in3, LOW);
digitalWrite(in4, HIGH);
}

