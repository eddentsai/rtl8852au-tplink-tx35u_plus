Here is the **English version** of the `README.md`. It contains the exact same technical details and instructions as the Chinese version, formatted for easy copying.

```markdown
# Realtek 8852AU Linux Driver (Linux Kernel 6.14+)

This repository contains the Linux driver for the Realtek 8852AU USB Wi-Fi adapter (802.11ax/Wi-Fi 6).
This version has been patched to support **Linux Kernel 6.14**, addressing build errors caused by recent kernel API changes.

## 🛠️ Build Environment

This driver has been successfully compiled and tested in the following environment:

*   **OS**: Ubuntu / Linux
*   **Kernel Version**: `6.14.0-37-generic`
*   **Driver Version**: 8852AU
*   **GCC Version**: Default system compiler

> **Note**: This release includes specific fixes for the `cfg80211` API changes in Kernel 6.14+ (specifically regarding `get_tx_power` and `set_monitor_channel` arguments).

---

## 🚀 Installation

### 1. Install Dependencies
Before compiling, ensure you have the necessary build tools and kernel headers installed:

```bash
sudo apt update
sudo apt install build-essential linux-headers-$(uname -r) network-manager
```

### 2. Build the Driver
Navigate to the directory and compile:

```bash
make
```
*(Note: Warnings during compilation are normal; as long as there are no errors, you can proceed.)*

### 3. Install the Driver
Install the compiled module into the system directory:

```bash
sudo make install
```

### 4. Activate the Driver
You can either reboot your system or load the module immediately with:

```bash
sudo modprobe 8852au
```

---

## 📶 Usage

### Check Interface Status
Verify that the system recognizes the network card (usually named `wlan0` or `wlxa...`):

```bash
ip link
```

### Connect to Wi-Fi
We recommend using the `nmtui` graphical interface for easy connection:

```bash
sudo nmtui
```
Select **"Activate a connection"**, find your Wi-Fi SSID, and enter the password.

---

## 🔄 Maintenance (Kernel Updates)

If you update your Linux Kernel (e.g., from `6.14.0-37` to `6.14.0-38`), the driver may stop working. To fix this, simply navigate back to this directory and reinstall:

```bash
make clean
make
sudo make install
sudo modprobe 8852au
```

---

## 📝 Patch Notes

The following fixes were applied for Linux Kernel 6.14 compatibility:

1.  **os_dep/linux/ioctl_cfg80211.c**:
    - Fixed `cfg80211_rtw_get_tx_power`: Added `link_id` argument.
    - Fixed `cfg80211_rtw_set_monitor_channel`: Added `struct net_device *dev` argument.
2.  **Makefile / Kconfig**:
    - Fixed `MODULE_IMPORT_NS` reference errors.
```
