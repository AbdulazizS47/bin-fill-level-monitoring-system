# bin-fill-level-monitoring-system
![image](https://github.com/AbdulazizS47/bin-fill-level-monitoring-system/assets/169719972/de25a6fd-d989-4356-9077-572c8351d460)


(( Computer engineering department at College of Computer Sciences & Information Technology )) 


Done By                                                    
Abdulaziz Al Abdulkarim & Yamin Alduraiwaish  

 

This model project leverages the power of Arduino and ultrasonic distance sensing technology to create a modern solution for efficient waste management.

At its core, the Smart Trash Bin utilizes an Arduino microcontroller to process data from an ultrasonic distance sensor. This sensor accurately measures the distance to the surface of the trash, enabling the system to calculate the fill level of the bin in real time. The data is then displayed on a LiquidCrystal I2C Display, providing users with clear and concise information about the current fill level.

With its intuitive interface and smart alerts, the Smart Trash Bin project offers a seamless user experience, ensuring timely notifications when the bin reaches capacity. Whether you're seeking to optimize waste collection processes at home, in the office, or in public spaces, this project exemplifies the potential of technology to revolutionize traditional tasks and enhance efficiency.

Explore the code, documentation, and schematics in this repository to learn more about how you can build your own Smart Trash Bin and contribute to the future of sustainable waste management. Join us in transforming the way we handle waste for a better and a clean environment around the world. 

Feel free to copy the source code! . 




#include <Wire.h>
#include <LiquidCrystal_I2C.h>

#define TRIGGER_PIN  11
#define ECHO_PIN     12

#define BIN_HEIGHT   18 // Maximum height of the bin in centimeters
#define BIN_EMPTY    5  // Distance from sensor to bottom of an empty bin

LiquidCrystal_I2C lcd(0x27, 16, 2); // Set the LCD address to 0x27 for a 16x2 display

void setup() {
  // Initialize pins
  pinMode(TRIGGER_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  
  // Initialize LCD
  lcd.init();
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("Smart Trash Bin");
  lcd.setCursor(0, 1);
  lcd.print("Ready...");
  delay(2000); // Display "Ready..." for 2 seconds
}

void loop() {
  long duration, distance;
  
  // Clear the LCD
  lcd.clear();
  
  // Trigger the ultrasonic sensor
  digitalWrite(TRIGGER_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIGGER_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIGGER_PIN, LOW);
  
  // Measure the duration of the pulse
  duration = pulseIn(ECHO_PIN, HIGH);
  
  // Calculate the distance
  distance = duration * 0.034 / 2; // Convert to centimeters
  
  // Map the distance to a percentage of the bin capacity
  int fillPercentage = map(distance, BIN_EMPTY, BIN_HEIGHT, 100, 0);
  fillPercentage = constrain(fillPercentage, 0, 100); // Ensure the percentage is within range
  
  // Display the fill level on the LCD
  lcd.setCursor(0, 0);
  lcd.print("Fill Level: ");
  lcd.print(fillPercentage);
  lcd.print("%");
  
  // Check if the bin is full
  if (fillPercentage <= 10) {
    lcd.setCursor(0, 1);
    lcd.print("Bin is Empty!");
  } else if (fillPercentage >= 90) {
    lcd.setCursor(0, 1);
    lcd.print("Full! Empty Me.");
    // Add additional actions here, such as triggering an alarm or sending a notification
  }
  
  delay(1000); // Update display every second
}
