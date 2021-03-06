## Summary
This example demonstrates how to use BigDL to load a BigDL or [Torch](http://torch.ch/) model trained on [ImageNet](http://image-net.org/) data, and then apply the loaded model to classify the contents of a set of images in Spark ML pipeline.

## Preparation

To start with this example, you need prepare your model and dataset.

1. Prepare model.

    The Torch ResNet model used in this example can be found in [Resnet Torch Model](https://github.com/facebook/fb.resnet.torch/tree/master/pretrained).
    The BigDL Inception model used in this example can be trained with [BigDL Inception](https://github.com/intel-analytics/BigDL/tree/master/dl/src/main/scala/com/intel/analytics/bigdl/models/inception).
    You can choose one of them, and then put the trained model in $modelPath, and set corresponding $modelType（torch or bigdl）.
   
2. Prepare predict dataset

    Put your image data for prediction in the ./predict foler. Alternatively, you may also use imagenet-2012 validation dataset to run the example, which can be found from <http://image-net.org/download-images>. After you download the file (ILSVRC2012_img_val.tar), run the follow commands to prepare the data.
    
     ```bash
    mkdir predict
    tar -xvf ILSVRC2012_img_val.tar -C ./predict/
    ```
  
## Run this example

Command to run the example in Spark local mode:

```
    ./bigdl.sh 
    spark-submit --master local[*] --driver-memory 10g --executor-memory 20g --class com.intel.analytics.bigdl.example.imageclassification.ImagePredictor bigdl-0.1.0-SNAPSHOT-jar-with-dependencies.jar 
        --modelPath ./resnet-18.t7 --folder ./predict --modelType torch -c 4 -n 1 --batchSize 32
```


Command to run the example in Spark cluster mode:

```
    MASTER=xxx.xxx.xxx.xxx:xxxx
    ./bigdl.sh 
    spark-submit --master ${MASTER} --driver-memory 10g --executor-memory 20g --class com.intel.analytics.bigdl.example.imageclassification.ImagePredictor bigdl-0.1.0-SNAPSHOT-jar-with-dependencies.jar 
    --modelPath ./resnet-18.t7 --folder ./predict --modelType torch -c 8 -n 4 --batchSize 32
```

where 

* ```--modelPath``` is the path to the model file.
* ```--folder``` is the folder of predict images.
* ```--modelType``` is the type of model to load, it can be ```bigdl``` or ```torch```.
* ```-n``` is the number of executor (or containter) in the Spark cluster.
* ```-c``` is the number of physical cores on each executor.
* ```--showNum``` is the result number to show, default 100.
* ```--batchSize``` is the batch size to use when do the prediction, default 32.
