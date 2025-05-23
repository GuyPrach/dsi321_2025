# Wildfire Dashboard — โครงการรายวิชา DSI324

## Project Overview

โปรเจกต์นี้มีเป้าหมายในการพัฒนา **ระบบแจ้งเตือนไฟป่าแบบ near real-time** โดยดึงข้อมูลจุดความร้อน (hotspot) จาก **FIRMS-NASA** และข้อมูลสภาพอากาศจาก **OpenWeatherAPI** ระบบนี้พัฒนาภายใต้แนวคิดของ data governance เป็นส่วนหนึ่งของรายวิชา **DSI324, DSI321**

แผนภาพที่สร้างขึ้นนี้มีเป้าหมายที่จะช่วยสนับสนุนการตัดสินใจของคณะอนุกรรมาธิการการจัดการภัยพิบัติแห่งชาติ โดยแสดงผลผ่าน dashboard แบบ interactive พร้อม data pipeline อัตโนมัติ และการวิเคราะห์ปัจจัยการเกิดไฟป่าด้วย machine learning

---

## Part 1: Technical Work

### 1. Repository Setup
- ชื่อ repository: `dsi321_2025`
- โครงสร้างไฟล์:
  - `df_thai/` สำหรับเก็บไฟล์ Parquet
  - `Machine_learning.ipynb` สำหรับโมเดล machine learning
  - `visualization/` สำหรับ dashboard
  - `api/` สำหรับ script ดึงข้อมูลจาก FIRMS และ OpenWeather

### 2. Commit Frequency
- มีการ commit มากกว่า 5 ครั้งต่อสัปดาห์ ติดต่อกัน 3 สัปดาห์
- ตัวอย่างข้อความ commit:
  - `Move table to top-right`
  - `Clean code and delete some part`
  - `Optimize load performance`

### 3. Dataset Quality

| เงื่อนไข                              | ทำได้? | หมายเหตุ |
|--------------------------------------|--------|----------|
| จำนวน record ≥ 1,000                 | ✔️     | เก็บข้อมูลทุก 15 นาที ผ่าน Prefect รวมมากกว่า 96 records/วัน |
| ครอบคลุมเวลา ≥ 24 ชั่วโมง           | ✔️     | ดึงข้อมูลแบบ schedule รายวันอัตโนมัติ |
| ความสมบูรณ์ของข้อมูล ≥ 90%         | ✔️     | ระบุเป็น KPI หลักใน data integration section |
| ไม่มี column ประเภท `object`        | ✔️     | มีการกำหนด schema และตรวจสอบผ่าน Parquet |
| ไม่มีข้อมูลซ้ำ                       | ✔️     | ETL flow ของ Prefect มีการจัดการ deduplication |

---

### 4. Schema Consistency
- โครงสร้างข้อมูล:
  - `timestamp` (datetime), `latitude`, `longitude` (float)
  - `brightness`, `confidence`, `temperature`, `humidity`, `wind_speed` (float)
- รูปแบบไฟล์: **Parquet**
- เครื่องมือที่ใช้: **GeoPandas**, **LakeFS**, CKAN Catalog

---

## Part 2: Project Reporting

### 1. Visualization
- Streamlit dashboard แสดง:
  - Heatmap จุด hotspot ต่อจังหวัด ด้วย Folium
  - Scatter plot เช่น ความสัมพันธ์ระหว่าง temperature กับ brightness
  - Correlation matrix
- ไลบรารีที่ใช้: Streamlit, GeoPandas, Folium, Matplotlib

### 2. Machine Learning
- โมเดล: **Random Forest**
- Features: `temperature`, `humidity`, `wind_speed`
- Target: `brightness`
- วิเคราะห์ Feature importance:
  - พบว่า temperature และ humidity มีอิทธิพลสูงสุด
- วัตถุประสงค์:
  - วิเคราะห์ความเสี่ยงเชิงสถิติที่ส่งผลต่อการเกิด hotspot

---

## Tools

| ส่วนงาน             | เทคโนโลยีที่ใช้                  |
|---------------------|-----------------------------------|
| Ingestion           | Prefect                           |
| Storage             | LakeFS + Parquet                  |
| Visualization       | Streamlit + Folium                |
| Geospatial Engine   | GeoPandas                         |
| ML Engine           | Scikit-learn (Random Forest)      |
| Metadata Catalog    | CKAN                              |

---

## Project Outcomes

- พิสูจน์การทำงานของระบบ ingestion แบบ near real-time
- สร้าง data pipeline ที่มีความเสถียรและสามารถใช้งานได้จริง
- วิเคราะห์ตัวแปรด้านสภาพอากาศที่มีผลต่อการเกิด hotspot ด้วย machine learning
- ออกแบบให้หน่วยงานรัฐสามารถนำไปใช้งานต่อได้

---

