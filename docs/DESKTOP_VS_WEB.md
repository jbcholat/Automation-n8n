# Différences entre Claude Code Desktop et Web

## Résumé des Tests

**Date** : 2025-11-06
**Résultat** : Confirmation d'un blocage proxy sur la version web

## Contexte

L'utilisateur a testé la connexion à n8n depuis **Claude Code dans Visual Studio Code (desktop)** et **tout fonctionne parfaitement**.

Lorsque je teste depuis **Claude Code Web (CloudDesk Hop)**, je reçois systématiquement une **erreur 403 Forbidden**.

## Analyse Technique

### Requête depuis Claude Code Web

```bash
> CONNECT n8n.srv972813.hstgr.cloud:443 HTTP/1.1
> Host: n8n.srv972813.hstgr.cloud:443
> Proxy-Authorization: Basic Y29udGFpbmVyX2NvbnRhaW5lcl8...
> User-Agent: curl/8.5.0
> Proxy-Connection: Keep-Alive

*  issuer: O=Anthropic; CN=sandbox-egress-production TLS Inspection CA

< HTTP/2 403
< content-type: text/plain
Access denied
```

### Observations Clés

1. **Proxy Anthropic** : Les requêtes passent par un proxy de sécurité Anthropic
   - Header : `Proxy-Authorization: Basic ...`
   - TLS Inspection : `sandbox-egress-production TLS Inspection CA`

2. **Erreur 403** : L'instance n8n (ou Hostinger) bloque les requêtes provenant du proxy Anthropic

3. **Raisons possibles du blocage** :
   - **Restriction IP** : L'API n8n peut être configurée pour accepter uniquement certaines IPs
   - **Blocage Proxy** : Hostinger peut bloquer les requêtes via proxy
   - **TLS Inspection** : La modification des certificats TLS peut déclencher des protections
   - **Cloudflare/WAF** : Un firewall applicatif peut bloquer les requêtes proxy

## Différences entre les Environnements

| Caractéristique | Claude Code Desktop (VS Code) | Claude Code Web (CloudDesk) |
|-----------------|--------------------------------|----------------------------|
| **Origine des requêtes** | Machine locale de l'utilisateur | Serveurs Anthropic (sandbox) |
| **Proxy** | ❌ Direct | ✅ Via proxy Anthropic |
| **TLS Inspection** | ❌ Non | ✅ Oui (sandbox-egress-production) |
| **IP source** | IP de l'utilisateur | IP du datacenter Anthropic |
| **Connexion n8n API** | ✅ **Fonctionne** | ❌ **Bloqué (403)** |

## Implications

### ✅ Ce qui fonctionne sur Desktop

- Connexion directe à l'API n8n
- Serveurs MCP (Context7 + n8n)
- Création et gestion de workflows
- Exécution de workflows
- Accès complet à l'API

### ❌ Ce qui ne fonctionne PAS sur Web

- Connexion à l'API n8n (erreur 403)
- Serveur MCP n8n (dépend de l'API)
- Toute opération nécessitant un appel direct à l'API

### ✅ Ce qui fonctionne sur Web

- Context7 MCP (ne dépend pas de n8n)
- Lecture/écriture de fichiers locaux
- Documentation et guides
- Tout ce qui ne nécessite pas d'appel externe à n8n

## Solutions et Recommandations

### Pour l'utilisateur (Desktop - VS Code)

**Statut** : ✅ Tout fonctionne
**Configuration** : Comme documenté dans `docs/MCP_SETUP.md`
**Action** : Aucune modification nécessaire

### Pour Claude Code Web

**Statut** : ⚠️ Limitation technique

**Options** :

1. **Whitelist IP Anthropic** (si possible sur Hostinger)
   - Identifier les IPs du sandbox Anthropic
   - Les ajouter à la whitelist de l'API n8n
   - Peut ne pas être pratique

2. **Proxy intermédiaire** (solution avancée)
   - Créer un proxy sur votre infrastructure
   - Le proxy fait le relai vers n8n
   - Claude Web → Votre proxy → n8n
   - Nécessite configuration serveur

3. **Webhooks n8n inverses** (workaround)
   - Au lieu que Claude appelle n8n
   - n8n expose des webhooks publics
   - Claude déclenche via webhooks (généralement autorisés)
   - Limitation : pas d'accès complet à l'API

4. **Utiliser uniquement Desktop** (recommandé)
   - Utiliser Claude Code dans VS Code pour les opérations n8n
   - Utiliser Claude Web pour tout le reste
   - Pas de configuration supplémentaire nécessaire

## Recommandation Finale

### Pour ce projet

**Utiliser Claude Code Desktop (VS Code)** pour :
- Gestion des workflows n8n
- Création/modification de workflows
- Exécution et monitoring
- Toute opération impliquant l'API n8n

**Utiliser Claude Code Web** pour :
- Consultation de documentation (Context7)
- Édition de fichiers du projet
- Questions générales
- Workflows qui ne nécessitent pas d'appel API direct

### Configuration Optimale

```
Claude Code Desktop (VS Code)
    ↓
    Serveurs MCP (Context7 + n8n)
    ↓
    API n8n Hostinger
    ✅ Accès complet
```

```
Claude Code Web
    ↓
    Proxy Anthropic
    ↓
    ❌ Bloqué par n8n/Hostinger (403)
```

## Impact sur le Projet

- ✅ **Documentation** : Complète et utilisable sur les deux plateformes
- ✅ **Configuration** : Fonctionne parfaitement sur Desktop
- ⚠️ **Version Web** : Limitation technique (pas un bug de configuration)
- ✅ **Workflow prévu** : L'utilisateur peut tout faire depuis VS Code

## Conclusion

Ce n'est **pas un problème de configuration** du projet. C'est une **limitation d'infrastructure** :

1. ✅ La configuration MCP est correcte
2. ✅ L'API n8n est bien activée
3. ✅ Les credentials sont valides
4. ⚠️ Les requêtes depuis le proxy Anthropic sont bloquées par Hostinger/n8n

**Le projet fonctionne parfaitement sur Claude Code Desktop** et c'est l'environnement recommandé pour ce use case.

---

## Historique

- **2025-11-06 11:40** : Tests confirmant le blocage proxy
- **2025-11-06 10:57** : Configuration initiale MCP
- **2025-11-06** : Utilisateur confirme que tout fonctionne sur Desktop

## Références

- [docs/MCP_SETUP.md](docs/MCP_SETUP.md) - Configuration complète
- [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) - Guide de dépannage
- [NEXT_STEPS.md](NEXT_STEPS.md) - Prochaines étapes
