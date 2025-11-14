PlaTiF Dataset: Code Usage Guide 💻🦴

Welcome to the PlaTiF (Tibial Plateau Fracture) Dataset repository\! 🎉 This guide will help you navigate and utilize the MATLAB and Python scripts we've provided to explore the fascinating world of tibial plateau fracture images and their segmentation masks.

🗺️ Your PlaTiF Journey: A Quick Roadmap

1.  **Clone This Repository**: Get all the goodies onto your machine\! 📥
2.  **Download the Dataset:** Grab the actual patient data ([Access Link to PlaTiF Dataset](https://www.kaggle.com/datasets/alikazemi8/bonetibia-plateau-fracure-schatzker-classification)). 💾
3.  **Choose Your Path:** Pythonista or MATLAB guru? Pick your preferred language\! 🐍📊
4.  **Install Dependencies:** A few quick installs to get everything running smoothly. 🛠️
5.  **Run & Visualize:** Read and Plot X-ray Dataset\! 🖼️

## 🚧 General Setup: Getting Started is Easy\!

First things first, let's get your environment ready.

### Step 1: Clone the Repository 📥

Open your terminal or command prompt and run this command:

```bash
git clone https://github.com/ali-kazemi8/PlaTiF-Tibial-Plateau-Fracture-Dataset.git
cd PlaTiF-Tibial-Plateau-Fracture-Dataset
```

Voila\! You now have all the scripts locally. ✨

### Step 2: Download the PlaTiF Dataset 💾

Our scripts need data to shine\!

  * Make sure you have downloaded the actual PlaTiF dataset.
  * Keep the `.mat` files in a readily accessible location.
  * **Important:** You'll need to point our scripts to the specific patient `.mat` files you want to visualize.

### Data Structure at a Glance 🧩

Each `.mat` file is a treasure trove of information, organized neatly into a structured variable (e.g., `patientData`) containing:

  * `image`: The 3D CT scan data – the raw beauty\! 📸
  * `segmentationMask`: The 3D mask highlighting the fracture – our focus\! 🎯
  * `patientInfo`: Essential metadata about the patient (ID, age, fracture type, etc.). 🧑‍⚕️

-----

## 🐍 Python Power-Up: For the Python Enthusiasts\!

Our Python scripts are located in the `Python/` directory. Get ready to `import` and explore\!

### Step 1: Set Up Your Python Playground 🏞️

We highly recommend using a virtual environment to keep your project dependencies clean and tidy.

```bash
# Navigate into the Python directory
cd Python

# Create a virtual environment (if you don't have one already)
python -m venv venv

# Activate your new environment!
# On macOS/Linux:
source venv/bin/activate
# On Windows:
.\venv\Scripts\activate
```

You'll see `(venv)` appear in your terminal prompt, indicating your environment is active\! 🎉

### Step 2: Install Essential Libraries 📚

We've made it super easy\! We've provided a `requirements.txt` file with everything you need.

```
# Python/requirements.txt
numpy
scipy
matplotlib
```

With your virtual environment active, simply run:

```bash
pip install -r requirements.txt
```

And just like that, you're ready\! ✅

### Step 3: Run Our Sample Visualization Script\! 🚀

We've prepared a friendly script, `visualize_data.py`, to help you visualize a patient's CT scan and fracture mask.

*(If you haven't already, create `Python/visualize_data.py` and paste the following content into it.)*

```python
# Python/visualize_data.py

import argparse
import scipy.io
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
import numpy as np

def load_data(file_path):
    """Loads and returns the .mat file data."""
    print(f"Loading data from {file_path}... 📂")
    try:
        data = scipy.io.loadmat(file_path)
        # Assuming the data is stored in a struct named 'patientData'
        patient_data = data['patientData'][0, 0]
        image = patient_data['image']
        mask = patient_data['segmentationMask']
        print("Data loaded successfully! 🎉")
        return image, mask
    except Exception as e:
        print(f"Oops! Error loading .mat file: {e} 💔")
        return None, None

def visualize_3d(image, mask, slice_index=None):
    """
    Visualizes a 2D slice or a 3D scatter plot of the data.
    """
    if slice_index is not None and slice_index < image.shape[2]:
        # Show a 2D slice
        print(f"Displaying 2D slice {slice_index}... 🖼️")
        plt.figure(figsize=(12, 6))
        
        plt.subplot(1, 2, 1)
        plt.imshow(image[:, :, slice_index], cmap='gray')
        plt.title(f'CT Image (Slice {slice_index}) 🦴')
        plt.axis('off')
        
        plt.subplot(1, 2, 2)
        plt.imshow(image[:, :, slice_index], cmap='gray')
        # Overlay the mask with transparency for a clear view!
        mask_overlay = np.ma.masked_where(mask[:, :, slice_index] == 0, mask[:, :, slice_index])
        plt.imshow(mask_overlay, cmap='jet', alpha=0.5)
        plt.title(f'Image with Fracture Mask (Slice {slice_index}) ✨')
        plt.axis('off')
        
        plt.tight_layout()
        plt.show()
    else:
        # Show a 3D plot of the mask (sub-sampled for smoother rendering!)
        print("Generating 3D mask visualization (sub-sampled for speed!)... 🌟")
        points = np.argwhere(mask > 0)
        sub_points = points[::100]  # Plot 1 in every 100 points for performance
        
        fig = plt.figure()
        ax = fig.add_subplot(111, projection='3d')
        ax.scatter(sub_points[:, 0], sub_points[:, 1], sub_points[:, 2], c='red', marker='o', s=1) # Smaller points for cleaner look
        
        ax.set_xlabel('X Axis')
        ax.set_ylabel('Y Axis')
        ax.set_zlabel('Z Axis')
        plt.title('3D Visualization of Fracture Mask 💫')
        plt.show()

def main():
    parser = argparse.ArgumentParser(description="Load and visualize PlaTiF dataset .mat files with style! ✨")
    parser.add_argument('--file', type=str, required=True, help="Path to your patient's .mat file. 📄")
    parser.add_argument('--slice', type=int, help="Optional: Specify a 2D slice index for detailed viewing! 🔍")
    
    args = parser.parse_args()
    
    image, mask = load_data(args.file)
    
    if image is not None and mask is not None:
        visualize_3d(image, mask, args.slice)

if __name__ == "__main__":
    main()
```

**How to Run It:**

Navigate to the `Python/` directory in your terminal (with your `venv` active) and try these commands:

  * **To see a specific 2D slice (e.g., slice 100):**
    ```bash
    python visualize_data.py --file /path/to/your/dataset/patient_001.mat --slice 100
    ```
    You'll see a beautiful side-by-side comparison\! 🖼️
  * **To explore the 3D fracture mask:**
    ```bash
    python visualize_data.py --file /path/to/your/dataset/patient_001.mat
    ```
    Prepare for an interactive 3D view of the fracture\! 🤩

-----

## 📊 MATLAB Magic: For the MATLAB Aficionados\!

Our MATLAB scripts reside in the `MATLAB/` directory. Let's get them running\!

### Step 1: MATLAB Environment Prep ⚙️

1.  **Open MATLAB:** Launch your MATLAB application.
2.  **Toolbox Check:** Ensure you have the **Image Processing Toolbox** installed. It's crucial for functions like `imshow` and `visboundaries` to display images and outlines beautifully. If not, you might need to install it via the Add-On Explorer in MATLAB. 📥

### Step 2: Unleash the Visualization Script\! ✨

We've crafted `visualize_data.m` to make visualizing your data a breeze.

*(If you haven't already, create `MATLAB/visualize_data.m` and paste the following content into it.)*

```matlab
% MATLAB/visualize_data.m

function visualize_data(filePath, sliceIndex)
    % visualize_data: Loads and beautifully visualizes a specific slice from a PlaTiF .mat file.
    %
    % Inputs:
    %   filePath (string): The full path to your patient's .mat file. 📄
    %   sliceIndex (int): The index of the 2D slice you want to admire. 🔍
    %
    % Example Usage:
    % visualize_data('C:\MyData\PlaTiF\patient_002.mat', 120);

    fprintf('--- Starting PlaTiF Visualization! ✨ ---\n');

    % --- Configuration (Defaults if not provided) ---
    if nargin < 1
        % Set a default file path for easy testing
        filePath = 'C:\path\to\your\dataset\patient_001.mat'; % <--- **UPDATE THIS PATH!**
        warning('No file path provided. Using default: %s (Please update for your data!)', filePath);
    end
    
    if nargin < 2
        % Set a default slice index
        sliceIndex = 100;
        disp(['No slice index provided. Using default: ' num2str(sliceIndex) ' 📏']);
    end

    % --- Load Data ---
    fprintf('Loading data from %s... 📂\n', filePath);
    try
        data = load(filePath);
        
        % Access the patient data struct
        % Adjust 'patientData' if your .mat file uses a different variable name
        patientData = data.patientData; 
        image = patientData.image;
        mask = patientData.segmentationMask;
        fprintf('Data loaded successfully! 🎉\n');
        
    catch ME
        fprintf('Oops! Error loading .mat file: %s 💔\n', ME.message);
        return;
    end

    % --- Validate Slice Index ---
    if sliceIndex > size(image, 3) || sliceIndex < 1
        fprintf('Error: The chosen sliceIndex (%d) is out of bounds (max: %d). Please pick a valid slice! 🛑\n', sliceIndex, size(image, 3));
        return;
    end

    fprintf('Preparing to visualize slice %d... 🖼️\n', sliceIndex);
    
    % Extract the specific 2D slice
    img_slice = image(:, :, sliceIndex);
    mask_slice = mask(:, :, sliceIndex);

    % --- Create a Stunning Figure! ---
    figure('Name', ['PlaTiF Visualization - Slice ' num2str(sliceIndex)], 'NumberTitle', 'off');
    
    % Display the CT image in grayscale
    subplot(1, 2, 1);
    imshow(img_slice, []); % [] scales intensity values appropriately
    title(['CT Image (Slice ' num2str(sliceIndex) ') 🦴'], 'FontSize', 14);
    colormap gray; % Ensure grayscale colormap

    % Display the image with a vibrant mask overlay!
    subplot(1, 2, 2);
    imshow(img_slice, []);
    hold on;
    % Use visboundaries to elegantly outline the fracture mask
    boundaries = bwboundaries(mask_slice);
    if ~isempty(boundaries)
        visboundaries(boundaries, 'Color', 'red', 'LineWidth', 1.5); % A bold red for clarity!
    end
    hold off;
    title(['Image with Fracture Mask (Slice ' num2str(sliceIndex) ') ✨'], 'FontSize', 14);
    colormap gray; % Ensure grayscale for the base image

    fprintf('Visualization complete! Enjoy the view! 😊\n');
end
```

**How to Run It:**

1.  In MATLAB, navigate to the `MATLAB/` directory in the Current Folder browser.

2.  In the MATLAB Command Window, call the function with your file path and desired slice:

    ```matlab
    % First, define the path to one of your downloaded .mat files!
    myPatientFile = 'C:\Your\Path\To\PlaTiF_Dataset\patient_001.mat'; % <--- **IMPORTANT: CHANGE THIS PATH!**

    % Then, choose a slice number!
    mySliceNumber = 125; 

    % Now, let's visualize!
    visualize_data(myPatientFile, mySliceNumber);
    ```

    You'll see a beautiful figure pop up with your CT image and the fracture mask\! 🤩

-----

## 💡 Explore and Innovate\!

This code is just a starting point\! Feel free to:

  * **Modify the Visualization:** Experiment with different colormaps, transparency levels, or 3D rendering techniques. 🎨
  * **Batch Processing:** Adapt the scripts to process multiple patient files automatically. 🔄
  * **Feature Extraction:** Use the masks to extract quantitative features for your research. 🔬
  * **Integrate into your workflow:** Incorporate these loading functions into your own machine learning pipelines\! 🤖
