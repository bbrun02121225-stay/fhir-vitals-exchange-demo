# VitalBridge  
**FHIR-based Cross-Institution Transfer Platform**

---

## 作品簡介｜Project Overview

**VitalBridge** 是一個以 **HL7 FHIR 標準**為基礎的跨院轉診資料共享系統，  
本作品採「**實作導向**」方式完成，完整涵蓋 **情境分析、系統規劃與可運行實作**，
並聚焦於解決真實臨床轉診過程中的資料斷裂問題。

本專案並非僅為技術展示，而是以**臨床實際需求**為出發點，
設計一套可被理解、可被操作、可被延伸的轉診資料交換流程。

---

## 競賽情境說明｜Scenario Description

### 中文
在病人跨院轉診時，轉入院往往無法即時取得轉出院的：

- 近期生命徵象  
- 關鍵檢驗結果（如腎功能、電解質、心肌指標）  
- 影像檢查結果  

導致以下問題：

- 重複量測、重複抽血、重複影像檢查  
- 治療延誤  
- 病人風險上升（抽血、輻射暴露）  

### English
During cross-institution transfers, receiving hospitals often lack access to:

- Recent vital signs  
- Critical laboratory results  
- Imaging examination outcomes  

This results in duplicated procedures, delayed decisions, and increased patient risk.

---

## 設計目標｜Design Goals

### 中文
本作品的設計目標為：

1. 建立清楚的跨院轉診資料交換流程  
2. 以 FHIR 標準確保資料可重用性  
3. 降低不必要的臨床重複作業  
4. 讓資料呈現方式符合臨床閱讀習慣  

### English
The project aims to:

1. Define a clear cross-institution transfer workflow  
2. Enable data reuse through FHIR standards  
3. Reduce unnecessary repeated clinical procedures  
4. Present data in a clinician-friendly format  

---

## 系統架構與流程｜System Workflow

### 核心設計原則｜Core Principle

> **Patient.id 作為唯一轉診識別**

---

### 轉出院（Upload Side）

- 建立病人（Patient）
- 上傳生命徵象（Observation – vital-signs）
- 上傳關鍵檢驗（Observation – laboratory）
- 上傳影像報告摘要（DiagnosticReport）

📌 目的：  
**在轉診發生前，完整準備可重用的臨床資料**

---

### 轉入院（Download Side）

- 不建立、不修改病人資料  
- 僅以 Patient.id 查詢  
- 提供：
  - 最近資料查詢
  - 全部資料查詢
  - **生命徵象＋檢驗＋影像的整合檢視**

📌 目的：  
**病人到院即可使用既有資料，避免重複處置**

---

## 實作內容｜Implementation Details

### 使用之 FHIR Resource

- **Patient**
- **Observation**
  - vital-signs
  - laboratory
- **DiagnosticReport**

### 資料範圍

- 生命徵象：體重  
- 關鍵檢驗：
  - Creatinine
  - Potassium
  - Hemoglobin
  - Glucose
  - Troponin I
- 影像報告：CT / X-ray / MRI / Ultrasound（摘要）

---

## 技術實作說明｜Technical Implementation

- **Frontend**
  - HTML / CSS / Vanilla JavaScript
  - 人類可讀表格呈現（避免 raw JSON）

- **Backend**
  - FHIR Server（RESTful API）

- **標準**
  - HL7 FHIR R4

- **實作特色**
  - 明確區分轉出院 / 轉入院角色
  - 查詢端不進行任何病人建立或修改
  - 提供「全部資料彙總」功能，模擬真實臨床需求

---

## 學習與實作重點｜Learning Outcomes

### 中文
透過本專案，實作者實際完成：

- FHIR Resource 設計與操作
- RESTful API 串接
- 臨床情境轉換為系統流程
- 前端資料可視化設計
- 從 Demo 重構為完整應用的系統思維

### English
This project demonstrates hands-on experience in:

- FHIR-based resource modeling
- RESTful API integration
- Translating clinical scenarios into system workflows
- Frontend data visualization
- Evolving a demo into a runnable end-to-end solution

---

## 專案價值｜Project Value

### 中文
VitalBridge 展示了：

- 對臨床痛點的理解  
- 對 FHIR 標準的正確應用  
- 對資料互通與病人安全的重視  

### English
VitalBridge demonstrates:

- Clear understanding of clinical pain points  
- Proper application of FHIR standards  
- Focus on interoperability and patient safety  

---

## 未來延伸｜Future Directions

- 多院 FHIR Server 串接
- 存取紀錄與審計機制
- Clinical Decision Support（CDS）
- SMART on FHIR 應用整合

---

## 聲明｜Disclaimer

本作品所使用之所有資料皆為**模擬資料**，不包含任何可識別個人資訊。  
本系統僅用於 **競賽展示、教學與研究用途**。
