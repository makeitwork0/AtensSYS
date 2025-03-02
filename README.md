# AtensSYS
# **Atensense and Atenvault: System Description and Components**

## **System Overview**
Atensense and Atenvault form an automated student attendance and emergency response system. Atensense is a classroom-based device that logs student attendance using RFID and fingerprint authentication, while Atenvault serves as a central server that processes attendance data, sends notifications, and manages emergency alerts. The system ensures real-time data transmission, high security, and automated notifications through SMS and email.

---

## **System Components**

### **Atensense (Classroom Device)**
- **ESP32-S3** – The primary microcontroller handling data processing and communication.
- **PN532 RFID Scanner** – Reads RFID cards for student identification.
- **RS608 Fingerprint Sensor** – Provides biometric authentication for attendance logging.
- **MPU6050 Sensor** – Detects earthquakes by monitoring sudden movements.
- **MQ2 Sensor** – Detects smoke and fire hazards.
- **ILI9341 TFT Display** – Displays attendance status, emergency alerts, and device status.
- **Buzzer** – Sounds alerts for successful attendance logging and emergencies.
- **Rotary Encoder** – Allows menu navigation and manual emergency activation.
- **INA219 Current Sensor** – Monitors battery voltage and power usage.
- **SIM800L Module** – Provides backup mobile data communication in case of Wi-Fi failure.
- **SD Card Module** – Stores attendance data when no network connection is available.

### **Atenvault (Central Server)**
- **ESP32-S3** – Handles communication with Atensense and data processing.
- **Wi-Fi Module** – Facilitates real-time data transmission to Google Sheets.
- **SIM800L Module** – Sends SMS notifications for absent or late students.
- **SD Card Module** – Stores backup data in case of network failure.
- **ILI9341 TFT Display** – Shows connection status and real-time updates.

---

## **Google Sheets, AppSheet, and Google Apps Script Integration**

### **Google Sheets**
The system uses Google Sheets for structured data management:
- **LOGDATA**: Captures raw attendance logs from Atensense.
- **REFDATA**: Stores student reference information (RFID, names, sections, and unique identifiers).
- **OUTPUTDATA**: Processes and finalizes attendance records for reporting.
- **ABSENT_9C**: Tracks students who failed to check in before the cutoff time.

### **AppSheet (Frontend Interface)**
AppSheet provides a user-friendly interface for administrators and teachers to:
- View attendance records in real-time.
- Monitor late and absent students.
- Access emergency alerts and historical attendance logs.

### **Google Apps Script (Automation Engine)**
Google Apps Script automates attendance tracking and notification processes:
- `checkSheet(spreadsheet, sheetName)`: Verifies the existence of required sheets.
- `processAttendance()`: Reads attendance logs, validates student information, and updates Google Sheets.
- `sendLateAndAbsentNotifications()`: Identifies late or absent students and triggers email/SMS notifications.
- `sendSmsNotifications()`: Sends SMS alerts to predefined recipients.
- `sendEmailNotifications()`: Emails attendance summaries and emergency reports to administrators.

## **System Logic and Functionality**

### **1. Attendance Logging Process**
1. A student scans their **RFID card** or **fingerprint** on Atensense.
2. The system verifies the student’s ID against the database.
3. The **TFT display** confirms attendance and logs the timestamp.
4. Data is transmitted to **Atenvault**, which updates **Google Sheets**.
5. If a student fails to check in before the cutoff time, their status is marked as **late** or **absent**.
6. Atenvault sends **SMS and email notifications** for late or absent students.

### **2. Emergency Detection and Response**
1. **Automatic Detection**
   - The **MPU6050** detects unusual vibrations (earthquakes), triggering an alert.
   - The **MQ2 sensor** detects smoke, indicating a potential fire.
   - The **buzzer and display** provide immediate alerts.
2. **Manual Activation**
   - A registered user can trigger an emergency using a **fingerprint scan**.
   - The **rotary encoder** allows selecting the emergency type before sending an alert.
3. **Atenvault receives the emergency signal** and updates Google Sheets, logging the event.

### **3. Network Management and Data Security**
1. **Wi-Fi First Approach** – The system attempts to transmit data via Wi-Fi.
2. **Mobile Data Backup** – If Wi-Fi is unavailable, it switches to **SIM800L mobile data**.
3. **Offline Storage** – If both connections fail, attendance and emergency data are stored on an **SD card**.
4. **Data Security**
   - All data is **encrypted** before transmission.
   - Stored data is protected from unauthorized access.

### **4. Automated Processing and Notifications**
1. **Google Apps Script** automates attendance tracking:
   - **LOGDATA**: Stores raw attendance logs.
   - **REFDATA**: Matches student IDs with names.
   - **OUTPUTDATA**: Processes and finalizes attendance records.
2. **Notification System**
   - `sendLateAndAbsentNotifications()` – Identifies students who are late or absent and sends notifications.
   - `sendSmsNotifications()` – Sends SMS alerts to predefined recipients.
   - `sendEmailNotifications()` – Emails attendance reports to administrators.

---



