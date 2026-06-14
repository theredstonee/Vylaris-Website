<div align="center">

<img src="https://media.vylaris.ch/logo/standard.png" alt="Vylaris" width="100" />

# Vylaris — Source Repo

**Lesbarer Source. Auto-Compile via GitHub Actions.**

![PHP](https://img.shields.io/badge/PHP-8.1%2B-777BB4?style=for-the-badge&logo=php&logoColor=white)
![Node](https://img.shields.io/badge/Node-22%2B-339933?style=for-the-badge&logo=node.js&logoColor=white)
![Private](https://img.shields.io/badge/Repo-Private-red?style=for-the-badge)

</div>

---

> [!IMPORTANT]
> **Privates Repo.** Source-Code für die Vylaris-Homepage. Push auf `main` triggert Build + Deploy.

---

## Architektur

```
┌─────────────────────────┐         push main
│ vylaris-source (PRIV)   │ ──────────────────────┐
│  ├ src/  (lesbar)       │                       │
│  ├ build.js             │                       ▼
│  └ .github/workflows/   │             ┌──────────────────┐
└─────────────────────────┘             │ GitHub Actions   │
                                        │  1. npm install  │
                                        │  2. node build   │
                                        │  3. FTP deploy   │
                                        │  4. push public  │
                                        └────────┬─────────┘
                                                 │
                          ┌──────────────────────┼─────────────────┐
                          ▼                                        ▼
                ┌──────────────────┐                    ┌─────────────────────┐
                │ FTP webspace     │                    │ build-the-future-   │
                │ (echte Seite)    │                    │ submission (PUBLIC) │
                └──────────────────┘                    └─────────────────────┘
```

## Erforderliche Secrets

In Repo-Settings → **Secrets and variables** → **Actions** → New repository secret:

| Secret | Wert |
|--------|------|
| `VY_KEY` | 64-Zeichen Hex-Key aus deiner lokalen `.vykey.php` |
| `FTP_SERVER` | FTP-Host (z.B. `ftp.deinedomain.de`) |
| `FTP_USER` | FTP-Benutzername |
| `FTP_PASSWORD` | FTP-Passwort |
| `PUBLIC_REPO_TOKEN` | GitHub PAT mit `repo`-Scope (für Push zum Public-Repo) |

### VY_KEY finden

Öffne lokal `.vykey.php`:
```php
<?php $VY_K='a1b2c3...';
```
Das ist der Wert von `VY_K` (64 Hex-Zeichen).

### Public Repo Token erstellen

1. github.com → Settings → Developer settings → Personal access tokens → **Fine-grained tokens**
2. Generate new token
3. Repository access: nur `theredstonee/build-the-future-submission`
4. Permissions → Repository → **Contents: Read and write**
5. Token kopieren, als `PUBLIC_REPO_TOKEN` Secret eintragen

## Lokal entwickeln

```bash
npm install
node build.js
```

Build liest aus `src/`, schreibt encoded/obfuskiert ins Repo-Root.

## Workflow

Jeder Push auf `main` triggert automatisch:

1. ✅ Build (`node build.js`)
2. ✅ FTP-Deploy zur echten Seite
3. ✅ Push der encoded Version ins Public-Repo

Manueller Run: Actions → **Build & Deploy** → Run workflow.

## License

**All Rights Reserved © 2026 Ohev Tamerin** — siehe [LICENSE](LICENSE)
