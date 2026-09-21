# Follow-up 1 - 25 Septembre 2026

## Architecture retenue

Pattern implant-to-controller inspiré de Sliver et Metasploit (reference architecturale uniquement).

```mermaid
graph LR
    subgraph Attaquant[Attaquant - Linux]
        C[Controller Python]
        L[Listener HTTPS :8443]
        Q[Task Queue]
        O[Operator Console]
        K[RSA-4096 + AES-256-GCM]
    end
    subgraph Cible[Cible - Windows 10 22H2]
        I[Implant Python]
        B[Beacon 30s ±30%]
        M[Modules]
        P[Persistence x3]
        A[Anti-analysis]
    end
    C --> L
    L <--> B
    B --> I
    I --> M
    I --> P
    I --> A
    Q --> L
    O --> Q
    K --> L
```

### Protocole de communication

Handshake initial :

```mermaid
sequenceDiagram
    participant I as Implant
    participant C as Controller
    I->>I: Génère clé AES-256 (32 bytes)
    I->>I: Chiffré clé avec RSA public (OAEP-SHA256)
    I->>C: POST /api/v1/session [RSA(id + cle)]
    C->>C: Décrypte avec clé privee RSA
    C->>C: Stocke clé de session AES
    C->>I: AES-GCM(config beacon)
    Note over I,C: Session établie - toutes les communications en AES-256-GCM
```

Beacon périodique :

```mermaid
sequenceDiagram
    participant I as Implant
    participant C as Controller
    loop Toutes les 30s ± jitter 30%
        I->>C: POST /api/v1/check [AES-GCM(status alive)]
        C->>C: Vérifie file de taches
        alt Tache en attente
            C->>I: AES-GCM(tache)
            I->>I: Execute la commande
            I->>C: POST /api/v1/result [AES-GCM(résultat)]
        else Pas de tache
            C->>I: AES-GCM(NOP)
        end
    end
```

Double couclé de chiffrement prouvable avec Wireshark (filtre: tls && ip.addr == <IP_CIBLE>).

### Choix techniques justifiés

| Choix | Raison | Alternative écartée |
|---|---|---|
| HTTPS (port 8443) | Protocole existant, trafic inaperçu | TCP custom (détectable par port) |
| RSA-4096 + OAEP | Echange de clé asymétrique exigé par le PDF | RSA sans padding (insécurisé) |
| AES-256-GCM | Chiffré authentifié (intégrité + confidentialite) | AES-CBC (pas authentifié) |
| Beacon 30s + jitter | Évite les patterns détectables par NIDS | Beacon fixe (signature temporelle) |
| ID = hash(hostname:user) | Identification unique par machine | UUID aléatoire (change a chaque reboot) |
| User-Agent rotation | Trafic ressemble a un navigateur normal | UA fixe (fingerprint) |

## Protocole des commandes C2

| Endpoint | Methode | Payload | Reponse |
|---|---|---|---|
| /api/v1/session | POST | RSA(id + clé session) | AES-GCM(config beacon) |
| /api/v1/check | POST | AES-GCM(status alive) | AES-GCM(tache ou NOP) |
| /api/v1/result | POST | AES-GCM(résultat commande) | AES-GCM(ACK) |

Format binaire : [16 bytes implant_id][nonce 12B][ciphertext+tag]

## Plan de persistence

```mermaid
flowchart TD
    P[Commande persist] --> R[Registry Run Key]
    P --> S[Scheduled Task]
    P --> V[Service Windows]
    R --> R1[HKCU Run WindowsTelemetry]
    S --> S1[OnLogon TelemetryCheck]
    V --> V1[WinTelemetrySvc auto start]
    R1 --> D[Détection: autoruns, audit clé Run]
    S1 --> D2[Détection: schtasks query, Sysmon EID 1]
    V1 --> D3[Détection: sc query, Event 7045]
```

| Methode | Mécanisme | MITRE ATT&CK | Détection blue team |
|---|---|---|---|
| Registry Run Key | HKCU\Run\WindowsTelemetry | T1547.001 | Audit clé Run, autoruns |
| Scheduled Task | \Microsoft\Windows\WindowsTelemetry\TelemetryCheck | T1053.005 | schtasks /query, Sysmon EID 1 |
| Service Windows | WinTelemetrySvc (auto start) | T1543.003 | sc query, Event 7045 |

## Flux d'execution d'une commande C2

```mermaid
flowchart LR
    O[Operateur] -->|commande| Q[Task Queue]
    Q -->|beacon suivant| I[Implant]
    I -->|decrypt| E[Executeur]
    E --> M1[shell / shell_builtin]
    E --> M2[keylog]
    E --> M3[cred_dump]
    E --> M4[scréénshot]
    E --> M5[...]
    M1 --> R[Resultat]
    M2 --> R
    M3 --> R
    M4 --> R
    R -->|chiffré AES-GCM| C2[Controller]
    C2 -->|affiche| O
```

## Lab environment

- Cible: Windows 10 22H2 (VM disposable, snapshot avant chaque test)
- Attaquant: Ubuntu 22.04 (WSL2)
- Outils: Wireshark (preuve chiffrement), Sysinternals (détection)
- Manifest: lab/MANIFEST.md pour reproductibilité de la demo

## Équipe

26 issues sur GitHub, 4 milestones alignes sur les follow-ups.

| Membre | Périmètre |
|---|---|
| Yohan Baechle | Architecture, controller, crypto, shell builtin |
| Lena Gonzalez Breton | Implant, persistence, anti-analysis, keylogger |
| Helene Rizzon (Pepper-Bots) | Modules étendus, tests, documentation |

## Avancement actuel

- Architecture définie et documentee
- Protocole de communication spécifie (endpoints, format binaire, handshake)
- 26 issues détaillées et assignees sur GitHub
- Repo organise avec kanban (Project KOVID)
- Choix techniques justifiés (tableau ci-dessus)
- Analyse des faiblesses identifiées (GAP_ANALYSIS.md)
- Lab environment defini avec manifest

## Objectifs avant Follow-up 2 (27/11/2026)

1. Beacon fonctionnel entre deux VM du lab
2. Handshake RSA vers AES-GCM opérationnel
3. Shell distant (version détectable acceptée dans un premier temps)
4. Registry Run key pour la persistence
5. Capture Wireshark prouvant le chiffrement
6. Au moins 3 modules étendus opérationnels

## Risques identifiés

| Risque | Impact | Mitigation |
|---|---|---|
| Python runtime détectable par l'AV | Le PDF exigé la furtivite | Documenter comme faiblesse, migration progressive |
| Pas de signature de binaire | SmartScréén bloque | Lab uniquement: pas de telechargement navigateur |
| spawn cmd.exe génère Event 4688 | Détection Sysmon | shell_builtin in-process comme alternative |
| VM cible patchee par Windows Update | Demo cassee | Snapshot VM, desactiver Update pendant la demo |
