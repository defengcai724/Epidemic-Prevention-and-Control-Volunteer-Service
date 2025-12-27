# 📱 基於 Android 的疫情防控志願服務系統  
Android-based Epidemic Prevention Volunteer Service System

本專案為一套 **基於 Android 的疫情防控志願服務管理系統**，結合 **Android App、ASP.NET Core Web API 與 MySQL** 建置，提供志願者管理、任務派遣、出勤簽到、防疫物資管理與疫情公告等功能。

系統採用 **前後端分離（RESTful API）** 與 **多層式架構（Layered Architecture）**，具備良好的可維護性、擴充性與實務應用價值，適用於校園、社區與政府防疫單位。

---

## 📌 系統特色
- 前後端分離（RESTful API）
- Android 行動裝置即時操作
- 分層架構（Controller / Service / Repository）
- 支援志願者、任務、簽到、物資、公告與報表管理
- 使用 Entity Framework Core 操作 MySQL
- 架構清楚，適合中大型防疫管理系統

---

## 🏗️ 系統架構概觀

[ Android App ]
|
| HTTP / JSON (REST API)
v
ASP.NET Core Web API
├─ Controllers
│ ├─ AuthController
│ ├─ VolunteersController
│ ├─ TasksController
│ ├─ AssignmentsController
│ ├─ AttendanceController
│ ├─ SuppliesController
│ ├─ AnnouncementsController
│ └─ ReportsController
│
├─ Application / Service Layer
│ ├─ VolunteerService
│ ├─ TaskService
│ ├─ AssignmentService
│ ├─ AttendanceService
│ ├─ SupplyService
│ ├─ AnnouncementService
│ ├─ ReportService
│ └─ NotificationService
│
├─ Infrastructure / Repository
│ ├─ VolunteerRepository
│ ├─ TaskRepository
│ ├─ AssignmentRepository
│ ├─ AttendanceRepository
│ ├─ SupplyRepository
│ ├─ AnnouncementRepository
│ ├─ ReportRepository
│ └─ AuditLogRepository
│
└─ MySQL Database


---

## 🧩 資料模型與關聯
- **志願者（Volunteer）** ⇄ **任務指派（Assignment）**
- **任務（Task）** ⇄ **出勤紀錄（Attendance）**
- **志願者** ⇄ **出勤紀錄**
- **防疫物資（Supply）** ⇄ **物資領用紀錄（SupplyRecord）**
- **管理員（Admin）** ⇄ **操作日誌（AuditLog）**
- **公告（Announcement）** ⇄ **志願者**

---

## 🚀 主要功能模組

### 👤 志願者管理
- 志願者註冊 / 編輯 / 停權
- 志願者服務紀錄查詢
- 出勤與任務歷史追蹤

### 🗓️ 任務派遣與管理
- 防疫任務建立（體溫量測、物資發放、管制站值勤）
- 任務指派與人力配置
- 任務狀態管理（未開始 / 進行中 / 已完成）

### ✅ 出勤與回報
- QR Code / GPS / 手動簽到
- 服務時數自動累積
- 任務完成回報（支援照片上傳）

### 📦 防疫物資管理
- 物資建檔（口罩、酒精、快篩）
- 庫存數量控管
- 領用與發放紀錄追蹤

### 📢 公告與通知
- 疫情防控公告發布
- 任務通知與異動提醒
- 志願服務推播通知

### ⚙️ 系統管理與報表
- 登入與角色權限（管理員 / 志願者）
- 操作日誌（Audit Log）
- 防疫服務統計報表
  - 志願服務時數
  - 任務完成率
  - 物資使用統計

---

## 📁 專案目錄結構

EpidemicVolunteerSystem/
├─ src/
│ ├─ Epidemic.Api/ # ASP.NET Core Web API
│ ├─ Epidemic.Application/ # Service Layer
│ ├─ Epidemic.Domain/ # Domain Model
│ ├─ Epidemic.Infrastructure/ # EF Core / Repository
│ └─ Epidemic.Android/ # Android App
│
└─ docs/ # UML / 系統文件


---

## 🛠️ 技術棧
- **Android**：Java / Kotlin
- **Backend**：ASP.NET Core Web API
- **ORM**：Entity Framework Core
- **Database**：MySQL
- **Architecture**：
  - Layered Architecture
  - Repository Pattern
  - Service Pattern
  - RESTful API

---

## 🔮 未來擴充方向
- 📍 GPS 定位與軌跡紀錄
- 🔔 Firebase Cloud Messaging（FCM）
- 📊 防疫數據分析儀表板
- 📱 Flutter / iOS 跨平台支援
- 🪪 志願服務證明 PDF 自動產生

---
