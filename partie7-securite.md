# Partie 7 — Sécurité des containers

```bash
lxc exec alpine323-container -- id
lxc exec alpine323-container -- touch /mnt/webdata/test-root.txt
ls -l /srv/webdata
```

<!-- CAPTURE : ls -l /srv/webdata montrant l'UID 1000000 pour les fichiers créés dans le container -->

**Isolation utilisateur** : les fichiers créés par root dans le conteneur apparaissent avec un UID/GID mappé (1000000) sur l'hôte.

La sécurité des conteneurs LXC repose principalement sur :
- L'utilisation de conteneurs **non privilégiés**
- Le durcissement du noyau hôte
- L'isolation par namespaces

**Analyse du mapping** : `idmap` → le conteneur est non privilégié.

```bash
id
# uid=1000(layla) gid=1000(layla) groupes=1000(layla),...,986(lxd)
```

**Questions/Réponses**

- **Container non privilégié** : conteneur où root à l'intérieur est mappé vers un UID non root sur l'hôte, empêchant l'accès aux privilèges système.

- **Pourquoi root dans le container n'est-il pas réellement root ?** LXD remappe l'UID 0 du conteneur vers un UID élevé sur l'hôte, bloquant les privilèges réels.

- **Risques avec les containers privilégiés** : root dans le conteneur = root sur l'hôte → risque d'évasion, accès direct aux fichiers et périphériques, compromission du système.

- **Rôle d'AppArmor** : limiter les accès aux fichiers et ressources via des profils de sécurité.

- **Rôle de Seccomp** : filtrer les appels système pour empêcher l'exécution de fonctions dangereuses.

- **Rôle des Linux capabilities** : découper les privilèges root en capacités spécifiques et n'accorder que celles nécessaires.

---