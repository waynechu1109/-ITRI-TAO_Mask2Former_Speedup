<h2 align="center">
  <strong>
    <a href="README.md">English</a> | <a href="README_ch.md">中文</a>
  </strong>
</h2>

# [工研院實習專案] [NVIDIA TAO](https://developer.nvidia.com/tao-toolkit) Mask2Former 加速

## NVIDIA TAO Toolkit
Mask2Former 透過 NVIDIA TAO Toolkit 進行加速，相關環境設定可以參考[官方教學](https://github.com/NVIDIA/tao_tutorials)。

此資料夾也包含在該 Repo. 中（原始路徑為`tao_tutorials/notebooks/tao_launcher_starter_kit/mask2former`），我已經將所有相關設定設置完成，只需要按照其中 Jupyter Notebook 的指示即可完成 Mask2Former 加速。


<!-- ```bash
$ cd mask2former/
# 接著進入 Jupyter Notebook: mask2former.ipynb
``` -->

## 加速

`mask2former.ipynb`將實現 Mask2Former 加速的所有步驟都清楚列出。

---

### 0. Set up env variables and map drives  
環境及工作路徑相關設定

---

### 1. Installing the TAO launcher  
安裝 TAO launcher 只需要在首次執行此腳本時執行即可。

---

### 2. Prepare dataset and download pretrained model

#### 2.1 Prepare dataset  
腳本中這一部份在下載 COCO 資料集。  
我也有自行整理一份混何資料集（Mapillary + ADE20k）來同時符合機器狗以及自駕車的使用需求並放在資料夾中 (Merged dataset)。

#### 2.2 Download pretrained Model  
腳本中這一部份在下載 Swin-Tiny 預訓練權重作為後續 Mask2Former 訓練的 Backbone。

---

### 3. Provide experiment spec file  
這一步驟秀出 `specs/spec_inst.yaml` 中的內容。  
可以在這個檔案中設定所有與訓練相關的參數，如訓練 epoch、backbone 路徑、訓練資料集路徑等等。  
如果需要微調可以到此檔案中調整。

---

### 4. Run TAO training  
此步驟執行訓練。

---

### 5.~6. 可視需求跳過

---

### 7. Deploy  
在進行此步驟前，需要先將訓練好的 `.pth` 從 `mask2former/train/swin_tiny` 移動至 `/experiments` 資料夾。  

此步驟將訓練好的模型轉換為 `.onnx` 格式，接著再將其轉換為我們要的 `.engine`。轉換出的 `.onnx` 可以在 `/experiments/export` 中找到，  
而 `.engine` 則可以在 `/experiments/gen_trt_engine` 中找到。

但是推薦不在此進行`.engine`轉換，因為機器狗上的 TensorRT 版本與 `x86` 上的版本不會相同，所以在 `x86` 上轉換的 `.engine` 無法在機器狗上使用。我將是用機器狗的`.onnx`轉換成`.engine`的腳本放在`ros2_tao_pointpillars`資料夾中。

---

到此，即完成了 Mask2Former TensorRT 加速。
如需要進行影像實時推論，我將程式碼提供在`ros2_tao_pointpillars`資料夾中。



