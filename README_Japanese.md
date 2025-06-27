# DeepLabCut-Ultrasound

**DeepLabCut-Ultrasound** は、DeepLabCut（深層学習を用いて、正確なトラッキングをGUIで可能にするツール）を用いて超音波動画から舌の輪郭を自動抽出するツールです。音声研究における舌の動きの分析を効率化するために開発され、WindowsおよびMacで動作します。抽出された輪郭座標はCSV形式で出力され、言語研究に活用できます。

## DeepLabCut-Ultrasound について

本ツールは、超音波動画から舌の輪郭を抽出し、座標データをCSV形式で保存します。2つのCSVファイルが出力されますが、分析にはファイル名が**filtered**で終わる方を使用してください。また、抽出された輪郭を視覚的に確認するための輪郭線付き動画も生成されます。

このモデルは、DeepLabCutを基盤として構築され、多様な超音波装置や話者データを用いて学習されています。詳細な手法については、以下の論文を参照してください：

> J. Sun, T. Kitamura, and R. Hayashi, "Extraction of Speech Organ Contours from Ultrasound and real-time MRI Data using DeepLabCut," *Acoustical Science and Technology*, 1-7 (2025).  
> [https://doi.org/10.1250/ast.e24.128](https://doi.org/10.1250/ast.e24.128)

### 引用と帰属

本ツールを研究で使用する場合、以下を引用してください：

```
@article{sun2025extraction,
  title={Extraction of Speech Organ Contours from Ultrasound and real-time MRI Data using DeepLabCut},
  author={Sun, J. and Kitamura, T. and Hayashi, R.},
  journal={Acoustical Science and Technology},
  pages={1--7},
  year={2025},
  doi={10.1250/ast.e24.128}
}
```

## インストール

以下の手順で、WindowsまたはMacにDeepLabCut-Ultrasoundをインストールしてください。


### Windowsでのインストール手順
1. **Miniconda**  
   - インストール：https://docs.anaconda.com/miniconda/
   - サイトからダウンロードし，画面の指示に従ってインストールする．

2. **Nvidiaドライバー**（GPUを使う場合のみ）
   - GPUを使うなら，Nvidiaのドライバーをインストールする．
   - インストール：https://www.nvidia.com/Download/index.aspx?lang=en-us
   - 自分のGPUに合うドライバーを選ぶ．

3. **CUDA Toolkit**（GPUを使う場合のみ）
   - GPUで高速処理するためのツール．
   - インストール：https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local
   - Windowsならバージョン11を選んでインストールする．

*注*: GPUがない場合は，MinicondaだけでOK！


Anaconda Promptで以下のコマンドを順に実行します。

```bash
conda create -n deeplabcut python=3.10 -y
conda activate deeplabcut
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --show channels
pip install deeplabcut[gui]==2.3.9
pip install tensorflow_gpu==2.10.0
conda install -c conda-forge cudatoolkit=11.8.0 cudnn=8.8.0 -y
python -m deeplabcut
```
*注*: GPUがない場合、この2行のコマンドは不要です。
```bash
pip install tensorflow_gpu==2.10.0
conda install -c conda-forge cudatoolkit=11.8.0 cudnn=8.8.0 -y
```

**インストール失敗時の対応**:
インストールが失敗した場合、以下のコマンドを再実行してください：

```bash
conda activate deeplabcut
pip install "numpy<2"
python -m deeplabcut
```

**DLCの起動**（必要に応じて）:
DLCの操作画面を閉じた後に、もう一度起動する場合、以下のコマンドを再実行してください：
```bash
conda activate deeplabcut
python -m deeplabcut
```

### Macでのインストール手順

Python 3.12をインストール![image](https://github.com/user-attachments/assets/22889b10-5ad3-4085-af4b-89771526ddfa)
https://www.python.org/downloads/macos/
![image](https://github.com/user-attachments/assets/860125de-7d4c-4332-8f3b-d280022e3493)


ターミナルで以下のコマンドを順に実行します。

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

**再起動**（必要に応じて）:

```bash
conda activate deeplabcut
python -m deeplabcut
```

### 注意事項

- **GPUの有無**: Macでは通常CPUを使用（M1/M2チップの場合、TensorFlowが最適化される）。WindowsでGPUを使用する場合、NVIDIAドライバーを別途インストール（[https://www.nvidia.com/Download/index.aspx](https://www.nvidia.com/Download/index.aspx)）。
- **動画サイズ**: 320x240ピクセルの超音波動画専用。
- **依存関係の互換性**: Python 3.10、TensorFlow 2.10.0、CUDA 11.8.0（Windowsのみ）、cuDNN 8.8.0（Windowsのみ）に従い、互換性を確保。

## データの準備

- **動画形式**: MP4、AVI、MKV、MOV
- **動画サイズ**: 320x240ピクセル（正確な輪郭抽出に必須）
- **舌の向き**: 動画内で舌先が右側、舌根が左側。

デモ用のサンプル超音波動画は、リポジトリの `demo` フォルダに含まれています。

## 使い方ガイド

### DeepLabCut-Ultrasound の使い始め

1. **DeepLabCutの起動**:

   ```bash
   conda activate deeplabcut
   python -m deeplabcut
   ```

2. **プロジェクトの読み込み**:
   - GUIで「Load Project」をクリックし、モデルフォルダ内の `config.yaml` を選択。

3. **動画の解析（輪郭抽出）**:
   - 「Analyze videos」で動画を選択。
   - 舌先が右、舌根が左になるように動画を準備。
   - チェックボックス：☑ Save result(s) as CSV, ☑ Filter predictions, ☑ Plot trajectories。
   - 「Analyze videos」をクリック。
   - **処理速度**:
     - Windows GPU（例：NVIDIA RTX 3060）: 約200フレーム/秒
     - Mac CPU（例：M1/M2）: 約10～20フレーム/秒

4. **輪郭線付き動画の作成**:
   - 「Create videos」をクリック。
   - チェックボックス：☑ Plot all bodyparts, ☑ Draw skeleton, ☑ Use filtered data, ☑ Plot trajectories。
   - 「Create videos」をクリック。

5. **結果の確認**:
   - 輪郭線付き動画で舌の形状を確認。
   - 「filtered」CSVファイルを使用して分析。

### 舌輪郭の抽出方法

- 舌の輪郭は**11個のポイント**で追跡。
- ポイントは等間隔ではなく、舌の形状を効率的かつ正確に捉えるよう配置。
- 11点で十分な精度を確保し、30点以上必要な従来手法に比べ、学習データ作成や計算時間を削減。

## トレーニング済みモデルのダウンロード

トレーニング済みモデルは [こちら](https://example.com/deeplabcut-ultrasound-model)（実際のリンクに置き換え）からダウンロード可能です。モデルの学習詳細：
- **データ量**: 4,821フレーム
- **使用機器**: GE HealthcareおよびTripleA超音波装置
- **話者**: アジア系および欧米系の話者
- **学習環境**: MSIノートPC（NVIDIA GeForce RTX 3060 GPU使用）、約10時間で103万回学習。

## 免責事項

本ソフトウェアは現状のまま提供され、バグや不具合を含む可能性があります。使用に際し、以下の点をご了承ください：

- 出力結果は完璧ではなく、生成された動画で輪郭の正確性を確認する必要があります。
- DeepLabCutや依存関係のアップデートにより、ソフトウェアが動作しなくなる可能性があります。
- 著者はバグ修正やサポートの義務を負いませんが、バグ報告は歓迎します（下記の連絡先参照）。
- 本ツールを使用、改変、または派生研究に利用する場合、著者を引用してください。
- ソフトウェアを再配布せず、最新版を公式リポジトリからダウンロードするよう案内してください。
- 著者および関連機関は、ソフトウェアの使用（適切・不適切問わず）による結果に責任を負いません。

DeepLabCut-Ultrasoundをご利用いただき、研究に役立つことを願っています！

## サポートおよび連絡先

サポートが必要な場合、以下に連絡してください：
- J. Sun ([email@example.com](mailto:email@example.com))
- T. Kitamura ([email@example.com](mailto:email@example.com))
- R. Hayashi ([email@example.com](mailto:email@example.com))

## 参考文献

- Mathis, A., et al. (2018). DeepLabCut: markerless pose estimation of user-defined body parts with deep learning. *Nature Neuroscience*, 21, 1281–1289. [https://doi.org/10.1038/s41593-018-0209-y](https://doi.org/10.1038/s41593-018-0209-y)

---

### 特徴と改訂ポイント
- **GitHub対応**: Markdownの標準的な構文（`#` 見出し、```bash コードブロック、`*` リスト、リンク）を採用し、GitHubでそのままレンダリング可能。
- **視覚的効果**: 太字（**）やリストで重要な部分を強調し、読みやすさを向上。
- **難解な用語の回避**: 「イテレーション」を「回」に置き換え、「クイックスタートガイド」を「使い方ガイド」に変更。
- **一貫性**: MTrackerの構造（概要、インストール、データ準備、使い方、免責事項など）を踏襲し、内容を簡潔に。
- **仮の情報**: リポジトリURLや連絡先メールは仮のまま（実際の情報が提供されれば更新可能）。

このREADME.mdをGitHubリポジトリに直接アップロードすれば、適切に表示されます。追加の修正や具体的なURL/連絡先があれば教えてください！
