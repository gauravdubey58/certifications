# 🔴 Module 3: Implement Azure Security (15–20%)

📘 [← Back to AZ-204 Index](./index.md)

---

## 3.1 Implement User Authentication and Authorization

### 🆔 Microsoft Identity Platform

The Microsoft Identity Platform (v2.0 endpoint) is the foundation for authentication and authorisation in Azure and Microsoft 365. It implements **OAuth 2.0** and **OpenID Connect (OIDC)**.

**Key Concepts:**

| Concept | Description |
|---------|-------------|
| **Tenant** | An organisation's Microsoft Entra ID directory instance |
| **App Registration** | Register your app to get a Client ID and define its identity |
| **Client ID** | Unique identifier assigned to your app registration |
| **Client Secret** | Password used by confidential clients to authenticate |
| **Certificate** | Preferred alternative to client secret (more secure) |
| **Scope** | Permission requested by the app (`User.Read`, `Mail.Send`, etc.) |
| **Access Token** | JWT presented to APIs to prove identity and permissions |
| **ID Token** | JWT containing user claims — consumed by the app, not APIs |
| **Refresh Token** | Used to obtain new access tokens without re-prompting the user |

**Endpoints:**
- Authorise: `https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize`
- Token: `https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token`
- Common (multi-tenant): replace `{tenant}` with `common`

---

### 🔁 OAuth 2.0 / OIDC Flows

| Flow | Use Case | Client Secret? |
|------|---------|---------------|
| **Authorization Code** | Web apps with server-side backend | Yes (confidential client) |
| **Authorization Code + PKCE** | SPAs, mobile, desktop apps | No (public client) |
| **Client Credentials** | Daemon / service-to-service (no user involved) | Yes |
| **On-Behalf-Of (OBO)** | API calling another API on behalf of a signed-in user | Yes |
| **Device Code** | CLI tools, IoT — no browser/keyboard | No |
| **Implicit** | ⚠️ **Deprecated** — do not use for new apps | No |

> 💡 For modern apps: SPAs → **Auth Code + PKCE**, Web Apps → **Auth Code**, Services → **Client Credentials**

---

### 📚 MSAL (Microsoft Authentication Library)

MSAL simplifies acquiring tokens from the Microsoft Identity Platform. Available for .NET, JavaScript, Python, Java, Go, etc.

**Client Types:**

| Type | Description | Example |
|------|-------------|---------|
| Public Client | Cannot keep a secret (browser, mobile) | PKCE, Device Code |
| Confidential Client | Can keep a secret securely (server-side) | Auth Code, Client Credentials |

**Confidential Client — Client Credentials Flow:**
```csharp
var app = ConfidentialClientApplicationBuilder
    .Create(clientId)
    .WithClientSecret(clientSecret)      // Or .WithCertificate(cert)
    .WithAuthority($"https://login.microsoftonline.com/{tenantId}")
    .Build();

var result = await app
    .AcquireTokenForClient(scopes: new[] { "https://graph.microsoft.com/.default" })
    .ExecuteAsync();

string accessToken = result.AccessToken;
```

**Public Client — Authorization Code + PKCE:**
```csharp
var app = PublicClientApplicationBuilder
    .Create(clientId)
    .WithRedirectUri("http://localhost")
    .Build();

var result = await app
    .AcquireTokenInteractive(new[] { "User.Read" })
    .ExecuteAsync();
```

**Token Caching:** MSAL caches tokens automatically. Call `AcquireTokenSilent` first; fall back to interactive only if needed.

---

### ✍️ Shared Access Signatures (SAS) — Security Context

*(Full SAS details in [02-storage.md](./02-storage.md))*

**Security best practices for SAS:**

| Practice | Why |
|----------|-----|
| Use **User Delegation SAS** | Tied to Entra ID — revocable without key rotation |
| Set **short expiry times** | Limits the blast radius if a SAS is leaked |
| Use **Stored Access Policies** | Allows instant revocation for service/account SAS |
| Use **HTTPS only** | Prevents interception of SAS tokens in transit |
| Apply **minimum permissions** | Only grant the exact permissions needed |

---

### 🔗 Microsoft Graph

Microsoft Graph is the unified REST API to access Microsoft 365, Entra ID, Teams, OneDrive, and related services.

