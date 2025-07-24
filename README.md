## Vechicle saftey system

This project in its current state can detect 4 diffrent classes trianed on this datatset: https://www.kaggle.com/datasets/andrewmvd/road-sign-detection. The end purpose of this project would be to assist drivers on the road to make it a safer place. In the future, it could warn a driver not to go at a stoplight if people are still crossing. Or, it could warn of cars in the blindspot of the driver. Currently it can use detectnet and draw bounding boxes around its 4 clases.

## Before running
<video width="630" height="300" src="https://github.com/user-attachments/assets/8262834a-45fa-4d79-ab2d-e893da27a526"></video>


## After running
<video width="630" height="300" src="https://github.com/user-attachments/assets/c6197dc8-cf41-4d75-af45-02afac6710fd"></video>

How it works:
This model runs using detectnet and was trained on 100 epochs

View a video explanation here 
https://youtu.be/D-FK56_lhzo

## Prerequisites

Nvidia jetson orin nano

Python 3.9+

Jetson containers

## How to run this project own your own nano

	Git clone https://github.com/Cole-Weems/Vechicle-Saftey-System.git

	NET=~/path_to_model/street_real_model

 	detectnet --model=path_to_model/street_real_model/ssd-mobilenet.onnx           --input-blob=input_0           --output-cvg=scores           --output-bbox=boxes           --labels=path_to_dataset/street_test/labels.txt light.mp4 light_output_self.mp4

## How you can do this project yourself

Guide to using your own dataset and training with detectnet (you will need a jetson orin nano I believe)

While looking for a dataset, ensure the annotations provided are in the .xml format

Download the dataset to your computer and unzip the dataset

Organize your dataset as follows(case and space sensitive):

Annotations

All xml files

labels.txt

All classes alphabetical order in a txt file

JPEGImages

All images, not in folders (if you have more then 6000 images, try to delete some to make the transfer to jetson easier)

ImageSets/Main:

trainval.txt (all images names)
val.txt (choose 10% of your overall images and leave their names here)
test.txt (choose 40% of your overall images and leave their names here)
train.txt (this should be the majority of your images, leave their names here)

Your dataset should now be prepared

Move the dataset to the data folder of detection on your jetson

The changed python files will be attached here, it will run 100 epochs but can be changed from within the file in the for loop:



Now in the terminal run the commands as follows (but read the commands too as there are things that will have to be replaced with your names)(Each command is seperrate):

	cd ~/jetson-inference/
	./docker/run.sh

	cd python/training/detection/ssd

	python3 train_ssd.py \
  	--dataset-type=voc \
  	--data data/signs_test \
  	--model-dir=models/sign_model
  	--num-workers=2
  	--batch-size=2
  



	python3 onnx_export.py --model-dir=models/name_of_model

	NET=~/jetson-inference/python/training/detection/ssd/models/name_of_model

	detectnet --model=jetson-inference/python/training/detection/ssd/models/name_of_model/ssd-mobilenet.onnx           --input-blob=input_0           --output-cvg=scores           --output-bbox=boxes           --labels=jetson-inference/python/training/detection/ssd/data/name_of_dataset/labels.txt input (photo or video) output (diffrent name, same file type)

Now you have a working detectnet model with your own dataset
