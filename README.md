# Waste-Detection-Bin
Recruitment Project for 1st Year.

  Project Report
  Waste Detecting Bin

Mentor:
Nitika Gupta
Pranjal Mangal

Team:
Himanshi
Tanisha Meena
Priyansh Bhandari
Sameer Talwar

Introduction :-

The dustbin has the following features-


* Detects the presence of an approaching human and automatically opens up the lid.
* Waste segregation is done, into dry and wet, using appropriate sensors.
* Metallic waste is also segregated into the dry compartment, prior to non-metallic waste.
* Measures the depth of the dustbin upto which it is filled, to prevent the overfilling of the dustbin.








Automatic lid opening using ultrasonic sensors:-

Ultrasonic Sensor is an instrument which measures the distance of a person/hand or any obstruction using ultrasonic sound waves which are emitted at a frequency much high than the audible range (approximately 40 kHz).

 It has two transducers (a transmitter and a receiver) which basically acts as  microphones that help to send and receive ultrasonic pulses. The transmitter converts the electrical signals into 40kHz ultrasonic sound pulses.

The receiver listens for the transmitted pulses and if it receives them then it produces an output pulse using the echo pin whose length is directly proportional to the time traversed by the sound waves and hence the distance between the sensor and the obstacle .

When a pulse of at least 10 us duration is applied to the Trigger pin ; the sensor in response generates  a sonic burst of eight pulses at 40 kHz . This 8-pulse pattern makes the ‘ultrasonic signature’ allowing the receiver to differentiate the transmitted pattern from the ambient ultrasonic noise if any.

Then, the servo motor helps in opening the lid of the dustbin. The arduino is programmed in such a way that after detecting the person/hand or any obstacle using an ultrasonic sensor, the lid will open automatically and this is done using the servo motor and similarly if the person or obstruction leaves the specific proximity then the lid automatically gets closed after a certain delay as programmed.

distance=[(speed of sound)*(time taken by pulse)]/2

The time taken by pulse to reach the ultrasonic sensor back is found out using a pulseIn function. The eight ultrasonic pulses travel away from the transmitter meanwhile the Echo pin goes to HIGH , and as soon as the pulses are reflected back the echo pin goes low as soon as the signal is received . The pulse then generated by the Echo pin helps in calculating the total time took by the sound waves as the length of the pulse generated is directly proportional to the time taken.

In case if those pulses are not reflected back then the Echo signal will  timeout after 38 mS and return low ; indicating there is no obstruction within the range of the sensor. 


                                                    CODE - 

#include<Servo.h>

int pulsetime;
int trigPin=5;
int echoPin=6;
float distance;
Servo myServo;
int lid_delaytime=5000;
  
void setup()
{
pinMode(trigPin,OUTPUT);
  pinMode(echoPin, INPUT);
  Serial.begin(9600);
myServo.attach(11);
myServo.write(0);
}

void loop()
{
  digitalWrite(trigPin, LOW);
  delayMicroseconds(5);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(5);
  digitalWrite(trigPin, LOW);
  
  pulsetime=pulseIn(echoPin, HIGH);
  //Serial.println(pulsetime);
  distance=(100.0/5742)*pulsetime;
  //Serial.println(distance);

if (distance<100.0){
myServo.write(85);
delay(lid_delaytime);
}
else myServo.write(0);
}

Explanation:

We have used the Servo library to work with servo motors, and a four pin ultrasonic distance sensor has been used.
After the setup, and in the loop, we emit a high pitched pulse, through the trigger pin. The pin is set to emit pulses in the time intervals of ten microseconds. The echo pin receives these pulses after a certain time, which is measured using the pulseIn function. It returns time in microseconds, for which the echo pin was high.
Here, distance is calculated through unitary method. In the simulator, a distance of 100 cm was measured in 5742 microseconds, hence we used this as a slope to find the value of distance for a value of pulse time, where, the distance is plotted on the y-axis and the pulse time on the x-axis.

The threshold value to determine the presence of a person has been kept to be 100 cm. I.e, when the ultrasonic distance sensor gives a reading of less than 100 cm, due to a human (or an obstruction) coming in front of it, the dustbin’s lid should open up. 

The angle of rotation of the lid has been kept 85 degrees, through the servo.write() function.
A delay of five seconds has been introduced, i.e. the lid will remain open for five seconds, even after the person in front has left, and thereafter, closes.






1. Waste Segregation:-


a. Detection of metallic objects before segregation of waste into wet and dry using NPN inductive proximity sensors:-
       Metal objects would fail the wet and dry test, as they are conductive, and will complete the moisture/wetness detecting circuit. Hence, they need to be segregated first.
     NPN Inductive proximity sensors can detect metal targets approaching the sensor, without physical contact with the target.

Working of NPN inductive Proximity Sensors

 Now talking about the working principle of the sensor; the working principle of inductive proximity sensors is based on Electromagnetic Induction.The inductive sensors consists of sensing coil which is ferromagnetic have copper turns, the coil will generate high frequency oscillation field. This sensor operates when a metal target enters the magnetic field created by the coil generating eddy currents which circulates within the target and as the proximity of the target to the sensor increases then the oscillation amplitude simultaneously decreases and in this way the sensor detects that whether the target is metallic or not.
     Two wires go into ground and 5V respectively, while the third wire can be connected to an analog pin. This voltage value can be read on the serial monitor, and a threshold can be determined to decide if a metallic object is present or not.

   CODE

