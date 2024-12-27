"# 在NoteBook(Windows)上執行MPI程式筆記" 
這份筆記將包括從頭開始編寫 MPI 程式的步驟。從安裝 Microsoft MPI 到成功運行 MPI 程式的完整流程

---

# 從安裝 Microsoft MPI 到成功運行 MPI 程式的完整流程  

## 步驟 1: 安裝 Microsoft MPI  

1. **下載 MS-MPI**  
   - 前往 Microsoft 官方網站，搜尋「Microsoft MPI」，或者直接訪問 [MS-MPI 官方下載頁面](https://learn.microsoft.com/en-us/message-passing-interface/microsoft-mpi)。  
   - 下載最新版本的 MS-MPI 安裝程式。  

2. **安裝 MS-MPI**  
   - 執行下載的安裝檔，按照指示完成安裝。  
   - 記下安裝路徑（通常是 `C:\Program Files (x86)\Microsoft SDKs\MPI`）。  

---

## 步驟 2: 安裝 Visual Studio  

1. **下載 Visual Studio**  
   - 前往 Visual Studio 官方網站，下載最新版本的 Visual Studio 社群版（Community，免費）。  

2. **安裝 Visual Studio**  
   - 安裝過程中，選擇「使用 C++ 的桌面開發」工作負載，以確保擁有必須的編譯工具和函式庫。  

---

## 步驟 3: 建立新的 C++ 專案  

1. **開啟 Visual Studio**  
   - 啟動 Visual Studio。  

2. **建立新專案**  
   - 點選「檔案」 > 「新增」 > 「專案」。  
   - 選擇「C++」分類下的「主控台應用程式」，然後按下「下一步」。  
   - 為專案命名（例如 `MPIExample`），選擇存放位置，然後按下「建立」。  

---

## 步驟 4: 新增程式碼檔案  

1. **新增 C++ 檔案**  
   - 在右側的「方案總管」中找到你的專案名稱（例如 `MPIExample`）。  
   - 右鍵點選 **`Source Files`** 資料夾，選擇「新增」 > 「新項目」。  
   - 選擇「C++ 檔案（.cpp）」，命名為 `main.cpp`，然後按下「確定」。  

2. **輸入程式碼**  
   - 打開 `main.cpp`，輸入以下程式碼：  

     ```cpp
     #include <mpi.h>
     #include <stdio.h>

     int main(int argc, char* argv[]) {
         MPI_Init(&argc, &argv);
         int rank;
         MPI_Comm_rank(MPI_COMM_WORLD, &rank);
         printf("Hello from process %d\n", rank);
         MPI_Finalize();
         return 0;
     }
     ```  

---

## 步驟 5: 設定專案屬性  

1. **打開專案屬性**  
   - 在方案總管中，右鍵點選專案名稱（例如 `MPIExample`），選擇「屬性」。  

2. **設定「包含目錄」**  
   - 在左側的「C/C++」 > 「一般」中，找到「附加包含目錄」，然後新增以下路徑：  
     ```
     C:\Program Files (x86)\Microsoft SDKs\MPI\Include
     ```  

3. **設定「附加函式庫目錄」**  
   - 在左側的「連結器」 > 「一般」中，找到「附加函式庫目錄」，新增以下路徑：  
     ```
     C:\Program Files (x86)\Microsoft SDKs\MPI\Lib\x64
     ```  

4. **新增依賴的函式庫**  
   - 在左側的「連結器」 > 「輸入」中，找到「附加相依性」，新增以下內容：  
     ```
     msmpi.lib
     ```  

---

## 步驟 6: 建置專案  

1. **建置方案**  
   - 在上方功能列中，選擇「建置」 > 「建置方案」，或者按下快捷鍵 `Ctrl + Shift + B`。  
   - 確認建置完成且沒有錯誤訊息。  

---

## 步驟 7: 執行 MPI 程式  

1. **開啟命令提示字元**  
   - 按下 `Win + R`，輸入 `cmd`，按下 Enter 開啟命令提示字元。  

2. **導航到執行檔所在資料夾**  
   - 使用 `cd` 指令切換到專案的執行檔所在資料夾，例如：  
     ```bash
     cd /d E:\Parallel Computing\MPIExample\x64\Debug
     ```  

3. **執行 MPI 程式**  
   - 使用以下指令執行程式：  
     ```bash
     mpiexec -n 4 MPIExample.exe
     ```  

4. **確認執行結果**  
   - 正常情況下，你應該看到以下輸出：  
     ```text
     Hello from process 0
     Hello from process 1
     Hello from process 2
     Hello from process 3
     ```  

---

/**
## 附錄步驟: main.cpp中可使用的程式碼
使用 opencv-4.6.0-vc14_vc15
這版的 opencv, 缺少了一些 DLL文件:
opencv_core_parallel_onetbb460_64d.dll
opencv_core_parallel_tbb460_64d.dll
opencv_core_parallel_openmp460_64d.dll
*/

```cpp
#include <iostream>
#include <opencv2/opencv.hpp>
#include "mpi.h"
#include <vector>
#include <chrono>

using namespace cv;
using namespace std;

// 應用不同的濾鏡到影像上
void applyFilter(Mat& image, const string& filterType) {
    if (filterType == "blur") {
        // 應用高斯模糊，核大小為5x5
        GaussianBlur(image, image, Size(5, 5), 0);
    }
    else if (filterType == "gray") {
        // 將影像從BGR色彩空間轉換為灰階
        cvtColor(image, image, COLOR_BGR2GRAY);
    }
    else if (filterType == "edge") {
        // 使用Canny邊緣檢測法來檢測邊緣
        Canny(image, image, 100, 200);
    }
    else {
        // 如果提供的濾鏡類型不被支持，則輸出錯誤訊息
        cerr << "不支援的濾鏡類型：" << filterType << endl;
    }
}

int main(int argc, char** argv) {
    // 初始化MPI環境
    MPI_Init(&argc, &argv);
    
    int rank, size;
    // 取得當前進程的等級（0通常是主進程）
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    // 取得總進程數
    MPI_Comm_size(MPI_COMM_WORLD, &size);
    
    // 設定OpenCV應使用的執行緒數量，控制每個節點內部的並行
    setNumThreads(4);
    
    // 開始計時整個過程
    auto startTime = chrono::high_resolution_clock::now();
    
    if (rank == 0) {
        // 只由主進程（等級0）輸出此消息
        cout << "開始使用 " << size << " 個進程進行影像處理" << endl;
    }
    
    vector<Mat> images;
    if (rank == 0) {
        // 主進程從磁碟讀取影像
        for (int i = 0; i < 4; ++i) {
            // 構建每個影像的檔案名
            string filename = "E:/Parallel Computing/Images/input_" + to_string(i) + ".jpg";
            Mat image = imread(filename);
            if (image.empty()) {
                // 如果無法讀取影像，輸出錯誤並中止
                cerr << "無法讀取影像：" << filename << endl;
                MPI_Abort(MPI_COMM_WORLD, -1);
                return -1;
            }
            cout << "成功讀取影像 " << i << endl;
            images.push_back(image);
        }
    }

    // 計算每個進程應處理的影像數量
    int imagesPerProcess = 4 / size;
    int remainder = 4 % size;  // 處理不均勻分配的情況
    int startIdx = rank * imagesPerProcess + min(rank, remainder);  // 此進程的起始索引
    int endIdx = startIdx + imagesPerProcess + (rank < remainder ? 1 : 0);  // 此進程的結束索引

    if (rank == 0) {
        // 主進程將影像發送給其他進程
        for (int i = 1; i < size; i++) {
            int start = i * imagesPerProcess + min(i, remainder);
            int count = imagesPerProcess + (i < remainder ? 1 : 0);
            
            for (int j = 0; j < count; j++) {
                int imgIndex = start + j;
                int rows = images[imgIndex].rows;
                int cols = images[imgIndex].cols;
                int type = images[imgIndex].type();
                
                // 傳送影像的元數據（尺寸和類型）
                vector<int> metadata = {rows, cols, type};
                MPI_Send(metadata.data(), 3, MPI_INT, i, 0, MPI_COMM_WORLD);
                
                // 傳送實際的影像數據
                MPI_Send(images[imgIndex].data, rows * cols * images[imgIndex].elemSize(), 
                        MPI_UNSIGNED_CHAR, i, 1, MPI_COMM_WORLD);
                
                cout << "已將影像 " << imgIndex << " 發送至進程 " << i << endl;
            }
        }

        // 處理分配給主進程的影像
        for (int i = 0; i < imagesPerProcess + (0 < remainder ? 1 : 0); i++) {
            cout << "進程0正在處理影像 " << i << endl;
            applyFilter(images[i], "blur");
            imwrite("output_" + to_string(i) + "_rank_0.jpg", images[i]);
        }
    }
    else {
        // 其他進程接收並處理影像
        for (int i = 0; i < imagesPerProcess + (rank < remainder ? 1 : 0); i++) {
            vector<int> metadata(3);
            // 接收元數據
            MPI_Recv(metadata.data(), 3, MPI_INT, 0, 0, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
            
            // 使用接收到的尺寸和類型創建一個新的Mat對象
            Mat img(metadata[0], metadata[1], metadata[2]);
            // 接收影像數據
            MPI_Recv(img.data, metadata[0] * metadata[1] * img.elemSize(), 
                    MPI_UNSIGNED_CHAR, 0, 1, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
            
            cout << "進程 " << rank << " 接收並處理影像 " << (startIdx + i) << endl;
            applyFilter(img, "blur");
            imwrite("output_" + to_string(startIdx + i) + "_rank_" + to_string(rank) + ".jpg", img);
        }
    }

    // 輸出此進程處理的影像範圍
    cout << "等級 " << rank << " 已完成處理來自 " 
         << startIdx << " 至 " << (endIdx - 1) << " 的影像" << endl;

    // 等待所有進程到達此點
    MPI_Barrier(MPI_COMM_WORLD);

    if (rank == 0) {
        // 只由主進程輸出總執行時間
        auto endTime = chrono::high_resolution_clock::now();
        chrono::duration<double> duration = endTime - startTime;
        cout << "影像處理完成，耗時 " << duration.count() << " 秒！" << endl;
    }

    // 結束MPI環境
    MPI_Finalize();
    return 0;
}
'''

關鍵概念：

    MPI（Message Passing Interface）：用於不同進程之間的通訊，在分散式系統中協調任務。
    OpenCV：用於影像處理操作，如模糊、邊緣檢測等。
    平行處理：將工作負載分擔給多個進程以加速計算，特別是在這裡用於分配影像處理任務到不同等級的進程上。


這個程式碼展示了如何在一個叢集環境中分發影像處理任務，對於處理大量數據或計算密集型任務非常有用。
