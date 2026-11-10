COMPUTER-BASED SYSTEMS

Universidad Politécnica de Madrid
Escuela Técnica Superior de Ingeniería de Sistemas Informáticos

FINAL COURSE PROJECT

SmartCane: Smart Walking Cane for Monitoring Patients with Multiple Sclerosis or ALS

PROJECT PROPOSAL

Group: SBC26M02

Team Members:

- Group Leader: Álvaro Alonso Bayarriz
- Juan Manuel Jiménez Pascual
- Luis Tereso Silva
- Yzan Martínez Manzano

---

1. Problem Statement

In Multiple Sclerosis (MS) and Amyotrophic Lateral Sclerosis (ALS), walking ability often deteriorates over time.

In MS, disability is assessed using the Expanded Disability Status Scale (EDSS), ranging from 0 to 10. From level 5.0 onwards, the scale is defined by walking ability, while level 6.0 indicates the need for a walking aid to walk approximately 100 metres [1].

In ALS, the ALS Functional Rating Scale–Revised (ALSFRS-R) is used. It consists of 12 items, with a total score ranging from 0 to 48, including one item specifically assessing walking ability [2].

Both scales are assessed during medical appointments, typically several months apart. As a result, changes in patients' conditions between appointments are not objectively recorded.

SmartCane is a smart walking cane equipped with an integrated ESP32 microcontroller. While the patient uses the cane normally, the system records walking distance, walking duration, walking continuity, rest periods, and the amount of weight applied to the handle.

Rather than evaluating these measurements against generic reference values, SmartCane compares them with each patient's individual baseline. The first two weeks of use establish this baseline, allowing the system to identify whether the patient's mobility is improving, remaining stable, or deteriorating.

All collected information is transmitted to ThingsBoard, where caregivers and healthcare professionals can monitor the patient's condition.

Through Telegram, the system provides periodic reports and notifications when necessary, including:

- Emergency assistance requests containing the patient's location.
- Low-battery warnings.
- Alerts indicating sustained deterioration in mobility.

---

2. Sensors and Actuators

Component (Suggested Model)| Type| Function| Interface
IMU (e.g., MPU-6050 or ADXL345)| Sensor| Detects cane contacts/steps, walking periods, cadence, and wake-up from deep sleep| I²C + INT
Handle FSR (e.g., FSR 402)| Sensor| Measures weight-bearing support on the cane and performs a weekly grip test (trend monitoring only)| ADC
GPS (e.g., u-blox NEO-6M)| Sensor| Provides location for SOS alerts and measures distance travelled during outdoor walking sessions| UART
SOS Push Button| Sensor| Triggers emergency alerts and wakes the device| GPIO
Battery Voltage Divider| Sensor| Monitors battery level and generates low-battery warnings| ADC
Buzzer| Actuator| Provides audible notifications (SOS sent, low battery, cane location assistance)| GPIO
Vibration Motor (with transistor and diode)| Actuator| Provides haptic notifications to the patient| GPIO
OLED Display (e.g., SSD1306), optional| Actuator| Displays time, battery level, and system status| I²C

Power Supply: Li-ion/LiPo battery with charging circuit.

---

3. System Architecture

SmartCane is built around an ESP32 microcontroller running ESP-IDF and FreeRTOS.

The ESP32 collects information from the sensors integrated into the walking cane and controls the notification actuators.

Sensors

- IMU: Accelerometer and gyroscope connected through I²C, with an interrupt signal for motion detection and wake-up.
- FSR: Force-sensitive resistor connected to an ADC input.
- GPS: GPS module connected through UART, providing outdoor positioning.
- SOS Button: Emergency push button connected to a GPIO pin.
- Battery Monitoring: Li-ion battery with an ADC voltage divider.

Actuators

- Buzzer: Audible notifications controlled through GPIO.
- Vibration Motor: Haptic feedback controlled through GPIO and a transistor.
- Optional OLED Display: I²C display showing time, battery level, and system status.

Communication and Data Processing

The ESP32 connects to the internet through a home Wi-Fi network or a mobile phone hotspot.

Using MQTT or HTTPS, it sends telemetry data, alerts, and over-the-air (OTA) update information to ThingsBoard.

ThingsBoard is responsible for:

- Telemetry collection and visualisation.
- Mobility trend analysis.
- Alert management.
- Dashboard monitoring.

Telegram Integration

ThingsBoard communicates with a Telegram bot, allowing caregivers and healthcare professionals to receive:

- Emergency notifications.
- Patient location.
- Periodic reports.
- Device-related alerts.

Configuration and Firmware Updates

The system supports OTA firmware updates and initial configuration through an ESP32 access point.

Users can configure the Wi-Fi connection, baseline settings, and alert thresholds through a mobile phone or computer.

---

4. Authorship and Originality of the Idea

The SmartCane project idea originated within our team.

The proposed system is a connected walking cane designed to locate the patient, send emergency SOS alerts, and continuously record daily mobility data to determine whether a person with MS or ALS is improving, remaining stable, or deteriorating.

This statement is included simply to document the original source of the project idea.

---

5. References

[1] Kurtzke, J. F. Neurology. 1983;33(11):1444–1452. (EDSS).

[2] Cedarbaum, J. M., et al. Journal of the Neurological Sciences. 1999;169(1–2):13–21. (ALSFRS-R).
