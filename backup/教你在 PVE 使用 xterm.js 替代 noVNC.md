### 教學文章：在 PVE 使用 xterm.js 替代 noVNC

#### 簡介
在 PVE 5.1 中，我們可以使用 xterm.js 替代 noVNC，以實現 KVM 虛擬機的複製和貼上功能。本文將介紹如何設定。

#### 步驟一：新增 Serial Port
1. **關閉 VM**。
2. 在 PVE Host 中執行以下命令:
   ```bash
   qm set 100 -serial0 socket
   ```
   （假設 VM ID 是 100）
![image](https://github.com/hhjjy/hhjjy.github.io/assets/45664168/31e8de1a-c631-4f03-99b9-595b2b04daa9)

3. **重開 VM**，並用 `dmesg | grep ttyS` 確認 serial port 是否偵測到。
![image](https://github.com/hhjjy/hhjjy.github.io/assets/45664168/2bd3313c-0d5d-4d8a-ac2c-72f677daec44)

#### 步驟二：修改 GRUB 設定
1. 編輯 `/etc/default/grub` 文件，將 `GRUB_CMDLINE_LINUX` 參數設為：
   ```bash
   GRUB_CMDLINE_LINUX="quiet console=tty0 console=ttyS0,115200"
   ```
![image](https://github.com/hhjjy/hhjjy.github.io/assets/45664168/f0322d3e-ca60-4f28-9757-7971033cc137)

   這樣可以確保內核在啟動時使用 ttyS0 作為控制台。
2. **更新 grub** 設定：
   - 對於 Debian-based 系統，執行：
     ```bash
     sudo update-grub
     ```
![image](https://github.com/hhjjy/hhjjy.github.io/assets/45664168/18ffdc33-089d-4007-b4ef-519db209d1e6)

3. **重開機**。

#### 步驟三：測試 xterm.js
1. 在 PVE UI 中測試 xterm.js。
2. 若畫面停滯，按下 Enter 確認連接成功。

#### 為何要修改 GRUB
GRUB 配置文件 `/etc/default/grub` 是用來設定 GNU GRUB 引導加載器的默認行為。透過修改此文件，可以指定內核啟動參數，使得系統在啟動時將輸出發送到特定的 ttyS0（串行端口）。這樣可以允許通過串行控制台訪問虛擬機，並在 Proxmox VE 中使用 xterm.js 操作虛擬機的命令行界面，無需依賴 SSH 連接。

#### 完整參考資料
請參考：[PVE 5.1 設定 KVM 虛擬機能夠使用 xterm.js](https://www.pigo.idv.tw/archives/3261)。