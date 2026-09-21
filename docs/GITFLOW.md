# Git Strategy

## Branches (minimum 3)

```text
main     ──●─────────────●──────────>  (stable, tagged)
             \           /
develop  ─────●─────●───●──────>       (integration)
               \
feature/controller ──●───●────>        (HTTPS listener, crypto, console)
feature/implant     ────●──●──>        (beacon, modules, persistence)
feature/modules     ──────●────>       (keylogger, cred_dump, crack, pth)
```

## Workflow

1. Creer une branche feature depuis develop
2. Developper, tester, committer
3. Merger vers develop quand la feature est complete
4. Merger develop vers main pour une release
5. Tagger sur main

## Convention de commits

Format : `type(scope): description`

Types : `feat`, `fix`, `docs`, `test`, `refactor`, `chore`

Exemples pour chaque membre (minimum 2 commits chacun) :

```bash
# Membre A (controller)
git checkout -b feature/controller develop
git add controller/
git commit -m "feat(controller): HTTPS listener with AES-GCM session crypto"
git commit -m "feat(controller): operator console with implant selection"
git checkout develop && git merge feature/controller

# Membre B (implant core)
git checkout -b feature/implant develop
git add implant/
git commit -m "feat(implant): beacon loop with jitter and exponential backoff"
git commit -m "feat(implant): persistence via registry, schtasks, service"
git checkout develop && git merge feature/implant

# Membre C (modules)
git checkout -b feature/modules develop
git add implant/modules/
git commit -m "feat(modules): keylogger via WH_KEYBOARD_LL hook"
git commit -m "feat(modules): credential dump, crack, pth, phish, scréénshot"
git checkout develop && git merge feature/modules
```

## Release

```bash
git checkout main
git merge develop
git tag -a v1.0.0 -m "s0P0wn3d v1.0.0"
git push origin main --tags
```
