# Recherche sur les Extended Capabilities

## Mapping MITRE ATT&CK

| Commande | Technique ATT&CK | Tactique |
|---|---|---|
| keylog | T1056.001 | Credential Access |
| crack | T1110.002 | Credential Access |
| pth | T1550.002 | Defense Evasion |
| loot | T1005 | Collection |
| phish | T1566.002 | Initial Access |
| propagate | T1021 | Lateral Movement |
| privesc | T1548 | Privilege Escalation |
| syscall | T1106 | Defense Evasion |
| shell | T1059 | Execution |
| rdp | T1021.001 | Lateral Movement |
| screenshot | T1113 | Collection |

## Recherche par module

### crack

Implémentation from scratch exigée par le PDF. Wordlist de base + mutations (capitalize, leet speak, suffixes numériques). Pas d'utilisation de hashcat ou john.

Source: MITRE ATT&CK T1110.002

### pth (pass-the-hash)

NTLMv2 response forgery via HMAC-MD5. Le hash NTLM suffit pour s'authentifier sans connaître le mot de passe.

Source: Microsoft Learn - NTLM Authentication

### propagate

Scan des ports SMB (445), WMI (135), WinRM (5985), RDP (3389) pour identifier les machines accessibles sur le réseau.

Source: MITRE ATT&CK T1021

### privesc

Énumération des vecteurs classiques: AlwaysInstallElevated, chemins de services non quotés, privilèges de token (SeImpersonate, SeDebug), fichiers unattend.xml.

Source: MITRE ATT&CK T1548
