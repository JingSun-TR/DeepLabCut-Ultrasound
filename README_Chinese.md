# DeepLabCut-Ultrasound
# 自动提取超声波舌头轮廓的模型
![1](https://github.com/user-attachments/assets/efbfdc63-50a8-4d36-9e76-ae6c50319f64)![2](https://github.com/user-attachments/assets/ae438dfb-cae2-4d0e-b55b-fec52bba5cc7)

## 本模型简介
- 本模型利用DeepLabCut（一种通过深度学习实现精准追踪的图形界面工具），从超声波视频中自动提取舌头的轮廓，旨在提高语音研究中舌头运动分析的效率，适用于Windows和Mac系统。
- 提取的轮廓坐标以CSV格式输出，并且可以生成带有轮廓线的视频以便直观验证。
- 本模型使用多种超声波设备和不同说话者的数据进行训练。详细方法请参阅[此论文](https://doi.org/10.1250/ast.e24.128)。

## 引用
在论文或研究中使用本模型时，请引用以下文献：
> J. Sun, T. Kitamura, and R. Hayashi, "Extraction of Speech Organ Contours from Ultrasound and real-time MRI Data using DeepLabCut", _Acoustical Science and Technology_, 1-7(2025).  
> https://doi.org/10.1250/ast.e24.128

## DeepLabCut的安装
### 在Windows系统中的安装步骤
1. **Miniconda**
   - 安装：https://docs.anaconda.com/miniconda/
   - 从官网下载并按照提示安装。

2. **Nvidia驱动程序**（仅限使用显卡的情况）
   - 若使用显卡（GPU），请安装Nvidia驱动。
   - 安装：https://www.nvidia.com/Download/index.aspx?lang=en-us
   - 根据您的GPU型号选择合适的驱动（按“Windows键+R”，输入“**dxdiag**”查看）。

3. **CUDA Toolkit**（仅限使用GPU的情况）
   - 用于GPU加速处理的工具。
   - 安装：https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local
   - Windows用户请选择版本11进行安装。

*注*：若无GPU，仅需安装Miniconda即可。

4. 在**Anaconda Prompt**中依次执行以下命令：

```bash
conda create -n deeplabcut python=3.10 -y
conda activate deeplabcut
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --show channels
pip install deeplabcut[gui]==2.3.9
pip install tensorflow_gpu==2.10.0
conda install -c conda-forge cudatoolkit=11.8.0 cudnn=8.8.0 -y
```
*注*：若无GPU，以下两行命令无需执行：
```bash
pip install tensorflow_gpu==2.10.0
conda install -c conda-forge cudatoolkit=11.8.0 cudnn=8.8.0 -y
```
**启动**
```bash
python -m deeplabcut
```

**安装失败时的处理方法**

若安装失败，请重新执行以下命令：

```bash
conda activate deeplabcut
pip install "numpy<2"
python -m deeplabcut
```

### 在Mac系统中的安装步骤
1. **Python 3.12**
   - 安装：https://www.python.org/downloads/macos/

2. **Miniconda**
   - 安装：https://docs.anaconda.com/miniconda/

3. 在**终端**中依次执行以下命令：

```bash
conda create -n deeplabcut python=3.10 -y
conda activate deeplabcut
conda install -c conda-forge opencv -y
conda install -c conda-forge tensorflow=2.10.0 -y
conda install -c conda-forge deeplabcut -y
pip install deeplabcut[gui]
```

*注*：若 `pip install deeplabcut[gui]` 失败，请尝试以下命令：

```bash
pip install 'deeplabcut[gui]'
```

**启动**：

```bash
python -m deeplabcut
```

## 超声波视频的分析方法
### 预训练模型下载
请下载 **`DLC-Model`** 。模型训练详情：
- **数据量**：4,821帧
- **使用设备**：GE Healthcare及AAA超声波设备
- **说话者**：亚洲及欧美说话者
- **训练环境**：MSI笔记本（使用NVIDIA GeForce RTX 3060 GPU），约10小时完成103万次训练。

### 数据准备
- **视频格式**：**MP4**、AVI、MKV、MOV
- **视频尺寸**：320x240像素（精确轮廓提取的必要条件）
- **舌头方向**：视频中舌尖在右侧，舌根在左侧。

**可从`demo`获取用于测试的超声波视频样本**。

### 舌头轮廓的关键点
- 舌头轮廓通过**11个关键点**提取。
- 关键点无需等间距排列，旨在高效且准确捕捉舌头形状。

### 本模型使用方法（点击下方图片查看视频教程）

[![舌轮廓提取教程视频](https://github.com/user-attachments/assets/e0b53433-387e-4873-afe7-2fe1a3bc3a5e)](https://www.youtube.com/watch?v=4pZpJK13p2I)

1. **启动DeepLabCut**：

   ```bash
   conda activate deeplabcut
   python -m deeplabcut
   ```

2. **加载项目**：
   - 在GUI中点击 **`Load Project`**，选择 **`DLC-Model`** 文件夹中的 **`config.yaml`** 。

3. **视频分析（轮廓提取）**：
   - 点击 **`Analyze videos`** 选择视频。
   - 确保视频中舌尖在右侧，舌根在左侧。
   - 勾选：☑ **Save result(s) as** CSV, ☑ **Filter predictions**, ☑ **Plot trajectories**。
   - 点击界面右下角的 **`Analyze videos`** 。
   - **处理速度**：
     - Windows GPU（例如：NVIDIA RTX 3060）：约200帧/秒
     - Windows/Mac CPU（例如：M1/M2）：约10～20帧/秒

4. **生成带轮廓线的视频**：
   - 点击 **`Create videos`**。
   - 勾选：☑ **Plot all bodyparts**, ☑ **Draw skeleton**, ☑ **Use filtered data**, ☑ **Plot trajectories**。
   - 点击界面右下角的 **`Create videos`**。

5. **结果验证**：
   - 通过带轮廓线的视频确认舌头形状（分析结果保存在与原视频相同的文件夹中）。
   - 使用 ***_filtered.csv**文件进行分析。

## 免责声明
本软件按现状提供，可能包含错误或缺陷。使用时请注意以下事项：
- 输出结果可能不完美，需通过生成视频确认轮廓的准确性。
- DeepLabCut或其依赖项的更新可能导致本模型无法正常运行。
- 作者不承担修复错误或提供支持的义务，但欢迎错误报告（请联系下方联系方式）。
- 作者及相关机构对本模型的使用（无论是否适当）所导致的后果不承担任何责任。

希望本模型能为您的研究提供帮助！

## 支持与联系方式
如需支持，请联系：
- J. Sun ([jsunsang901126@gmail.com](mailto:jsunsang901126@gmail.com))
- T. Kitamura ([t-kitamu@konan-u.ac.jp](mailto:t-kitamu@konan-u.ac.jp))