**Base Endpoint:** `https://graph.microsoft.com/v1.0/`

**Common Endpoints:**

| Endpoint | Description |
|----------|-------------|
| `GET /me` | Signed-in user's profile |
| `GET /me/messages` | User's emails |
| `GET /me/drive/root/children` | User's OneDrive files |
| `GET /users/{id}` | Specific user (admin) |
| `GET /groups` | List groups (admin) |
| `GET /me/calendar/events` | Calendar events |

**Permission Types:**

| Type | Description | Consent |
|------|-------------|---------|
| **Delegated** | App acts on behalf of a signed-in user | User or Admin |
| **Application** | App acts as itself (no user — daemon scenarios) | Admin only |

**Using the Graph SDK (.NET):**
```csharp
// Setup with Managed Identity or credential
var credential = new DefaultAzureCredential();
var graphClient = new GraphServiceClient(credential,
    new[] { "https://graph.microsoft.com/.default" });

// Get signed-in user
var user = await graphClient.Me.GetAsync();
Console.WriteLine(user.DisplayName);

// List users (application permission)
var users = await graphClient.Users.GetAsync();

// Send email
await graphClient.Me.SendMail.PostAsync(new SendMailPostRequestBody
{
    Message = new Message
    {
        Subject = "Hello from Graph",
        Body = new ItemBody { Content = "Test message", ContentType = BodyType.Text },
        ToRecipients = new List<Recipient>
        {
            new Recipient { EmailAddress = new EmailAddress { Address = "user@example.com" } }
        }
    }
});
```

---

## 3.2 Implement Secure Azure Solutions

### 🔑 Azure Key Vault

Azure Key Vault centrally stores and controls access to secrets, cryptographic keys, and certificates. Eliminates hard-coded credentials in code or config.

**Object Types:**

| Type | Description | Example Use |
|------|-------------|------------|
| **Secrets** | Opaque string values | Connection strings, API keys, passwords |
| **Keys** | Cryptographic keys (RSA 2048/4096, EC P-256/P-384) | Encryption, signing, key wrapping |
| **Certificates** | X.509 certificates with automated lifecycle | TLS/SSL certs, code signing |

**Access Models:**

| Model | Description |
|-------|-------------|
| **Azure RBAC** (recommended) | Role-based — granular, inherits Azure governance |
| **Vault Access Policy** (legacy) | Permissions set per identity at vault level |

**Key RBAC Roles:**

| Role | Permissions |
|------|------------|
| `Key Vault Administrator` | Full management |
| `Key Vault Secrets Officer` | Create, delete, manage secrets |
| `Key Vault Secrets User` | Read secret values |
| `Key Vault Crypto Officer` | Create and manage keys |
| `Key Vault Crypto User` | Use keys for crypto operations |

**SDK — Secrets:**
```csharp
var client = new SecretClient(
    vaultUri: new Uri("https://mykeyvault.vault.azure.net/"),
    credential: new DefaultAzureCredential());

// Store a secret
await client.SetSecretAsync("MyConnectionString", "Server=tcp:...");

// Read a secret
KeyVaultSecret secret = await client.GetSecretAsync("MyConnectionString");
string value = secret.Value;

// List secrets
await foreach (var secretProp in client.GetPropertiesOfSecretsAsync())
    Console.WriteLine(secretProp.Name);

// Delete a secret (soft-delete by default)
await client.StartDeleteSecretAsync("MyConnectionString");
```

**SDK — Keys:**
```csharp
var keyClient = new KeyClient(
    vaultUri: new Uri("https://mykeyvault.vault.azure.net/"),
    credential: new DefaultAzureCredential());

// Create an RSA key
KeyVaultKey key = await keyClient.CreateRsaKeyAsync(
    new CreateRsaKeyOptions("MyKey") { KeySize = 2048 });

// Encrypt using the key
var cryptoClient = new CryptographyClient(key.Id, new DefaultAzureCredential());
var encryptResult = await cryptoClient.EncryptAsync(
    EncryptionAlgorithm.RsaOaep, plaintext);
```

