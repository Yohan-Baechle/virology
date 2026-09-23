# Recherche sur les modules de l'implant

## Keylogger (T1056.001)

Hook WH_KEYBOARD_LL via SetWindowsHookExW. Le buffer reste en mémoire, jamais écrit sur disque. Gestion de la touche Shift pour les caractères spéciaux.

Sources consultées:
- Microsoft Learn: SetWindowsHookExW
- MITRE ATT&CK: T1056.001

## Persistence (T1547.001, T1053.005, T1543.003)

Trois méthodes redondantes:
1. Registry Run Key (HKCU\...\Run\WindowsTelemetry)
2. Scheduled Task (OnLogon, nom: TelemetryCheck)
3. Service Windows (WinTelemetrySvc, auto start)

Le nom WindowsTelemetry est choisi pour ressembler à un service système légitime.

Sources consultées:
- MITRE ATT&CK: TA0003 Persistence
- Sysinternals Autoruns (côté détection)

## Anti-analysis

Voir implant/ARCHITECTURE.md, section Anti-analysis.

Sources consultées:
- Check Point: Sandbox Evasion Techniques
- MITRE ATT&CK: TA0005 Defense Evasion
