# Contributing to ad-semantic-release-action

Merci de votre intérêt pour contribuer à cette action! 🎉

## 🐛 Signaler un bug

1. Vérifier si le bug n'a pas déjà été signalé dans les [Issues](https://github.com/Desjardins/ad-semantic-release-action/issues)
2. Créer une nouvelle issue avec:
   - Description claire du problème
   - Steps pour reproduire
   - Comportement attendu vs observé
   - Version de l'action utilisée
   - Logs pertinents

## 💡 Proposer une amélioration

1. Ouvrir une issue pour discuter de la fonctionnalité
2. Attendre l'approbation avant de commencer le développement
3. Créer une Pull Request liée à l'issue

## 🔧 Développement

### Setup local

```bash
git clone git@github.com:Desjardins/ad-semantic-release-action.git
cd ad-semantic-release-action
```

### Tests

Les tests se lancent automatiquement sur chaque push/PR. Pour tester localement:

```bash
# Créer un repo de test
mkdir test-repo
cd test-repo
git init

# Tester l'action
# (voir .github/workflows/test-action.yml pour les scénarios)
```

### Convention de commits

Utiliser [Conventional Commits](https://www.conventionalcommits.org/):

```bash
feat: ajout nouvelle fonctionnalité
fix: correction de bug
docs: mise à jour documentation
test: ajout/modification de tests
chore: tâches de maintenance
```

### Pull Request

1. Fork le repo
2. Créer une branche: `git checkout -b feature/ma-fonctionnalite`
3. Commiter avec des messages conventionnels
4. Pousser: `git push origin feature/ma-fonctionnalite`
5. Ouvrir une PR avec:
   - Description claire des changements
   - Référence à l'issue liée
   - Screenshots si pertinent
   - Tests ajoutés/modifiés

### Code Review

- Les PRs nécessitent au moins 1 approbation
- Tous les tests doivent passer
- Le code doit respecter les conventions existantes

## 📝 Documentation

Toute modification du comportement doit être documentée dans:
- `README.md` - Usage et exemples
- `action.yml` - Descriptions des inputs/outputs
- Commentaires dans le code si nécessaire

## 🚀 Release Process

Les releases sont automatiques via Semantic Release:
1. Merge vers `main` avec commits conventionnels
2. Semantic Release calcule la version
3. Tag et release GitHub créés automatiquement
4. CHANGELOG mis à jour

## ❓ Questions

Pour toute question, ouvrir une [Discussion](https://github.com/Desjardins/ad-semantic-release-action/discussions) ou contacter l'équipe DevOps.

## 📜 Code of Conduct

Soyez respectueux et professionnel dans toutes les interactions.

Merci de contribuer! 🙏
