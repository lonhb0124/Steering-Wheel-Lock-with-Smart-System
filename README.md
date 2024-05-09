# Steering Wheel Lock with Smart System 
The project includes IoT - Application - Facial Recognition.

# Main Components
This code based IoT to control the hardware components and communicate with application through firebase. 

1. ESP32_S3 Wroom Board
2. OV2640 Camera 
3. Adafruit_SSD1306 (OLED)
4. Servo Motor
5. Vibration Sensor Module
6. PIR Motion Sensor
7. Push Button
8. LiPo Rider Plus Charger/Booster
9. Lithium-Ion Polymer (LiPo) Battery
10. 1GB SD card


# State_Flow
![image](https://github.com/lonhb0124/Steering-wheel-Lock-project/assets/111609834/e15f5873-621f-4f8b-84c8-0952d54aac1c)

Required States of the Product:
After the microcontroller is powered on (i.e. Adjacent sliding power switch), it sets up all the necessary components from the get go. This includes the hardware components’ GPIO, WiFi, and Firebase connections, while initializing the variables. It then goes to the first verification state [0] after it’s done initializing.
INIT STATE:
First initial state after the PIR motion sensor is tripped (i.e. Motion is detected).
The user can either enroll themselves or open direct access (i.e. Bypassing both identification processes) with the application.
On-going measurement of the vibration sensor’s threshold value to detect any intense movement incurred onto the steering wheel by unauthorized users, which triggers the alarm system. 
Note at any point in the states described below if the vibration sensor is triggered, the system will jump into the Alarm state.

VERIFICATION STATE: 
The user needs to press the push button to take a picture within 20 seconds to initiate the facial recognition system. 
GPS coordinates will be sent to the application to show the vehicle’s current location.
Otherwise, the system will jump into the Alarm state.

2FA STATE:
Users will be prompted to type the correct 6-digit code that’s being displayed on the OLED and onto the application.
Check if the user passes both facial verification and code identification. 
3 tries will be given if the user passes face verification. After 3 tries, a different code will be given.
If a user fails the facial verification, it will move towards the Alarm state.
If both tests are passed, jump towards the User state.

USER STATE:
Check the current time and start the cognitive game if the current time is between 5 PM - 3 AM for DUI verification.
If not then, open the servo lock for user convenience and turn on the green LED as a status indicator.

INTRUDER STATE:
Take the current picture from the unauthorized user and transmit it to the Firebase storage, which will be shown in the application (to see the intruder).

USERCONTROL STATE:
The user is able to control the servo lock through the app by pressing a button to unlock SHIELD.
Measure the last movement angle of the servo motor and the microcontroller will go back to Sleep mode if the time taken already exceeds the set time. Before going back to Sleep mode, it will lock the servo for user convenience if it’s not already done so

SLEEP MODE STATE:
The microcontroller goes back into sleep mode until motion is detected from the PIR motion sensor, drawing significantly less current overall.

ALARM STATE:
The piezo buzzer will alternate between its on and off state and can only be turned off via the application.
The microcontroller will take 10 images of the unauthorized user and send them to Firebase. 
If the user presses the “FORCE AUTHORIZATION” button in the application, it will leave the Alarm state and go into the User state.
Send GPS coordinates to the application for as long as possible (i.e. Until the GPS is destroyed or cannot establish a connection with an adjacent satellite).

COGNITIVEGAME STATE:
The user will be prompted to play the cognitive game through the application (via text on the OLED screen) and the microcontroller will check if the user passed the cognitive game or not within 3 attempts.
If the game is won, the servo lock will open and turn on a green LED to indicate a clear status.
If the game fails 3 times, the user must re-enter the 6 digit pin again.
If the game fails less than 3 times, a penalty will be given based on the number of failures and this information will be sent to Firebase to use for future use (i.e. Tracking failures).

WAITCOGNITIVEGAME STATE:
Based on the penalty time, the user must wait until the game restarts to play again.
First Failure: 10 mins.
Second Failure: 30 minutes.
Third Failure: 1 hour and then re-enter the 6-digit code.

ENROLLTRIGGER STATE:
The user will put in their details to register their account to the application. This includes a ID #, password, and name. Afterwards, the user must press the “sign up” button below.

ENROLLMENT STATE:
By pressing the push button switch for 1 second, the user will have 30 pictures taken from them to train the facial recognition model. The Verification state will commence after the training is done.

During every process, the microcontroller will display the current description and status on the OLED screen in front of the user. Also, it will send its GPS data to the application as long as it can establish a connection between itself and a nearby satellite. In any case, a visual model of how the states interact with each other.


# 
