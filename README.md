# 🚀 OpenCode GOpilot Chat

**Parce que taper du code tout seul, c'est has-been.**

L'extension VS Code qui te permet de chatter avec les modèles OpenCode Go directement dans Copilot Chat. Kimi, DeepSeek, GLM, Qwen, MiniMax, MiMo... ils sont tous là, prêts à répondre à tes questions existentielles (ou juste à t'aider à debug ce `undefined is not a function` à 3h du mat').

> ⚡ **Fork maintenu** de l'excellent travail original de [Hidenobu Nagai](https://github.com/hidenobunagai/opencode-go-provider), patché pour VS Code 1.120+ et avec un soupçon d'amour en plus.

---

## ✨ Ce que ça fait

- Connecte tes modèles OpenCode Go préférés à Copilot Chat
- Gère ta clé API en toute sécurité (chiffrement local, on ne plaisante pas avec ça)
- Supporte le vision (analyse d'images) pour les modèles qui voient mieux que nous
- Compatible outils (tool calling) pour les workflows avancés
- Anti-doublon intelligent dans le sélecteur de modèles (parce qu'un seul Kimi K2.6 suffit)

---

## 📋 Prérequis

- VS Code **1.104.0** ou plus récent (idéalement 1.120+ pour le model picker tout neuf)
- L'extension **GitHub Copilot** installée et active
- Une clé API OpenCode Go ([récupère-la ici](https://opencode.ai/) — oui, ça coûte un peu, mais moins cher qu'un café par jour à San Francisco)

---

## 🛠️ Installation

### Depuis le VSIX (la méthode rapide)

1. Télécharge le fichier `.vsix` depuis les releases
2. Dans VS Code, va dans l'onglet **Extensions** (`Ctrl+Shift+X`)
3. Clique sur `...` → **Install from VSIX...**
4. Sélectionne le fichier, croise les doigts, et c'est parti

### Depuis les sources (la méthode pour les barbus)

```bash
git clone https://github.com/TON-REPO/opencode-go-provider.git
cd opencode-go-provider
bun install --ignore-scripts
bun run compile
# Appuie sur F5 dans VS Code pour lancer le Extension Development Host
```

---

## 🎛️ Configuration

1. Ouvre **Copilot Chat** (`Ctrl+Alt+I` ou `Cmd+Alt+I` sur Mac)
2. Clique sur le sélecteur de modèle (en haut, là où c'est écrit "GPT-4" ou autre)
3. Choisis **Manage Models** → **Add Language Model**
4. Sélectionne **OpenCode GOpilot**
5. Rentre ta clé API quand on te la demande
6. Ferme les yeux, respire, ouvre les yeux — tes modèles apparaissent

> 💡 **Astuce pro** : si tu veux changer ta clé plus tard, cherche `OpenCode GOpilot: Manage API Key` dans la palette de commandes (`Ctrl+Shift+P`).

---

## 🤖 Modèles supportés

La liste est hardcodée dans l'extension (oui, on sait, c'est pas idéal, mais c'est comme ça pour l'instant). Quand OpenCode Go ajoute un nouveau modèle, on met à jour et on republie.

| Modèle | Spécialité |
|--------|-----------|
| **GLM-5 / GLM-5.1** | Le cerveau chinois qui cartonne |
| **DeepSeek V4 Pro / Flash** | Raisonnement poussé, mais ne dit pas toujours son vrai nom |
| **Kimi K2.5 / K2.6** | Notre préféré. Long contexte, rapide, et il sait faire les blagues |
| **MiMo-V2-Pro / Omni / V2.5** | Le nouveau venu qui monte |
| **MiniMax M2.5 / M2.7 / M3** | Polyvalent, efficace, M3 le nouveau venu |
| **Qwen3.5 Plus / 3.6 Plus** | L'expert Alibaba qui en sait trop |
| **Qwen3.7 Max** | Le nouveau boss d'Alibaba, contexte 1M tokens |

---

## 🎮 Utilisation

1. Ouvre Copilot Chat (`Ctrl+Alt+I`)
2. Sélectionne **OpenCode GOpilot** dans le menu déroulant
3. Choisis un modèle (Kimi K2.6 si tu veux notre avis, mais on est biaisés)
4. Pose ta question. N'importe laquelle. Même "pourquoi le code ne marche pas ?" (spoiler : c'est toujours un point-virgule)

---

## 🔧 Développement

Envie de contribuer ? Génial ! Voici les commandes utiles :

```bash
# Installer les dépendances
bun install --ignore-scripts

# Compiler
bun run compile

# Compiler en mode watch (pour les fainéants productifs)
bun run watch

# Linter (pour éviter les hontes en code review)
bun run lint

# Linter + auto-fix (pour les hontes qu'on veut cacher)
bun run lint:fix

# Tests (pour les courageux)
bun run test -- --runInBand

# Créer le VSIX
bun run package:vsix
```

Appuie sur `F5` dans VS Code pour lancer l'Extension Development Host et tester en live.

---

## 🐛 Dépannage

### DeepSeek dit qu'il s'appelle Claude

C'est pas un bug de l'extension, c'est DeepSeek qui est timide. Pour vérifier :

```bash
export OPENCODE_GO_API_KEY="ta-clef-super-secrete"
bun run repro:deepseek -- "What model are you?"
```

Si la réponse est toujours "Claude", le problème est chez OpenCode Go (routing upstream). On y peut rien, désolé.

### Les modèles n'apparaissent pas dans le picker

1. Vérifie que l'extension est bien activée
2. Vide le cache : supprime/réajoute OpenCode GOpilot dans **Language Models** (`Ctrl+Shift+P` → "Developer: Show Language Models")
3. Redémarre VS Code (la solution universelle à 90% des problèmes informatiques)

### "API key not configured"

Tu n'as pas encore rentré ta clé. Retourne dans la configuration (voir section Configuration ci-dessus) et fais les choses bien.

---

## 🔒 Confidentialité

- Ta clé API est stockée dans le **SecretStorage** de VS Code (chiffrée, locale, on n'y a pas accès)
- Les requêtes de chat partent vers `https://opencode.ai/zen/go/v1`
- On ne collecte rien. Pas de telemetry, pas de tracking, pas de cookies qui te suivent sur Internet. On déteste ça autant que toi.

---

## 📦 Packaging Marketplace

```bash
bun run package:vsix
```

Le fichier `.vsix` généré peut être uploadé sur le portail éditeur du VS Code Marketplace. Si tu veux publier toi-même, crée un compte éditeur chez Microsoft et suis leur doc (c'est long, c'est bureaucratique, mais ça marche).

---

## 🙏 Remerciements

- [Hidenobu Nagai](https://github.com/hidenobunagai) pour l'extension originale — sans lui, on serait encore en train de copier-coller des prompts dans un terminal
- L'équipe OpenCode Go pour leur API rapide et stable
- Les modèles qui répondent à 3h du mat' sans se plaindre

---

## 📄 Licence

MIT. Fais-en ce que tu veux. Copie, modifie, vend — on s'en fiche. Juste mentionne l'original si tu republies, c'est la moindre des politesses.

---

> **PStAntArt** — *"Parce que l'IA c'est cool, mais l'IA dans VS Code c'est encore mieux.* 🎯
