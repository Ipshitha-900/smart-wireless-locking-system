# OTP bsed smart-wireless-locking-system
#include <Servo.h>

Servo lockServo;

String receivedOTP = "";
String validOTP = "1234"; // Set your OTP here or generate dynamically
bool isUnlocked = false;

void setup() {
  Serial.begin(9600); // For Bluetooth (HC-05)
  lockServo.attach(9); // Servo connected to pin 9
  lockServo.write(0); // Locked position
  Serial.println("Enter OTP to unlock:");
}

void loop() {
  if (Serial.available()) {
    char ch = Serial.read();
    
    if (ch == '\n' || ch == '\r') {
      if (receivedOTP == validOTP) {
        Serial.println("OTP correct! Unlocking...");
        lockServo.write(90); // Unlock position
        delay(5000);         // Keep it unlocked for 5 sec
        lockServo.write(0);  // Lock again
        Serial.println("Locked again.");
      } else {
        Serial.println("Wrong OTP. Try again.");
      }
      receivedOTP = ""; // Clear for next attempt
    } else {
      receivedOTP += ch;
    }
  }
}
