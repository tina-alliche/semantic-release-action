# Guide de configuration personnalisée

Ce guide explique comment personnaliser la configuration de Semantic Release pour votre projet.

## Fonctionnement

L'action `ad-semantic-release-action` vérifie automatiquement la présence d'un fichier `.releaserc.json` à la racine de votre projet:

1. ✅ **Si `.releaserc.json` existe** → Utilise votre configuration personnalisée
2. ✅ **Sinon** → Utilise la configuration par défaut

## Configuration par défaut

Voici la configuration par défaut utilisée par l'action:

```json
{
  "branches": ["main", "master"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    [
      "semantic-release-jira-notes",
      {
        "jiraHost": "jira.desjardins.com",
        "preset": "conventionalcommits",
        "presetConfig": {
          "types": [
            { "type": "feat", "section": "Features" },
            { "type": "fix", "section": "Bug Fixes" },
            { "type": "perf", "section": "Performance Improvements" },
            { "type": "revert", "section": "Reverts" },
            { "type": "docs", "section": "Documentation", "hidden": false },
            { "type": "style", "section": "Styles", "hidden": true },
            { "type": "chore", "section": "Miscellaneous Chores", "hidden": true },
            { "type": "refactor", "section": "Code Refactoring", "hidden": true },
            { "type": "test", "section": "Tests", "hidden": true },
            { "type": "build", "section": "Build System", "hidden": true },
            { "type": "ci", "section": "Continuous Integration", "hidden": true }
          ]
        }
      }
    ],
    [
      "@semantic-release/changelog",
      {
        "changelogFile": "CHANGELOG.md"
      }
    ],
    [
      "@semantic-release/exec",
      {
        "prepareCmd": "echo ${nextRelease.version} > .version"
      }
    ],
    [
      "@semantic-release/git",
      {
        "assets": ["CHANGELOG.md"],
        "message": "chore(release): ${nextRelease.version} [skip ci]\n\n${nextRelease.notes}"
      }
    ],
    "@semantic-release/github"
  ]
}
```

## Exemples de personnalisation

### Exemple 1: Ajouter des emojis

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    [
      "semantic-release-jira-notes",
      {
        "jiraHost": "jira.desjardins.com",
        "preset": "conventionalcommits",
        "presetConfig": {
          "types": [
            { "type": "feat", "section": "✨ Features" },
            { "type": "fix", "section": "🐛 Bug Fixes" },
            { "type": "perf", "section": "⚡ Performance" },
            { "type": "docs", "section": "📚 Documentation", "hidden": false },
            { "type": "test", "section": "✅ Tests", "hidden": false }
          ]
        }
      }
    ],
    "@semantic-release/changelog",
    "@semantic-release/git",
    "@semantic-release/github"
  ]
}
```

### Exemple 2: Afficher tous les types de commits

Par défaut, certains types sont cachés (style, refactor, test, etc.). Pour tout afficher:

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    [
      "semantic-release-jira-notes",
      {
        "jiraHost": "jira.desjardins.com",
        "preset": "conventionalcommits",
        "presetConfig": {
          "types": [
            { "type": "feat", "section": "Features", "hidden": false },
            { "type": "fix", "section": "Bug Fixes", "hidden": false },
            { "type": "perf", "section": "Performance", "hidden": false },
            { "type": "docs", "section": "Documentation", "hidden": false },
            { "type": "style", "section": "Styles", "hidden": false },
            { "type": "refactor", "section": "Refactoring", "hidden": false },
            { "type": "test", "section": "Tests", "hidden": false },
            { "type": "build", "section": "Build", "hidden": false },
            { "type": "ci", "section": "CI/CD", "hidden": false },
            { "type": "chore", "section": "Chores", "hidden": false }
          ]
        }
      }
    ],
    "@semantic-release/changelog",
    "@semantic-release/git",
    "@semantic-release/github"
  ]
}
```

### Exemple 3: Personnaliser le titre du CHANGELOG

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "semantic-release-jira-notes",
    [
      "@semantic-release/changelog",
      {
        "changelogFile": "CHANGELOG.md",
        "changelogTitle": "# 📝 Historique des versions\n\nToutes les modifications notables de ce projet sont documentées dans ce fichier.\n\nLe format est basé sur [Keep a Changelog](https://keepachangelog.com/fr/1.0.0/)."
      }
    ],
    "@semantic-release/git",
    "@semantic-release/github"
  ]
}
```

### Exemple 4: Commiter des fichiers supplémentaires

Si vous voulez commiter d'autres fichiers lors de la release (ex: `package.json`, `version.txt`):

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "semantic-release-jira-notes",
    "@semantic-release/changelog",
    [
      "@semantic-release/git",
      {
        "assets": [
          "CHANGELOG.md",
          "package.json",
          "version.txt",
          "docs/**/*.md"
        ],
        "message": "chore(release): ${nextRelease.version} [skip ci]\n\n${nextRelease.notes}"
      }
    ],
    "@semantic-release/github"
  ]
}
```