**Key Vault References (App Service & Functions):**  
Reference Key Vault secrets directly in app settings — no code changes needed:
```
@Microsoft.KeyVault(SecretUri=https://myvault.vault.azure.net/secrets/MySecret/)
```
Or using secret name + version:
```
@Microsoft.KeyVault(VaultName=myvault;SecretName=MySecret;SecretVersion=abc123)
```

> 💡 The App Service / Function must have a Managed Identity with `Key Vault Secrets User` role on the vault.

---

### 🔐 Managed Identities

Managed Identities allow Azure resources to authenticate to other Azure services without any credentials in code or configuration.

**Types:**

| Type | Lifecycle | Shared Across Resources? | Use Case |
|------|-----------|--------------------------|----------|
| **System-assigned** | Tied to the resource — deleted when resource is deleted | No | Single-resource identity |
| **User-assigned** | Independent Azure resource | Yes | Multiple resources sharing one identity |

**Setup — System-assigned (App Service):**
```bash
az webapp identity assign \
  --name myWebApp --resource-group myRG
```

**Grant access to Key Vault:**
```bash
az role assignment create \
  --assignee <managed-identity-object-id> \
  --role "Key Vault Secrets User" \
  --scope /subscriptions/.../resourceGroups/.../providers/Microsoft.KeyVault/vaults/myVault
```

**Use in Code — `DefaultAzureCredential`:**
```csharp
// Works locally (dev credentials) AND in Azure (Managed Identity) — no code change
var credential = new DefaultAzureCredential();

// Cosmos DB
var cosmosClient = new CosmosClient(accountEndpoint, credential);

// Key Vault
var secretClient = new SecretClient(new Uri(vaultUri), credential);

// Storage
var blobClient = new BlobServiceClient(new Uri(storageUri), credential);
```

**`DefaultAzureCredential` Resolution Order:**

| Priority | Source |
|----------|--------|
| 1 | Environment variables (`AZURE_CLIENT_ID`, `AZURE_CLIENT_SECRET`, `AZURE_TENANT_ID`) |
| 2 | Workload Identity (AKS pods) |
| 3 | Managed Identity (Azure-hosted resources) |
| 4 | Visual Studio credential |
| 5 | Visual Studio Code credential |
| 6 | Azure CLI (`az login`) |
| 7 | Azure PowerShell (`Connect-AzAccount`) |
| 8 | Interactive browser (if enabled) |

> 💡 In production, Managed Identity (priority 3) is used. Locally, Azure CLI (priority 6) is the most common.

---

### ⚙️ Azure App Configuration

Centrally manages application settings and feature flags — separate from code, config files, and Key Vault.

**Key Features:**

| Feature | Description |
|---------|-------------|
| Hierarchical keys | `AppName:Section:Key` structure |
| Labels | Differentiate values by environment: `production`, `staging`, `dev` |
| Feature flags | Enable/disable features at runtime — no redeployment |
| Key Vault references | Store the Key Vault secret URI in App Config; SDK resolves it automatically |
| Configuration snapshots | Point-in-time snapshots of all settings |

**SDK Integration (.NET):**
```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Configuration.AddAzureAppConfiguration(options =>
{
    options
        .Connect(new Uri(appConfigEndpoint), new DefaultAzureCredential())
        .Select(KeyFilter.Any, labelFilter: "production")  // Filter by label
        .UseFeatureFlags(ff =>
        {
            ff.CacheExpirationInterval = TimeSpan.FromMinutes(5);
        })
        .ConfigureKeyVault(kv =>
        {
            kv.SetCredential(new DefaultAzureCredential()); // Resolve KV references
        });
});
```

**Feature Flags:**
```csharp
// Inject IFeatureManager
public class HomeController : Controller
{
    private readonly IFeatureManager _features;

    public HomeController(IFeatureManager features) => _features = features;

    public async Task<IActionResult> Index()
    {
        if (await _features.IsEnabledAsync("NewDashboard"))
            return View("NewDashboard");
        return View("Classic");
    }
}
```

**Built-in Feature Filters:**

| Filter | Description |
|--------|-------------|
| `Percentage` | Enable for X% of requests |
| `TimeWindow` | Enable during a specific time range |
| `Targeting` | Enable for specific users or groups |
| `Custom` | Implement `IFeatureFilter` for custom logic |

---

📘 [← Back to AZ-204 Index](./index.md)
