# OpenCode Go Provider — Correctif local VS Code 1.120

Date : 2026-05-13

## Objectif

Restaurer l’apparition des modèles OpenCode Go dans le sélecteur de modèles de Copilot Chat après la mise à jour VS Code 1.120.0.

## Diagnostic résumé

- L’extension Marketplace installée était `hidenobunagai.opencode-go-provider@0.1.40`.
- Les modèles OpenCode Go apparaissaient encore dans la vue `Language Models`.
- Les mêmes modèles n’apparaissaient plus dans le sélecteur de modèles Copilot Chat.
- Le manifeste `0.1.40` utilisait `activationEvents: ["onStartupFinished"]` et ne déclarait pas `extensionKind`.
- Le cœur VS Code 1.120 filtre les modèles du sélecteur avec `metadata.isUserSelectable`; les modèles OpenCode Go ne déclaraient pas explicitement ce champ.

## Décision d’implémentation

Le dépôt GitHub upstream cloné indique `0.1.38`, alors que l’installation locale Marketplace contient `0.1.40`. Pour éviter de régresser les correctifs `0.1.39` et `0.1.40`, le VSIX local corrigé a été produit à partir du payload installé `0.1.40`, copié dans `local-vsix-0.1.40`.

## Correctif appliqué

Dans `local-vsix-0.1.40/package.json` :

- Version portée à `0.1.41-local.5`.
- Ajout de `extensionKind: ["ui"]`.
- Remplacement de `activationEvents: ["onStartupFinished"]` par `activationEvents: []`.
- Suppression du `required: ["apiKey"]` dans le schéma de configuration du provider, afin de ne pas bloquer le picker.
- Suppression du script `vscode:prepublish`, car ce payload local contient déjà les fichiers compilés `out/` et ne contient pas les sources TypeScript `0.1.40`.
- Ajustement du script `package:vsix` vers un packaging direct.

Dans `local-vsix-0.1.40/out/extension.js` :

- Ajout d’un signal `fireModelInfoChanged()` juste après l’enregistrement du provider.

Dans `local-vsix-0.1.40/out/provider.js` :

- Ajout de `isUserSelectable: true` aux objets retournés par `provideLanguageModelChatInformation()`.
- Ajout de métadonnées UI compatibles avec VS Code/Copilot (`multiplierNumeric`, `pricing`).
- Passage de `family: "opencode-go"` à une famille unique par modèle (`family: info.id`) pour éviter les collisions de familles.
- Ajout d’un garde anti-doublon : les modèles ne sont retournés que lorsqu’un contexte de groupe/configuration est fourni.

## Artefact généré

- `local-vsix-0.1.40/opencode-go-provider-0.1.41-local.5.vsix`

## Installation effectuée

Le VSIX local a été installé avec le wrapper CLI VS Code :

- Extension installée : `hidenobunagai.opencode-go-provider@0.1.41-local.5`
- Dossier installé : `C:\Users\Philippe\.vscode\extensions\hidenobunagai.opencode-go-provider-0.1.41-local.5`

## Vérifications réalisées

- Validation syntaxique des fichiers JavaScript précompilés dans `out/`.
- Packaging VSIX réussi avec `@vscode/vsce`.
- Installation VSIX réussie via `code.cmd --install-extension --force`.
- Manifeste installé relu et confirmé : `extensionKind: ["ui"]`, `activationEvents: []`, version `0.1.41-local.5`.
- Test utilisateur confirmé : les modèles OpenCode Go sont revenus dans le sélecteur Copilot Chat.
- Doublons initialement observés après `local.3`/`local.4`, puis supprimés après suppression et ré-ajout d’OpenCode Go dans la fenêtre `Language Models`, ce qui a nettoyé le groupe/cache de modèles.

## Vérification restante recommandée

Après reload de VS Code ou dans une nouvelle fenêtre :

1. Ouvrir Copilot Chat.
2. Ouvrir le sélecteur de modèles.
3. Vérifier que les modèles OpenCode Go réapparaissent une seule fois.
4. Tester un prompt court sans donnée sensible.
5. Vérifier les logs Extension Host si les modèles restent absents.

## Note

La version Marketplace `0.1.40` et les révisions locales intermédiaires peuvent rester présentes dans le dossier d’extensions, mais VS Code liste désormais la version active comme `0.1.41-local.5`.
