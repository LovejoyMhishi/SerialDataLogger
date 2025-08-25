<h1 align="center" style="color:#4CAF50;">
 💻 Serial Data Logger
</h1>

<p align="center">
  <em>A Python-based logger for capturing real-time data from serial devices into CSV files</em><br>
</p>

---
## 🌟 What It Does

This Python script enables you to **read real-time data from microcontrollers or sensors** connected via a serial port and store timestamped readings in a structured CSV file.

### It supports:

- 📥 Reading data from any COM port (e.g., COM21)  
- 📤 Logging sensor data  
- 🕒 Timestamping every entry automatically  
- 💾 Writing to CSV files with immediate flushing to avoid data loss  
- 🔁 Optional auto-naming of CSV files to prevent overwriting previous logs  

---

## 🧠 Core Concepts

The logger handles serial data efficiently, assuming:

- 🔌 The device is connected to a valid COM port  
- ⏱️ The serial data is formatted with comma-separated values  
- 🛡️ Each read line is decoded and cleaned before logging  
- 📝 CSV files are opened in write mode with headers defined  

---

## 🔧 Key Features

### 🔁 Serial Data Handling

- `serial.Serial(port, baudrate, timeout)` – sets up the serial connection  
- `ser.readline()` – reads one line of data at a time  
- `.decode('utf-8').strip()` – ensures clean string parsing  

### 💾 CSV Logging

- `csv.DictWriter()` – writes structured rows with fieldnames  
- `writer.writerow(row)` – logs data in real-time  
- `csvfile.flush()` – forces immediate write to disk  

### ⏳ Time Tracking

- `time.strftime()` – captures timestamps for each entry  
- Running second counter increments automatically for monitoring elapsed time  

---

## 📦 Configuration

- Change `'COM21'` to your device’s COM port  
- Adjust baud rate to match your device (e.g., 9600, 115200)  
- Modify `fieldnames` to log additional sensor data  
- Optional: auto-generate CSV filenames using timestamps  

---

## 🧪 Example Usage

```python
import serial
import csv
import time

# Setup serial
ser = serial.Serial('COM21', 115200, timeout=5)

# Setup CSV
fieldnames = ["Timestamp", "FenceBreachCount", "Temp", "SecondCount"]
with open("Samples_data_logger.csv", mode="w", newline='') as csvfile:
    writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
    writer.writeheader()
    timecount = 0

    while True:
        line = ser.readline().decode('utf-8').strip()
        if line:
            data = line.split(',')
            row = {
                "Timestamp": time.strftime("%Y-%m-%d %H:%M:%S"),
                "FenceBreachCount": data[0],
                "Temp": data[3],
                "SecondCount": timecount
            }
            print(row)
            writer.writerow(row)
            csvfile.flush()

