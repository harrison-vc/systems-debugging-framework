# Network Connectivity Triage Checklist

A step-by-step guide to isolating network failures across the OSI model layers.

## Layer 3: Network (IP & Routing)
*Can I reach the host?*

- **Test Connectivity**: `ping -c 4 <ip_address>`
- **Trace Path**: `mtr -rw <host>` or `traceroute <host>` (Look for where packets are dropped).
- **Check Routing Table**: `ip route show` or `netstat -rn` (Verify default gateway).

## Layer 4: Transport (TCP/UDP)
*Is the port open and listening?*

- **Remote Port Scan**: `nc -zv <host> <port>`
- **Local Listener Check**: `ss -tulpn | grep :<port>` (Check if process is bound to `0.0.0.0` or `127.0.0.1`).
- **Check Firewall (iptables/nftables)**: `iptables -L -n -v` (Look for REJECT or DROP rules).

## Layer 7: Application (HTTP/TLS)
*Is the application responding correctly?*

- **Inspect Headers**: `curl -Iv https://<domain>`
- **Trace HTTP Flow**: `curl -Lv https://<domain>` (Check for infinite redirects or 3xx loops).
- **TLS Handshake Verification**: `openssl s_client -connect <domain>:443 -servername <domain>` (Check for certificate expiry or SNI issues).

## Cloud-Specific (AWS)
*Are cloud infrastructure rules blocking traffic?*

- **Security Groups**: Verify Ingress rules for the CIDR and Port.
- **Network ACLs (NACLs)**: Check if a stateless subnet rule is blocking traffic.
- **VPC Flow Logs**: Search for `REJECT` actions in the logs for the specific ENI.

## Troubleshooting Logic
1. **Source vs Destination**: Can I reach any other host? (If yes, source is likely OK).
2. **Local vs Remote**: Does it work from the localhost of the target? (If yes, it's a network/firewall issue).
3. **Internal vs External**: Does it work from within the VPC? (If yes, it's an Ingress/Gateway issue).
