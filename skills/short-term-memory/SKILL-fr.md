---
name: short-term-memory
description: Maintenir le contexte de travail via current-context.md - lire avant et mettre à jour après chaque réponse avec timestamp YYYYMMDD-HHMMSS
---

# Skill Mémoire Court Terme

**Objectif :** Maintenir le contexte de travail entre les sessions et les compactions via `current-context.md`

---

## Problème & Solution

**Après une compaction de contexte, tu perds :**
- L'élan en cours, les décisions récentes, la prochaine étape logique, l'état mental, les timestamps des décisions

**Solution :** Un fichier mémoire concis et vivant = ton post-it entre les sessions

---

## Workflow Obligatoire

### LIRE -> TRAVAILLER -> METTRE À JOUR -> VÉRIFIER -> ENVOYER

**AVANT chaque réponse :** Lire `current-context.md`
**APRÈS chaque réponse :** Mettre à jour `current-context.md` avec un timestamp

```
1. Lire current-context.md
2. Faire le travail
3. Mettre à jour current-context.md (ajouter timestamp YYYYMMDD-HHMMSS)
4. Vérifier le nombre de lignes (>200 ? archiver les anciennes entrées)
5. Envoyer la réponse
```

---

## Règles Critiques

### 1. Le fichier vit dans le projet

Le fichier est `RÉPERTOIRE_PROJET/current-context.md`. Il vit dans le répertoire du projet, pas dans `~/.claude/`.

### 2. Format des Timestamps

**Utiliser :** préfixe `YYYYMMDD-HHMMSS` pour toutes les entrées. Toujours récupérer le timestamp depuis l'horloge système — jamais en inventer un.

```markdown
✅ 20251230-143022 : shipping-mapping.md créé (cascade relais)
⏸️ 20251230-150133 en attente : décision API vs FTP (contacter fournisseur en jan.)
```

### 3. Des résumés, pas des romans

**Garder les entrées courtes**

❌ MAUVAIS : Créé shipping-mapping.md avec la détection complète de cascade pour les points relais incluant la priorité WCMultiShipping, le fallback Boxtal...

✅ BON : `✅ 20251230-143022 : shipping-mapping.md (relais : WCMultiShipping->Boxtal->transporteur). Corrigé un bug legacy.`

Garder : le QUOI, OÙ, COMMENT — noms de fonctions, noms de fichiers, déplacements.
Ne pas garder : le POURQUOI (sauf si c'est une décision), les phrases complètes.

### 4. C'est une routine, pas une tâche

Ne jamais ajouter "[ ] Mettre à jour current-context.md" dans une todo list. C'est une routine, exécutée à chaque étape.

---

## Structure du Fichier

```markdown
# Current Context

**Updated:** YYYYMMDD-HHMMSS
**Phase:** [SPEC/Implémentation/Debug/etc.]

---

## 🎯 EN COURS (<10 lignes)
- YYYYMMDD-HHMMSS : Action en cours
- YYYYMMDD-HHMMSS : Vient de finir

## ✅ Récemment Terminé (pas de limite)
- YYYYMMDD-HHMMSS : Action 1
- YYYYMMDD-HHMMSS : Action 2

## ✅ Décisions Prises (pas de limite)
- YYYYMMDD-HHMMSS : on ne garde pas la logique package dans la classe modèle
- YYYYMMDD-HHMMSS : on a choisi refacto plutôt qu'évolution

## 🔄 Prochaine Étape Logique (<10 lignes)
- Avant de demander "quoi ensuite ?" -> vérifier [fichier]

## 💡 Décisions Fraîches (<15 lignes)
- YYYYMMDD-HHMMSS : Décision en attente de formalisation

## ⏸️ Bloqueurs Actifs (<10 lignes)
- YYYYMMDD-HHMMSS en attente : Description du bloqueur

## ⚠️ Ne Pas Oublier (<15 lignes)
- YYYYMMDD-HHMMSS : NE PAS faire X explicitement

---

**Archivé :** [si applicable] Voir YYYYMMDDHHMMSS.pastcontext.md
```

---

## Mantra d'Auto-Contrôle

**Première pensée :** "Ai-je lu current-context.md ?"
**Dernière action :** "Ai-je mis à jour current-context.md avec un timestamp ?"

**LIRE -> TRAVAILLER -> METTRE À JOUR (timestamp !) -> VÉRIFIER LIGNES -> ENVOYER**

---

## Critères de Succès

✅ L'utilisateur ne dit jamais "je te l'ai déjà dit"
✅ L'utilisateur ne dit jamais "regarde le contexte"
✅ Tu sais "quoi ensuite" sans demander
✅ Continuation fluide après compaction

❌ Les entrées sont des paragraphes
❌ Timestamps manquants
❌ Poser des questions dont la réponse est déjà dans current-context.md
