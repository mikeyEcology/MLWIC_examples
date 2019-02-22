Set up your Windows computer to run `MLWIC` in R
================
Feb 22, 2019

<!-- MLWIC_Windows_Set_up.md is generated from MLWIC_Windows_Set_up.Rmd. Please edit that file -->
*This document is meant to be a companion resource to the README files in [`MLWIC`](https://github.com/mikeyEcology/MLWIC) and [`MLWIC_examples`](https://github.com/mikeyEcology/MLWIC_examples) specific to Windows users.* ***Please read the directions in those locations carefully before using this guide.***

<br><br>

### Install R and RStudio.

-   Ensure that you have the latest builds so that your issues aren't related to working with older versions.

### Follow along with the directions to install and use `MLWIC`.

-   Go to the [`MLWIC`](https://github.com/mikeyEcology/MLWIC) README file and the [`MLWIC_examples`](https://github.com/mikeyEcology/MLWIC_examples) README file, but **note the changes and additions in this document.**

### Install Anaconda.

-   While Anaconda is available from <https://www.anaconda.com/download/>, the latest version may not work with `MLWIC`.
-   Go to <https://repo.anaconda.com/archive/> and chose the latest version of Anaconda that works on 64-bit Windows, but uses **Python 3.6** - this is the version `MLWIC` uses. **Be VERY careful to choose the right one**; e.g., Anaconda3, not Anaconda2, etc.
-   Alternatively, clicking the link below will start the download.

> [Anaconda3-5.2.0-Windows-x86\_64.exe 631.3M 2018-05-30 13:04:18](https://repo.anaconda.com/archive/Anaconda3-5.2.0-Windows-x86_64.exe)

-   Install Anaconda for "just me". You should note the location of the install, e.g., `C:/Users/UserName/AppData/Local/Continuum/Anaconda3`. You will need to know this path when you use the `MLWIC` package in R, so record it somewhere.
-   Use the recommended settings for the rest of the installation; there is no need to install Microsoft VSCode.

### Install Tensorflow using Anaconda Navigator.

-   In your "Start" menu on the bottom left-hand side of your taskbar, scroll through the list of installed programs on your computer (or, alternatively, use the search bar in your task bar) and select "Anaconda Navigator" to open the program.
-   Click "Environments" tab on the left hand side of the window.
-   Ensure you are in the `base (root)` environment. If you have other environments and you don't know what they do, it may be safest to remove them at this point.
-   Use the search bar to search for the package `setuptools`.
-   Update the package `setuptools` (if necessary) by single-clicking on the small green checkbox beside the package and selecting "Mark for Update". Then, at the bottom of the window, click "Apply".
-   Install the package `tensorflow`. You can search for it using the search bar at the upper right corner of the Anaconda Navigator window. When it appears in the table, click the hollow box to select it; it will become a hollow green box with a downwards arrow. Click "Apply" in the bottom left part of the window.
-   Search for the package `cudnn`. Single-click the small green box with the checkmark inside it and choose "Mark for specific version installation". Choose version 6.0. This may prompt you to install tensorflow 1.11 instead of a later version. Accept this and click Apply.

### Download the L1 folder.

-   The link is available [here](https://drive.google.com/file/d/1dY-49drRrSotFMHOOPZXrTgl5gqozGVL/view?usp=sharing).
-   Download the zip file and extract it to your computer, in the location where you plan to process images.

    -   IMPORTANT: On some Windows computers, the default is to place the folder "inside" another folder. For example, when you right-click on the zip file and choose "Extract all", Windows might prompt you to save it somewhere like:

    > "C:\\Users\\UserName\\MyFiles\\CameraTrap\\L1".

    -   This will save the "L1" folder inside another "L1" folder and could cause issues. Extract the folder here instead (note: removed word "L1"):

    > "C:\\Users\\UserName\\MyFiles\\CameraTrap"

    -   To be safe, choose a location where the file path does NOT have spaces in it (e.g., not "OneDrive Documents"" or "John Files").
    -   Some users have reported issues running the scripts on a **network location**. Choose to work on folder that is **local to your machine.**

### Set up `MLWIC` in R.

#### Download the example data

-   Before starting to use `MLWIC` with your own data, you should test that everything is working as expected with some example data. If you choose not to do this, please read the directions below and apply them to your own data.
-   Example data can be downloaded from the [MLWIC\_examples](https://github.com/mikeyEcology/MLWIC_examples/tree/master) page. **Follow along carefully with the directions in that document.**
-   **Important: to download the data, do NOT download each file individually**. The `image_labels.csv` will not be formatted correctly. Choose the green "Clone or download" button and download the entire `MLWIC_examples.zip` folder to your computer.
-   Once unzipped, place the "images" folder in the same location as you did your "L1" folder from above.
-   **Rename the `image_labels.csv` file to `data_info.csv` and place it inside the "L1" folder.** The `MLWIC` function `classify` command attempts to find this file and copy it into the "L1" folder, but this has met with mixed success on Windows computers; so, it's best to name and locate the file as the `classify` command expects.

#### Install and load the `MLWIC` library.

``` r
devtools::install_github("mikeyEcology/MLWIC")
```

``` r
library(MLWIC)
```

#### Set up your environment to run `MLWIC` in R.

-   You should only need to run this command once on your computer, because this command just installs necessary packages to run `MLWIC`, and tells R where your version of Anaconda lives.
-   You should know where Anaconda is stored because you will have recorded during the step **"Install Anaconda"**, above.

``` r
setup(python_loc = "C:/Users/UserName/AppData/Local/Continuum/anaconda3/")
```

-   After running the `setup` function, your console will be updated with messages about installed packages and any errors the program encountered. **Some errors are acceptable**; for example, the following errors will likely not hinder your ability to run the `classify` or `train` commands:

``` r
# ...Many messages about packages installing...
# 
# Solving environment: ...working... failed
# UnsatisfiableError: The following specifications were found to be in conflict:
#   - argparse
#   - tensorflow
# 
# Use "conda search <package> --info" to see the dependencies for each package.
# 
# Error: Error 1 occurred installing packages into conda environment r-reticulate
```

#### Run the classify command.

-   Here, I provide some information about each argument in the `classify` command; then, provide an example of what the code might look.

    -   `path_prefix`
        -   This is the absolute path to the images containing wildlife to classify.
        -   No image should have more than one species, but there can be more than once individual of the same species.
        -   There should be no sub-directories in the "images" folder.
        -   All images should be 256 x 256 pixels.
            <br>
    -   `data_info`
        -   This is the location of the .csv containing information about the images. It has Unix linebreaks and no headers.
        -   The first column contains the file name of the .jpg image e.g., "CA-01\_0006501.jpg"
        -   The second column MAY contain the NUMBER corresponding to the species in the image, e.g., 25 - this can be used to check the accuracy of the classification.
        -   **This file should be named data\_info.csv and it should be located in the L1 folder.**
            <br>
    -   `model_dir`
        -   This is where you stored the L1 folder
            <br>
    -   `python_loc`
        -   The location of Python on your computer
        -   **You MUST have a slash ("/") at the end of the path!!!**
            <br>
    -   `save_predictions`
        -   model\_predictions.txt is the default; use it unless you have reason otherwise.

``` r
classify(
    path_prefix = "C:/Users/UserName/CameraTrap/images",  
    data_info = "C:/Users/UserName/CameraTrap/L1/data_info.csv", 
    model_dir = "C:/Users/UserName/CameraTrap", 
    python_loc = "C:/Users/UserName/AppData/Local/Continuum/anaconda3/", # remember to include the last slash
    save_predictions = "model_predictions.txt" 
)
```

#### Create readable output

-   This function takes as input the text file created above ("model\_predictions.txt") and creates a nicely formatted .csv for further work.

-   Here, I provide some information about each argument in the `make_output` command; then, provide an example of what the code might look.

    -   `output_location`
        -   Where you would like to store the output csv (path of a folder). <br>
    -   `output_name`
        -   The name you'd like to use for the output csv. <br>
    -   `model_dir`
        -   Where your L1 folder is stored <br>
    -   `saved_predictions`
        -   The same name that you used for save\_predictions in the `classify` function (if you didn't use default, you would need to change this).

``` r
make_output(
    output_location = "C:/Users/UserName/CameraTrap", 
    output_name = "example_results.csv", 
    model_dir = "C:/Users/UserName/CameraTrap", 
    saved_predictions = "model_predictions.txt"
)
```

<br> <br>

##### Contributors

[Nova-Scotia](https://github.com/Nova-Scotia), with help from [fabstp](https://github.com/fabstp) & [christinamaiello](https://github.com/christinamaiello)
