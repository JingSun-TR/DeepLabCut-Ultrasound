# DeepLabCut-Ultrasound 
**DLCを用いた超音波舌輪郭抽出モデルの説明**

**モデル概要**  
1. このモデルは，DeepLabCut（DLC）というAIツールを用いて，超音波動画から舌の輪郭を自動抽出する
2. 抽出された舌の輪郭点（キーポイント）は座標としてCSVファイルに保存される
3. 保存されたCSVファイルは2つあるが，「folder」で終わるファイルを分析に使用する
4. これにより，キーフレームを選び，同一の図に輪郭線を重ねて分析できる
※ 抽出された輪郭データが使用可能かどうかは，まず保存された輪郭線付きの動画で確認する必要がある

**学術的引用**  
このモデルを学術研究で使用する場合は，以下を引用する：
J.Sun, T.Kitamura, and R.Hayashi, "Extraction of Speech Organ Contours from Ultrasound and real-time MRI Data using DeepLabCut", _Acoustical Science and Technology_,1-7(2025).  
https://doi.org/10.1250/ast.e24.128  

**環境設定**  
本モデルを使用する前に，以下のソフトウェアをインストールする：  
1. **Miniconda**  
   https://docs.anaconda.com/miniconda/ （GPUあり・なしでは必要）  
2. **Nvidiaドライバー**  
   https://www.nvidia.com/Download/index.aspx?lang=en-us （GPUなしでは不要）  
3. **CUDA Toolkit**  
   [https://developer.nvidia.com/cuda/downloads?target_os=Windows&arch=x86_64&version=11&type=exe_local](https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local) （GPUなしでは不要）  
※ WindowsおよびMacOSに対応．GPUがなくても動作するが，GPUがある方が処理速度が速い．
  
**処理速度の比較**  
1. GPUを使用（例：NVIDIA GeForce RTX 3060）   
　　1. 1秒あたり200フレーム以上の処理が可能
  　2. 1本または複数の動画を処理できる 
2. GPUなし（CPUのみ）：  
  　1. 1秒あたり10～20フレーム程度の処理
    2. 短い超音波動画に適している  

**処理対象**  
このモデルは，以下のサイズの超音波動画に対応する：  
1. 動画サイズ：320ピクセル（幅）×240ピクセル（高さ） 
2. このサイズは，トレーニング時に使用したため，異なるサイズだと正確に輪郭を抽出できない可能性がある  

**キーポイントの設定**  
1. 舌の輪郭を抽出するため，11個のキーポイントを使用する  
2. 舌の形状を還元するため，舌の輪郭線に沿って主観的に入れる
3. キーポイント間の距離は等間隔である必要はない

**なぜ等間隔にしないのか？**  
1. 11点を等間隔に配置すると，実際の舌形状を正確に抽出できない場合がある   
2. DLCでは少ないキーポイントが理想的だが，等間隔では30点以上が必要になるため，キーポイントを多く使うと精度が下がる可能性がある
3. キーポイントの数が増えると，学習データの作成やトレーニングに時間がかかる

**学習データ**
1.合計4,821フレームのデータを使用
2. GE Healthcareとトリプルエーの2種類の超音波装置のデータを含む
3. アジア系と欧米系の話者のデータを含む．

**学習時間・回数**
1. MSIノートPC（GPU: NVIDIA GeForce RTX 3060）で約10時間を要したが，より高性能なGPUなら学習時間は短縮可能
2. 学習回数は103万回


