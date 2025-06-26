# DeepLabCut-Ultrasound 
**DLCを用いた超音波舌輪郭抽出モデルの説明**

**モデル概要**  
1. このモデルは，DeepLabCut（DLC）というAIツールを用いて，超音波動画から舌の輪郭を自動抽出する
2. 抽出された舌の輪郭点（キーポイント）は座標としてCSVファイルに保存される
3. 保存されたCSVファイルは2つあるが，「folder」で終わるファイルを分析に使用する
4. これにより，キーフレームを選び，同一の図に異なる音の輪郭線を重ねて分析できる

※ 抽出された輪郭データ（CSVファイル）が使用可能かどうかは，まず保存された輪郭線付きの動画で確認する必要がある

**モデルの学習データの詳細**
1. 合計4,821フレームのデータを使用
2. GE Healthcareとトリプルエーの2種類の超音波装置のデータを含む
3. アジア系と欧米系の話者のデータを含む
4. MSIノートPC（GPU: NVIDIA GeForce RTX 3060）で約10時間を要したが，より高性能なGPUなら学習時間は短縮可能
5. 学習回数は103万回

**学術的引用**  
このモデルを学術研究で使用する場合は，以下を引用する：

J.Sun, T.Kitamura, and R.Hayashi, "Extraction of Speech Organ Contours from Ultrasound and real-time MRI Data using DeepLabCut", _Acoustical Science and Technology_,1-7(2025).  
https://doi.org/10.1250/ast.e24.128  

**対応OS**
1. WindowsおよびMacOSに対応
2. GPUがなくても動作するが，GPUがある方が処理速度が速い

**環境設定**  
本モデルを使用する前に，以下のソフトウェアをインストールする：  
1. **Miniconda**  
   https://docs.anaconda.com/miniconda/
2. **Nvidiaドライバー**  （GPUなしの場合は不要）  
   https://www.nvidia.com/Download/index.aspx?lang=en-us 
3. **CUDA Toolkit**  （GPUなしの場合は不要） 
   [https://developer.nvidia.com/cuda/downloads?target_os=Windows&arch=x86_64&version=11&type=exe_local](https://developer.nvidia.com/cuda-downloads?target_os=Windows&target_arch=x86_64&target_version=11&target_type=exe_local)  
  
**処理速度の比較**  
1. GPUを使用（例：NVIDIA GeForce RTX 3060）   
   ① 1秒あたり200フレーム以上の処理が可能
   ② 1本または複数の動画を処理できる 
2. GPUなし（CPUのみ）：  
   ① 1秒あたり10～20フレーム程度の処理
   ② 短い超音波動画に適している  

**処理対象**  
このモデルは，以下のサイズの超音波動画に対応する：  
1. 動画サイズ：320ピクセル（幅）×240ピクセル（高さ） 
2. このサイズは，モデルのトレーニング時に使用したため，異なるサイズだと正確に輪郭を抽出できない  

**キーポイントの設定**（設定理由は下記参照）  
1. 舌の輪郭を抽出するため，11個のキーポイントを使用する  
2. 舌の形状を還元するため，舌の輪郭線に沿って入れる
3. キーポイント間の距離は等間隔である必要はない

**なぜ等間隔にしないのか？**  
1. 11点を等間隔に配置すると，実際の舌形状を正確に抽出できない場合がある   
2. DLCでは少ないキーポイントが理想的だが，等間隔では30点以上が必要になる
3. キーポイントの数が増えると，学習データの作成やトレーニングに時間がかかる

**DLCモデルの使い方（輪郭抽出）**
1. DeepLabCutの起動
   Windows：Anaconda Prompt (miniconda3) で以下を実行
    1. conda activate deeplabcut
    2. python -m deeplabcut
   Mac：ターミナルで上記と同じコマンドを実行

2. プロジェクトの読み込み
   ①GUIが開いたら 「Load Project」 をクリック
   ②DLCモデル内の 「config.yaml」 を選択
   
3. 動画の解析（輪郭抽出）
   ①「Analyze videos」 をクリック
   ②「Select videos」 をクリックし、解析したい動画（MP4/AVI/MKV/MOV）を選択
　　※注意：動画内で舌先は右側，舌根は左側にする必要がある
   ③以下のオプションにチェックを入れる
    ✅ Save result(s) as csv
    ✅ Filter predictions
    ✅ Plot trajectories
   ④右下の「Analyze videos」 をクリックして解析開始
   
4. 輪郭線付き動画の作成
   ①「Create videos」 をクリック
   ②以下のオプションにチェックを入れる
    ✅ Plot all bodyparts
    ✅ Draw skeleton
    ✅ Use filtered data
    ✅ Plot trajectories
   ③右下の「Create videos」 をクリックして動画を保存

5. 結果の確認
保存された動画を確認し，輪郭線が正しく抽出されているかチェック



