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
 
 ## How it works?
 The system works in 2 stages:
 <ol>
  <li>We traverse the entire P&ID sheet and process crops of size 150 x 150. (50& overlap of crops is considered) <br>
    The trained model provides 6 symbol classes 1 background class as inference. <br>
    We reject background classes and save only the regions which contain the symbols. Several overlapping bounding boxes are detected, which are processed to provide a single bounding box and inference.
  </li>
  
  <li>
    The regions containing symbols are passed through EAST text detection model (pretrained) to determine the orientation of the text i.e vertical or horizontal <br>.
    The vertical text is roatated 90° right and brought into a horizontal format. <br>
    These text regions are cropped and passes through the MMOCR mode (pretrained) for performing OCR. <br>
    Rubbish inferences are rejected and replaced with defaut values. 
  </li>
</ol>
