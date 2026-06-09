# FlowArmor Architecture

FlowArmor introduces a standardized reliability and observability pattern for Power Automate flows.

---

## 🧭 Core Architecture

```text
[ SCOPE: TRY ] --------> (On Failure) --------> [ SCOPE: CATCH ]
              |                                             |
       Business Logic                              Normalize Error
              |                                  Set varFailedFlag = true
              v                                             v
       -----------------------------------------------------------------------
                              [ SCOPE: FINALLY ]
                       Flush Standardized Telemetry JSON
                                      |
                                      v
                        [ CONDITIONAL OUTCOME CHECK ]
                                      |
                 +--------------------+--------------------+
                 v                                         v
        (varFailedFlag == true)                 (Default)
        [ TERMINATE: FAILED ]                 [ NATURAL SUCCESS ]
  (FlowArmor.BusinessFailure)               (Green Run History)
```

---

## 🧱 Core Layers

### 1. Execution Layer
- TRY → runs business logic  
- CATCH → captures and normalizes errors  
- FINALLY → executes always and prepares telemetry  

---

### 2. Reliability Layer
- `varFailedFlag` ensures consistent failure signaling  
- `Terminate` ensures no silent success  
- Parent flows receive accurate status  

---

### 3. Telemetry Layer
- Standard JSON telemetry contract  
- Captures:
  - Flow name  
  - Status  
  - CorrelationId  
  - Error details  

---

### 4. Persistence Layer
- Logs written to external system:
  - SharePoint (v1)
  - Dataverse (planned)
  - Application Insights (planned)

---

### 5. Consumption Layer (External)
- Logs are consumed outside the flow:
  - Monitoring dashboards  
  - Alerting systems  
  - Analytics  

---

## 🔑 Key Principles

### ✅ Separation of Concerns
- FlowArmor → generates telemetry  
- External systems → consume telemetry  

---

### ✅ Failure Propagation
- Ensures accurate flow status using Terminate  
- Eliminates misleading "Succeeded" runs  

---

### ✅ Standardization
- Consistent structure across all flows  
- Predictable debugging experience  

---

### ✅ Extensibility
- Supports evolution without changing core pattern  
- Storage and monitoring systems can vary independently  

---

## 🔗 Data Flow Summary


Flow Execution 
↓ 
Error Handling 
↓
Telemetry Generation
↓
External Storage (SharePoint)
↓
Monitoring / Analytics / Alerts
