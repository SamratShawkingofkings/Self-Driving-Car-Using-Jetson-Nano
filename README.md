# Jetson-Nano-Self-Driving-Car

   This self driving car is a robot with four wheels powered by Arduino UNO, a Motor driver, a LIPO battery, 4 motors and wheels. The intelligent operations are carried out by NVIDIA's Jetson Nano board, which is a single board computer with GPU cores and CUDA support. The Jetson board processes the images obtained in real time from a web camera and feeds them into a Neural network with convolutional and fully connected layers. Neural networks are universal function approximators which can be used for a wide variety of problems(Classification, Regression,Segmentation,etc). Our neural network performs regression to predict the steering angle of the car and the angle is used to control the speed of the wheels of the robot, which makes it turn left, right or forward.To understand this, consider a computer game where a user can drive a car by pressing the arrow keys. In order for an AI to drive the car, it should be able to predict the key to be pressed and the duration for which the key should be pressed based on the steering angle. To do this, it obtains a screen shot of the game for every fraction of a second and passes it through a neural network. The neural network predicts the steering angle at that instant and the AI presses the appropriate key. This keeps the car at the center of the road.

   Instead of making the AI drive the car in a computer game, we use real world data obtained from a robotic car prototype. The AI processes the real world feed and keeps the prototype at the center of the lane. We have used the following architecture which consists of 4 Convolutional layers, 1 fully connected layer and 1 output neuron which predicts the normalised steering angle.

## Components And Connections

We have used the following components to build our prototype:
 
 1) Battery (11.1V, 35C) - 1 pc.
 2) IC-7805 regulators - 4pcs.
 3) L298N motor driver - 1 pc.
 4) RPM Motors - 4 pcs.
 5) Arduino UNO - 1 pc.
 6) NVIDIA Jetson Nano board - 1 pc.
 7) Web Camera - 1 pc.
 8) Metal Chassis - 1 pc.
 9) Wheels - 4 pcs.
 10) Wires and Jumper Cables - as per required.
 11) Metal spacers - 4 pcs
 12) Perf Board - 1
 13) Wireless Keyboard and Mouse.
 14) Wifi adapter chip for Jetson Nano(Optional).
 
 #### Custom regulator circuit
 
To setup the Jetson Nano Board visit the link https://www.youtube.com/watch?v=km0yT99eVTY

<p align="center">
<img width="275" height="250" src="https://user-images.githubusercontent.com/34810513/79977737-5f8cc300-84bc-11ea-94d3-12505b291ee8.jpg">
</p> 

<p align="center">
  The custom regulator circuit. a) Circuit Diagram 
</p>

#### The Complete Circuit

   Once the custom regulator circuit is ready, it is time to build the complete circuit. Use the circuit diagram shown below to make the connections. In addition to this, connect Motors 3 and 4 in parallel with motors 1 and 2 respectively. The spacers can be elevate the position of the Jetson board which makes it easier to fit all the components onto a small chassis. Feel free to tinker with the design.
   
<p align="center">
  <img width="580" height="640" src="https://user-images.githubusercontent.com/34810513/80275548-9ef72180-86ff-11ea-9af5-98eba22c82bf.jpeg">
</p>

## Using the Code

After making the connections as shown above, it is time to setup the software of the Self Driving Car. There are 3 folders in the repository.

1) Arduino Code
2) Jetson Nano
3) PC

As the name suggests, each folder contains codes to be run in Arduino UNO, Jetson Nano and the PC respectively. Follow the procedure given below to get the self driving car up and running.

#### 1. Download the repository

This is the most simple and obvious step. Download the repository into your PC using the git command or the download option. All the folders have already been arranged for you in a way that the codes will run seamlessly. Copy the PC folder into you PC and Jetson Nano folder into your Jetson Board.

#### 2. Upload the Arduino UNO Code

Once you have downloaded the repo, connect the Arduino UNO to your PC and flash Motor.ino into it using the Arduino IDE. This code communicates with the Jetson Nano board using serial communication and controls the speeds of the motors.

#### 3. Collect the Training data

Now that the Arduino Code is ready, it is time to drive the car and collect the training data. Make your own track using A4 sheets or a flex banner with a track printed on it. Make sure that the "Images" folder and Rec.py exist in the same folder and run Rec.py program from terminal. The car starts to move after pressing either of the w a or d keys. To turn left press a, to turn right press d and to go forward press either w or press nothing. The car moves forward by default. Drive the car for atleast 20 laps to collect a decent amount of data to train the model. Make sure that the car has equal number of left and right turns and straight roads occasionally to acquired a robust data with all possible inputs and outputs. Press the "Esc" key to terminate the program. Open the images folder to look at all the images. The Targets.txt contains the respective steering angles for each images in a sequence.

#### 4.Prepare the Dataset

Copy the Images folder and Target.txt file to the SetUp folder. Run Data.py to convert the images into a .npy file containing Inputs and targets to train the neural network. The program creates both the balanced and unbalanced datasets. Use the balanced dataset available in the "Datasets" folder to train the model. This reduces the possibility of overfitting the neural network.

#### 5. Train the Model

Run MODEL.py to run the training program. This code create a custom neural network with the architecture specified above and train it for 30 epochs. To prevent overfitting it uses validation data and performs checkpointing. The models are saved in Models/Model HDF5/Regression by default. The finally created model is the most accurate model. 



#### 6. Convert the model to a TF-Lite model

Run CONVERT.py to convert the hdf5 tensorflow model to a TF-Lite model. This quantises the hdf5 model by converting the weight from float to int. This retains the accuracy of the model while making it much lighter. This allows Jetson Nano to perform the computations much faster than usual. The TF Lite model is saved in Models/TF Lite Models folder.

#### 7. Run the testing program.

Once the TF Lite Model is ready, copy the model into the Jetson Nano/Models folder in your Jetson Nano and run the TEST.py program. Keep the bot in the center of the track.





# Self-Driving-Car-Using-Jetson-Nano