### Exemple 5: Branches multiples avec pre-releases

```json
{
  "branches": [
    "main",
    {
      "name": "develop",
      "prerelease": "beta"
    },
    {
      "name": "next",
      "prerelease": "alpha"
    }
  ],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "semantic-release-jira-notes",
    "@semantic-release/changelog",
    "@semantic-release/git",
    "@semantic-release/github"
  ]
}
```

**Résultat:**
- `main` → `1.0.0`, `1.1.0`, `2.0.0`
- `develop` → `1.1.0-beta.1`, `1.1.0-beta.2`
- `next` → `2.0.0-alpha.1`, `2.0.0-alpha.2`

### Exemple 6: Désactiver le plugin Jira

Si vous ne voulez pas de liens Jira:

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    "@semantic-release/git",
    "@semantic-release/github"
  ]
}
```

⚠️ **Note:** Remplacer `semantic-release-jira-notes` par `@semantic-release/release-notes-generator`

### Exemple 7: Personnaliser le message de commit de release

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "semantic-release-jira-notes",
    "@semantic-release/changelog",
    [
      "@semantic-release/git",
      {
        "assets": ["CHANGELOG.md"],
        "message": "🚀 Release v${nextRelease.version}\n\n${nextRelease.notes}\n\nGenerated by Semantic Release"
      }
    ],
    "@semantic-release/github"
  ]
}
```

## Options avancées

### Analyser les commits différemment

```json
{
  "branches": ["main"],
  "plugins": [
    [
      "@semantic-release/commit-analyzer",
      {
        "preset": "angular",
        "releaseRules": [
          { "type": "docs", "scope": "README", "release": "patch" },
          { "type": "refactor", "release": "patch" },
          { "type": "style", "release": "patch" }
        ]
      }
    ],
    "semantic-release-jira-notes",
    "@semantic-release/changelog",
    "@semantic-release/git",
    "@semantic-release/github"
  ]
}
```

Cette configuration fait en sorte que les commits `docs:`, `refactor:` et `style:` génèrent aussi des PATCH releases.

### Exécuter des commandes custom

```json
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "semantic-release-jira-notes",
    "@semantic-release/changelog",
    [
      "@semantic-release/exec",
      {
        "prepareCmd": "echo ${nextRelease.version} > version.txt && npm run build-docs",
        "publishCmd": "npm run notify-slack -- --version ${nextRelease.version}"
      }
    ],
    "@semantic-release/git",
    "@semantic-release/github"
  ]
}
```

## Migration depuis la config par défaut

1. **Copier la config par défaut** dans `.releaserc.json`
2. **Personnaliser** selon vos besoins
3. **Tester** sur une branche de test
4. **Commiter** le fichier

```bash
# Créer le fichier
cat > .releaserc.json << 'EOF'
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "semantic-release-jira-notes",
    "@semantic-release/changelog",
    "@semantic-release/git",
    "@semantic-release/github"
  ]
}
EOF

# Commiter
git add .releaserc.json
git commit -m "chore: add custom Semantic Release configuration"
git push
```

## Validation de la configuration

Pour tester votre configuration sans créer de release:

```bash
# Installer Semantic Release localement
npm install -D semantic-release

# Dry-run
npx semantic-release --dry-run
```

## Plugins disponibles

Les plugins suivants sont pré-installés par l'action:
- `@semantic-release/commit-analyzer`
- `@semantic-release/release-notes-generator`
- `@semantic-release/git`
- `@semantic-release/github`
- `@semantic-release/changelog`
- `@semantic-release/exec`
- `semantic-release-jira-notes`
- `conventional-changelog-conventionalcommits`

Si vous voulez utiliser d'autres plugins, contactez l'équipe DevOps.

## Ressources

- [Documentation Semantic Release](https://semantic-release.gitbook.io/)
- [semantic-release-jira-notes](https://github.com/iamludal/semantic-release-jira-notes)
- [Configuration de Semantic Release](https://semantic-release.gitbook.io/semantic-release/usage/configuration)
