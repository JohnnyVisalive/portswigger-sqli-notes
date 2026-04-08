## **1. Obsidian KQL Cheat Sheet (Dataview & Syntax)**

In Obsidian, "KQL" often refers to using **Dataview** (which uses a KQL-like logic) or simply documenting **Kusto** for work.

### **Dataview (Obsidian's "KQL")**

Use this to query your own notes.

- **List all notes in a folder:**
    
    Code snippet
    
    ```
    LIST FROM "Security/Research"
    ```
    
- **Table of notes with a specific tag:**
    
    Code snippet
    
    ```
    TABLE date, status FROM #threat-hunt
    WHERE contains(file.name, "APT")
    SORT date DESC
    ```
    

### **Kusto (KQL) Code Blocks**

To get proper syntax highlighting for your KQL snippets in Obsidian, use the `kusto` or `kql` language identifier:

Markdown

````
```kusto
DeviceProcessEvents
| where FileName == "powershell.exe"
| take 10
```
````

---

## **2. Microsoft Defender for Endpoint (MDE) Cheat Sheet**

MDE uses the **Advanced Hunting** schema. The focus here is on **Devices** and **Processes**.

|**Goal**|**KQL Operator / Table**|**Example**|
|---|---|---|
|**Find a Process**|`DeviceProcessEvents`|`|
|**Network Connections**|`DeviceNetworkEvents`|`|
|**File Creation**|`DeviceFileEvents`|`|
|**Specific Device**|`where DeviceName has "L-Host"`|Filters for a specific laptop or host.|
|**Time Filter**|`|where Timestamp > ago(7d)`|

**Pro Tip for Interview:** Mention that in Defender, the time column is always **`Timestamp`**.

---

## **3. Microsoft Sentinel Cheat Sheet**

Sentinel is "The Big Picture." It aggregates logs from everywhere (Office 365, Firewalls, AWS, etc.).

|**Goal**|**KQL Operator / Table**|**Example**|
|---|---|---|
|**Search All Logs**|`search "malicious-ip"`|Quickest way to find a string everywhere.|
|**Sign-in Logs**|`SigninLogs`|`|
|**Security Alerts**|`SecurityAlert`|`|
|**Audit Logs**|`AuditLogs`|`|
|**Aggregation**|`summarize count()`|`|

**Pro Tip for Interview:** In Sentinel, the time column is usually **`TimeGenerated`**. Knowing the difference between `Timestamp` (Defender) and `TimeGenerated` (Sentinel) shows you’ve actually used the tools.

---

## **4. The "Golden" KQL Operators (Both Tools)**

If you forget everything else, remember these five:

1. **`| where`**: Filters the data (like a sieve).
    
2. **`| project`**: Selects only the columns you want to see.
    
3. **`| extend`**: Creates a _new_ column (e.g., `| extend FullPath = concat(FolderPath, FileName)`).
    
4. **`| summarize`**: Groups data (e.g., `count() by AccountName`).
    
5. **`| join`**: Combines two tables (e.g., matching a `DeviceID` from an Alert to a User in `SigninLogs`).
    

### **Common Security Logic**

> **Detecting "Impossible Travel" (Basic):**
> 
> `SigninLogs | summarize count() by UserPrincipalName, Location | where count > 1`