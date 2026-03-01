# 🟡 Module 4: Monitor and Troubleshoot Azure Solutions (5–10%)

📘 [← Back to AZ-204 Index](./index.md)

---

## 4.1 Azure Monitor and Application Insights

### 📡 Azure Monitor Overview

Azure Monitor is the overarching observability platform for Azure resources. It collects and analyses **metrics** and **logs**.

**Key Components:**

| Component | Description |
|-----------|-------------|
| **Metrics** | Numerical time-series data; low latency, retained 93 days |
| **Log Analytics Workspace** | Centralised log storage; query with KQL |
| **Alerts** | Notify or trigger actions based on conditions |
| **Action Groups** | Reusable notification bundles (email, SMS, webhook, Function, etc.) |
| **Workbooks** | Interactive dashboards combining metrics, logs, and visuals |
| **Diagnostic Settings** | Route resource logs and metrics to Log Analytics, Storage, or Event Hubs |

**Enabling Diagnostic Settings (CLI):**
```bash
az monitor diagnostic-settings create \
  --name myDiagSettings \
  --resource /subscriptions/.../resourceGroups/myRG/providers/Microsoft.Web/sites/myApp \
  --logs '[{"category": "AppServiceHTTPLogs","enabled": true}]' \
  --metrics '[{"category": "AllMetrics","enabled": true}]' \
  --workspace /subscriptions/.../resourceGroups/myRG/providers/Microsoft.OperationalInsights/workspaces/myLAW
```

---

### 🔍 Application Insights

A fully managed **Application Performance Monitoring (APM)** service built on Azure Monitor. Instruments your application to automatically collect and visualise telemetry.

**Telemetry Types:**

| Type | Description | Auto-Collected? |
|------|-------------|----------------|
| **Requests** | Incoming HTTP requests to the app | ✅ Yes |
| **Dependencies** | Outbound calls — SQL, HTTP, Storage, Service Bus | ✅ Yes |
| **Exceptions** | Handled and unhandled exceptions with stack traces | ✅ Yes |
| **Traces** | Custom log messages (via `ILogger` or `TraceSource`) | ✅ Yes |
| **Custom Events** | Business events: `TrackEvent("OrderPlaced")` | Manual |
| **Custom Metrics** | Numerical measurements: `TrackMetric("CartSize", 3)` | Manual |
| **Page Views** | Browser-side navigation telemetry | ✅ (JS snippet) |
| **Performance Counters** | CPU, memory, request rate, exception rate | ✅ Yes |

---

### ⚙️ Instrumentation

**Automatic (zero-code) — .NET:**
```csharp
// Add NuGet: Microsoft.ApplicationInsights.AspNetCore
builder.Services.AddApplicationInsightsTelemetry();
```

**Configure in appsettings.json:**
```json
{
  "ApplicationInsights": {
    "ConnectionString": "InstrumentationKey=...;IngestionEndpoint=..."
  }
}
```

> 💡 Use **Connection String** instead of the older Instrumentation Key — it's more robust and supports regional endpoints.

**Manual / Custom Telemetry:**
```csharp
public class OrderService
{
    private readonly TelemetryClient _telemetry;

    public OrderService(TelemetryClient telemetry)
        => _telemetry = telemetry;

    public async Task PlaceOrderAsync(Order order)
    {
        // Track a business event
        _telemetry.TrackEvent("OrderPlaced", new Dictionary<string, string>
        {
            ["OrderId"] = order.Id,
            ["CustomerId"] = order.CustomerId,
            ["Channel"] = "Web"
        });

        // Track a metric
        _telemetry.TrackMetric("OrderValue", order.Total);

        // Track a dependency manually (e.g., external API)
        var startTime = DateTimeOffset.UtcNow;
        var timer = Stopwatch.StartNew();
        try
        {
            await externalPaymentApi.ChargeAsync(order);
            _telemetry.TrackDependency("PaymentAPI", "Charge", order.Id,
                startTime, timer.Elapsed, success: true);
        }
        catch (Exception ex)
        {
            _telemetry.TrackDependency("PaymentAPI", "Charge", order.Id,
                startTime, timer.Elapsed, success: false);
            _telemetry.TrackException(ex);
            throw;
        }
    }
}
```

---

### 📈 Availability Tests

Proactively monitors your app's reachability and responsiveness from Azure PoP locations globally.

| Test Type | Description | Use When |
|-----------|-------------|----------|
| **URL Ping** | Simple HTTP GET — is the endpoint alive? | Basic uptime monitoring |
| **Standard** | HTTPS, SSL validation, custom headers, response body check | Production health checks |
| **Custom TrackAvailability** | Multi-step SDK-based tests (e.g., login → navigate → assert) | Complex user journey testing |

---

### 📊 Kusto Query Language (KQL) — Key Queries

```kusto
// ─── Requests ───────────────────────────────────────────────────
// Failed requests in the last 24 hours
requests
| where timestamp > ago(24h) and success == false
| summarize count() by name, resultCode
| order by count_ desc

// Average response time per endpoint (last hour)
requests
| where timestamp > ago(1h)
| summarize avg(duration) by name
| top 10 by avg_duration desc

// ─── Exceptions ─────────────────────────────────────────────────
// Exception count by type
exceptions
| summarize count() by type, outerMessage
| order by count_ desc

// ─── Dependencies ───────────────────────────────────────────────
// Slow or failed outbound calls
dependencies
| where timestamp > ago(1h)
| where success == false or duration > 2000
| project timestamp, target, name, duration, success, resultCode

// ─── Custom Events ──────────────────────────────────────────────
// Count of OrderPlaced events by hour
customEvents
| where name == "OrderPlaced"
| summarize count() by bin(timestamp, 1h)
| render timechart

// ─── Performance ────────────────────────────────────────────────
// P95 response time over time
requests
| where timestamp > ago(6h)
| summarize percentile(duration, 95) by bin(timestamp, 5m)
| render timechart
```

---

### 🔔 Alerts

**Alert Types:**

| Type | Trigger | Example |
|------|---------|---------|
| **Metric Alert** | Metric exceeds threshold | CPU > 80% for 5 minutes |
| **Log Alert** | KQL query returns results | More than 10 exceptions in 5 minutes |
| **Activity Log Alert** | Azure management operation | Resource group deleted |
| **Smart Detection** | ML-based anomaly detection | Unusual spike in failure rate |

**Action Groups:** Reusable bundles defining WHO is notified and HOW:
- Email / SMS / Voice
- Azure Function (run code)
- Logic App (complex workflows)
- Webhook (ITSM tools, PagerDuty, etc.)
- Event Hub (stream alerts)
- Automation Runbook

```bash
# Create an action group
az monitor action-group create \
  --name myActionGroup \
  --resource-group myRG \
  --short-name myAG \
  --email-receiver name=devTeam email=devteam@example.com

# Create a metric alert
az monitor metrics alert create \
  --name highCpuAlert \
  --resource-group myRG \
  --scopes /subscriptions/.../resourceGroups/myRG/providers/Microsoft.Web/sites/myApp \
  --condition "avg Percentage CPU > 80" \
  --window-size 5m \
  --evaluation-frequency 1m \
  --action myActionGroup
```

---

📘 [← Back to AZ-204 Index](./index.md)
