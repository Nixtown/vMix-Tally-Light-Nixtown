# vMix Tally Light - Nixtown Edition

An ESP32-powered tally light system for live video production with [vMix](https://www.vmix.com/). This project lights up WS2812 (NeoPixel-style) LEDs to indicate **Preview** and **Program** status via the vMix TCP TALLY protocol. It includes a full web interface for setup, Wi-Fi configuration via AP mode, and reliable backup via double-reset fallback.

---

## ⚙️ How It Works

* The ESP32 connects to **vMix** using TCP (port `8099`) and subscribes to tally updates.
* Based on the input number configured, the LEDs light up:

  * 🔴 **Red (Active Color)** when the input is live in **Program**
  * 🟡 **Yellow (Preview Color)** when the input is in **Preview**
  * ⚫ **Off** when idle

---

## 📦 Features

* WS2812 LED support (FastLED)
* Web UI served from ESP32 using LittleFS
* Wi-Fi setup using **WiFiManager**
* Color picker for customizing Preview/Active colors
* Stores settings in **non-volatile Preferences**
* **Double-reset fallback**: enter AP mode by unplugging & replugging quickly
* Reset settings via the web UI

---

## 🔧 Setup Instructions

### 1. Flashing the ESP32

You will need:

* ESP32 board (e.g., ESP32-C3, ESP32 DevKit)
* Arduino IDE with ESP32 board support installed
* Required libraries:

  * `WiFiManager`
  * `FastLED`
  * `LittleFS` for ESP32
  * `Preferences`

Also install the **ESP32 LittleFS Uploader plugin**:
[https://github.com/lorol/arduino-esp32fs-plugin](https://github.com/lorol/arduino-esp32fs-plugin)

Then:

1. Put your `index.html` file into a `/data/` folder.
2. Use the menu: **Tools > ESP32 Sketch Data Upload**
3. Flash the `.ino` file normally.

### 2. First Boot (or Reset)

When first booted or reset:

* If no saved Wi-Fi, the ESP32 will enter **AP Mode**.
* The device will broadcast a Wi-Fi network: `TallyLight-Setup`
* Connect to it with your phone or computer.
* The captive portal will let you choose a Wi-Fi network and enter:

  * vMix IP address
  * Input number
  * Device name (for mDNS like `http://tallylight-0.local/`)
  * Preview/Active colors

---

## 🌐 Web Portal Features

Once connected to your Wi-Fi network, visit the ESP32:

* At its IP (check your router)
* Or at: `http://your-device-name.local` (e.g., `http://tallylight-0.local`)

### Web UI Includes:

* Change device name, vMix IP, and input number
* Pick custom colors for preview/program
* Save settings (applies immediately)
* **Reset Settings** button (clears Wi-Fi and preferences)

---

## 🔁 Double Reset Fallback

If something goes wrong:

* Quickly **unplug and replug the device twice** (within 3 seconds)
* This triggers a **double-reset detector**
* You'll see an **orange blink**, and then it enters AP mode again
* Use the captive portal to reconfigure

---

## 🖥️ Finding the vMix IP Address

In vMix:

1. Go to **Settings** > **Web Controller**
2. Look for the IP listed under **Local Network Address** (e.g., `192.168.1.123`)
3. Enter this IP into the Tally Light web UI

**Note**: Your ESP32 must be on the **same local network** as the computer running vMix.

---

## 🧪 Advanced Notes

* vMix sends TALLY data like `TALLY OK 1000...`, and the device checks the state at the `input_number - 1` index
* Colors are stored as **hex strings** (e.g., `#ff0000`) and parsed into `CRGB` objects
* Preferences are stored in NVS (`Preferences`), so settings persist after reboot
* mDNS (Multicast DNS) allows access like `http://tallylight-0.local/` on most modern networks

---

## 📜 License

MIT License. Use freely, modify, distribute — no warranty. See `LICENSE` file.

---

## 👋 Credits

Created by **Nix\_the\_world** as part of the Bag-o-meter project.

Inspired by the need for a simple, reliable tally light system for live production.

---

Let me know if you want this exported as a `README.md` file for GitHub!
