# MLWIC_examples: Using the MLWIC (Machine Learning for Wildlife Image Classification) 

<b> First follow steps [here](https://github.com/mikeyEcology/MLWIC/blob/master/README.md) to get your computer ready to run `MLWIC`. </b>

Then download the example csv and folder (test_images and image_labels.csv) to your computer and note the location. In this example, I have downloaded to my desktop.

Then you can use the model to classify the images in the folder using `classify`.
```
classify(path_prefix = "/Users/mikeytabak/Desktop/test_images", # this is the absolute path to the images. 
         data_info = "/Users/mikeytabak/Desktop/image_labels.csv", # this is the location of the csv containing image information. It has Unix linebreaks and no headers.
         model_dir = "/Users/mikeytabak/Desktop", # assuming this is where you stored the L1 folder in Step 3 of the instructions: github.com/mikeyEcology/MLWIC/blob/master/README
         python_loc = "/anaconda2/bin/", # the location of Python 2.7 on your computer. 
         save_predictions = "model_predictions.txt" # this is the default and you should use it unless you have reason otherwise.
         )
```         

The classification predictions would then be stored at `/Users/mikeytabak/Desktop/L1/model_predictions.txt`. 
But it is not the most friendly format. If you want your output in a nice csv, you can the `make_output` function. 

```
make_output(output_location = "/Users/mikeytabak/Desktop", # the output csv will be stored on my dekstop
            output_name = "example_results.csv", # the name of the csv I want to create with my output
            model_dir = "/Users/mikeytabak/Desktop", # the location where I stored the L1 folder
            saved_predictions = "model_predictions.txt" # the same name that I used for save_predictions in the classify function (if I didn't use default).
            )
```           
