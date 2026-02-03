# Realtek 8852AU Linux Driver (Linux Kernel 6.14+)

本專案為 Realtek 8852AU USB 無線網卡 (802.11ax/Wi-Fi 6) 的 Linux 驅動程式。(Based on lwfinger/rtl8852au)
此版本已針對 **Linux Kernel 6.14** 進行適配與修正，解決了新版核心 API 變更導致的編譯錯誤。

## 🛠️ 建置環境 (Build Environment)

此驅動程式已在以下環境測試編譯成功：

*   **OS**: Ubuntu / Linux
*   **Kernel Version**: `6.14.0-37-generic`
*   **Driver Version**: 8852AU
*   **GCC Version**: Default system compiler

> **注意**：此版本包含了針對 Kernel 6.14+ 的 `cfg80211` API 修正 (包含 `get_tx_power` 與 `set_monitor_channel` 的參數調整)。

---

## 🚀 安裝步驟 (Installation)

### 1. 安裝必要套件
在開始編譯之前，請確保系統已安裝編譯工具與核心標頭檔：

```bash
sudo apt update
sudo apt install build-essential linux-headers-$(uname -r) network-manager
```

### 2. 編譯驅動程式 (Build)
進入目錄並執行編譯：

```bash
make
```
*(編譯過程中若出現 `warning` 警告訊息屬於正常現象，只要沒有 `error` 即可)*

### 3. 安裝驅動程式 (Install)
將編譯好的模組安裝至系統目錄：

```bash
sudo make install
```

### 4. 啟用驅動程式 (Activate)
你可以選擇重新開機，或執行以下指令立即載入：

```bash
sudo modprobe 8852au
```

---

## 📶 連線設定 (Usage)

### 檢查網卡狀態
確認系統是否抓到網卡 (通常介面名稱為 `wlan0` 或 `wlxa...`)：

```bash
ip link
```

### 連接 Wi-Fi
建議使用 `nmtui` 圖形化介面進行連線：

```bash
sudo nmtui
```
選擇 **"Activate a connection"**，找到你的 Wi-Fi 名稱並輸入密碼。

---

## 🔄 核心更新後的維護 (Kernel Update)

如果你更新了 Linux Kernel (例如從 `6.14.0-37` 升級到 `6.14.0-38`)，驅動程式可能會失效。請回到此目錄執行以下指令重新安裝：

```bash
make clean
make
sudo make install
sudo modprobe 8852au
```

---

## 📝 修正紀錄 (Patch Notes)

針對 Linux 6.14 Kernel 進行了以下修正：
1.  **os_dep/linux/ioctl_cfg80211.c**:
    - 修正 `cfg80211_rtw_get_tx_power`：新增 `link_id` 參數。
    - 修正 `cfg80211_rtw_set_monitor_channel`：新增 `struct net_device *dev` 參數。
2.  **Makefile / Kconfig**:
    - 修正 `MODULE_IMPORT_NS` 相關引用錯誤。
