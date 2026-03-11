
# Use Case: Publish Local API for External Testing

## Scenario

A developer needs to expose a locally running API so external systems can test integration with real-time changes during development.

Example:

Local ASP.NET API running on `http://localhost:5000` needs to be accessible to external testers or third-party integrations.

## Solution

Use a tunneling tool to create a secure, publicly accessible URL:

- **ngrok** - Simple, reliable tunneling with custom domains
- **Dev Tunnels** - Microsoft's built-in tunneling for Visual Studio

## Steps (ngrok)

### 1. Install ngrok

Download from [ngrok.com](https://ngrok.com) or use package manager:

```powershell
# Using Chocolatey on Windows
choco install ngrok

# Or download binary and add to PATH
```

### 2. Authenticate

Get your authtoken from [ngrok dashboard](https://dashboard.ngrok.com).

```powershell
ngrok config add-authtoken <your-authtoken>
```

### 3. Start Your Local API

Ensure your API is running:

```powershell
# Example: ASP.NET Core API
dotnet run --urls=http://localhost:5000
```

### 4. Start Tunnel

```powershell
ngrok http 5000 --host-header=rewrite
```

Output will show:

```
Session Status          online
Account                 your-email@example.com
Version                 3.x.x
Region                  us (United States)
Forwarding              https://abcd-1234-5678.ngrok.io -> http://localhost:5000
```

Your API is now accessible at: `https://abcd-1234-5678.ngrok.io`

For detailed information about the `--host-header=rewrite` flag and other ngrok options, see [Setup ngrok HTTP Tunnel](../tools/ngrok/setup-ngrok-http-tunnel.md).

### 5. Share Public URL

Share the forwarding URL with external testers. They can access your local API without additional setup.

---

## Steps (Dev Tunnels)

Dev Tunnels is Microsoft's native tunneling solution, integrated with Visual Studio.

### 1. Prerequisites

- Visual Studio 2022 (Update 7 or later)
- Signed in to Microsoft Account

### 2. Create a Dev Tunnel

In Visual Studio:

1. Click the **Run** dropdown → **Create Tunnel...**
2. Select tunnel type: **Temporary** or **Persistent**
3. Enter a name for your tunnel

### 3. Start Your API

Run your application normally in Visual Studio:

```powershell
# If using command line
dotnet run --urls=http://localhost:5000
```

### 4. Access the Tunnel URL

Visual Studio displays a tunnel URL:

```
https://your-tunnel-name-abcdef.devtunnels.ms
```

Alternative method using CLI:

```powershell
devtunnel create --name my-api
devtunnel port create -p 5000
devtunnel host
```

### 5. Share with Testers

Share the tunnel URL. External systems can connect without knowing your local IP or machine.

---

## Verification & Testing Steps

### Verify Tunnel is Active

```powershell
# Test endpoint from local machine
Invoke-WebRequest -Uri "http://localhost:5000/api/health" -Verbose

# Test endpoint from public URL
Invoke-WebRequest -Uri "https://your-tunnel-url/api/health" -Verbose
```

### Monitor API Traffic

**ngrok**: Web dashboard at `http://localhost:4040`

```
Requests | Response Code | Method | Path | Duration
```

**Dev Tunnels**: Monitor in Visual Studio Output window

### Test with External Tools

Use Postman, cURL, or your test runner to hit the public URL:

```bash
curl -X GET https://your-tunnel-url/api/health
curl -X POST https://your-tunnel-url/api/users \
  -H "Content-Type: application/json" \
  -d '{"name":"John","email":"john@example.com"}'
```

### Verify Headers & Auth

Ensure proper headers are passed through the tunnel:

```powershell
$headers = @{
    "Authorization" = "Bearer token123"
    "Content-Type" = "application/json"
}

Invoke-WebRequest `
  -Uri "https://your-tunnel-url/api/data" `
  -Headers $headers `
  -Method GET
```

---

## Best Practices

| Practice | Why | How |
|----------|-----|-----|
| Use HTTPS only | Tunnels use HTTPS to encrypt traffic | Verify URL starts with `https://` |
| Rotate auth tokens | Prevent unauthorized access | Change ngrok/Dev Tunnel tokens monthly |
| Monitor activity | Detect suspicious requests | Check ngrok dashboard or logs |
| Use temporary URLs | Limit exposure duration | Use ngrok temp tunnels or Dev Tunnels sessions |
| Disable when done | Stop unintended access | Stop tunnel process after testing |

---

## Troubleshooting

### Connection Refused

Ensure your local API is running and listening on the correct port:

```powershell
# Check if port is listening
netstat -an | Select-String 5000
```

### Tunnel Won't Start

```powershell
# Check for port conflicts
Get-Process | Where-Object {$_.Id -eq (Get-NetTCPConnection -LocalPort 5000).OwningProcess}

# Restart ngrok or dev tunnel
# Kill previous process and start fresh
```

### External Access Fails

- Verify firewall isn't blocking localhost
- Confirm tunnel URL is correct
- Check API logs for request details
- Test with `curl` from another machine

---

## Related Documentation

- [Setup ngrok HTTP Tunnel](../tools/ngrok/setup-ngrok-http-tunnel.md)
- [Setup Dev Tunnels](../tools/dev-tunnels/setup-dev-tunnels.md)
- [Expose SQL Server from VM](../infrastructure/virtual-machines/expose-sql-server-from-vm.md)
