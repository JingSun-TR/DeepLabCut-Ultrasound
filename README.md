# DeepLabCut-Ultrasound

## Overview of This Model
- This model is a tool that uses DeepLabCut (a deep learning-based tool for accurate tracking with a GUI) to automatically extract tongue contours from ultrasound videos. It was developed to streamline the analysis of tongue movements in speech research and is compatible with both Windows and Mac.
- Extracted contour coordinates are output in CSV format. Videos with overlaid contour lines are also generated for visual confirmation of the extracted contours.
- The model has been trained using data from various ultrasound devices and speakers. For detailed methodology, refer to the following paper:  
  [https://doi.org/10.1250/ast.e24.128](https://doi.org/10.1250/ast.e24.128)

## Citation
When using this model in papers or research, please cite the following:  
> J. Sun, T. Kitamura, and R. Hayashi, "Extraction of Speech Organ Contours from Ultrasound and real-time MRI Data using DeepLabCut," _Acoustical Science and Technology_, 1-7 (2025).  
> [https://doi.org/10.1250/ast.e24.128](https://doi.org/10.1250/ast.e24.128)

## DeepLabCut Installation
### Installation Instructions for Windows
1. **Miniconda**  
   - Install: [https://docs.anaconda.com/miniconda/](https://docs.anaconda.com/miniconda/)  
   - Download from the website and follow the on-screen instructions.

2. **Nvidia Driver** (only if using a GPU)  
   - If using a GPU, install the Nvidia driver.  
   - Install: [https://www.nvidia.com/Download/index.aspx?lang=en-us](https://www.nvidia.com/Download/index.aspx?lang=en-us)  
   - Select the driver compatible with your GPU (check by pressing “Windows key + R” and typing “**dxdiag**”).

3. **CUDA Toolkit** (only if using a GPU)  
   - A tool for high-speed processing with a GPU.  
   - Install: [https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local](https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local)  
   - Select version 11 for Windows and install.

*Note*: If you don’t have a GPU, Miniconda alone is sufficient!

4. Run the following commands in **Anaconda Prompt** in sequence:

```bash
conda create -n deeplabcut python=3.10 -y
conda activate deeplabcut
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --show channels
pip install deeplabcut[gui]==2.3.9
pip install tensorflow_gpu==2.10.0
conda install -c conda-forge cudatoolkit=11.8.0 cudnn=8.8.0 -y
```

*Note*: If you don’t have a GPU, the following two commands are unnecessary:
```bash
pip install tensorflow_gpu==2.10.0
conda install -c conda-forge cudatoolkit=11.8.0 cudnn=8.8.0 -y
```

**Launch**:
```bash
python -m deeplabcut
```

**If Installation Fails**:  
If the installation fails, rerun the following commands:

```bash
conda activate deeplabcut
pip install "numpy<2"
python -m deeplabcut
```

### Installation Instructions for Mac
1. **Python 3.12**  
   - Install: [https://www.python.org/downloads/macos/](https://www.python.org/downloads/macos/)

2. **Miniconda**  
   - Install: [https://docs.anaconda.com/miniconda/](https://docs.anaconda.com/miniconda/)

3. Run the following commands in **Terminal** in sequence:

```bash
conda create -n deeplabcut python=3.10 -y
conda activate deeplabcut
conda install -c conda-forge opencv -y
conda install -c conda-forge tensorflow=2.10.0 -y
conda install -c conda-forge deeplabcut -y
pip install deeplabcut[gui]
```

*Note*: If `pip install deeplabcut[gui]` fails, try the following:

```bash
pip install 'deeplabcut[gui]'
```

**Launch**:
```bash
python -m deeplabcut
```

## Analyzing Ultrasound Videos
### Downloading the Pre-trained Model
Download the DLC-Model. Training details:  
- **Data Volume**: 4,821 frames  
- **Devices Used**: GE Healthcare and AAA ultrasound devices  
- **Speakers**: Asian and Western speakers  
- **Training Environment**: MSI laptop (using NVIDIA GeForce RTX 3060 GPU), trained for approximately 1.03 million iterations over 10 hours.

### Data Preparation
- **Video Formats**: MP4, AVI, MKV, MOV  
- **Video Size**: 320x240 pixels (required for accurate contour extraction)  
- **Tongue Orientation**: Tongue tip on the right, tongue root on the left in the video.  

**Sample ultrasound videos for demonstration are available in the demo folder.**

### Tongue Contour Keypoints
- The tongue contour is extracted using **11 keypoints**.  
- Keypoints are not equally spaced but are arranged to efficiently and accurately capture the tongue's shape.

### How to Use This Model (Click the image below for a video tutorial)

[![Tongue Contour Extraction Tutorial Video](https://github.com/user-attachments/assets/e0b53433-387e-4873-afe7-2fe1a3bc3a5e)](https://www.youtube.com/watch?v=4pZpJK13p2I)

1. **Launch DeepLabCut**:
   ```bash
   conda activate deeplabcut
   python -m deeplabcut
   ```

2. **Load Project**:
   - In the GUI, click “**Load Project**” and select the **`config.yaml`** file from the DLC-Model folder.

3. **Analyze Videos (Contour Extraction)**:
   - Select videos via “**Analyze videos**”.  
   - Ensure the tongue tip is on the right and the tongue root is on the left.  
   - Check the following boxes: ☑ **Save result(s) as** CSV, ☑ **Filter predictions**, ☑ **Plot trajectories**.  
   - Click “**Analyze videos**” in the bottom right corner.  
   - **Processing Speed**:  
     - Windows GPU (e.g., NVIDIA RTX 3060): ~200 frames/second  
     - Windows/Mac CPU (e.g., M1/M2): ~10–20 frames/second  

4. **Create Videos with Contours**:
   - Click “**Create videos**”.  
   - Check the following boxes: ☑ **Plot all bodyparts**, ☑ **Draw skeleton**, ☑ **Use filtered data**, ☑ **Plot trajectories**.  
   - Click “**Create videos**” in the bottom right corner.

5. **Review Results**:
   - Check the tongue shape in the videos with overlaid contours (results are saved in the same folder as the original videos).  
   - Use the “**…filtered**” CSV file for analysis.

## Disclaimer
This software is provided as-is and may contain bugs or issues. Please note the following:  
- The output results are not perfect, and the accuracy of contours must be verified using the generated videos.  
- Updates to DeepLabCut or its dependencies may cause this model to stop functioning.  
- The authors are not obligated to fix bugs or provide support, but bug reports are welcome (see contact details below).  
- The authors and related institutions are not responsible for any outcomes resulting from the use (appropriate or otherwise) of this model.  

We hope this model proves useful for your research!

## Support and Contact
For support, please contact:  
- J. Sun ([jsunsang901126@gmail.com](mailto:jsunsang901126@gmail.com))  
- T. Kitamura ([t-kitamu@konan-u.ac.jp](mailto:t-kitamu@konan-u.ac.jp))
