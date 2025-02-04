# Reverse-Vending-Machine
Reproducible Software Artifact for Cost Effective Reverse Vending Project

Our aim is to build a compact, cost-effective, and fraud-resistant reverse vending machine using minimalistic design. After 2021, our government will enact a container deposit legislation that will put monetary deposits on plastic, metal, glass, and carton containers. With this legislation, reverse vending machines will be ever more important and prevalent. Therefore, we wanted to be a part of the movement incentivizing a more sustainable life.

Our reverse vending machine uses AI-powered computer vision and a load cell to sort the containers. The computer vision algorithm classifies the containers into three categories (metal, plastic and carton) and the load cell checks whether they contain liquid or not. After accepting the containers, the machine transfers money to the user’s card.

YouTube video link: https://youtu.be/w6POGiyXDMc 

## Files contained
1) ```ai_training``` is the folder of files for training the AI model. 
    * ```AI_training.ipynb``` is the Google Colab notebook for custom training.
    * ```ai.py``` is the main training file.
    * ```ai_train.sh``` is the shell script for training the AI model from command line.
2) ```debug``` is the folder for debugging the individual parts of the project.
    * ```check_ai.py``` checks the AI model.
    * ```motor.py``` is the class for motors.
    * ```motor_check.py``` checks the motors.
    * ```take_photo.py``` takes photo from PiCamera.
    * ```weight_sensor_check.py``` checks the weight sensor.
3) ```hx711.py``` is the class for the weight sensor for Raspberry Pi 4.
4) ```labels.txt``` includes the labels of the types of containers our machine classifies (large_plastic, large_carton, small_plastic, small_carton, metal).
5) ```main_main_flow.py``` includes the main functionality of the machine (classification, weight_check, storage, GUI).
6) ```main_main_flow.sh``` inclues the shell script for custom machine functionality.
7) ```model.tflite``` includes the TFLite deep learning model. 
8) ```motor.py``` is the class for motors.
9) ```rasp_classify.py``` includes the functions to classify containers using PiCamera and AI.

## Setup Requirements
1) 5kg Load Cell + HX711 Amplifier
2) Raspberry Pi Camera Module
3) Raspberry Pi 7" Touch Screen Display
4) 3 DC motors
5) 2 L298N Motor Driver Board

## How to reproduce it
### Dataset
You can either use our dataset provided here or construct your own one. In order to construct one, you just need to take images of the classes of containers you would like your machine to classify and place each image into their respective class folders: (an example is shown below)
```
--> Dataset
    --> class_1
        --> image1.jpg (this is just an example, you can name your images whatever you wish)
        --> image2.jpg
        ...
    --> class_2
    --> class_3
    ...
```

### AI training
You can  either modify the paraneters in ```ai_train.sh``` for your own project or use ```ai_train_notebook.ipynb``` in order to interactively modify the training code: (specific instructions on how the code works are present in ```ai.py```)
```sh
$ bash ai_train.sh
```
After training you will have ```model.tflite``` and ```labels.txt``` files which are customized for your own training. Again, if you don't want custom training, you can just use the files we have provided here. In order to test the accuracy of your model as well as see the images that your model makes a mistake at (useful in diagnosing problems with the model or the dataset such as the model's inability to classify one of the brand's plastic bottle in the dataset) use this command:
```sh
$ python3 ai_training/test.py --labels_file "labels.txt" \
--model_file "model.tflite" \
--dataset_path "dataset"  # Assuming these are the path to your labels, model, and dataset 
```


### Raspberry Pi 4
Transfer all of your AI training files into your Raspberry Pi 4 (Be sure to clone this repository into your Raspberry Pi 4 if you still haven't done so, and replace the model.tflite and labels.txt files with your own ones. If you haven't trained your own AI, you can simply use our project's). 
After assembling your machine, you must start debugging it. You must be sure that every component of the machine works correctly by using the debugging scripts inside the ```debug``` folder. The motors (```motor_check.py```), the load cell (```weight_sensor_check.py```), AI (```check_ai.py```), and the camera (```take_photo.py```), all have their own debugging scripts. Be sure to specify the arguments when running the programs on the command line. 
Then, you can simply modify the parameters in ```main_main_flow.sh``` for the motors' and the weight sensor's GPIO pins and also the path to your project folder, and run  this command line and there you have yourself your own machine!! 
```sh
$ bash main_main_flow.sh
```
What's left to do is to use your machine in your local subway, school, supermarket... Save your environment! Promote recycling... for a better future!!
