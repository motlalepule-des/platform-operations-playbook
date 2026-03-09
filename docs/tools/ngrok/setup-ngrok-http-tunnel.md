# Setup ngrok HTTP Tunnel

ngrok is a command-line application that exposes local ports to the internet via secure tunnels. This guide covers installation, configuration, and best practices.

## Installation

### Windows

#### Option 1: Chocolatey

```powershell
choco install ngrok
```

#### Option 2: Direct Download

1. Download from [ngrok.com](https://ngrok.com/download)
2. Extract the executable
3. Add to PATH or run directly

#### Option 3: Scoop

```powershell
scoop install ngrok
```

### Mac

```bash
brew install ngrok
```

### Linux

```bash
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null && echo "deb https://ngrok-agent.s3.amazonaws.com buster main" | sudo tee /etc/apt/sources.list.d/ngrok.list && sudo apt update && sudo apt install ngrok
```

## Authentication

### Get Your Auth Token

1. Sign up at [ngrok.com](https://ngrok.com)
2. Go to [Dashboard](https://dashboard.ngrok.com/get-started/your-authtoken)
3. Copy your authtoken

### Configure ngrok

```powershell
ngrok config add-authtoken <your-authtoken>
```

This stores the token in `~/.ngrok2/ngrok.yml` for future use.

## Basic Usage

### Start HTTP Tunnel

```powershell
ngrok http 8080
```

Output:
```
ngrok                                                                 (Ctrl+C to quit)

Session Status                online
Account                       user@example.com
Version                       3.x.x
Region                        us (United States)
Forwarding                    https://1234-5678-abcd.ngrok.io -> http://localhost:8080
Forwarding                    http://1234-5678-abcd.ngrok.io -> http://localhost:8080
```

## Advanced Options

### Host Header Rewrite

The `--host-header=rewrite` flag is **critical** for most APIs.

#### Problem Without It

When a request comes through the tunnel:
- External URL: `https://1234-5678-abcd.ngrok.io`
- Request header shows `Host: 1234-5678-abcd.ngrok.io`
- Your API expects `Host: localhost` or a specific domain
- Header validation fails → **API rejects the request**

#### Solution With It

```powershell
ngrok http 8080 --host-header=rewrite
```

How it works:
- External request arrives with public Host header
- ngrok intercepts and rewrites to `Host: localhost:8080`
- Your API receives the expected header
- Request succeeds ✓

#### When You Need It

- **ASP.NET Core APIs** with host validation
- **APIs checking origin headers** (CORS, WebSocket)
- **Session cookies** tied to specific hosts
- **APIs with security policies** validating Host headers

#### When It's Optional

- REST APIs without host validation
- Stateless microservices
- APIs accepting any origin

**Recommendation:** Always use `--host-header=rewrite` unless you have a specific reason not to.

### Custom Domain

Use a paid ngrok account to reserve custom domains:

```powershell
ngrok http 8080 --domain=myapi.ngrok.io
```

### Custom Subdomain

```powershell
ngrok http 8080 --subdomain=myapi
```

Results in: `https://myapi.ngrok.io`

### Basic Authentication

Add password protection:

```powershell
ngrok http 8080 --basic-auth=user:password
```

Testers must provide credentials:
```powershell
curl -u user:password https://myapi.ngrok.io/api/data
```

### Custom Headers

Add headers to tunneled requests:

```powershell
ngrok http 8080 --request-header-add "X-Custom-Header: value"
```

### Region Selection

Choose closer region for lower latency:

```powershell
# List available regions
ngrok http --help | grep region

# Use specific region
ngrok http 8080 --region=eu
```

Available regions: `us`, `eu`, `au`, `ap`, `jp`, `in`, `sa`

### Multiple Tunnels

Start ngrok with config file for multiple tunnels:

**~/.ngrok2/ngrok.yml:**
```yaml
authtoken: your-token
tunnels:
  api:
    proto: http
    addr: 8080
    host_header: rewrite
  web:
    proto: http
    addr: 3000
    host_header: rewrite
```

Run all:
```powershell
ngrok start --all
```

Run specific:
```powershell
ngrok start api
```

## Monitoring & Debugging

### Web Inspection Interface

ngrok provides a local web dashboard at `http://127.0.0.1:4040`

View:
- All HTTP requests/responses
- Request headers and body
- Response code and latency
- Real-time traffic monitoring

### Command Line Inspection

```powershell
# View all tunnels
ngrok tunnels list

# View session details
ngrok config list
```

### Logs

```powershell
# View logs
ngrok config list

# Log file location
# Windows: %AppData%/ngrok/ngrok.log
# Mac/Linux: ~/.ngrok2/ngrok.log
```

## Complete Examples

### ASP.NET Core API

```powershell
# Terminal 1: Start API
dotnet run --urls=http://localhost:5000

# Terminal 2: Start tunnel
ngrok http 5000 --host-header=rewrite

# Share: https://1234-5678-abcd.ngrok.io
```

### Node.js Express API

```powershell
# Terminal 1: Start API
npm start  # Runs on port 3000

# Terminal 2: Start tunnel
ngrok http 3000 --host-header=rewrite

# Share: https://1234-5678-abcd.ngrok.io
```

### Python Flask API

```powershell
# Terminal 1: Start API
python app.py  # Runs on port 5000

# Terminal 2: Start tunnel
ngrok http 5000 --host-header=rewrite --basic-auth=admin:password

# Share with authentication: https://admin:password@1234-5678-abcd.ngrok.io
```

## Best Practices

| Practice | Why | How |
|----------|-----|-----|
| **Always use `--host-header=rewrite`** | Prevents host validation errors | Add to every command |
| **Protect sensitive APIs** | Prevent unauthorized access | Use `--basic-auth` or IP whitelist |
| **Monitor traffic** | Detect issues early | Check `http://localhost:4040` |
| **Use HTTPS only** | Encrypt data in transit | ngrok provides HTTPS by default |
| **Rotate tokens** | Prevent account compromise | Change monthly |
| **Stop when done** | Prevent unintended access | Ctrl+C to stop tunnel |
| **Test before sharing** | Verify API works | Test locally first, then through tunnel |

## Troubleshooting

### Connection Refused

```powershell
# Verify API is running
Get-Process | Where-Object {$_.Name -like "*dotnet*"}

# Check port is listening
netstat -ano | Select-String 5000
```

### Tunnel Won't Start

```powershell
# Check authentication
ngrok config list

# Verify auth token
ngrok config add-authtoken <your-new-token>
```

### External Access Fails

1. Verify tunnel started successfully
2. Check ngrok dashboard: `http://localhost:4040`
3. Test with curl:
   ```powershell
   curl -v https://your-tunnel-url/api/test
   ```
4. Check API logs for errors

### Slow Response Times

- Check network latency
- Try different region: `ngrok http 8080 --region=eu`
- Monitor via dashboard

## Related Documentation

- [Publish Local API for Testing](../../use-cases/publish-local-api-for-testing.md)
- [Setup Dev Tunnels](./setup-dev-tunnels.md)
