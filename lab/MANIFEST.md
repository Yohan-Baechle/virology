# Environment Manifest

## Target VM

- OS: Windows 10 Pro 22H2 (build 19045)
- RAM: 4 GB
- CPU: 2 vCPU
- Hypervisor: Proxmox
- Network: LAN (accessible from attacker)
- Windows Defender: Configured per `lab/setup_target.ps1`
- Snapshot: `clean-target` (taken after setup, before implant deployment)
- Test user: `svc_backup` / `P@ssw0rd2024!`

## Attacker Machine

- OS: Windows 11 (Python natif) or Linux (WSL Ubuntu)
- Python: 3.10+
- Network: Same LAN as target VM
- TLS cert: Self-signed (`certs/server.pem`)
- C2 port: 8443 (HTTPS)

## Network Topology

```text
Attacker (Windows 11)         Target VM (Windows 10 22H2, Proxmox)
192.168.27.65        <────>   192.168.1.175
      │                            │
   Port 8443 (C2 HTTPS)        Beacon outbound to 8443
```

## Reproducibility

1. Provision fresh Win10 22H2 VM on Proxmox
2. Run `lab/setup_target.ps1` as Administrator (create with Notepad on the VM)
3. Snapshot the VM (name: `clean-target`)
4. On attacker: generate TLS certs, start C2
5. Generate implant config (C2 URL + public key)
6. Copy `implant/` to VM, configure `config.py`
7. On VM: `python -m pip install cryptography`
8. On VM: `python run_implant.py`
9. Observe implant check-in on C2 console

## Tool Versions

- Python: 3.10+ (tested on 3.12 and 3.14)
- cryptography: >=41.0.0
- OpenSSL: any recent version
