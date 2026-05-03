# 🐦 BirdBox – Raspberry & Microphone Enclosure & E-Paper Display

A 3D-printed bird box with an integrated e-paper display, powered by an ESP32 and [BirdNET-Go](https://github.com/tphakala/birdnet-go). The display shows the **Top 10 detected bird species of the day** in a live table, updated every 30 minutes directly from the BirdNET-Go API.


---

## 🌟 Features

- **Live bird detection table** – Top 10 bird species of the day, broken down into 3-hour time blocks (0h–21h)
- **Visual ranking** – Top 3 detections per time block are highlighted with red dot patterns (1st, 2nd, 3rd place)
- **Current time block** highlighted with a red border
- **CPU temperature monitoring** – Warning icon displayed if BirdNET-Go host exceeds 70 °C
- **Stale data warning** – Shows `(!)` if data is older than 15 minutes
- **Bird facts** – Displays a fun bird fact (from Home Assistant) when no birds have been detected yet, or when fewer than 5 species are shown
- **Sleep & absence modes** – Display goes blank (absence) or shows a moon icon (sleep), controlled via Home Assistant
- **Auto-refresh** every 30 minutes, manual refresh button in Home Assistant
- **Fallback hotspot** for easy Wi-Fi reconfiguration
- **OTA updates** via ESPHome

---

## 🖥️ Display

The firmware runs on an **ESP32** and drives a **Waveshare 4.2" black/white/red e-paper display** (`4.20in-bv2-bwr`). Change the code to the Display you have. 



---

## 🗂️ Repository Contents

| File | Description |
|---|---|
| `e-paper-birdbox.yaml` | ESPHome firmware configuration for the ESP32 |
| `BirdNET_Pi_Box_v2_scliced_hanging.3mf` | 3D print file – hanging wall-mount version (pre-sliced) |
| `BirdNET_Pi_Box_v3_sliced_stand.3mf` | 3D print file – tabletop stand version (pre-sliced) |
| `BirdBox_hanging.shapr` | CAD source file – hanging version (Shapr3D) |
| `BirdBox_with_stand.shapr` | CAD source file – stand version (Shapr3D) |

---

## 🛒 Hardware Requirements

- ESP32 development board (e.g. ESP32-DevKitC)
- [Waveshare 4.2" e-paper display – Black/White/Red (V2)](https://www.waveshare.com/4.2inch-e-paper-module-b.htm)
- 3D-printed enclosure (see `.3mf` files above)
- A running [BirdNET-Go](https://github.com/tphakala/birdnet-go) instance on your local network

### Wiring (SPI)

| ESP32 Pin | E-Paper Pin |
|---|---|
| GPIO 18 | CLK |
| GPIO 23 | MOSI (DIN) |
| GPIO 14 | CS |
| GPIO 17 | DC |
| GPIO 16 | RST |
| GPIO 4 | BUSY |

---

## ⚙️ Setup & Configuration the E-Paper Display

### 1. Prerequisites

- [ESPHome](https://esphome.io) installed (CLI or Home Assistant add-on)
- A running [BirdNET-Go](https://github.com/tphakala/birdnet-go) instance
- Home Assistant (optional, but required for sleep/absence modes and bird facts)

### 2. Configure `e-paper-birdbox.yaml`

Edit the `substitutions` section at the top of the file:

```yaml
substitutions:
  api_url: "http://<YOUR_BIRDNET_GO_IP>:8080/api/v2/analytics/species/daily"
  api_temp_url: "http://<YOUR_BIRDNET_GO_IP>:8080/api/v2/system/temperature/cpu"
  mqtt_broker: "<YOUR_MQTT_BROKER_IP>"
```

Replace `<YOUR_BIRDNET_GO_IP>` with the IP address of your BirdNET-Go host and `<YOUR_MQTT_BROKER_IP>` with your MQTT broker.

### 3. Add Wi-Fi credentials

Create a `secrets.yaml` file in the same directory:

```yaml
wifi_ssid: "YourNetworkName"
wifi_password: "YourPassword"
```

### 4. Add required assets

Place the following files in the same directory as the YAML:

- `Terminus.ttf` – Font file ([download here](https://terminus-font.sourceforge.net/))
- `moon.stars.fillv2.jpeg` – Sleep mode icon (any small icon image)

### 5. Flash the ESP32

```bash
esphome run e-paper-birdbox.yaml
```

---

## 🏠 Home Assistant Integration

The display integrates with Home Assistant for the following features:

| HA Entity | Purpose |
|---|---|
| `input_boolean.abwesenheitsmodus` | Absence mode → display goes completely blank |
| `input_boolean.schlafmodus` | Sleep mode → display shows moon icon |
| `sensor.bird_fact` | Text sensor providing bird facts for the display |

> The entity names can be customized in the `substitutions` section of the YAML.

A **manual refresh button** (`BirdBox Display Refresh`) and a **restart button** (`BirdBox Neustart`) are exposed to Home Assistant automatically.

---

## 🖨️ 3D Printing

Two enclosure variants are available:

- **Hanging version** (`v2`) – Designed to mount on a wall or fence
- **Stand version** (`v3`) – Freestanding tabletop design

The `.3mf` files are pre-sliced and ready to print. The `.shapr` files are the original CAD sources for modifications in [Shapr3D](https://www.shapr3d.com/).

---

## 🔗 Based on BirdNET-Go

This project relies on **[BirdNET-Go](https://github.com/tphakala/birdnet-go)** by [@tphakala](https://github.com/tphakala) for all bird sound detection and analysis. BirdNET-Go is a high-performance, real-time soundscape analyzer based on the BirdNET AI model, capable of identifying over 6,500 bird species.

> If you don't have BirdNET-Go running yet, check out the [BirdNET-Go installation guide](https://github.com/tphakala/birdnet-go/wiki/BirdNET%E2%80%90Go-Guide).

---

## 📄 License

This project is licensed under Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0).
This means:

Use & share – You may use and redistribute this project
Adapt – You may modify and build upon it
Non-commercial only – Commercial use is not permitted
ShareAlike – Derivatives must be released under the same license
Attribution – Credit must be given to the original authors
