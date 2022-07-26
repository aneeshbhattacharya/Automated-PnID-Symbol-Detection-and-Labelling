# Automated-PnID-Symbol-Detection-and-Labelling 
## We have developed a system for detecting and labelling various P&ID symbols. <br>
The system is designed to: <br>
<ul>
  <li>Take a raw P&ID diagram sheet as input</li>
<li>Output the following:
  <ul>
    <li>A JPG image with bounding boxes drawn around each detected symbol with their object ID</li>
    <li>A PDF file containing the object ID, class ID, Component Name, Item Label and bounding box regions </li>
  </ul>
  </li>
 </ul>
 <img width="1132" alt="image" src="https://user-images.githubusercontent.com/68210639/180232710-c5eba1ff-3a54-490c-966b-fbb274209d9e.png">

## Tech Stack
PyTorch, MMOCR, EAST text detection, OpenCV, PIL, Numpy
 
 ## How it works?
 The system works in 2 stages:
 <ol> 
  <li>Custom object detection step:
    <ul>
    <li>We traverse the entire P&ID sheet and process crops of size 150 x 150. (50% overlap of crops is considered).</li>
    <li>Our custom trained model provides 6 symbol classes 1 background class as inference.</li>
    <li>We reject background classes and save only the regions which contain the symbols.</li> 
      <li>Several overlapping bounding boxes are detected, which are processed to provide a single bounding box and inference.</li>
    </ul>
  </li>
  
  <li>OCR and text processing step:
    <ul>
    <li>The regions containing symbols are passed through EAST text detection model (pretrained) to determine the orientation of the text i.e vertical or horizontal.</li>
    <li>The vertical text is roatated 90° right and brought into a horizontal format.</li>
    <li>The processed text regions are cropped and passes through the MMOCR mode (pretrained) for performing OCR.</li>
    <li>Rubbish inferences are rejected and replaced with defaut values.</li> 
    </ul>
  </li>
</ol>

## Setup
The solution has been tested on Ubuntu 20.04, Mac OS Montery, Windows 10 and 11 (Git Bash) and Google Colab.
### Conda Environment:
```
conda create -n PnID python=3.7.13
conda activate PnID
```
### Installing requirements:
```
git clone https://github.com/aneeshbhattacharya/Automated-PnID-Symbol-Detection-and-Labelling.git
cd Automated-PnID-Symbol-Detection-and-Labelling
sudo chmod +x build.sh
./build.sh
```
### Download EAST model:
```
 https://www.dropbox.com/s/r2ingd0l3zt8hxs/frozen_east_text_detection.tar.gz?dl=1
```
Put the file in the MMOCR directory
### Run the code:
```
cd main_driver
jupyter-notebook
```
Open Aneesh_Risav_P&ID_Detection_and_Labelling_System.ipynb

## Tweaking the P&ID symbol detection model
<ul>
    <li> Invert the images to create greyscale and normalized images (symbols are active pixels) and create a dataset (use invert function from Aneesh_Risav_P&ID_Detection_and_Labelling_System.ipynb) </li>
    <li> 2. Save different images of different classes as follows:<br><br>
|--main folder<br><br>
|----0 <- this is class ID<br>
|------1.jpg<br>
|------2.jpg<br>
|------3.jpg<br>
|------...<br><br>
|----1<br>
|------1.jpg<br>
|------2.jpg<br>
|------3.jpg<br>
|------...<br><br>
|----.<br><br>
|----.<br><br>
|----n <- For upto N classes 
  
  </li>
  
  <li>Place this folder with name 'dataset' in the same directory as FINAL_Inverted_PyTorch_Model_P&ID.ipynb</li>
  <li>Set the number of classes output required by the model as "n" (last softmax layer of model)</li>
  <li>Run FINAL_Inverted_PyTorch_Model_P&ID.ipynb</li>

</ul>

