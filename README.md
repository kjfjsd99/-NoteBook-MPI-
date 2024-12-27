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
