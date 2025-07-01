# DeepLabCut-Ultrasound

![1](https://github.com/user-attachments/assets/efbfdc63-50a8-4d36-9e76-ae6c50319f64)![2](https://github.com/user-attachments/assets/ae438dfb-cae2-4d0e-b55b-fec52bba5cc7)


## 本モデルの概要
- 本モデルは、DeepLabCut（深層学習を用いて、正確なトラッキングをGUIで可能にするツール）を用いて超音波動画から舌の輪郭を自動抽出するツールです。音声研究における舌の動きの分析を効率化するために開発され、WindowsおよびMacで動作します。
- 抽出された輪郭座標はCSV形式で出力されます。抽出された輪郭を視覚的に確認するための輪郭線付き動画も生成されます。
- 本モデルは、多様な超音波装置や話者データを用いて学習されています。詳細な手法については、[こちら](https://doi.org/10.1250/ast.e24.128)の論文を参照してください。

## 引用
論文や研究でこのモデルを使う場合、以下の論文を引用してください：
> J. Sun, T. Kitamura, and R. Hayashi, "Extraction of Speech Organ Contours from Ultrasound and real-time MRI Data using DeepLabCut", _Acoustical Science and Technology_, 46(4), 338-344 (2025).  
> https://doi.org/10.1250/ast.e24.128

## DeepLabCutのインストール
### Windowsでのインストール手順
1. **Miniconda**  
   - インストール：https://docs.anaconda.com/miniconda/
   - サイトからダウンロードし，画面の指示に従ってインストールする．

2. **Nvidiaドライバー**（GPUを使う場合のみ）
   - GPUを使うなら，Nvidiaのドライバーをインストールしてください。
   - インストール：https://www.nvidia.com/Download/index.aspx?lang=en-us
   - 自分のGPUに合うドライバーを選んでください（「Windowsキー＋R」を押して「**dxdiag**」と入力して、確認してください）。

3. **CUDA Toolkit**（GPUを使う場合のみ）
   - GPUで高速処理するためのツール。
   - インストール：https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local
   - Windowsならバージョン11を選んでインストールしてください．

*注*: GPUがない場合は，MinicondaだけでOK！


4. **`Anaconda Prompt`** で以下のコマンドを順に実行します。

```bash
conda create -n deeplabcut python=3.10 -y
conda activate deeplabcut
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --show channels
pip install deeplabcut[gui]==2.3.9
pip install tensorflow_gpu==2.10.0
conda install -c conda-forge cudatoolkit=11.8.0 cudnn=8.8.0 -y
```
*注*: GPUがない場合、以下の2行のコマンドは不要です。
```bash
pip install tensorflow_gpu==2.10.0
conda install -c conda-forge cudatoolkit=11.8.0 cudnn=8.8.0 -y
```
**起動**
```bash
python -m deeplabcut
```

**インストール失敗時の対応**:
インストールが失敗した場合、以下のコマンドを再実行してください：

```bash
conda activate deeplabcut
pip install "numpy<2"
python -m deeplabcut
```

### Macでのインストール手順
1. **Python 3.12**  
   - インストール：https://www.python.org/downloads/macos/

2. **Miniconda**  
   - インストール：https://docs.anaconda.com/miniconda/

3. **`ターミナル`** で以下のコマンドを順に実行します。

```bash
conda create -n deeplabcut python=3.10 -y
conda activate deeplabcut
conda install -c conda-forge opencv -y
conda install -c conda-forge tensorflow=2.10.0 -y
conda install -c conda-forge deeplabcut -y
pip install deeplabcut[gui]
```

*注*: 上記の `pip install deeplabcut[gui]` が失敗した場合、以下を試してください：

```bash
pip install 'deeplabcut[gui]'
```

**起動**:

```bash
python -m deeplabcut
```

## 超音波動画の解析方法について
### トレーニング済みモデルのダウンロード

DLC-Modelをダウンロードしてください。モデルの学習詳細：
- **データ量**: 4,821フレーム
- **使用機器**: GE HealthcareおよびAAA超音波装置
- **話者**: アジア系および欧米系の話者
- **学習環境**: MSIノートPC（NVIDIA GeForce RTX 3060 GPU使用）、約10時間で103万回学習。

### データの準備

- **動画形式**: **MP4**、AVI、MKV、MOV
- **動画サイズ**: 320x240ピクセル（正確な輪郭抽出に必須）
- **舌の向き**: 動画内で舌先が右側、舌根が左側。

**デモ用のサンプル超音波動画をdemoから入手できます**。

### 舌輪郭のキーポイントについて

- 舌の輪郭は**11個のキーポイント**で抽出します。
- キーポイントは等間隔ではなく、舌の形状を効率的かつ正確に捉えるよう配置しています。

### 本モデルの使い方（動画による解説は下の画像をクリックしてください）

[![舌輪郭抽出の解説動画](https://github.com/user-attachments/assets/e0b53433-387e-4873-afe7-2fe1a3bc3a5e)](https://www.youtube.com/watch?v=4pZpJK13p2I)

1. **DeepLabCutの起動**:

   ```bash
   conda activate deeplabcut
   python -m deeplabcut
   ```

2. **プロジェクトの読み込み**:
   - GUIで **`Load Project`** をクリックし、**`DLC-Model`** フォルダ内の **`config.yaml`** を選択。

3. **動画の解析（輪郭抽出）**:
   - **`Analyze videos`** で動画を選択。
   - 舌先が右、舌根が左になるように動画を準備。
   - チェックボックス：☑ **Save result(s) as** CSV, ☑ **Filter predictions**, ☑ **Plot trajectories**。
   - 画面の右下の **`Analyze videos`** をクリック。
   - **処理速度**:
     - Windows GPU（例：NVIDIA RTX 3060）: 約200フレーム/秒
     - Windows/Mac CPU（例：M1/M2）: 約10～20フレーム/秒

4. **輪郭線付き動画の作成**:
   - **`Create videos`** をクリック。
   - チェックボックス：☑ **Plot all bodyparts**, ☑ **Draw skeleton**, ☑ **Use filtered data**, ☑ **Plot trajectories**。
   - 画面の右下の **`Create videos`** をクリック。

5. **結果の確認**:
   - 輪郭線付き動画で舌の形状を確認（分析結果は元の動画と同じフォルダに保存されます）。
   - **`*_filtered.csv`** ファイルを使用して分析。


## 免責事項

本ソフトウェアは現状のまま提供され、バグや不具合を含む可能性があります。使用に際し、以下の点をご了承ください：

- 出力結果は完璧ではなく、生成された動画で輪郭の正確性を確認する必要があります。
- DeepLabCutや依存関係のアップデートにより、本モデルが動作しなくなる可能性があります。
- 著者はバグ修正やサポートの義務を負いませんが、バグ報告は歓迎します（下記の連絡先参照）。
- 著者らおよび関連機関は、本モデルの使用（適切・不適切問わず）による結果に責任を負いません。

本モデルをご利用いただき、研究に役立つことを願っています！

## サポートおよび連絡先

サポートが必要な場合、以下に連絡してください：
- Jing Sun ([jsunsang901126@gmail.com](jsunsang901126@gmail.com))
- Tatsuya Kitamura ([t-kitamu@konan-u.ac.jp](t-kitamu@konan-u.ac.jp))
- Ryoko Hayashi ([rhayashi@kobe-u.ac.jp](rhayashi@kobe-u.ac.jp))

