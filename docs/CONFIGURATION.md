# CONFIGURATION — Engineering Command Center

Référence de configuration des deux intégrations. Toutes les valeurs ci-dessous
sont des **inputs officiels vérifiés** depuis les `action.yml` des projets
(2026-11-02) — rien d'inventé.

---

## 1. Orbit — `.github/workflows/orbit.yml`

Action : [`NiconiKimg/github-profile-orbit`](https://github.com/NiconiKimg/github-profile-orbit),
épinglé sur le commit SHA `c0c956e19bdab11cc75be32613f433a5528a3539` (main, 2026-11-02).

### Inputs utilisés (et pourquoi)

| Input | Valeur | Rôle |
|---|---|---|
| `token` | `${{ secrets.GITHUB_TOKEN }}` | GraphQL — données publiques uniquement |
| `template` | `landscape` | **choix assumé** — matrice architecturale data-driven, esthétique tableau de bord (vs `space` cosmique/flagship). Voir justification §1.1 |
| `theme` | `auto` |adaptation dark/light du viewer (embeds `prefers-color-scheme`) |
| `role` | `"DevOps & Cloud Engineer \| Full-Stack Developer"` | titre professionnel épinglé |
| `custom_title` | `"Engineering Command Center"` | header de la visualisation |
| `output_path` | `assets/github-orbit.svg` | chemin commité automatiquement |
| `exclude_repositories` | `mortadhaechawech-` `auth-dashboard-photo` `react-native-reanimated-loginscreen` | placeholder vide, dépôt vide, fork |
| `max_repositories` | `25` | dépasse largement les 8 dépôts réels retenus |
| `show_ufo/spaceship/satellite/comet` | `false` | rendu sobre, aucune décoration cosmique |
| `show_stats` | `true` | télémétrie **réelle** (stars/commits/forks des dépôts) |
| `auto_commit` | `true` (défaut officiel) | commit seulement si le SVG change (diff SHA-256) |
| `commit_message` | `chore(orbit): … [skip ci]` | anti-boucle de worflows |

`include_forks` et `include_archived` restent à leur défaut officiel `false`
désactivés → le fork `react-native-reanimated-loginscreen` est doublement exclus.

**`assay` n'est PAS exclu** : dès que son code est poussé, il apparaîtra
automatiquement dans l'écosystème.

### 1.1 Choix de template : `landscape` vs `space`

- `landscape` : matrice « architectural », cartes data (radar/telemetry) —
  correspond à l'identité *command center* demandée, meilleur rendu d'un
  écosystème de dépôts DevOps.
- `space` : flagship cosmique (planètes, max 10 dépôts visibles) — spectaculaire
  mais davantage décoratif.
- `network` (constellation) et `minimal` (exécutif) : laissés de côté.
- Les toggles de décoration restent désactivables quel que soit le template ; la
  config actuelle garde `landscape` + déco off. Pour basculer : changer la ligne
  `template:` dans `orbit.yml` (rien d'autre à modifier).

## 2. Metrics — `.github/workflows/metrics.yml`

Action : [`lowlighter/metrics`](https://github.com/lowlighter/metrics), épinglée
sur le SHA `366f8b9dfe3a59656c67d5dcad9950f59c9bc96d` (master, 2026-11-02).

### Inputs utilisés

| Groupe | Inputs | Détail |
|---|---|---|
| Token | `token: ${{ secrets.METRICS_TOKEN }}` | PAT requis (minimal `public_repo`) |
| Sortie | `filename: assets/github-metrics.svg` | même structure de répertoire que l'Orbit |
| Timezone | `config_timezone: Europe/Paris` | cible Europe/France |
| Base | `base: header, activity, community, repositories, metadata` · `base_hireable: no` | compact, sans bandeau hireable |
| Calendrier | `plugin_isocalendar: yes` · `duration: full-year` | calendrier isométrique réel |
| Langages | `plugin_languages: yes` · `limit: 8` | analyse des dépôts publics |
| Habitudes | `plugin_habits: yes` (+ facts + charts) | rythme de commits réel |
| Activité | `plugin_activity: yes` · `limit: 6` · `visibility: public` | événements publics uniquement |
| Dépôts | `plugin_repositories_featured: aws-cloudops-platform, aws-devops-springboot-postgres, spring-boot-rest-api-postgresql` · `forks: no` | repos vérifiés existants |
| Mode sombre | `extras_css` (@media prefers-color-scheme: dark) | sélecteurs calibrés sur le SVG généré réel (2026-11-02) — palette #C9D2E3/#38BDF8/#94A3B8, cellules calendrier #111C2E |

Plugin `languages` en mode `indepth` **non activé** (il clone les dépôts, needs
permissions supplémentaires et étire le rendu vertical — contraire à la
contrainte « compact »).

### Non configuré (et pourquoi)

- `plugin_achievements` — **retiré** : sur un compte avec 3 stars et 0 release,
  les rangs affichés sont décoratifs et allongent le rendu vertical ; à
  ré-activer tardivement si les rangs deviennent significatifs (input
  `plugin_achievements`, display `compact`).
- `base_hireable`: no — statut employeur géré dans le header SVG plutôt que
  dans le rendu metrics.

## 3. Planification

| Workflow | Cron | Déclencheurs |
|---|---|---|
| orbit.yml | `30 3 * * 1` (lundi 03:30 UTC) | schedule + `workflow_dispatch` |
| metrics.yml | `0 4 * * 1` (lundi 04:00 UTC) | schedule + `workflow_dispatch` |

Écart de 30 min entre les deux : aucune ressource concurrente, logs lisibles.
`concurrency` (groupes dédiés, `cancel-in-progress: false`) et messages
`[skip ci]` évitent tout déclenchement en boucle.

## 4. Droits minimums passés dans les fichiers

- `permissions: contents: write` — le strict nécessaire pour committer les SVG.
- Aucun secret codé en dur : `${{ secrets.GITHUB_TOKEN }}` (Orbit) et
  `${{ secrets.METRICS_TOKEN }}` (Metrics).
- Les deux actions tierces sont épinglées sur des **SHAs immuables** (exigence), séparés
  de leur tag lisible en commentaire.
