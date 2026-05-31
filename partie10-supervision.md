# Partie 10 — Supervision et diagnostic

```bash
# 1. Observer les événements LXD en temps réel
lxc monitor

# 2. Consulter les logs
lxc info alpine-template --show-log
journalctl -u lxd -f

# 3. Consulter les infos système
lxc info alpine-template
```

<!-- CAPTURE : lxc monitor en cours d'exécution -->

<!-- CAPTURE : journalctl -u lxd -f -->

> `journalctl -u lxd -f` est intéressante pour suivre les logs du service LXD en temps réel. L'avertissement affiché signifie que l'utilisateur n'a pas les droits pour voir tous les messages système.

> ⚠️ **Erreur rencontrée** : j'ai voulu faire `lxc restart alpine-template` alors que le container était stoppé. **Solution** : après un `stop`, toujours faire un `start` et pas un `restart`.

**Questions**

- **Pourquoi la supervision est-elle importante ?** Pour détecter les anomalies, comprendre l'état des conteneurs et garantir la disponibilité des services.
- **Outils de diagnostic LXD** : `lxc monitor`, `lxc info --show-log`, `journalctl -u lxd`, `top`, `dmesg`, `service`.
- **Comment identifier un container défaillant ?** En observant son état (STOPPED, ERROR), ses logs, ou en constatant qu'un service ne répond pas.
- **Informations importantes lors d'un diagnostic** :
  - État du conteneur (running, stopped, error)
  - Logs (messages système, erreurs)
  - Ressources consommées (CPU, RAM, disque, réseau)
  - Événements récents (arrêt, redémarrage, crash)

---
