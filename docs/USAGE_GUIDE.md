# Guide d'Utilisation - MCP n8n & Context7

## Table des matières

1. [Exemples Context7](#exemples-context7)
2. [Exemples n8n MCP](#exemples-n8n-mcp)
3. [Cas d'usage combinés](#cas-dusage-combinés)
4. [Workflows recommandés](#workflows-recommandés)

---

## Exemples Context7

Context7 vous donne accès à la documentation la plus récente des frameworks et bibliothèques.

### Exemple 1 : Obtenir de la documentation Next.js

**Prompt** :
```
use context7 - Comment implémenter le streaming dans Next.js 14+ ?
```

**Résultat attendu** :
Claude vous fournira la documentation la plus récente sur le streaming dans Next.js, incluant les nouvelles APIs et les meilleures pratiques.

### Exemple 2 : Comparer des versions

**Prompt** :
```
@context7 Quelles sont les différences entre React 18 et React 19 pour les hooks ?
```

### Exemple 3 : Syntaxe spécifique

**Prompt** :
```
use context7 - Montre-moi la syntaxe pour créer un Server Component dans Next.js
```

### Frameworks supportés

- **Frontend** : React, Vue, Svelte, Angular
- **Fullstack** : Next.js, Nuxt, SvelteKit, Remix
- **Backend** : Express, Fastify, NestJS
- **Styling** : Tailwind CSS, Styled Components
- **State Management** : Redux, Zustand, Jotai

---

## Exemples n8n MCP

Le serveur MCP n8n vous permet de contrôler votre instance n8n directement depuis Claude Code.

### Exemple 1 : Lister les workflows

**Prompt** :
```
Liste tous mes workflows n8n
```

**Actions** :
- Récupère la liste complète des workflows
- Affiche : nom, ID, statut (actif/inactif), dernière modification

### Exemple 2 : Créer un workflow simple

**Prompt** :
```
Crée-moi un workflow n8n qui :
1. Se déclenche avec un webhook
2. Parse le JSON reçu
3. Envoie un email avec les données

Nomme-le "Webhook to Email"
```

**Actions** :
- Crée le workflow avec les nodes nécessaires
- Configure les connections
- Retourne l'ID et l'URL du webhook

### Exemple 3 : Modifier un workflow existant

**Prompt** :
```
Ajoute une condition IF au workflow "Webhook to Email" pour envoyer l'email seulement si le champ "priority" est "high"
```

### Exemple 4 : Exécuter un workflow

**Prompt** :
```
Exécute le workflow "Webhook to Email" avec ces données de test :
{
  "name": "Test User",
  "email": "test@example.com",
  "priority": "high",
  "message": "Ceci est un test"
}
```

### Exemple 5 : Consulter l'historique

**Prompt** :
```
Montre-moi les 10 dernières exécutions du workflow "Webhook to Email"
```

### Exemple 6 : Debugger un workflow

**Prompt** :
```
Pourquoi le workflow "Webhook to Email" a échoué lors de sa dernière exécution ?
```

**Actions** :
- Analyse les logs d'erreur
- Identifie le node problématique
- Suggère des corrections

---

## Cas d'Usage Combinés

### Cas 1 : Créer un workflow avec la dernière syntaxe

**Prompt** :
```
use context7 - Quelle est la meilleure façon de faire des requêtes HTTP avec retry logic en 2025 ?

Ensuite, crée-moi un workflow n8n qui utilise cette approche pour :
1. Appeler une API externe
2. Retry 3 fois en cas d'échec
3. Logger les résultats
```

**Avantages** :
- Documentation à jour via Context7
- Implémentation directe via n8n MCP

### Cas 2 : Migration de code vers n8n

**Prompt** :
```
use context7 - Montre-moi comment faire de l'authentification OAuth2 moderne

Puis transforme cet exemple en workflow n8n pour authentifier avec l'API de Google
```

### Cas 3 : Optimisation de workflow

**Prompt** :
```
Analyse mon workflow "Data Processing" et utilise context7 pour trouver les meilleures pratiques actuelles pour le traitement de données en streaming
```

---

## Workflows Recommandés

### 1. Webhook to Database

**Description** : Reçoit des données via webhook et les stocke dans une base de données

**Prompt de création** :
```
Crée un workflow n8n "Webhook to Database" qui :
1. Reçoit des données JSON via webhook
2. Valide les données (champs requis : name, email, timestamp)
3. Transforme la date en format ISO
4. Stocke dans une base de données PostgreSQL
5. Retourne une confirmation au client
```

**Use cases** :
- Collecte de leads
- Enregistrement d'événements
- Formulaires web

### 2. Scheduled Data Sync

**Description** : Synchronise des données entre deux services toutes les heures

**Prompt de création** :
```
Crée un workflow n8n "Scheduled Data Sync" qui :
1. Se déclenche toutes les heures (cron)
2. Récupère les nouveaux enregistrements de l'API A
3. Transforme les données au format de l'API B
4. Envoie les données à l'API B
5. Log les succès et échecs
```

**Use cases** :
- Synchronisation CRM
- Backup de données
- Agrégation multi-sources

### 3. Error Monitoring & Alerts

**Description** : Monitore les workflows et envoie des alertes en cas d'erreur

**Prompt de création** :
```
Crée un workflow n8n "Error Monitor" qui :
1. Se déclenche toutes les 5 minutes
2. Vérifie l'état de tous les workflows actifs
3. Détecte les exécutions échouées
4. Groupe les erreurs par type
5. Envoie un email récapitulatif si des erreurs sont trouvées
```

**Use cases** :
- Monitoring production
- Alerting équipe
- Rapports d'incidents

### 4. API Aggregator

**Description** : Agrège des données de plusieurs APIs

**Prompt de création** :
```
Crée un workflow n8n "API Aggregator" qui :
1. Reçoit une requête avec un ID utilisateur
2. Fait des appels parallèles à 3 APIs différentes
3. Attend que toutes les réponses arrivent
4. Merge les données dans un seul objet
5. Retourne le résultat combiné
```

**Use cases** :
- Dashboard unifié
- Enrichissement de données
- Backend for Frontend (BFF)

### 5. Content Moderation Pipeline

**Description** : Pipeline de modération de contenu automatisé

**Prompt de création** :
```
use context7 - Quelles sont les meilleures pratiques pour la modération de contenu avec AI en 2025 ?

Crée un workflow n8n "Content Moderation" qui :
1. Reçoit du contenu utilisateur (texte + images)
2. Analyse le texte avec un service de modération
3. Analyse les images avec un service de vision AI
4. Applique des règles de validation
5. Retourne : approved/rejected/review_needed
6. Si rejected, envoie une notification
```

**Use cases** :
- Modération forums
- Validation UGC (User Generated Content)
- Compliance automatisée

---

## Commandes Rapides

### Context7

```bash
# Documentation
use context7 - [votre question]

# Avec mention
@context7 [votre question]
```

### n8n MCP

```bash
# Liste
liste workflows
liste mes workflows n8n

# Créer
crée un workflow n8n [description]

# Modifier
modifie le workflow "[nom]" pour [action]

# Exécuter
exécute le workflow "[nom]" avec [données]

# Debug
debug le workflow "[nom]"
pourquoi le workflow "[nom]" a échoué ?

# Historique
montre les exécutions du workflow "[nom]"
```

---

## Tips & Astuces

### 1. Combiner les deux MCP

Toujours commencer par Context7 pour obtenir la documentation la plus récente, puis implémenter avec n8n MCP.

### 2. Itération rapide

```
Crée le workflow → Teste → Analyse les erreurs → Corrige → Reteste
```

Claude peut faire tout ce cycle automatiquement !

### 3. Documentation automatique

```
Documente le workflow "Mon Workflow" en expliquant :
- Ce qu'il fait
- Quand l'utiliser
- Les paramètres requis
- Les cas d'erreur possibles
```

### 4. Templates réutilisables

Créez des templates de workflows que vous pourrez réutiliser :

```
Sauvegarde le workflow "API Caller" comme template pour mes futurs workflows d'API
```

### 5. Monitoring continu

```
Crée un dashboard qui montre l'état de tous mes workflows actifs
```

---

## Exemples Avancés

### Workflow avec AI Integration

```
use context7 - Comment intégrer GPT-4 dans une API en 2025 ?

Crée un workflow n8n qui :
1. Reçoit une question via webhook
2. Appelle GPT-4 avec la question
3. Formate la réponse
4. Stocke dans un cache Redis
5. Retourne la réponse
```

### Data Pipeline Complexe

```
Crée un pipeline de données n8n qui :
1. Récupère des données de Salesforce (toutes les heures)
2. Nettoie et normalise les données
3. Enrichit avec des données de Clearbit
4. Calcule des scores (lead scoring)
5. Envoie vers Segment
6. Archive dans BigQuery
7. Envoie un rapport quotidien par email
```

### Multi-tenant Workflow

```
Crée un workflow n8n multi-tenant qui :
1. Identifie le tenant depuis le header
2. Charge la config spécifique au tenant
3. Applique les transformations tenant-specific
4. Route vers la bonne destination
5. Log séparément par tenant
```

---

## Ressources Supplémentaires

- [Documentation n8n](https://docs.n8n.io/)
- [n8n Community](https://community.n8n.io/)
- [Workflow Templates](https://n8n.io/workflows)
- [Context7 GitHub](https://github.com/upstash/context7)

---

## Feedback et Améliorations

Ce guide sera mis à jour au fur et à mesure de vos découvertes et use cases !

N'hésitez pas à documenter vos propres workflows et patterns ici.
