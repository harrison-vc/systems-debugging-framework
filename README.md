# Systems Debugging Framework

A comprehensive mental model and technical framework for troubleshooting complex distributed systems. This repository defines a structured, evidence-based approach to identifying, isolating, and resolving system-level failures.

[![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)](https://www.linux.org/)
[![Bash](https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=gnu-bash&logoColor=white)](https://www.gnu.org/software/bash/)
[![SRE](https://img.shields.io/badge/SRE-blue?style=for-the-badge)](https://en.wikipedia.org/wiki/Site_reliability_engineering)
[![DevOps](https://img.shields.io/badge/DevOps-007ACC?style=for-the-badge)](https://en.wikipedia.org/wiki/DevOps)
[![Networking](https://img.shields.io/badge/Networking-orange?style=for-the-badge)](https://en.wikipedia.org/wiki/Computer_network)
[![Security](https://img.shields.io/badge/Security-red?style=for-the-badge)](https://en.wikipedia.org/wiki/Computer_security)

## Debugging Philosophy

1. **Be Methodical, Not Random**: Avoid the "shotgun" approach. Formulate a hypothesis based on evidence before taking action.
2. **Isolate the Fault Domain**: Use a binary search approach (e.g., local vs. remote, frontend vs. backend) to narrow the problem space.
3. **Verify Assumptions**: Always check the simplest possible point of failure (e.g., Is the server on? Is the service listening?).
4. **Reproduce the Failure**: If you cannot reproduce the failure, you cannot reliably verify the fix.
5. **Blame-Free Root Cause Analysis**: Focus on the technical and procedural failure, not the individual.

## Layered Debugging Model

When troubleshooting, move from the lowest layer to the highest to ensure the foundation is stable.

1. **DNS**: Can the hostname be resolved? (`dig`, `nslookup`)
2. **Network (Connectivity)**: Is the destination reachable on the required port? (`ping`, `nc`, `traceroute`)
3. **Compute (OS/Kernel)**: Is the server responsive? Are resources available? (`df`, `free`, `top`, `dmesg`)
4. **Application (Process)**: Is the process running and listening? (`systemctl`, `ss`, `ps`, `lsof`)
5. **Dependencies (Upstream)**: Are external APIs or databases responding? (`curl`, `sqlcmd`)

## Triage Flow

1. **Detection**: Identify the deviation from baseline behavior.
2. **Scope**: Quantify the impact (e.g., One server? One region? All users?).
3. **Initial Triage**: Rapid checks for "easy wins" (e.g., disk full, service crashed).
4. **Deep Investigation**: Detailed log analysis, tracing, and metric inspection.
5. **Resolution**: Apply a temporary fix (mitigation) or permanent fix (remediation).
6. **Post-Mortem**: Document the incident and implement preventative measures.

## Command Reference

### Networking Toolkit
- `curl -I <url>`: Check HTTP headers and connectivity.
- `nc -zv <host> <port>`: Verify TCP port reachability.
- `dig <hostname> +short`: Fast DNS lookup.
- `ss -tulpn`: List all listening TCP/UDP ports and their processes.

### System Resource Checks
- `df -h`: Check disk space across all partitions.
- `free -h`: Check available system memory.
- `top -b -n 1`: Capture a snapshot of CPU and memory by process.
- `journalctl -u <service> -f`: Real-time log monitoring for a systemd service.

### Process and File Inspection
- `ps aux | grep <name>`: Find a specific running process.
- `lsof -p <pid>`: List all open files and network sockets for a process.
- `strace -p <pid>`: Trace system calls of a running process (use with caution in production).

## Issue Isolation Strategies

- **Local Verification**: If a service fails externally, test it from the server itself to isolate network/firewall issues.
- **Differential Analysis**: Compare configurations and logs between failing and healthy servers.
- **Component Bypassing**: Bypass Load Balancers or API Gateways to identify exactly where requests are being dropped.

## Repository Structure

- `checklists/`: Standardized troubleshooting guides for common scenarios (Disk, CPU, Memory).
- `models/`: Conceptual diagrams and mental models for system failure modes.
- `docs/`: In-depth documentation on specialized debugging tools (`mtr`, `tcpdump`, `strace`).

## License

This project is licensed under the MIT License - see the LICENSE file for details.
