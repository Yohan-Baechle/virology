# Stratégie de test

## Approche

Tests pytest pour chaque module. Skip automatique sur non-Windows pour les modules spécifiques (keylogger, cred_dump, etc.).

## Tests prévus

### Crypto (test_crypto.py)
- Roundtrip AES-256-GCM (chiffrer puis déchiffrer)
- Deux chiffrages du même plaintext donnent des ciphertexts différents
- Mauvaise clé → exception
- Payload volumineux (100KB)

### Beacon/Controller (test_beacon.py)
- Enregistrement d'un implant
- Mise à jour du last_seen
- Isolation des files de tâches entre implants
- Callback appelé quand un résultat arrive

### Modules (test_modules.py)
- crack: hash MD5 connu → CRACKED
- crack: hash inconnu → NOT CRACKED
- shell_builtin: whoami, pwd, ls
- rdp: status

## Convention

- Un fichier test_*.py par domaine
- Fixtures dans conftest.py
- Tests marqués @pytest.mark.skipif(sys.platform != 'win32') pour les modules Windows uniquement
