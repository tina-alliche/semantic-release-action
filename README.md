# ad-semantic-release-action

Action GitHub composite pour automatiser le versioning avec Semantic Release et intégration Jira.

## 🎯 Fonctionnalités

- ✅ Versioning automatique basé sur Conventional Commits
- ✅ Génération de tags Git et releases GitHub
- ✅ Création automatique du CHANGELOG.md
- ✅ Intégration Jira (liens vers tickets dans les release notes)
- ✅ Support de multiples branches de release

## 📦 Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `branches` | ❌ | `["main", "master"]` | Branches pour les releases (JSON array) |
| `jira-host` | ❌ | `jira.desjardins.com` | Domaine Jira pour les liens |
| `github-token` | ✅ | - | Token GitHub pour créer releases/tags |

## 📤 Outputs

| Output | Description |
|--------|-------------|
| `new-version` | Nouvelle version générée (ex: 1.2.3) |
| `new-release-published` | true/false - Une release a été publiée |
| `artifactory-repo` | Repository Artifactory (release-candidates-local ou snapshots-local) |

## 🚀 Utilisation

### Exemple de base

```yaml
- name: Run Semantic Release
  uses: Desjardins/ad-semantic-release-action@v1
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
```

### Exemple avec configuration personnalisée

```yaml
- name: Run Semantic Release
  uses: Desjardins/ad-semantic-release-action@v1
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}
    branches: '["main", "develop", "release/*"]'
    jira-host: jira.desjardins.com
```

### Utilisation des outputs

```yaml
- name: Run Semantic Release
  id: semantic-release
  uses: Desjardins/ad-semantic-release-action@v1
  with:
    github-token: ${{ secrets.GITHUB_TOKEN }}

- name: Use version
  run: |
    echo "Version publiée: ${{ steps.semantic-release.outputs.new-version }}"
    echo "Release créée: ${{ steps.semantic-release.outputs.new-release-published }}"
    echo "Repository: ${{ steps.semantic-release.outputs.artifactory-repo }}"
```

## 📝 Convention Conventional Commits

L'action utilise les commits conventionnels pour déterminer la version:

| Type | Impact Version | Exemple |
|------|----------------|---------|
| `fix:` | PATCH (1.0.0 → 1.0.1) | `fix: correction validation email` |
| `feat:` | MINOR (1.0.0 → 1.1.0) | `feat: ajout export PDF` |
| `feat!:` ou `BREAKING CHANGE:` | MAJOR (1.0.0 → 2.0.0) | `feat!: refonte API` |

### Avec tickets Jira

```bash
git commit -m "feat(INFRA-123): ajout système de cache

Implémentation d'un cache Redis pour améliorer les performances.

Closes INFRA-123"
```

**Résultat**: Lien automatique vers `https://jira.desjardins.com/browse/INFRA-123` dans les release notes.

**⚠️ Important**: Les préfixes Jira avec underscores (ex: `DDD_SD`) ne génèrent pas de liens corrects. Utiliser des préfixes alphanumériques uniquement (ex: `DDDSD`, `INFRA`, `SEC`).

## 🔧 Ce que l'action fait

1. **Installation** des plugins Semantic Release
2. **Configuration** automatique avec support Jira
3. **Analyse** des commits depuis la dernière release
4. **Calcul** de la nouvelle version selon Conventional Commits
5. **Génération** du CHANGELOG.md
6. **Création** du tag Git et release GitHub
7. **Détermination** du repository Artifactory approprié

## 🎯 Comportement selon les branches

| Branche | Dans `branches` input? | Comportement |
|---------|------------------------|--------------|
| `main` | ✅ Oui | Release complète avec tag/version |
| `develop` | ✅ Oui (si configuré) | Release complète |
| `feature/*` | ❌ Non | Dry-run (analyse uniquement, pas de release) |

## 📊 Exemples de résultats

### Release notes générées

```markdown
## [1.2.0](https://github.com/org/repo/compare/v1.1.0...v1.2.0) (2025-02-05)

### Features

* **[INFRA-123](https://jira.desjardins.com/browse/INFRA-123)**: ajout système de cache
* ajout export PDF des rapports

### Bug Fixes

* **[SEC-456](https://jira.desjardins.com/browse/SEC-456)**: correction faille XSS
```

### CHANGELOG.md

```markdown
# Changelog

## [1.2.0] - 2025-02-05

### Features
- **[INFRA-123](https://jira.desjardins.com/browse/INFRA-123)**: ajout système de cache
- ajout export PDF des rapports

### Bug Fixes
- **[SEC-456](https://jira.desjardins.com/browse/SEC-456)**: correction faille XSS
```

## 🔒 Permissions requises

L'action nécessite les permissions GitHub suivantes:

```yaml
permissions:
  contents: write      # Pour créer tags et pusher CHANGELOG
  issues: write        # Pour commenter sur les issues
  pull-requests: write # Pour commenter sur les PRs
```

## 🐛 Troubleshooting

### Problème: "No commits since last release"

**Cause**: Aucun commit `feat:` ou `fix:` depuis le dernier tag.

**Solution**: C'est normal. L'action utilise une version dev dans ce cas.

### Problème: Lien Jira cassé

**Cause**: Préfixe Jira contient des underscores (ex: `DDD_SD`).

**Solution**: Utiliser un préfixe sans underscore (ex: `DDDSD`).

### Problème: "Permission denied"

**Cause**: Token GitHub sans permissions suffisantes.

**Solution**: Vérifier que `github-token` a les permissions `contents: write`.

## 📚 Ressources

- [Semantic Release](https://semantic-release.gitbook.io/)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)

## 📝 Changelog de l'action

### v1.0.0 (2025-02-05)
- Version initiale
- Support Semantic Release avec plugin Jira
- Configuration automatique
- Support multi-branches

## 🤝 Support

Pour toute question ou problème:
- Ouvrir une issue dans ce repo
- Contacter l'équipe DevOps: [email]
