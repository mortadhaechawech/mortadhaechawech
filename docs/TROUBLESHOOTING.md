# TROUBLESHOOTING — Engineering Command Center

## Principe général

Deux familles de vérifications :

- **Validé en local** : XML des SVG (viewBox, structure), YAML des deux
  workflows, chemins référencés par le README — tout vérifié sans pousser.
- **GitHub-hosted (après push)** : la génération réelle des graphiques, les
  couleurs de rendu, les quotas API et les tokens.

---

## 1. Orbit (`orbit.yml`)

### `assets/github-orbit.svg` reste le placeholder
- Cause : le workflow n'a jamais été lancé → **Actions → Orbit — Engineering
  Ecosystem → Run workflow**, ou `orbit.yml` a échoué → lire le log du step
  *Generate orbit visualization*.

### Erreur d'authentification GraphQL
- `GITHUB_TOKEN` fourni par l'instance est suffisant (données publiques).
- Si le job échoue sur les permissions de commit : **Settings → Actions →
  General → Workflow permissions** → cocher *Read and write permissions*
  (le workflow déclare déjà `permissions: contents: write`).

### Rendu sombre / clair
- `theme: auto` embarque un switch `prefers-color-scheme` : le rendu s'adapte au
  **thème GitHub de l'observateur**, pas au vôtre. Si besoin, passer
  `theme: dark` en dur (input officiel) et relancer.

### Changer les dépôts visualisés
- Ajouter/enlever une ligne dans `exclude_repositories` (multi-ligne et
  wildcards supportés, ex. `*-test`).
- `assay` n'est **pas** exclu : dès que son code est poussé, il interviendra
  automatiquement dans la visualisation.
- Repos de référence externe : input officiel `external_repositories`.

## 2. Metrics (`metrics.yml`)

### Le run échoue immédiatement : secret manquant
- Symptôme : *token is required* / *401 Bad credentials*.
- Cause : secret `METRICS_TOKEN` inexistant → **Settings → Secrets and
  variables → Actions → New repository secret**, nom exact `METRICS_TOKEN`,
  PAT minimal `public_repo` (classic) — voir `docs/INSTALLATION.md` §3.

### Le SVG n'est pas commité dans `assets/`
- `filename: assets/github-metrics.svg` est un chemin relatif au dépôt ; le
  sous-répertoire doit exister dans le repo avant la première exécution.
  Il existe ici (les placeholders `assets/github-metrics.svg` sont poussés).
- Si une évolution de l'action refusait un sous-répertoire : repasser
  `filename: github-metrics.svg` (racine) **et** mettre à jour la source du
  `<img>` dans le README en conséquence — les deux points doivent toujours
  s'accorder.

### Contraste du SVG metrics sur fond sombre
- Ne rien patcher à l'aveugle : d'abord **ouvrir le SVG généré** et constater.
- Si un contraste réel manque, `extras_css` (input officiel « Extra CSS »)
  permet d'injecter du CSS dans le rendu, ex. :

  ```yaml
          extras_css: |
            @media (prefers-color-scheme: dark) {
              /* sélecteurs à cibler d'après les classes lues dans le SVG généré */
            }
  ```
- Ne pas deviner les classes : elles dépendent du template rendu réel.

### Quotas / rate limit
- L'action possède ses propres retry (`retries`, `retries_delay`).
- Ne pas partager le `METRICS_TOKEN` avec d'autres workflows tiers — quota
  indépendant par token.

### Instance cachée / données périmées
- Les SVG sont régénérés depuis l'API live à chaque run planifié ; si les
  données affichées semblent périmées, re-lancer le workflow manuellement
  pour vérifier.

## 3. Généralités GitHub Actions

### Le cron ne démarre jamais seul
- Après le **premier** push d'un nouveau fichier de workflow, aucune instance
  planifiée n'existe encore : un run manuel (`workflow_dispatch`) ou un push
  plus tard active la planification.
- Les crons sont en UTC : 03:30/04:00 UTC ≈ 04:30/05:00 en Europe/Paris.

### Boucle infinie de commits
- Ne peut pas se produire ici : aucun des deux workflow n'écoute `on: push`
  (schedule + workflow_dispatch seulement), et les deux messages de commit
  portent `[skip ci]`. `auto_commit` d'Orbit ne commite en plus que si le SVG
  a réellement changé (diff SHA-256).

### Action tierce cassée après une mise à jour
- Les deux actions sont épinglées sur des SHAs immuables ; pour monter de
  version, remplacer le SHA par le commit d'une nouvelle release et relancer
  manuellement les deux workflows une fois (test), le tag lisible reste en
  commentaire.

## 4. Palette

Palette unique des SVG statiques et placeholders :

| Rôle | Hex |
|---|---|
| Background | `#080D18` |
| Secondary / panel | `#0D1524` |
| Cards | `#111C2E` |
| Borders | `#23334B` |
| Primary | `#38BDF8` |
| Secondary | `#818CF8` |
| Text | `#E5EDF7` |
| Muted | `#94A3B8` |
| Success | `#34D399` |

Les SVG générés par les deux actions suivent leur propre gabarit officiel ;
toute correction de couleur passe par les inputs officiels (`theme` pour
Orbit, `extras_css` pour metrics) — jamais par retouche manuelle du fichier
généré, qui serait écrasé au run suivant.
