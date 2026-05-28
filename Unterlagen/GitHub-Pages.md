# GitHub Pages – Webseiten kostenlos hosten

## Was ist GitHub Pages?

GitHub Pages ist ein Hosting-Service von GitHub, der statische Websites (HTML, CSS, JS) direkt aus einem GitHub-Repository veröffentlicht. Die Seite ist dann unter `https://<benutzername>.github.io/<repository>/` erreichbar.

## Voraussetzungen

- Ein GitHub-Repository mit deinem Projekt
- Der Code liegt auf dem `main`-Branch (oder einem anderen Branch deiner Wahl)
- Dein Projekt besteht aus statischen Dateien (HTML, CSS, JS, Bilder)

## Einrichtung

### 1. GitHub Pages aktivieren

Im Repository: **Settings → Pages → Source** → `GitHub Actions` auswählen.

### 2. Workflow-Datei erstellen

Lege im Repository die Datei `.github/workflows/deploy.yml` an:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: ["main"]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/configure-pages@v5
      - uses: actions/upload-pages-artifact@v3
        with:
          path: './mein-projekt-ordner'
      - id: deployment
        uses: actions/deploy-pages@v4
```

**Wichtig:** Ersetze `./mein-projekt-ordner` durch den Pfad zu deinem gewünschten Verzeichnis (z. B. `./docs`, `./dist`, `./meine-app`).

### 3. Commit & Push

```
git add .github/workflows/deploy.yml
git commit -m "Add GitHub Pages deploy workflow"
git push
```

Nach dem Push wird das Deployment automatisch gestartet. Den Fortschritt siehst du im Repository unter **Actions**.

## Wichtige Hinweise

- **Öffentlich:** GitHub Pages ist immer öffentlich – jeder kann die Seite sehen.
- **Nur statische Inhalte:** Kein PHP, keine Datenbank, kein Server-Backend.
- **Eigenes Repository:** Jedes Repository hat genau eine GitHub-Pages-Website.
- **Benutzerdefinierte Domain:** Eigene Domain ist möglich (Settings → Pages → Custom domain).
- **Build-Schritt möglich:** Falls dein Projekt einen Build-Schritt braucht (z. B. npm run build), füge vor dem Upload-Artifact-Step einen Build-Step ein.
