# MLWIC_examples: Using the [MLWIC](https://github.com/mikeyEcology/MLWIC) (Machine Learning for Wildlife Image Classification) R Package

<b> First follow steps [here](https://github.com/mikeyEcology/MLWIC/blob/master/README.md) to get your computer ready to run `MLWIC`. </b>\
For more details on the methods, you can see [our paper](https://www.biorxiv.org/content/early/2018/06/14/346809), but I'm assuming that you are already somewhat familiar with this and that you have already installed and setup `MLWIC`. 

Then download the example csv and folder of images from this repository ("image_labels.csv" and "images") to your computer and note the location. In this example, I have downloaded to my desktop. If you put the csv, the images folder, and the L1 folder in the same folder, you can avoid specifying paths. You just need to set your working directory to this folder before running functions and leave the path_prefix, data_info, and model_dir empty. In the examples below, I specify paths because this is a more efficient way to store data (the function will find files and folders anywhere on your computer if you specify their paths). 

<b>Use the model to classify the images in the folder using `classify`.</b> Here, I show code using the paths that are on my computer. Unless your username on your computer is mikeytabak, you will have a different path. Also note that if you are using windows your path would look something like `C:/Users/apmatabak/Desktop/...`.
```
classify(path_prefix = "/Users/mikeytabak/Desktop/images", # this is the absolute path to the images. 
         data_info = "/Users/mikeytabak/Desktop/image_labels.csv", # this is the location of the csv containing image information. It has Unix linebreaks and no headers.
         model_dir = "/Users/mikeytabak/Desktop", # assuming this is where you stored the L1 folder in Step 3 of the instructions: github.com/mikeyEcology/MLWIC/blob/master/README
         python_loc = "/anaconda2/bin/", # the location of Python on your computer. 
         save_predictions = "model_predictions.txt" # this is the default and you should use it unless you have reason otherwise.
         )
```         

The classification predictions would then be stored at `/Users/mikeytabak/Desktop/L1/model_predictions.txt`, but it is not the most user-friendly format. The `make_output` function produces a csv for easier use. 

```
make_output(output_location = "/Users/mikeytabak/Desktop", # the output csv will be stored on my dekstop
            output_name = "example_results.csv", # the name of the csv I want to create with my output
            model_dir = "/Users/mikeytabak/Desktop", # the location where I stored the L1 folder
            saved_predictions = "model_predictions.txt" # the same name that I used for save_predictions in the classify function (if I didn't use default, I would need to change this).
            )
```           
Based on this code, my csv output will be stored at "/Users/mikeytabak/Desktop/example_results.csv". When you look at this output csv, each row is a different image. The answer column contains the species ID that was provided to the model for the image ([species ID lookup table](https://github.com/mikeyEcology/MLWIC/blob/master/speciesID.csv)). guess1 is the top guess (what the model thinks is in the image). If `guess1 == answer`, the model was correct for that image. If this is not the case, the fileName column contains the exact path to the image, so you can inspect the image manually. guess2 is the next best guess, and so on. confidence1 is the model's confidence in guess1, confidence2 is the model's confidence in guess2, ... [Tabak et al](https://www.biorxiv.org/content/early/2018/06/14/346809) discuss the top guess vs. the top 5 guesses and how these can be used. \
The species ID in the `data_info` file will not affect how the model assigns images to species. If you'd like to verify this, try changing all of the values in this column to 0 (or any other number between 0-27) and run the model again (but remember that if you modify and save this file to save it with Unix linebreaks). 

<b>Example of training a machine learning model that classifies images.</b> I this exammple, I'll use the same images and data to train a model. There are fewer than 2,000 total images, so the model would perform very poorly, but I am presenting the code for illustration. If you are running on a CPU, this will take at least 30 minutes to run. It will take much less time on a GPU
```
train(path_prefix = "/Users/mikeytabak/Desktop/images", # this is the absolute path to the images. 
         data_info = "/Users/mikeytabak/Desktop/image_labels.csv", # this is the location of the csv containing image information. It has Unix linebreaks and no headers.
         model_dir = "/Users/mikeytabak/Desktop", # assuming this is where you stored the L1 folder in Step 3 of the instructions: github.com/mikeyEcology/MLWIC/blob/master/README
         python_loc = "/anaconda2/bin/", # the location of Python on your computer. 
         num_classes = 28, # this is the number of species from our model. When you train your own model, you will replace this with the number of species/groups of species in your dataset
         log_dir_train = "training_output" # this will be a folder that contains the trained model (call it whatever you want). You will specify this folder as the "log_dir" when you classify images using this trained model. For example, the log_dir for the model included in this package is called "USDA182"
         )
```
As mentioned above, you would not want to use this model for anything because it has only been trained on 2,000 images, but hopefully this illustrates the method. What you specified as your `log_dir_train` is what you will specify as your `log_dir` when using `classify`. As this example training is only for illustration, once the output in your R console starts to look like this:
```
Filling queue with 2000 images before starting to train. This may take some times.
2018-06-28 19:46:25.235301: I tensorflow/core/platform/cpu_feature_guard.cc:140] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX2 AVX512F FMA
2018-06-28 19:39:17.631641: epoch 55, step 0, loss = 2.74, Top-1 = 0.50 Top-5 = 0.83 (8.6 examples/sec; 1.394 sec/batch)
2018-06-28 19:39:37.900512: epoch 55, step 10, loss = 0.95, Top-1 = 0.75 Top-5 = 0.92 (65.7 examples/sec; 0.183 sec/batch)
2018-06-28 19:39:53.390196: epoch 56, step 0, loss = 5.56, Top-1 = 0.17 Top-5 = 0.75 (65.3 examples/sec; 0.184 sec/batch)
2018-06-28 19:40:11.718238: epoch 56, step 10, loss = 0.07, Top-1 = 1.00 Top-5 = 1.00 (65.4 examples/sec; 0.183 sec/batch)
2018-06-28 19:40:26.435674: epoch 57, step 0, loss = 0.05, Top-1 = 1.00 Top-5 = 1.00 (65.9 examples/sec; 0.182 sec/batch)
2018-06-28 19:40:44.369809: epoch 57, step 10, loss = 1.46, Top-1 = 0.67 Top-5 = 1.00 (68.0 examples/sec; 0.177 sec/batch)
2018-06-28 19:40:59.095571: epoch 58, step 0, loss = 0.70, Top-1 = 0.83 Top-5 = 1.00 (67.4 examples/sec; 0.178 sec/batch)
```
This confirms that you are successfully training a model. You can stop training with the example dataset and train a model using your images.
