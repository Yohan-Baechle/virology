# Controller Architecture

## Listener HTTPS

Endpoints: /api/v1/session, /api/v1/check, /api/v1/result
TLS avec cert auto-signé, port 8443.

## Console opérateur

Commandes: implants, use, shell, keylog, creds, persist, results.

## Gestion des sessions

Chaque implant génère une clé AES-256 envoyée chiffrée en RSA-OAEP au handshake.
Le controller stocke les clés de session et route les tâches par implant_id.
