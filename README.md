# Evaporative Cooling System (Swamp Cooler)

Introduction

This project presents the design and implementation of an evaporative cooling system (commonly known as a swamp cooler) using the Arduino Mega 2560 microcontroller. The system is developed for use in dry, hot climates where water-based cooling is more effective than traditional air conditioning. It draws in warm air through a water-soaked pad, where evaporation reduces the air temperature and adds humidity before it is released through a vent.

The system uses environmental sensors and a finite state machine to control its behavior. It monitors temperature, humidity, and water levels, allowing for automated transitions between system states. It also allows user control through physical buttons and vent direction control via a stepper motor. A real-time clock (RTC) module timestamps system transitions and sends event logs to a host machine via serial communication.

Features

Dynamic State Management
The system operates in four distinct states: DISABLED, IDLE, RUNNING, and ERROR. Transitions occur automatically based on sensor inputs or user actions.
Environmental Monitoring
The DHT11 sensor collects ambient temperature and humidity data. These readings are used to decide when to activate the cooling process and are shown on an LCD display.
Water Level Detection
A water level sensor monitors the reservoir. If the level falls below a set threshold, the system enters the ERROR state and halts all operations.
Vent Control
A stepper motor allows users to manually adjust the direction of the cooled air using a potentiometer or directional buttons.
User Feedback
Real-time environmental data and system status are displayed on a 16x2 LCD screen. A DS3231 RTC module logs all key state transitions with timestamps, which are sent via serial output.
Component List

Component	Description
Arduino Mega 2560	Main microcontroller for system control
DHT11 Sensor	Monitors ambient temperature and humidity
Water Level Sensor	Detects if the water reservoir level is too low
Stepper Motor + Driver	Adjusts vent angle based on user input
Fan Motor	Intended to provide airflow (non-functional due to hardware failure)
LCD Display (16x2 or I2C)	Displays temperature, humidity, and error messages
DS3231 RTC Module	Provides real-time timestamps for logging transitions
LED Indicators	Indicate system state: Yellow (DISABLED), Green (IDLE), Blue (RUNNING), Red (ERROR)
Push Buttons	Start, Stop, and Reset buttons for user input
Potentiometer/Buttons	Manual control for vent direction via stepper motor
System Overview

The system follows a state-machine model, where each state represents a specific mode of operation:

DISABLED State
All components are inactive. The yellow LED is turned on. The system waits for the Start button to be pressed (triggered via external interrupt).
IDLE State
The system monitors the environment (temperature, humidity, and water level). If the temperature exceeds a threshold and water is sufficient, it transitions to RUNNING. If the water is low, it transitions to ERROR. The green LED is activated.
RUNNING State
The fan motor was designed to activate in this state, but it was non-operational due to hardware failure. The stepper motor still functions, allowing vent adjustments. The blue LED is on. Real-time environmental data is displayed on the LCD and logged.
ERROR State
Triggered by low water level. The system halts and waits for user Reset. The red LED is turned on. An error message appears on the LCD.
LCD updates occur every 60 seconds, using a timer based on millis(). The Start button and water sensor are implemented using direct register access without restricted libraries.

Software Setup

To run the system, the following tools and libraries must be installed:

1. Install Arduino IDE
Download and install the Arduino IDE.
2. Install Required Libraries
Use the Arduino Library Manager to install the following:

RTClib by Adafruit
DHTLib by Rob Tillaart
LiquidCrystal by Arduino or Adafruit
3. Upload the Code
Open the main.ino file in the Arduino IDE.
Connect the Arduino Mega 2560 to your computer via USB.
Verify and upload the code to the board.
Challenges Faced

Humidity Sensor Failure
The DHT11 sensor stopped functioning during testing, leading to unreliable or missing environmental data. The code was modified to allow the system to continue running and display a warning when no readings are available.
Fan Motor Failure
The fan motor failed completely during development and was not repaired or replaced in time for final testing. Although cooling logic was implemented in the software, physical airflow could not be demonstrated.
Future Work

Replace the DHT11 sensor with a DHT22 for improved accuracy and reliability.
Install a fully functional fan motor with proper power handling.
Implement persistent storage (e.g., SD card) to log environmental data and state transitions.
Add a remote monitoring interface for real-time performance tracking.
Credits

This project was developed by Mary Ann Affo as part of the CPE 301 Embedded Systems Design course at the University of Nevada, Reno. The author thanks course instructors and peers for their support and feedback throughout the project.

License

This project is licensed under the MIT License.

