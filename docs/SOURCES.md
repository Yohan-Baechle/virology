# Sources et Recherches

## C2 et Architecture

- [Sliver C2 (GitHub)](https://github.com/BishopFox/sliver) - Repo étudié pour comprendre le pattern implant-to-controller. Architecture du listener, gestion des sessions, beacon avec jitter.
- [Metasploit Framework (GitHub)](https://github.com/rapid7/metasploit-framework) - Reference architecturale. On a regarde comment les handlers et les payloads communiquent.
- [Malware Development for Ethical Hackers (ScholarVox)](https://www.scholarvox.com/) - Livre conseille par le bootstrap. Tradecraft, evasion, C2 internals. Accessible via compte IONIS.

## Crypto et Chiffrément

- [Python cryptography library](https://cryptography.io/en/latest/) - Documentation officielle de la lib qu'on utilisé. Pages consultées: hazmat/primitives/asymmetric/rsa (OAEP padding), hazmat/primitives/ciphers/aead (AESGCM).
- [MITRE ATT&CK T1573.002](https://attack.mitre.org/techniques/T1573/002/) - Asymmetric Cryptography. Technique directement liee a notre handshake RSA.
- [TLS 1.2 (RFC 5246)](https://datatracker.ietf.org/doc/html/rfc5246) - Lu pour comprendre ce qui se passe sous le capot quand on fait du HTTPS. Notamment le handshake et le Change Cipher Spec.

## Windows API et Syscalls

- [Windows API (Microsoft Learn)](https://learn.microsoft.com/en-us/windows/win32/api/) - Documentation officielle. Pages consultées: winreg, winhttp, gdi32 (BitBlt, GetDIBits), kernel32 (CreateToolhelp32Snapshot, OpenProcess).
- [SysWhispers2 (GitHub)](https://github.com/jthuraisamy/SysWhispers2) - Inspiration directe pour notre module syscall. Génère des stubs de direct syscall en C. On a étudié la technique de résolution des SSN.
- [Hells Gate (GitHub)](https://github.com/am0nsec/HellsGate) - Technique de résolution dynamique des SSN depuis les stubs ntdll. Alternative a SysWhispers, plus simple a comprendre.
- [Direct Syscalls (Outflank, 2019)](https://outflank.nl/blog/2019/06/19/red-team-tactics-combining-direct-system-calls-and-new-arming-techniques-to-run-the-unknown/) - Articlé fondateur sur les direct syscalls. Explique pourquoi ca bypass les hooks EDR.
- [Windows Syscall Tables (j00ru)](https://j00ru.vexillium.org/syscalls/nt/64/) - Table de reference complete des syscall numbers par version de Windows. Utilise pour vérifier nos SSN resolus.

## Persistence

- [MITRE ATT&CK T1547.001](https://attack.mitre.org/techniques/T1547/001/) - Registry Run Keys. Détection guidance inclus.
- [MITRE ATT&CK T1053.005](https://attack.mitre.org/techniques/T1053/005/) - Scheduled Task.
- [MITRE ATT&CK T1543.003](https://attack.mitre.org/techniques/T1543/003/) - Windows Service.
- [Autoruns (Sysinternals)](https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns) - Outil utilisé cote blue team pour detecter la persistence. On l'a lance sur notre VM lab pour voir si notre persistence etait visible.

## Credential Access

- [MITRE ATT&CK TA0006](https://attack.mitre.org/tactics/TA0006/) - Vue d'ensemble de la tactique Credential Access.
- [MITRE ATT&CK T1003.002](https://attack.mitre.org/techniques/T1003/002/) - Security Account Manager. Directement lie a notre module cred_dump.
- [MITRE ATT&CK T1555.003](https://attack.mitre.org/techniques/T1555/003/) - Credentials from Web Browsers.
- [NTLM Authentication (Microsoft Learn)](https://learn.microsoft.com/en-us/windows/win32/secauthn/microsoft-ntlm) - Documentation officielle du protocole NTLM. Necessaire pour comprendre le pass-the-hash.
- [Pass-the-Hash (SANS)](https://www.sans.org/white-papers/36783/) - Paper SANS sur le PtH. Explique pourquoi le hash suffit sans le mot de passe.

## Keylogging

- [SetWindowsHookExW (Microsoft Learn)](https://learn.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-setwindowshookexw) - API officielle utilisée dans notre keylogger.
- [MITRE ATT&CK T1056.001](https://attack.mitre.org/techniques/T1056/001/) - Input Capture: Keylogging.

## Evasion et Anti-Analysis

- [MITRE ATT&CK TA0005](https://attack.mitre.org/tactics/TA0005/) - Tactique Defense Evasion.
- [AMSI Reference (Microsoft Learn)](https://learn.microsoft.com/en-us/windows/win32/amsi/amsi-reference) - Architecture officielle de l'Antimalware Scan Interface.
- [Evasion Techniques Catalog (Check Point)](https://evasions.checkpoint.com/) - Catalogue interactif des techniques d'evasion de sandbox. Ram, uptime, debugger, processus.

## Log Manipulation

- [wevtutil (Microsoft Learn)](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/wevtutil) - Outil natif pour les event logs.
- [MITRE ATT&CK T1070.001](https://attack.mitre.org/techniques/T1070/001/) - Indicator Removal: Clear Windows Event Logs.

## Wireshark

- [Wireshark User Guide](https://www.wireshark.org/docs/wsug_html_chunked/) - Guide officiel. Chapitre sur les filtres d'affichage.
- [Wireshark TLS Wiki](https://wiki.wireshark.org/TLS) - Comment inspecter le trafic TLS.

## Outils

- [PyInstaller](https://pyinstaller.org/en/stable/) - Packaging de l'implant en exe autonome.
- [MinGW-w64](http://mingw-w64.org/) - Cross-compilation C pour Windows depuis Linux.

## Livres

- Mastering Active Directory - Architecture AD, attaques et defenses.
- Privilege Escalation Techniques - Escalation locale et domaine sur Windows/Linux.
- Malware Development for Ethical Hackers - Tradecraft, evasion, C2 internals.

## Videos et Ressources Complementaires

- [13Cubed YouTube](https://www.youtube.com/@13Cubed) - Chaines d'analyse forensique. Videos sur AMSI, event logs, persistence détection.
- [MITRE ATT&CK Navigator](https://mitre-attack.github.io/attack-navigator/) - Outil en ligne pour visualiser notre coverage ATT&CK. Utilise pour génèrer notre MITRE_MAPPING.md.
