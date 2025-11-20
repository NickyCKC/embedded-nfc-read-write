# NFC Read/Write Tool – Engineering Showcase

This repository documents a project I developed involving a **custom NFC Read/Write
desktop application** and its integration with an **embedded NFC reader running
on a custom STM32-based board using STM32G0B1RET6 MCU**.  

---

## 🎯 Project Goal

The objective of the project was to create a **reliable and user-friendly NFC
interaction tool** that could:

- Communicate with an NFC reader over serial (USB CDC)
- Read NFC tag information and memory blocks
- Write specific blocks in hex format
- Display structured JSON responses
- Automatically reconnect to the device when unplugged/replugged
- Provide a simple GUI suitable for demos and validation

The final result is a polished desktop utility used internally for engineering,
testing, and demonstration purposes.

---

## 🧩 System Overview

The complete solution consists of:

### **1. Embedded Firmware (STM32 MCU)**
- Based on X-Cube-NFC and RFAL libraries  
- Implements NFC Type V (ISO 15693) read/write logic  
- Streams tag information over USB CDC in JSON format  
- Receives simple ASCII commands for block reads and writes  
- Designed for low-latency and stable operation  

### **2. Desktop GUI Application**
A custom GUI (Python + Qt) that I designed and implemented with:

- Serial auto-detection and dynamic port scanning  
- Auto-reconnect using VID/PID matching  
- Live logging window  
- Real-time JSON parsing and structured display  
- Block read/write interface  
- Embedded images and branding  
- Safe, isolated input validation (hex-only write fields)  
- Multithreaded design (non-blocking serial reader)

---

## 🖥️ Graphical User Interface

Below are demonstration screenshots of the GUI and workflow.

### **Read Block Workflow**
![NFC_GUI_READ](https://github.com/user-attachments/assets/ff2d205a-7d9d-4714-9960-c24697ef0ff2)

### **Write Block Workflow**
![NFC_GUI_WRITE](https://github.com/user-attachments/assets/d643c577-cc4b-47bc-bc4d-a950df916723)

---

## 🔧 How the Tool Works (Conceptual Description)

Although the source is not published, the tool works as follows:

### **1. Connect to NFC Reader**
The GUI enumerates all COM ports and allows selection of baud rate.

### **2. Read NFC Tag**
The firmware detects NFC tags and responds with JSON like:

```json
{
  "tag_type": "Type V",
  "id_type": "ISO15693",
  "id": "E0 04 ...",
  "block": 3,
  "data": "12 34 56 78"
}
```
The GUI parses this and displays it clearly.

### **3. Write NFC Block**

Users can enter exactly 8 hex characters.
The GUI transmits:
<block_number><hex_data>
and displays confirmation JSON when successful.

### **4. Auto-Reconnect**
If the device is unplugged:
- GUI detects serial disconnect
- Searches for the same COM or VID/PID
- Reconnects automatically
- Resumes reading without restarting the program
This makes the tool robust for lab work and demos.

---

## 🧠 Engineering Highlights
### **✔ Serial handling**
- Background thread for non-blocking reads
- Exception-safe disconnect handling
- Clean shutdown on window close
- Uses PyQt signals/slots or thread-safe UI updates

### **✔ JSON parsing**
- Automatic detection of JSON payloads
- Falls back to raw text log
- Graceful handling of partial frames

### **✔ Input Validation**
- Strict hex-only validation for write operations
- Input trimming and formatting
- Field range limits for block numbers

### **✔ Auto-Reconnect Logic**
- Tracks:
-- Selected COM port
-- VID/PID of the device
- Reopens when device reappears
- Uses retry loops with backoff
- Offers seamless demo experience

### **✔ UI Design**
- Clean Qt layout
- Split panel (image + controls)
- Resizable, DPI-friendly fonts
- Suitable for marketing demos
