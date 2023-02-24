# Process-Symbol-detection

A Piping and Instrumentation Diagram (P&ID) is a type of engineering diagram that uses symbols, text, and lines to represent the components and ﬂow of an industrial process. Although used universally across industries such as manufacturing and oil & gas, P&IDs are usually trapped in image ﬁles with limited metadata, making their contents unsearchable and siloed from operational or enterprise systems. In this project I have used computer vision technique to detect symbols in this diagram.

I have trained Object detection model by retraining final layer of SSD MobileNet v2 320x320 pretrained model.

**P&ID diagram**

![image](https://user-images.githubusercontent.com/49098763/125462171-8cec37ff-33ae-4041-8775-815b51075b66.png)

Optional Additional Software:
<ul>

<li> <a href = "https://github.com/tzutalin/labelImg">labelImg</a> for bounding box annotation on training and test data</li>

</ul>

<h2>Training Preparation</h2>

<h3>Conversion from PDF</h3>

For my use case, the images to be processed were 11 x 17 size pages in a PDF. I extracted and converted each page from the PDF into an invidual jpg file. Segmentation of the file was done using code similar to the segmentation block in <a href="https://github.com/siddiqaa/psvcounter/blob/master/DetectPSVs.py"> detection application script</a>.

<h3>Image Segmentation into Smaller Sizes</h3>

The Google SSD Inception V2 model is trained on images of size 299 by 299 so I had to segment the images into smaller sections that were 200 by 200. This size gave me a good ratio of segment image dimensions to target object bounding box dimension. In other words, the bounding box for the object to be detected was not too small or too large compared to the overall segment image dimensions. To handle situations where the target object to be detected might be split between two different segments, I overlapped the segments by 20 pixels on each side.

<h3>Training Data Annotation</h3>

After saving the image segments, I used <a href="https://github.com/tzutalin/labelImg">labelImg</a> to draw bounding boxes.

<h3>Conversion of Annotation from XML to Text</h3>

lableImg saves the bounding box annotation data in COCO format as XML. I then used a modified version of the <a href="https://github.com/Guanghan/darknet/blob/master/scripts/voc_label.py">conversion script</a> from <a href="https://github.com/Guanghan">Guanghan</a> to convert the individual annotation file for each trainiing image into a single CSV master file that had one line per training image containing the image file name and data on the class(es) and bounding box(es). The <a href="https://github.com/siddiqaa/psvcounter/tree/master/data">data folder</a> in my repo contains the image files, the xml annotation from labelIMG and the combined csv text file describing annotations for the all the images used for training and testing. The script for combined and convert xml annotation is <a href="https://github.com/siddiqaa/psvcounter/blob/master/data/xml_to_csv.py">xml_to_csv.py</a>. It should be run in the same directory as where all the xml files from labelIMG are stored. Line 31 should be modified to change the file name for the csv file for your own project.

<h3>Manual Split of Annotation Text Files into Train and Test</h3>

After the python script generated the csv file, I then used a spreadsheet to split the records into two files - "train_labels.csv" and "test_labels.csv". It was pure cut and paste operation. No data was edited in generating the two files. I aimed for about 10% of the records for testing. There is probably a way to automate this in Tensorflow to have random splits for each training step but I did not implement this for my project. Modifying the code in this example for random split of test and train would deliver better results and reduce the over-fitting. In this case, over-fitting and a high number of false positives was not a concern in the production and actual use environment.

<h3>Creation of label map file</h3>

A simple json file is also needed to tell the class names for the bounding boxes in the training data. So I created the <a href="https://github.com/siddiqaa/psvcounter/blob/master/data/label_map.pbtxt">label_map.pbtxt</a> file containing the following text.<br>
```
{
  id: 1
  name: 'psv'
}
```
The id for the first object class in the map must start at 1 and not 0

<h3>Dynamic symbols</h3>
To detect dynamic symbol like Pump, motorised valve, control valve using Object detection technique

**Pump**

![image](https://user-images.githubusercontent.com/49098763/125462394-583ab407-c6f5-4f6c-a4fa-e677bf970ce1.png)

**Motorised Valve**

![image](https://user-images.githubusercontent.com/49098763/125462458-5d03307b-416b-4bff-af4c-abcb3e2e5cdb.png)

**Control Valve**

![image](https://user-images.githubusercontent.com/49098763/125462480-d3b3a2e7-f121-403b-9235-d8700c34e349.png)

**Tensorboard Visualisation**

![image](https://user-images.githubusercontent.com/49098763/125624978-6f0a0623-a599-4e54-b9d3-686e8c028fee.png)


**Model Evaluation**

![image](https://user-images.githubusercontent.com/49098763/125624844-269ec57d-ca29-4f58-b05d-e1779a36c033.png)

![image](https://user-images.githubusercontent.com/49098763/125625041-c9760c2d-8604-4c9b-8389-5b56f54582a1.png)

**Model Output**

![image](https://user-images.githubusercontent.com/49098763/125625133-dd542178-2d86-4b0f-abc1-d67b4495fa4a.png)




**References**
https://towardsdatascience.com/how-to-train-your-own-object-detector-with-tensorflows-object-detector-api-bec72ecfe1d9
https://tensorflow-object-detection-api-tutorial.readthedocs.io/en/latest/training.html
https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_detection_zoo.md


