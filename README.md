# Object-Detection-Covid-Mask

## Introduction
This project is focused on detecting masks in images using TensorFlow 2. It involves training a neural network to recognize and classify images where people are wearing masks.

## Installation

### Step 1: Set Up Your Environment
Before installing TensorFlow 2, it's recommended to set up a virtual environment to manage dependencies and avoid conflicts.

1. **Create a Virtual Environment**:
   ```bash
   python -m venv tf2_env
   ```
   
This command creates a virtual environment named tf2_env.

2. **Activate the Virtual Environment**:
* On Windows:
   ```bash
   .\tf2_env\Scripts\activate
   ```

* On macOS/Linux:
   ```bash
   source tf2_env/bin/activate
   ```

## Step 2: Install TensorFlow 2
Once your virtual environment is activated, you can install TensorFlow 2 using pip.

1. Install TensorFlow 2:
   ```bash
   pip install tensorflow
   ```

## Step 3: Verify the Installation
To verify that TensorFlow 2 is installed correctly, you can run a simple Python script.

1. Create a Python Script to Verify Installation:
   ```bash
   import tensorflow as tf
   print("TensorFlow version:", tf.__version__)
   ```

2. Run the Script:

Save the script as verify_tf.py and run it:
   ```bash
   python verify_tf.py
   ```

You should see the TensorFlow version printed to the console if the installation was successful.

## Next Steps 🔥

After setting up TensorFlow 2, you can proceed with the following steps to annotate your dataset:


I found this Github repository that has a annotation tool for images that facilitates making the xml files that we want for training. Annotation Tool:

[[Label Studio is a modern, multi-modal data annotation tool](https://github.com/bryanmax9/labelImg)]

How to Use the Annotation Tool:

Once installed. Put the folder you downloaded of the tool on you project folder. Than, you should run cmd inside the folder "labelImg-master".

run the following command:
   ```bash
   python .\labelImg.py
   ```



You will get something like this:
<img src='https://i.imgur.com/VtIX6Z2.png' title='Video Demo' width='' alt='Video Demo' />



There, you need to open the images folder. Click on "Open Dir" and select the folder of you images:
<img src='https://i.imgur.com/xKibZp0.png' title='Video Demo' width='' alt='Video Demo' />


Now, create rectangle boxes and save for each image on the bottom right list. Do what is done on the gif for every image in order to have an xml for each image:
![imageAnnotation](https://github.com/bryanmax9/Object-Detection-Covid-Mask/assets/69496341/6d9598e5-6068-4e4a-9be8-279f49bab0ab)


Than add this script on the folder of the images with their respective xmls. You will get an output file called "output.csv" that would later be used to train our model:
```bash
import os
import glob
import pandas as pd
import xml.etree.ElementTree as ET


def xml_to_csv(path):
    xml_list = []
    for xml_file in glob.glob(path + '/*.xml'):
        tree = ET.parse(xml_file)
        root = tree.getroot()
        for member in root.findall('object'):
            value = (root.find('filename').text,
                     int(root.find('size')[0].text),
                     int(root.find('size')[1].text),
                     member[0].text,
                     int(member[4][0].text),
                     int(member[4][1].text),
                     int(member[4][2].text),
                     int(member[4][3].text)
                     )
            xml_list.append(value)
    column_name = ['filename', 'width', 'height', 'class', 'xmin', 'ymin', 'xmax', 'ymax']
    xml_df = pd.DataFrame(xml_list, columns=column_name)
    return xml_df


def generate_csv_file(path_to_images, path_to_output_csv_file):
    
    xml_df = xml_to_csv(path_to_images)
    xml_df.to_csv(path_to_output_csv_file, index=None)



if __name__ == '__main__':
    # Get the directory where the script resides
    script_dir = os.path.dirname(os.path.abspath(__file__))

    # Set the paths to the current directory
    path_to_images = script_dir
    path_to_csv_file = os.path.join(script_dir, 'output.csv')

    generate_csv_file(path_to_images, path_to_csv_file)

```






Feel free to reach out if you have any questions or run into any issues. Happy coding!

   



