

# Firewall Whitelisting

## Purpose
Allow a known external IP to access a service securely.

## Example
Whitelist:

- IP: `34.89.7.83`
- Port: `1433`
- Protocol: `TCP`

## Steps
1. Open firewall or perimeter device configuration
2. Add inbound rule
3. Restrict source to the approved public IP
4. Restrict destination to required internal server
5. Restrict destination port to required port
6. Save and apply

## Verification
Test from approved source IP only.

## Security Notes
- Never open SQL Server broadly to the internet
- Prefer source-IP-restricted rules
- Review whitelist entries regularly