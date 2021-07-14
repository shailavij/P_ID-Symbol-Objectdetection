# Process-Symbol-detection

A Piping and Instrumentation Diagram (P&ID) is a type of engineering diagram that uses symbols, text, and lines to represent the components and ﬂow of an industrial process. Although used universally across industries such as manufacturing and oil & gas, P&IDs are usually trapped in image ﬁles with limited metadata, making their contents unsearchable and siloed from operational or enterprise systems. In this project I have used computer vision technique to detect symbols in this diagram.

I have trained Object detection model by retraining final layer of SSD MobileNet v2 320x320 pretrained model.

**P&ID diagram**

![image](https://user-images.githubusercontent.com/49098763/125462171-8cec37ff-33ae-4041-8775-815b51075b66.png)

To detect dynamic symbol like Pump, motorised valve, control valve using Object detection technique

**Pump**

![image](https://user-images.githubusercontent.com/49098763/125462394-583ab407-c6f5-4f6c-a4fa-e677bf970ce1.png)

**Motorised Valve**

![image](https://user-images.githubusercontent.com/49098763/125462458-5d03307b-416b-4bff-af4c-abcb3e2e5cdb.png)

**Control Valve**

![image](https://user-images.githubusercontent.com/49098763/125462480-d3b3a2e7-f121-403b-9235-d8700c34e349.png)

**Tensorboard Visaulisation**

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


