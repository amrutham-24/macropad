# 🎛️ Waveshare RP2040 Zero Macro Pad with Rotary Encoder (5 V Logic Safe)

A compact **CircuitPython-based macro keyboard** using the **Waveshare RP2040 Zero**,
three **6×6×5 mm tactile push buttons**, and a **5 V rotary encoder brick module** for media control.

It acts as a **USB HID keyboard**, letting you trigger shortcuts and adjust system volume — all from a tiny, customizable board.

---

## ⚙️ Features

* **3 Programmable Buttons**
  → Send single keys or combos like `CTRL+C` or `SHIFT+ALT+S`.

* **Rotary Encoder (with Push Button)**
  → Rotate → Volume Up / Down
  → Press → Mute Toggle

* **JSON-based configuration** stored on the CIRCUITPY drive.

* **Plug-and-Play HID** — no drivers needed for Windows, macOS, or Linux.

---

## 🧩 Hardware Setup

| Component   | RP2040 Zero Pin | Connection               | Notes                   |
| ----------- | --------------- | ------------------------ | ----------------------- |
| Button 1    | GP3             | To GND + GP3             | Active-low              |
| Button 2    | GP4             | To GND + GP4             | Active-low              |
| Button 3    | GP5             | To GND + GP5             | Active-low              |
| Encoder CLK | GP6             | From encoder “CLK”       | Use divider or resistor |
| Encoder DT  | GP7             | From encoder “DT”        | Use divider or resistor |
| Encoder SW  | GP8             | From encoder “SW”        | Use divider or resistor |
| Encoder VCC | 5 V             | From RP2040 Zero 5 V pin |                         |
| Encoder GND | GND             | Common ground            |                         |

---

### ⚠️ Important – Logic Level Safety

Your encoder outputs **5 V logic**, but the RP2040’s GPIO pins accept only **3.3 V max**.
To protect your board, use **one of these options**:

**Option 1 – Resistor Dividers**
Add two resistors per signal (e.g. 10 kΩ → 20 kΩ) to drop 5 V → 3.3 V.

**Option 2 – Series Resistors (Quick Hack)**
Put a **4.7 kΩ–10 kΩ** resistor in series with each of CLK, DT, SW.
It usually works because encoder pull-ups are weak, but a divider is safer.

**Option 3 – Use a 3.3 V Encoder**
If available, this removes the need for level shifting.

---

## 💾 Software Setup

### 1️⃣ Install CircuitPython

Get the latest firmware for **Waveshare RP2040 Zero**:
👉 [https://circuitpython.org/board/waveshare_rp2040_zero/](https://circuitpython.org/board/waveshare_rp2040_zero/)

Flash it by holding **BOOTSEL** and dragging the `.uf2` file onto the board.

---

### 2️⃣ Required Libraries

Only one library is needed in your `/lib/` folder:

```
/lib/
└── adafruit_hid/
```

> 🧠 `rotaryio` is built into CircuitPython for RP2040 boards — no extra `.mpy` file required.
> You do **not** need `adafruit_bus_device` or `adafruit_busio` for this project.

---

## 📁 File Structure

```
CIRCUITPY/
├── code.py          ← main macro script
├── config.json      ← customizable key mappings
└── lib/
    └── adafruit_hid/
```

---

## 🧠 About `config.json`

This file lives on your **CIRCUITPY** drive and defines what each button does.

Example:

```json
{
  "btn1": "A",
  "btn2": "CTRL+C",
  "btn3": "SHIFT+ALT+S"
}
```

✅ Supported keys
Letters A–Z, Numbers 0–9, Modifiers CTRL/SHIFT/ALT, and Specials ENTER, TAB, ESC, SPACE, BACKSPACE, DELETE.

If `config.json` is missing or invalid, a default mapping is used automatically.

---

## ▶️ Usage

1. Plug in your **RP2040 Zero** — it appears as a USB keyboard.
2. Press a button → sends your assigned macro.
3. Rotate encoder → adjusts system volume.
4. Press encoder → toggles mute.

---

## 🧰 Troubleshooting

* Open **Thonny** or **Mu Editor** to view debug prints via REPL.
* If encoder doesn’t work, check level shifting on CLK/DT/SW.
* If `rotaryio` import fails, update to the latest CircuitPython release.

---

## 💡 Example Serial Output

```
Macro board with volume knob ready! Current config: {'btn1': 'A', 'btn2': 'CTRL+C', 'btn3': 'SHIFT+ALT+S'}
Btn2 → CTRL+C
Volume Up
Mute Toggle
```

---

## 🧱 Components Used

| Part                | Description                                      |
| ------------------- | ------------------------------------------------ |
| **Microcontroller** | Waveshare RP2040 Zero (RP2040 MCU, USB-C)        |
| **Buttons**         | 6×6×5 mm tactile push buttons (2-pin side-mount) |
| **Rotary Encoder**  | Brick module (CLK, DT, SW, VCC=5 V, GND)         |

---

## 🖥️ Macropad Configurator (Desktop Tool)

A companion **Tkinter-based GUI app** that edits `config.json` on your **CIRCUITPY drive**.
No need to manually open or format the JSON file!

### ✨ Features

* Auto-detects `config.json` on connected macropad
* Retro pixel interface with 3 editable button mappings
* Save, reset, and quit buttons
* Works on Windows, macOS, and Linux

### ▶️ Run

```bash
python macropad_configurator.py
```

### 💾 Build as Standalone App

```bash
pyinstaller --onefile --noconsole --name "Macropad Configurator" --icon=icon.ico macropad_configurator.py
```

### 📁 Requirements

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## 📜 License

Open-source under the **MIT License** — modify and share freely.
Designed for **makers, streamers, and automation enthusiasts**.

---

✨ *Built with CircuitPython + Adafruit HID + Waveshare RP2040 Zero + Tkinter Configurator.*
