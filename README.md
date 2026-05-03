<center>
<h1>Systems Debugging Framework</h1>
<p><em>A systematic approach to diagnosing and resolving mission-critical system failures.</em></p>
<img src="https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white" alt="Terraform">&nbsp;<img src="https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white" alt="AWS">&nbsp;<img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker">&nbsp;<img src="https://img.shields.io/badge/Ansible-EE0000?style=for-the-badge&logo=ansible&logoColor=white" alt="Ansible">
</center>

---

<h1>Systems Debugging Framework</h1>


4. **Reproduce the Failure**: If you cannot reproduce the failure, you cannot reliably verify the fix.
5. **Blame-Free Root Cause Analysis**: Focus on the technical and procedural failure, not the individual.

2. **Network (Connectivity)**: Is the destination reachable on the required port? (`ping`, `nc`, `traceroute`)
3. **Compute (OS/Kernel)**: Is the server responsive? Are resources available? (`df`, `free`, `top`, `dmesg`)
4. **Application (Process)**: Is the process running and listening? (`systemctl`, `ss`, `ps`, `lsof`)
5. **Dependencies (Upstream)**: Are external APIs or databases responding? (`curl`, `sqlcmd`)

3. **Initial Triage**: Rapid checks for "easy wins" (e.g., disk full, service crashed).
4. **Deep Investigation**: Detailed log analysis, tracing, and metric inspection.
5. **Resolution**: Apply a temporary fix (mitigation) or permanent fix (remediation).
6. **Post-Mortem**: Document the incident and implement preventative measures.

- `dig <hostname> +short`: Fast DNS lookup.
- `ss -tulpn`: List all listening TCP/UDP ports and their processes.

- `top -b -n 1`: Capture a snapshot of CPU and memory by process.
- `journalctl -u <service> -f`: Real-time log monitoring for a systemd service.

- `strace -p <pid>`: Trace system calls of a running process (use with caution in production).

- **Component Bypassing**: Bypass Load Balancers or API Gateways to identify exactly where requests are being dropped.

- `docs/`: In-depth documentation on specialized debugging tools (`mtr`, `tcpdump`, `strace`).