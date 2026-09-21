# s0P0wn3d

C2 (Command & Control) framework - projet pédagogique de sécurité offensive.

> **USAGE AUTORISÉ UNIQUEMENT EN LABORATOIRE.** Toute attaque sans consentement écrit est illégale.

## Objectif

Design et construction d'un outil d'intrusion offensive nommé s0P0wn3d, dans le cadre d'un projet Epitech (T-SEC-901). L'outil agit comme un C2: le hub central depuis lequel on surveille, contrôle et interagit avec les machines compromises pendant une intrusion.

Le projet récompense le cheminement autant que la destination: explorer, se tromper, ajuster, améliorer.

## Structure du repo

```
├── docs/           Documentation, recherche, analyse
│   ├── FOLLOWUP_1.md       Point de suivi 1 (25/09/2026)
│   ├── SOURCES.md          Recherches et références
├── lab/            Environnement de laboratoire
│   └── MANIFEST.md         Manifest pour demo reproductible
└── README.md
```

Le code sera ajoute progressivement selon les milestones définis sur GitHub.

## Lab environment

- Cible: Windows 10 22H2 (VM disposable, snapshot avant chaque test)
- Attaquant: Ubuntu 22.04
- Outils: Wireshark, Sysinternals

## Équipe

| Membre | Périmètre |
|---|---|
| Yohan Baechle | Architecture, controller, crypto |
| Lena Gonzalez Breton | Implant, persistence, anti-analysis |
| Helene Rizzon | Modules étendus, tests, documentation |

## Suivi

| Milestone | Deadline |
|---|---|
| Core C2 | 25/09/2026 |
| Credential Access & Stealth | 27/11/2026 |
| Extended Capabilities | 27/11/2026 |
| Delivery & Soutenance | 15/01/2027 |

26 issues détaillées sur GitHub, rattachées au Project KOVID.