#include<Servo.h>
Servo myServo1;
Servo myServo2;
int servoPin1=9;
int servoPin2=10;
float metalDetected;
int reading;
int metalDetectionPin = 1;
 void setup(){
Serial.begin(9600);
myServo1.attach(servoPin1);
myServo2.attach(servoPin2);
myServo1.write(90);
myServo2.write(90);
}
void loop(){
reading = analogRead(metalDetectionPin);
metalDetected = (float) reading*100/1024.0;
Serial.print("Initializing Proximity Sensor");
delay(500);
Serial.print("Please wait...");
delay(1000);
Serial.print("Metal is Proximited = ");
Serial.print(metalDetected);
Serial.println("%");
if (reading > 250){
Serial.println("Metal is Detected");
myServo1.write(15);
myServo2.write(15);
delay(5000);
}
else {
        	myServo1.write(90);
myServo2.write(90);
}
}





b. Differentiating between Dry and wet waste:-
     If the target is metallic then the two platforms ( one with metal detector and the other with moisture sensor) are programmed in such a way that both rotate simultaneously towards the compartment containing dry waste and if the target waste is non metallic then only the plate with metal detector will rotate, dropping the waste onto the second plate to segregate the target waste into wet and dry using the IR sensor and the moisture sensor.


CODE:-

#include<Servo.h>

Servo myServo1;
int servoPin  2

int sensor_pin = A3;
int output_value ;

void setup()
{
Serial.begin(9600);

Serial.println("Reading From the Sensor ...");
delay(2000);

myServo1.attach(servoPin);
myServo1.write(90);
 }
void loop() {
output_value= analogRead(sensor_pin);
output_value = map(output_value,550,0,0,100);

Serial.print("Mositure : ");
Serial.print(output_value);
Serial.println("%");

if (output_value>15) {

myServo1.write(165);
Serial.println(Wet waste);
delay(5000);

myServo1.write(90);}

else {

myServo1.write(15);
Serial.println(Dry waste);
delay(5000);

myServo1.write(90);
}

delay(1000);
}
Working of the Moisture and IR sensors -

IR sensor basically detects the infrared radiations radiated by any body as well as the movement of the body in its proximity; hence when the target gets over the second lid IR sensor basically detects that the target waste is over the lid and then the moisture sensor starts its working.

Moisture sensors detect the amount of moisture/water content present in the material, by keeping it over two electrodes. The electrical resistance through the sensor is measured. The higher the water content of the material, the lower the electrical resistance.

Measures the depth of dustbin upto which it is filled :-

We will use ultrasonic sensors which measure the depth by ultrasonic waves.The sensor head emits an ultrasonic wave and receives the wave reflected back from the waste at the base. It measures the distance to the target by measuring the time between the emission and reception. The output or the result which it gives can be displayed on an lcd monitor, as well as a buzzer could also be attached to generate an alarming beeping noise. The sensors are mounted on the dustbin’s inner wall just below the second platform.



CODE: 

#include<LiquidCrystal.h>

int trigPin=4;
int echoPin=3;
int pulseTime;
float distance;

int rs=7;
int en=8;

int d1=10,d2=11,d3=12,d4=13;
LiquidCrystal lcd(rs,en,d1,d2,d3,d4);

void setup()
{
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  lcd.begin(16,2);
}

void loop()
{
  digitalWrite(trigPin, LOW);
  delayMicroseconds(5);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(5);
  digitalWrite(trigPin, LOW);
  
  pulseTime=pulseIn(echoPin, HIGH);
  distance=(100.0/5810)*pulseTime;
  
  if (distance<20.0){
  lcd.setCursor(0,0);
  lcd.print("Dustbin filled!");
  lcd.setCursor(0,1);
  lcd.print("Please Empty");
  }
}

Explanation:

We have used the Liquid Crystal library to work with LCD monitors, and a four pin ultrasonic distance sensor has been used.
After the setup, and in the loop, we emit a high pitched pulse, through the trigger pin. The pin is set to emit pulses in the time intervals of ten microseconds. The echo pin receives these pulses after a certain time, which is measured using the pulseIn function. It returns time in microseconds, for which the echo pin was high.
Here, distance is calculated through unitary method. In the simulator, a distance of 100 cm was measured in 5810 microseconds, hence we used this as a slope to find the value of distance for a value of pulse time, where, the distance is plotted on the y-axis and the pulse time on the x-axis.

The threshold value to determine whether the dustbin is full or not, has been taken 20 cm here. For a value below 20, the LCD should display “Dustbin filled! Please empty.”

An alternative could be to use an LED bar which operates on certain value ranges from the ultrasonic distance sensor. For example, an empty dustbin should show zero leds ON, on the led bar, while a value of less than 20 should mean all ON. 







Future Possibilities:-


* Extended base can be constructed which will prompt user to throw the waste into the dustbin if it falls outside. This can be achieved using capacitive sensor or any other sensor which should be able to detect the presence of any kind of waste. If the waste is thrown out on the extended base then it can be displayed on a LED screen using arduino  along with a buzzer . 




* “Automatic emptying out of dustbin” system can also be implemented which helps in emptying the bin by just pressing the button on the bin itself. The feature can be implemented at the base of bin which will have two gates operated using servo motors which will open the gates on pressing the button. We just need to put a polythene bag below the bin and press the button
.



* Level tracking of dustbin using IOT can also be implemented like we have used the ultrasonic sensors to detect the depth upto which the dustbin is filled so by using IOT the output or result which it gives can be extracted by making a website or an app which instantaneously displays the level of the dustbin and we can check the level of dustbin from anywhere.


Challenges:-


1. Unnecessary opening of the lid, due to any obstruction in the close proximity of the opening lid, even when there is no intention of waste disposal.



2. The effectiveness of moisture/wetness sensor needs to be tested out on different kinds of waste. Also, there are drawbacks like dry leaves, which should ideally go into wet waste bin, but the sensor would not detect it, and hence it would go in the dry compartment instead.


3. The inductive proximity sensor also need to be tested out, in different scenarios, to identify any practical situation errors.



