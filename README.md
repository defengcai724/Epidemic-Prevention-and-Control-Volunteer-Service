🧾 一、需求說明書（Software Requirements Specification, SRS）
1.1 專案簡介

本系統旨在建立一套 疫情防控志願服務數位化管理平台，整合志願者管理、任務派遣、出勤紀錄、物資追蹤與公告通知等功能，協助校園、社區與政府單位於疫情期間進行高效率的志願者協調與防疫資訊管理。

1.2 系統目標

以 Android App 為前端，提供即時、直覺的志願者行動操作介面

提供 RESTful API 以支援跨平台應用整合

強化志願者任務派遣與出勤簽到效率

提供防疫物資與公告資訊的即時管理

1.3 使用者角色
角色	權限與職責
管理員	管理志願者、任務、物資、公告、報表、權限與日誌
志願者	查看任務、簽到出勤、上傳任務回報、接收公告與推播通知
1.4 功能需求
模組	功能描述
志願者管理	註冊、查詢、停權、服務紀錄查閱
任務派遣	任務建立、指派、進度管理
出勤簽到	GPS/QR/手動簽到、時數自動計算
物資管理	庫存登錄、領用紀錄、數量警示
公告通知	發布公告、任務提醒、推播通知
系統管理	登入驗證、角色權限、日誌追蹤
報表管理	任務完成率、志願時數、物資使用統計
1.5 非功能需求

效能：API 回應時間 < 1 秒

安全性：JWT 驗證、資料庫存取權限控管

可維護性：採分層架構與 Repository Pattern

擴充性：支援跨平台（未來 Flutter/iOS）

🧩 二、概要設計說明書（System Design Specification, SDS）
2.1 系統總體結構

系統採用 前後端分離架構：

Android 前端：志願者行動操作

ASP.NET Core Web API：後端服務邏輯

MySQL：資料儲存與查詢

2.2 模組設計概述
模組	功能概要
AuthController	登入、註冊、Token 驗證
VolunteersController	志願者資料 CRUD
TasksController	任務建立與指派
AttendanceController	出勤紀錄上傳與查詢
SuppliesController	防疫物資存取管理
AnnouncementsController	公告與推播管理
ReportsController	統計報表產生
2.3 系統資料流

App 透過 REST API 發送 JSON 請求

Controller 接收後交由 Service 處理業務邏輯

Repository 層透過 EF Core 存取 MySQL

結果回傳 JSON 給 Android 前端顯示

2.4 技術框架

Android：Kotlin + MVVM + Retrofit

Backend：ASP.NET Core + EF Core

Database：MySQL + Code First Migration

Authentication：JWT + ASP.NET Identity

🧠 三、詳細設計說明書（Detailed Design Document, DDD）
3.1 類別設計（Class Design）
3.1.1 實體類別（Domain Models）
類別	屬性	關聯
Volunteer	Id, Name, Phone, Status, TotalHours	Assignment*, Attendance*
Task	Id, Title, Description, Status, Date	Assignment*, Attendance*
Assignment	Id, VolunteerId, TaskId, AssignedDate	Volunteer, Task
Attendance	Id, VolunteerId, TaskId, CheckInTime, Location	Volunteer, Task
Supply	Id, Name, Quantity, Type	SupplyRecord*
Announcement	Id, Title, Content, PublishDate	Volunteer*
AuditLog	Id, AdminId, Action, Time	Admin
3.1.2 Service 層範例
public class TaskService {
    private readonly ITaskRepository _taskRepo;
    public TaskService(ITaskRepository taskRepo) {
        _taskRepo = taskRepo;
    }
    public async Task<TaskDto> CreateTaskAsync(TaskDto dto) {
        var entity = new Task { Title = dto.Title, Status = "未開始" };
        await _taskRepo.AddAsync(entity);
        return dto;
    }
}

3.2 資料庫設計

Volunteer(VolunteerId PK, Name, Phone, Role, Status)

Task(TaskId PK, Title, Date, Status)

Assignment(AssignmentId PK, VolunteerId FK, TaskId FK)

Attendance(AttendanceId PK, VolunteerId FK, TaskId FK, CheckInTime)

Supply(SupplyId PK, Name, Quantity)

Announcement(AnnouncementId PK, Title, Content, PublishDate)

AuditLog(LogId PK, AdminId, Action, Time)

3.3 API 介面格式範例
POST /api/tasks
{
  "title": "體溫量測",
  "description": "學校入口體溫測量任務",
  "date": "2025-01-05"
}
Response:
{
  "status": "success",
  "taskId": 12
}

🧪 四、測試計畫（Software Test Plan, STP）
4.1 測試目標

驗證系統各模組功能正確性、穩定性、安全性與效能，確保符合需求規格。

4.2 測試範圍
模組	測試項目
Auth	登入驗證、Token 有效性
Volunteer	註冊、編輯、查詢、停權
Task	任務建立、指派、狀態更新
Attendance	簽到時間、GPS 紀錄、重複簽到檢查
Supply	庫存更新、領用紀錄、低量警示
Announcement	發布、查詢、推播通知
Report	統計計算、PDF 匯出
4.3 測試類型

單元測試 (Unit Test)：Service 與 Repository

整合測試 (Integration Test)：Controller 與 API 流程

系統測試 (System Test)：端到端操作（Android ↔ API ↔ DB）

效能測試 (Performance Test)：JMeter 模擬 1000 並發使用者

安全測試 (Security Test)：SQL Injection、Token 篡改、存取控制

使用者驗收測試 (UAT)：由實際志願者操作驗證流程

4.4 測試環境
項目	規格
作業系統	Windows Server 2022 / Android 12
開發工具	Visual Studio 2022, Android Studio
資料庫	MySQL 8.0
測試工具	Postman, JMeter, xUnit
4.5 測試通過標準

所有核心功能測試案例通過率 ≥ 95%

API 平均回應時間 < 1 秒

無高嚴重性安全漏洞
---
