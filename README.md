# DeepLabCut-Ultrasound  
![1](https://github.com/user-attachments/assets/efbfdc63-50a8-4d36-9e76-ae6c50319f64)![2](https://github.com/user-attachments/assets/ae438dfb-cae2-4d0e-b55b-fec52bba5cc7)

## About DeepLabCut-Ultrasound 
This model is a DeepLabCut-based tool (a deep learning framework with a GUI for precise markerless pose estimation) designed to extract tongue contours from ultrasound videos automatically. It streamlines the analysis of tongue movements in speech research and is compatible with both Windows and macOS.

The extracted contour coordinates are output in CSV format, and videos with overlaid contour lines are generated for visual verification. The model has been trained on data from multiple ultrasound devices and speakers. For methodological details, refer to the accompanying [paper](https://doi.org/10.1250/ast.e24.128):  

## Citation  
When using this model in publications or research, please cite: 
> J. Sun, T. Kitamura, and R. Hayashi, "Extraction of Speech Organ Contours from Ultrasound and Real-time MRI Data using DeepLabCut," *Acoustical Science and Technology*, 46(4), 338-344 (2025).  
> [https://doi.org/10.1250/ast.e24.128](https://doi.org/10.1250/ast.e24.128)  

## Installing DeepLabCut  
### Windows Instructions  
1. **Miniconda**  
   - Download and install from: [https://docs.anaconda.com/miniconda/](https://docs.anaconda.com/miniconda/)  

2. **Nvidia Driver** (GPU users only)  
   - Install the driver compatible with your GPU (check via `Windows Key + R` → `dxdiag`):  
   [https://www.nvidia.com/Download/index.aspx?lang=en-us](https://www.nvidia.com/Download/index.aspx?lang=en-us)  

3. **CUDA Toolkit** (GPU users only)  
   - Download CUDA 11 for Windows:  
   [https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=11](https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=11)  

*Note*: Miniconda alone suffices if not using a GPU.  

4. Run these commands in **`Anaconda Prompt`**:  
```bash
conda create -n deeplabcut python=3.10 -y
conda activate deeplabcut
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --show channels
pip install deeplabcut[gui]==2.3.9
pip install tensorflow_gpu==2.10.0
conda install -c conda-forge cudatoolkit=11.8.0 cudnn=8.8.0 -y
```

**Start Up**:  
```bash
python -m deeplabcut
```  

**Troubleshooting**:  
If installation fails, rerun:  
```bash
conda activate deeplabcut
pip install "numpy<2"
python -m deeplabcut
```  

### macOS Instructions  
1. **Python 3.12**  
   - Install from: [https://www.python.org/downloads/macos/](https://www.python.org/downloads/macos/)  

2. **Miniconda**  
   - Install from: [https://docs.anaconda.com/miniconda/](https://docs.anaconda.com/miniconda/)  

3. Run these commands in **`Terminal`**:  
```bash
conda create -n deeplabcut python=3.10 -y
conda activate deeplabcut
conda install -c conda-forge opencv -y
conda install -c conda-forge tensorflow=2.10.0 -y
conda install -c conda-forge deeplabcut -y
pip install 'deeplabcut[gui]'  # Use quotes if errors occur
```  

**Start Up**:  
```bash
python -m deeplabcut
```  

## Analyzing Ultrasound Videos  
### Pre-trained Model  
**Download the DLC-Model**. Training specifications:  
- **Dataset**: 4,821 frames  
- **Devices**: GE Healthcare and AAA ultrasound systems  
- **Speakers**: Asian and Western participants  
- **Training Hardware**: MSI laptop (NVIDIA GeForce RTX 3060 GPU), trained for ~1.03 million iterations (10 hours).  

### Preparing your Data
- **Supported Formats**: MP4, AVI, MKV, MOV  
- **Resolution**: 320×240 pixels (required for accuracy)  
- **Orientation**: The tongue tip is on the right, and the root is on the left.
- **Demo videos** are provided in the `demo` folder.  

### Keypoints  
- The contour is defined by **11 unequally spaced keypoints** optimized for shape accuracy.  

### Start Guide  
1. **Start Up DeepLabCut**:  
   ```bash
   conda activate deeplabcut
   python -m deeplabcut
   ```  

2. **Load Project**:  
   - In the GUI, click **`Load Project`** and select the **`config.yaml`** file from the **`DLC-Model`** folder.  

3. **Analyze Videos**:  
   - Select videos via **`Analyze Videos`**.  
   - Verify tongue orientation (right: tongue tip, left: tongue root ).  
   - Enable:  
     ☑ **Save as CSV**  
     ☑ **Filter predictions**  
     ☑ **Plot trajectories**  
   - Click **`Analyze Videos`**.  
   - **Processing Speed**:  
     - GPU (e.g., RTX 3060): ~200 fps  
     - CPU (e.g., M1/M2): ~10–20 fps  

4. **Generate Contour Videos**:  
   - Click **`Create Videos`**.  
   - Enable:  
     ☑ **Plot all bodyparts**  
     ☑ **Draw skeleton**  
     ☑ **Use filtered data**  
     ☑ **Plot trajectories**  
   - Click **`Create Videos`**.  

5. **Review Outputs**:  
   - Inspect overlaid contours in the generated videos.  
   - Use the **`*_filtered.csv`** files for analysis.  

## Disclaimer  
This software is provided **as-is** with no warranties. Note the following:  
- Contour accuracy should be validated using the output videos.
- Compatibility may be affected by future DeepLabCut updates.
- Bug fixes and support are not guaranteed; however, reports are welcome (see contact information below).
- The authors and affiliated institutions are not liable for any outcomes resulting from the use of this tool.

We hope this tool advances your research! 

## Support or Contact  
Please contact Jing Sun (jsunsang901126[at]gmail.com), Tatsuya Kitamura (t-kitamu[at]konan-u.ac.jp) and Ryoko Hayashi (rhayashi[at]kobe-u.ac.jp) for support.
