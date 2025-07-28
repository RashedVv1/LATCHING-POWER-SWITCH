#PROJECT NAME Auto-Latching Power Switch Using Arduino

📋 Description
This project demonstrates how to build a latching power switch circuit using an Arduino. The circuit allows you to toggle a power load (like an LED or relay) ON with a single button press and automatically turn it OFF after a preset time interval or condition (e.g., 10 seconds timeout).

It simulates a soft-latch system like those found in power buttons on electronics, with the added feature of automatic shut-off.

🎯 Features
Single push-button toggles the circuit ON and OFF.

Output remains latched (ON) after button release.

Circuit automatically turns OFF after a set timeout.

Useful for battery saving or safety shutoffs.

⚙️ Components Used
Component	Quantity
Arduino Uno	1
Push Button	1
LED (as output load)	1
220Ω Resistor (for LED)	1
10kΩ Resistor (pull-down)	1
Breadboard & Wires	As needed


💻 Arduino Code

const int buttonPin = 2;    // Pushbutton on digital pin 2
const int latchPin = 5;     // Transistor base for MOSFET control
const int pwmPin = 9;       // PWM pin for first LED brightness

const unsigned long onDuration = 10000;  // 10 seconds on
const unsigned long offDuration = 5000;  // 5 seconds off
unsigned long lastButtonPress = 0;
bool isPowered = false;

void setup() {
  pinMode(buttonPin, INPUT);
  pinMode(latchPin, OUTPUT);
  pinMode(pwmPin, OUTPUT);
  digitalWrite(latchPin, LOW);  // MOSFET LED off initially
  analogWrite(pwmPin, 0);       // First LED off initially
  Serial.begin(9600);
}

void loop() {
  int buttonState = digitalRead(buttonPin);
  if (buttonState == HIGH && !isPowered) {
    isPowered = true;
    lastButtonPress = millis();
    digitalWrite(latchPin, HIGH);  // Turn on MOSFET LED
    analogWrite(pwmPin, 64);       // Set first LED to 25% brightness
    Serial.println("Power ON");
  }
  if (isPowered && millis() - lastButtonPress >= onDuration) {
    digitalWrite(latchPin, LOW);   // Turn off MOSFET LED
    analogWrite(pwmPin, 0);        // Turn off first LED
    isPowered = false;
    lastButtonPress = millis();
    Serial.println("Power OFF");
  }
  if (!isPowered && millis() - lastButtonPress >= offDuration) {
    isPowered = true;
    lastButtonPress = millis();
    digitalWrite(latchPin, HIGH);  // Turn on MOSFET LED
    analogWrite(pwmPin, 64);       // Set first LED to 25% brightness
    Serial.println("Auto Power ON");
  }
}

🔌 How It Works
Press the button once → power turns ON.

Arduino remembers the state and keeps the load ON.

After 10 seconds → Arduino turns the load OFF automatically.

Press again to restart the process.

