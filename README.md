### 使用 Google Colab 進行目標追蹤代碼測試的報告

#### 前言
由於疫情影響無法進入實驗室運行代碼，加上老師要求測試一段目標追蹤的test代碼，但個人筆記本電腦的運行速度和性能不足，因此決定在Google Colab上進行測試。本文將介紹如何在Colab上配置並執行SiamFC++代碼。

---

### 1. 配置 Google Drive

在開始使用Colab之前，首先需要將目標追蹤代碼上傳至Google Drive，以便在雲端環境中進行存取。本文以SiamFC++為例，具體步驟如下：

1. **下載代碼**：可以從GitHub下載SiamFC++代碼到本地，或者直接在Colab上使用以下命令從GitHub克隆代碼：
   ```python
   !git clone https://github.com/MegviiDetection/video_analyst.git
   ```

2. **上傳至Google Drive**：將下載的代碼目錄上傳至Google Drive。建議將項目放入便於識別的目錄中，例如“video_analyst”。

3. **準備模型**：從[model zoo](https://modelzoo.co/)下載已訓練的模型，並上傳至Google Drive中代碼目錄下。此時Google Drive的準備工作就完成了。

---

### 2. 配置 Google Colab

在Google Drive中準備好代碼和模型後，接下來需要在Colab中配置環境：

1. **新建 Colab 筆記本**：首先，新建一個Colab筆記本，並在筆記本設置中選擇GPU作為硬體加速器。

2. **掛載 Google Drive**：導入相關庫並掛載Google Drive，以便能夠訪問上傳的代碼和模型。執行以下代碼掛載Google Drive：
   ```python
   import os
   from google.colab import drive
   drive.mount('/content/drive/')
   ```

3. **設置工作路徑**：將工作路徑設置為代碼的根目錄，方便後續執行代碼。假設代碼上傳到了`/content/drive/My Drive/video_analyst/`，可使用以下命令：
   ```python
   os.chdir('/content/drive/My Drive/video_analyst/')
   ```

4. **安裝依賴庫**：SiamFC++代碼中的`requirements.txt`已經列出了必要的依賴庫。可以通過以下命令快速安裝：
   ```python
   !pip install -r ./requirements.txt
   ```

5. **編譯 VOT 包**：SiamFC++使用了VOT（Visual Object Tracking）包，編譯過程需要以下命令：
   ```python
   %cd videoanalyst/evaluation/vot_benchmark/pysot/utils/
   !python3 setup.py build_ext --inplace
   ```

6. **返回根目錄**：完成編譯後返回至項目根目錄，檢查當前目錄是否為根目錄。
   ```python
   %cd /content/drive/My Drive/video_analyst/
   ```

---

### 3. 配置模型路徑

在代碼中設定模型的路徑，使其能夠正確加載預先訓練的模型。可以將模型路徑指定為Google Drive中的位置。修改完成後，保存代碼。

---

### 4. 執行測試代碼

運行代碼進行目標追蹤測試。可以根據具體情況選擇不同的配置，例如：
```python
!python3 tracking/test.py --config=configs/siamfcpp/test/otb.yaml
```

若有自定義的測試數據集，也可以在代碼中指定數據集路徑。

---

### 結論
本文介紹了如何在Google Colab上進行目標追蹤代碼的配置與測試，通過掛載Google Drive實現雲端運行SiamFC++，解決了個人電腦性能不足的問題，達到了遠端高效運行深度學習代碼的目的。
