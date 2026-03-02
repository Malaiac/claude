## Journal

CHAQUE session, tu DOIS ajouter une entrée au journal mensuel.

### Quand écrire

- À chaque étape significative (bug fixé, feature terminée, refacto fait)
- À chaque a-ha moment ou pivot décisionnel
- Quand l'utilisateur signale la fin de session ("bonne nuit", "on arrête là", "je file")

**"merci", "c'est bon", "done", "ok" = accusé de réception, PAS fin de session.** Seules les expressions explicites de départ comptent.

**Un peu de bruit vaut mieux que des épisodes manqués.** Plusieurs entrées par session = normal.

### Fichier

`JOURNAL_DIR/YYYY-MM.journal.md` (remplacer YYYY-MM par l'année-mois courant).

Créer le fichier du mois s'il n'existe pas.

### Exclusions

Certains répertoires peuvent être exclus de la journalisation. Les définir ici :

```
EXCEPTION : `PROJECT_ROOT/projet-prive` — AUCUN journal, AUCUN log, AUCUNE trace externe.
```

### Format

```
## YYYY-MM-DD HH:MM | [répertoire projet] | [contexte libre]
[Résumé naturel de ce qui a été fait, discuté, décidé. 2 à 10 lignes selon la substance de la session. Privilégier le contenu utile plutôt que la brièveté, mais ne pas sombrer dans le détail technique inutile.]
```

Le répertoire projet = chemin relatif depuis la racine (ex: `web/plugins/mon-plugin`, `projets/acme-app`). Le contexte libre = description courte optionnelle.

### Exemples

```
## 2026-01-11 14:30 | web/plugins/entrestock | v1.2
Travaillé sur la v1.2 du plugin. Amélioré la sécurité des requêtes SQL avec prepare(). Ajouté la fonction de rollback en cas d'échec de sync. Pas mis en prod, on attend lundi pour valider avec le client.

## 2026-01-11 16:00 | web/plugins/payfip-paiements | architecture webhooks
Réflexion sur la structure des webhooks pour le projet payfip. Décidé de partir sur une queue async plutôt que du traitement synchrone. À creuser : Redis vs Action Scheduler.

## 2026-01-11 18:00 | .claude | idée en passant
L'utilisateur a mentionné vouloir un système de tags pour ses projets. Noté pour plus tard, pas de suite immédiate.
```

### Règles

- **TOUJOURS écrire**, même pour une session courte ou une simple question (une ligne suffit)
- **Append only** — ne jamais éditer les entrées précédentes, sauf demande explicite
- **Créer le fichier du mois** s'il n'existe pas
- **Timestamps = horloge système** — ne jamais inventer un timestamp
- Si la quantité de contenu entre en conflit avec la brièveté du journal, écrire un fichier détaillé dans le projet et garder l'entrée journal concise

### Concurrence

Utiliser Bash avec `>>` (append) plutôt que Read/Edit pour éviter les conflits entre sessions simultanées :

```bash
printf '## 2026-01-19 13:30 | projet\nContenu de la session...\n\n' >> "JOURNAL_DIR/2026-01.journal.md"
```
