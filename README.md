//Bluetooth and Voice control
<br>
#include <Servo.h>
#include <AFMotor.h>
#define Echo A0
#define Trig A1
#define motor 10
#define Speed 170
#define spoint 103
char value;
int distance;
int Left;
int Right;
int L = 0;
int R = 0;
int L1 = 0;
int R1 = 0;
Servo servo;
AF_DCMotor M1(1);
AF_DCMotor M2(2);
AF_DCMotor M3(3);
AF_DCMotor M4(4);
void setup() {
 Serial.begin(9600);
 pinMode(Trig, OUTPUT);
 pinMode(Echo, INPUT);
 servo.attach(motor);
 M1.setSpeed(Speed);
 M2.setSpeed(Speed);
 M3.setSpeed(Speed);
 M4.setSpeed(Speed);
}
void loop() {
 //Bluetoothcontrol();
 //voicecontrol();
}
void Bluetoothcontrol() {
 if (Serial.available() > 0) {
 value = Serial.read();
 Serial.println(value);
 }
 if (value == 'F') {
 forward();
 } else if (value == 'B') {
 backward();
 } else if (value == 'L') {
 left();
 } else if (value == 'R') {
 right();
 } else if (value == 'S') {
 Stop();
 }
}
void Obstacle() {
 distance = ultrasonic();
 if (distance <= 12) {
 Stop();
 backward();
 delay(100);
 Stop();
 L = leftsee();
 servo.write(spoint);
 delay(800);
 R = rightsee();
 servo.write(spoint);
 if (L < R) {
 right();
 delay(500);
 Stop();
 delay(200);
 } else if (L > R) {
 left();
 delay(500);
 Stop();
 delay(200);
 }
 } else {
 forward();
 }
}
void voicecontrol() {
 if (Serial.available() > 0) {
 value = Serial.read();
 Serial.println(value);
 if (value == '^') {
 forward();
 } else if (value == '-') {
 backward();
 } else if (value == '<') {
 L = leftsee();
 servo.write(spoint);
 if (L >= 10 ) {
 left();
 delay(500);
 Stop();
 } else if (L < 10) {
 Stop();
 }
 } else if (value == '>') {
 R = rightsee();
 servo.write(spoint);
 if (R >= 10 ) {
 right();
 delay(500);
 Stop();
 } else if (R < 10) {
 Stop();
 }
 } else if (value == '*') {
 Stop();
 }
 }
}
 digitalWrite(Trig, LOW);
 delayMicroseconds(4);
 digitalWrite(Trig, HIGH);
 delayMicroseconds(10);
 digitalWrite(Trig, LOW);
 long t = pulseIn(Echo, HIGH);
 long cm = t / 29 / 2; //time convert distance
 return cm;
}
void forward() {
 M1.run(FORWARD);
 M2.run(FORWARD);
 M3.run(FORWARD);
 M4.run(FORWARD);
}
void backward() {
 M1.run(BACKWARD);
 M2.run(BACKWARD);
 M3.run(BACKWARD);
 M4.run(BACKWARD);
}
void right() {
 M1.run(BACKWARD);
 M2.run(BACKWARD);
 M3.run(FORWARD);
 M4.run(FORWARD);
}
void left() {
 M1.run(FORWARD);
 M2.run(FORWARD);
 M3.run(BACKWARD);
 M4.run(BACKWARD);
}
void Stop() {
 M1.run(RELEASE);
 M2.run(RELEASE);
 M3.run(RELEASE);
 M4.run(RELEASE);
}
int rightsee() {
 servo.write(20);
 delay(800);
 Left = ultrasonic();
 return Left;
}
int leftsee() {
 servo.write(180);
 delay(800);
 Right = ultrasonic();
 return Right;
}
Line Tracking
//ARDUINO LINE FOLLOWING CAR//
// YOU HAVE TO INSTALL THE AFMOTOR LIBRARY BEFORE UPLOAD
THE CODE//
// GO TO SKETCH >> INCLUDE LIBRARY >> ADD .ZIP LIBRARY >>
SELECT AF MOTOR ZIP FILE //
//including the libraries
#include <AFMotor.h>
//defining pins and variables
#define left A0
#define right A1
//defining motors
AF_DCMotor motor1(1, MOTOR12_1KHZ);
AF_DCMotor motor2(2, MOTOR12_1KHZ);
AF_DCMotor motor3(3, MOTOR34_1KHZ);
AF_DCMotor motor4(4, MOTOR34_1KHZ);
void setup() {
 //declaring pin types
 pinMode(left,INPUT);
 pinMode(right,INPUT);
 //begin serial communication
 Serial.begin(9600);

}
void loop(){
 //printing values of the sensors to the serial monitor
 Serial.println(digitalRead(left));
 Serial.println(digitalRead(right));
 //line detected by both
 if(digitalRead(left)==0 && digitalRead(right)==0){
 //Forward
 motor1.run(FORWARD);
 motor1.setSpeed(150);
 motor2.run(FORWARD);
 motor2.setSpeed(150);
 motor3.run(FORWARD);
 motor3.setSpeed(150);
 motor4.run(FORWARD);
 motor4.setSpeed(150);
 }
 //line detected by left sensor
 else if(digitalRead(left)==0 && !analogRead(right)==0){
 //turn left
 motor1.run(FORWARD);
 motor1.setSpeed(200);
 motor2.run(FORWARD);
 motor2.setSpeed(200);
 motor3.run(BACKWARD);
 motor3.setSpeed(200);
 motor4.run(BACKWARD);
 motor4.setSpeed(200);

 }
 //line detected by right sensor
 else if(!digitalRead(left)==0 && digitalRead(right)==0){
 //turn right
 motor1.run(BACKWARD);
 motor1.setSpeed(200);
 motor2.run(BACKWARD);
 motor2.setSpeed(200);
 motor3.run(FORWARD);
 motor3.setSpeed(200);
 motor4.run(FORWARD);
 motor4.setSpeed(200);

 }
 //line detected by none
 else if(!digitalRead(left)==0 && !digitalRead(right)==0){
 //stop
 motor1.run(RELEASE);
 motor1.setSpeed(0);
 motor2.run(RELEASE);
 motor2.setSpeed(0);
 motor3.run(RELEASE);
 motor3.setSpeed(0);
 motor4.run(RELEASE);
 motor4.setSpeed(0);

 }

}
