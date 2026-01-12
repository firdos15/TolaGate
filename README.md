# TolaGate
TolaGate is an automated entry management system designed to streamline traffic flow in toll booths and parking lots. By leveraging ultrasonic sensing and micro-controller logic, the system provides a hands-free solution for vehicle detection and gate actuation.
#include <Servo.h>

// Pin Definitions
const int trigPin = 9;
const int echoPin = 10;
const int servoPin = 6;

// Variables
Servo gateServo;
long duration;
int distance;
int threshold = 10; // Distance in cm to trigger the gate

void setup() {
  pinMode(trigPin, OUTPUT); 
  pinMode(echoPin, INPUT);
  gateServo.attach(servoPin);
  
  // Initial position: Closed
  gateServo.write(0); 
  Serial.begin(9600);
}

void loop() {
  // Clear the trigPin
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  // Trigger the sensor
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Read the echoPin (returns travel time in microseconds)
  duration = pulseIn(echoPin, HIGH);

  // Calculate distance in cm
  distance = duration * 0.034 / 2;

  // Logic for Gate Operation
  if (distance > 0 && distance <= threshold) {
    Serial.println("Vehicle Detected: Opening Gate");
    gateServo.write(90); // Open position
    delay(3000);         // Wait for vehicle to pass
  } else {
    gateServo.write(0);  // Closed position
  }

  delay(100); // Small stability delay
}
