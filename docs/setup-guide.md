# FlowArmor Setup Guide

Follow these steps to configure FlowArmor in your environment.

---

## ✅ Step 1: Import Solution

Import the FlowArmor solution into your Power Platform environment:

- Locate the solution zip in `/solution`
- Import into your target environment

---

## ✅ Step 2: Create SharePoint List

Create a SharePoint list named: `FA_Telemetry_Log`

---

### Required Columns

| Column Name   | Type                          | Purpose |
|--------------|-------------------------------|--------|
| Title         | Single Line of Text           | Flow Name |
| CorrelationId | Single Line of Text           | Correlation tracking |
| Status        | Choice (Success, Failed)      | Execution result |
| Severity      | Choice (Info, Warning, Error) | Log level |
| ActionName    | Single Line of Text           | Failed action |
| ErrorMessage  | Multiple Lines of Text        | Error details |
| RunId         | Single Line of Text           | Flow run identifier |
| TimestampUtc  | Date and Time                 | Execution time |
| DurationMs    | Number                        | Execution duration |

---

## ✅ Step 3: Configure Environment Variables

Set the following variables in the solution:

- `fa_env_sp_siteUrl` → SharePoint site URL  
- `fa_env_sp_listName` → `FA_Telemetry_Log`  

---

## ✅ Step 4: Validate Flow

Run:


FA_Demo_Reliability_Scaffold_v1

---

## ✅ Expected Behavior

### Success Scenario
- Flow completes successfully  
- No SharePoint log entry  

---

### Failure Scenario
- Flow fails (Terminate)  
- SharePoint entry created  

---

## ⚠️ Important Configuration

Ensure:

- CATCH scope Run After:
  - Has Failed ✅  
  - Has Timed Out ✅  
  - Is Skipped ✅  

---

## ✅ Recommended Enhancements

- Index SharePoint columns:
  - CorrelationId  
  - TimestampUtc  

---

## 🎯 Verification

After execution:

- Check SharePoint list  
- Validate:
  - CorrelationId present  
  - ErrorMessage populated  
  - Status = Failed for failures  

---

FlowArmor is now successfully configured ✅
