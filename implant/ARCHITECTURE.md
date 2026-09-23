# Architecture de l'Implant

## Boucle beacon

L'implant fait un check-in toutes les 30 secondes avec un jitter de ±30% pour éviter les signatures temporelles. Backoff exponentiel si le C2 est injoignable (5s → 10s → 20s → ... → 1h max).

## Handshake initial

1. Génération d'une clé AES-256 aléatoire
2. Chiffrement de la clé avec la clé publique RSA du C2
3. Envoi vers /api/v1/session
4. Réception de la configuration (interval, jitter)

## Identifiant unique

Hash MD5 du hostname:username — stable entre les redémarrages, unique par machine.

## Anti-analysis

- Détection debugger (IsDebuggerPresent, CheckRemoteDebuggerPresent)
- Détection outils d'analyse (tasklist scan: wireshark, procmon, ida)
- Check timing (sandbox émulé = lent)
- Si sandbox détecté: boucle bénigne pendant 1h
