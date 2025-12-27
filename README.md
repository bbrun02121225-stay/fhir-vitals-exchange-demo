# VitalBridge  
**FHIR-based Cross-Institution Transfer Platform**

---

## 作品簡介｜Project Overview

**VitalBridge** 是一個以 **HL7 FHIR 標準** 為基礎的跨院轉診資料共享系統。  
本作品採「**實作導向**」方式完成，完整涵蓋 **情境分析、系統規劃與可運行實作**，
並聚焦於解決真實臨床轉診過程中的資料斷裂問題。

本專案並非僅為技術展示，而是以 **臨床實際需求** 為出發點，
設計一套 **可被理解、可被操作、可被信任、可被延伸** 的轉診資料交換流程。

---

## 競賽情境說明｜Scenario Description

### 中文
在病人跨院轉診時，轉入院往往無法即時取得轉出院的：

- 近期生命徵象  
- 關鍵檢驗結果（如腎功能、電解質、心肌指標）  
- 影像檢查結果  

導致以下問題：

- 重複量測、重複抽血、重複影像檢查  
- 治療決策延誤  
- 病人風險上升（抽血不適、輻射暴露）  

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

1. 建立清楚且可重現的跨院轉診資料交換流程  
2. 以 FHIR 標準確保資料可重用性與一致性  
3. 降低不必要的臨床重複作業與病人風險  
4. 讓資料呈現方式符合臨床人員的閱讀與判讀習慣  

### English
The project aims to:

1. Define a clear and reproducible cross-institution transfer workflow  
2. Enable data reuse through FHIR standards  
3. Reduce unnecessary repeated clinical procedures and patient risk  
4. Present data in a clinician-friendly and interpretable format  

---

## 系統架構與流程｜System Workflow

### 核心設計原則｜Core Principle

> **Patient.id 作為唯一轉診識別（Transfer Credential）**

VitalBridge 不嘗試建立跨院主索引或合併身份，
而是以 FHIR 原生設計為基礎，將 **Patient.id** 視為一次轉診過程中的唯一識別憑證。

---

### 轉出院（Upload Side）

- 建立病人（Patient）
- 上傳生命徵象（Observation – vital-signs）
- 上傳關鍵檢驗（Observation – laboratory）
- 上傳影像報告摘要（DiagnosticReport）

📌 目的：  
**在轉診發生前，完整準備可被重用且具臨床價值的資料**

---

### 轉入院（Download Side）

- 不建立、不修改病人資料  
- 僅以 Patient.id 進行查詢（Read-only）  
- 提供：
  - 最近資料查詢
  - 全部資料查詢
  - **生命徵象＋檢驗＋影像的整合檢視**

📌 目的：  
**病人到院即可使用既有資料，避免重複處置**

---

## 🔐 轉入院可信任歷史｜Trusted Clinical History（評審重點）

在跨院轉診場景中，「資料是否存在」並不足以支撐臨床決策，  
**資料是否可信（Trustworthiness）才是轉入院能否採用的關鍵。**

VitalBridge 在轉入院查詢流程中，明確納入 **可信任歷史（Trusted Clinical History）** 的設計概念：

### 1️⃣ 資料來源明確（Source Transparency）
- 所有資料皆來自 FHIR Server 中的正式資源  
- 僅使用標準 FHIR Resource（Patient / Observation / DiagnosticReport）  
- 非人工轉抄、非自由輸入資料  

### 2️⃣ 不可變更原則（Read-only on Receive Side）
- 轉入院端 **不進行任何 Create / Update 操作**  
- 僅透過 GET 查詢既有資料  
- 避免責任歸屬混淆與資料污染  

### 3️⃣ 時序可追溯（Temporal Validity）
- 所有資料皆包含：
  - `effectiveDateTime` 或 `issued`
- 轉入院可依時間判斷資料是否仍具臨床參考價值  

### 4️⃣ 臨床語意清楚（Clinical Context Integrity）
- 依 FHIR `category` 與 `resourceType` 明確區分：
  - 生命徵象（vital-signs）
  - 檢驗（laboratory）
  - 影像（DiagnosticReport）
- 避免不同臨床語意資料混用  

### 5️⃣ 臨床可讀呈現（Clinician-readable Evidence）
- 不顯示 raw JSON  
- 以結構化表格呈現：
  - 清楚欄位
  - 明確分類
  - 方便快速判讀  

### English Summary
VitalBridge ensures trusted clinical history by enforcing read-only access,
clear resource typing, timestamp traceability, and clinician-friendly presentation,
aligning with real-world clinical adoption requirements.

---

## 實作內容｜Implementation Details

### 使用之 FHIR Resource

- **Patient**
- **Observation**
  - vital-signs
  - laboratory
- **DiagnosticReport**

### 資料範圍

- **生命徵象**：體重  
- **關鍵檢驗**：
  - Creatinine
  - Potassium
  - Hemoglobin
  - Glucose
  - Troponin I
- **影像報告**：CT / X-ray / MRI / Ultrasound（摘要層級）

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
  - 嚴格遵循 FHIR Resource 職責
  - 提供「全部資料彙總」功能，模擬真實臨床使用情境

---

## 學習與實作重點｜Learning Outcomes

### 中文
透過本專案，實作者實際完成：

- FHIR Resource 設計與實作
- RESTful API 串接與錯誤處理
- 將臨床情境轉換為系統流程
- 前端臨床資料可視化
- 從 Demo 重構為完整系統的設計思維

### English
This project demonstrates hands-on experience in:

- FHIR-based resource modeling
- RESTful API integration
- Translating clinical scenarios into system workflows
- Clinician-oriented data visualization
- Evolving a demo into a transfer-ready application

---

## 專案價值｜Project Value

### 中文
VitalBridge 展示了：

- 對真實臨床痛點的理解  
- 對 FHIR 標準的正確且克制的應用  
- 對資料可信度與病人安全的重視  

### English
VitalBridge demonstrates:

- Strong understanding of clinical pain points  
- Proper and disciplined use of FHIR standards  
- Emphasis on data trust and patient safety  

---

## 未來延伸｜Future Directions

- 多院 FHIR Server federation  
- 存取紀錄與審計（Audit Trail / Provenance）  
- Clinical Decision Support（CDS）  
- SMART on FHIR 應用整合  

---

## 聲明｜Disclaimer

本作品所使用之所有資料皆為 **模擬資料**，不包含任何可識別個人資訊。  
本系統僅用於 **競賽展示、教學與研究用途**。
