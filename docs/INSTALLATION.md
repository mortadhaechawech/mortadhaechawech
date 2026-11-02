# INSTALLATION — Engineering Command Center

Checklist d'installation du profil `mortadhaechawech`, étapes manuelles incluses.
Le contenu du dépôt est déjà complet et validé localement ; seul ce qui nécessite
l'accès GitHub (push, secrets, première exécution) est manuel.

---

## 0. Prérequis

- Un compte GitHub : `mortadhaechawech` (existe déjà).
- Git installé. Aucun `gh` CLI n'est requis ; tout est faisable dans le navigateur.

## 1. Le dépôt de profil doit s'appeler exactement `mortadhaechawech`

Le README d'un profil n'est affiché que si le dépôt **prive, public, et nommé à
l'identique du nom d'utilisateur** existe : `mortadhaechawech/mortadhaechawech`.

> ⚠️ État vérifié le 2026-11-02 : un dépôt public `mortadhaechawech-` (avec tiret
> final, vide) existe, mais le dépôt `mortadhaechawech` (sans tiret) n'est pas
> joignable publiquement ("Repository not found" — privé ou inexistant).

- **S'il existe en privé** : passez-le en **Public** (Settings → Danger Zone →
  Change repository visibility) — un dépôt de profil doit être public pour
  s'afficher.
- **S'il n'existe pas** : créez-le sur
  <https://github.com/new> avec exactement le nom `mortadhaechawech`, **Public**,
  sans README initial (le repo contient déjà un README).
- Astuce possible : renommer le dépôt vide `mortadhaechawech-` en
  `mortadhaechawech` (Settings → General → Repository name), puis ajouter le
  contenu de ce dossier.

## 2. Pousser le contenu

Depuis `D:\github-profile` :

```bash
git init
git add README.md assets .github docs
git commit -m "feat: engineering command center profile (orbit + metrics)"
git branch -M main
git remote add origin https://github.com/mortadhaechawech/mortadhaechawech.git
git push -u origin main
```

Contenu poussé :

```
README.md
assets/engineering-header.svg      (SVG statique — palette sombre)
assets/engineering-stack.svg       (SVG statique — matrice 9 domaines)
assets/project-showcase.svg        (SVG statique — SHOOTLY/VAYMOS/ASSAY)
assets/github-orbit.svg            (placeholder → remplacé par orbit.yml)
assets/github-metrics.svg          (placeholder → remplacé par metrics.yml)
.github/workflows/orbit.yml        (cron lundi 03:30 UTC + manual)
.github/workflows/metrics.yml      (cron lundi 04:00 UTC + manual)
docs/INSTALLATION.md · CONFIGURATION.md · TROUBLESHOOTING.md
```

## 3. Créer le secret `METRICS_TOKEN` (requis pour metrics.yml)

Un PAT personnel est nécessaire : lowlighter/metrics exige un token
personnel pour interroger les profils/événements via l'API — le
`GITHUB_TOKEN` fourni par le runner n'est pas accepté comme `token:`
principal par cette action (c'est sa convention officielle : `token` est un
input required de type PAT).

1. <https://github.com/settings/tokens?type=beta> (fine-grained) **ou**
   <https://github.com/settings/tokens/new> (classic).
2. Minimal recommandé :
   - **Classic PAT** : scopes = `public_repo` uniquement (lecture/écriture du
     dépôt public de profil, données personnelles via API sans scope `repo`).
   - **Fine-grained PAT** : Repository access = "Only select repositories" →
     `mortadhaechawech/mortadhaechawech` ; Permissions = `Contents: Read and
     write` + `Metadata: Read` (Metadata est auto-cochée).
3. Expiration : 90 jours ou *custom*, à renouveler (noter la date).
4. Dans le dépôt de profil : **Settings → Secrets and variables → Actions →
   New repository secret** :
   - Name : `METRICS_TOKEN` (nom exact — c'est celui référencé dans
     `metrics.yml`)
   - Secret : la valeur du token copiée.

> Orbit (`orbit.yml`) n'a **pas** besoin de secret : il utilise
> `GITHUB_TOKEN` fourni automatiquement par GitHub Actions.

## 4. Activer les Actions et lancer la première génération

1. **Settings → Actions → General** → *Allow all actions and reusable
   workflows* (ou défaut déjà actif).
2. Onglet **Actions** du dépôt → sélectionner
   **Orbit — Engineering Ecosystem** → **Run workflow** → vérifier qu'il passe
   en vert et que `assets/github-orbit.svg` est commité.
3. Idem pour **Metrics — Engineering Intelligence** → `github-metrics.svg`
   doit être commité.
4. Le README affiche alors les graphiques réels à la place des placeholders.

## 5. Épingler 6 dépôts sur le profil

Profil → **Customize your pins**. Liste recommandée (repos réels vérifiés) :

1. `aws-cloudops-platform` — plateforme CI/CD production : Spring Boot 3 → AWS
   ECS Fargate (flagship DevOps).
2. `aws-devops-springboot-postgres` — chaîne complète AWS + Spring Boot +
   PostgreSQL.
3. `spring-boot-rest-api-postgresql` — API REST JVM de référence.
4. `devops-pro-big-project` — projet DevOps étendu.
5. `Pomodoro-Timer` — produit full-stack type application complète.
6. `assay` — dès que le code y est poussé (repository aujourd'hui vide) ; en
   attendant, garder `auth-dashboard-photo` **exclu** (vide) et préférer le
   dépôt de profil lui-même.

> SHOOTLY et VAYMOS n'ont **pas** de dépôt public vérifiable → non épinglables
> tant que les sources ne sont pas publiées.

## 6. Vérification finale sur le profil

- <https://github.com/mortadhaechawech> : README affiché, palette sombre
  cohérente, sections A→H lisibles en mobile.
- Onglet **Actions** : deux workflows au vert ; graphiques générés.
- Badges header : Portfolio/LinkedIn/Email cliquables.
