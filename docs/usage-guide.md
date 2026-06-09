# FlowArmor Usage Guide

This document demonstrates how to use FlowArmor in real-world enterprise scenarios.

---

## 💼 Scenario 1: SharePoint-Based Order Processing Flow

### 📌 Use Case

An organization processes incoming customer orders stored in a SharePoint list.

Each order must:

- Be validated  
- Be sent to an external API  
- Be updated back in SharePoint  

---

## 🚨 The Problem (Without FlowArmor)

In a typical flow:

- API failures may not stop the flow ❌  
- Flow run shows **Succeeded**, even when API failed ❌  
- Debugging requires opening each action manually ❌  
- No centralized logging ❌  

---

## ✅ Solution (Using FlowArmor)

FlowArmor is used to wrap the business logic and enforce:

- ✅ Standard error handling (TRY/CATCH/FINALLY)  
- ✅ Failure propagation (no silent success)  
- ✅ Structured telemetry logging  
- ✅ CorrelationId for traceability  

---

## 🧭 Implementation Steps

---

### 1. Trigger Flow from SharePoint

Use:
```
When an item is created or modified
```

---

### 2. Place Business Logic inside TRY Scope

Inside `FA_SCOPE_TRY`:

- Get item details  
- Validate order  
- Call external API  
- Update SharePoint  

---

### 3. Configure CATCH Scope

- Set:
```
varHasFailure = true
```

- Normalize error:
- Capture action name  
- Capture error message  

---

### 4. Use FINALLY Scope

- Always runs  
- Generates telemetry output  
- Decides final outcome  

---

### 5. Logging (SharePoint)

If failure:

- Create item in:
```
FA_Telemetry_Log
```
---

## 🧪 Testing the Framework

FlowArmor includes sample **Compose actions** in both parent and child flows to help you simulate success and failure scenarios quickly.

You do NOT need to create new actions, just modify the existing Compose expressions.

---

### 📍 Where to find it

1. Open the flow:
```
FA_Demo_Reliability_Scaffold_v1
```
2. Locate the **Compose action** inside the TRY scope  
(example: `fa_cmp_fakeError`)

---

### ✅ Success Scenario

Edit the Compose expression to:
```
div(1,1)
```
👉 Result:
- Flow runs successfully ✅  
- No SharePoint log is created ✅  

---

### ❌ Failure Scenario

Edit the Compose `fa_cmp_child_forceFailure` expression to:
```
div(1,0)
```
👉 Result:
- Flow throws an error ✅  
- CATCH block captures it ✅  
- SharePoint log entry is created ✅  
- Flow is marked as **Failed** ✅  

---

### 🔁 Apply same logic in Child Flow

You can also repeat this in the child flow:

1. Open:
```
FA_Child_Process_Sample_v1
```
2. Modify the test Compose action  
3. Use `div(1,1)` or `div(1,0)`  

---

✅ This allows you to test:

- Error handling behavior  
- Failure propagation to parent  
- Telemetry logging  
- End-to-end execution flow  

---

## 🔗 How CorrelationId Helps

Each execution has a unique `CorrelationId`.

In this scenario:

- A parent flow processes an order  
- It may call child flows (validation, API calls, updates)  
- The same `CorrelationId` is passed across all steps  

✅ This helps by:

- Identifying which SharePoint log belongs to a specific run  
- Linking parent execution with child actions  
- Simplifying debugging when multiple flows are involved  

ℹ️ Note:  
In the current version, one log entry is created per execution. CorrelationId helps match logs to runs rather than aggregating multiple entries.

---

## 🎯 Outcome

With FlowArmor:

- ✅ No silent failures  
- ✅ Accurate run status  
- ✅ Centralized error visibility  
- ✅ Faster debugging  
- ✅ Repeatable pattern across flows  

---

## 🧠 When to Use FlowArmor

Use FlowArmor when:

- Your flow interacts with APIs  
- Your flow has multiple steps or child flows  
- You need reliable error handling  
- You want centralized logging  
- You need consistent behavior across multiple flows  

---

FlowArmor ensures your flows behave predictably and are easier to debug in production environments ✅
