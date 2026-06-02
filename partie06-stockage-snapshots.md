# Partie 6 — Stockage et Snapshot

> **Problème rencontré** : je n'avais pas les droits pour modifier le contenu du container. Solution : `sudo chown 100000:100000 /srv/webdata` depuis l'hôte.

Backend de stockage utilisé — contenu dans `/srv/webdata`

**Étapes réalisées**

```bash
# 1. Identifier le backend de stockage
lxc storage list

# 2. Créer un snapshot
lxc snapshot alpine323-container snap1

# 3. Modifier le contenu
echo "contenu modifié" | sudo tee /srv/webdata/test.txt

# 4. Vérifier dans le conteneur
lxc exec alpine323-container -- cat /mnt/webdata/test.txt

# 5. Restaurer le snapshot
lxc restore alpine323-container snap1

# 6. Vérifier après restauration
lxc exec alpine323-container -- cat /mnt/webdata/test.txt
```

<!-- CAPTURE : lxc snapshot puis lxc restore avec vérification avant/après -->

**Questions/Réponses**

- **Qu'est-ce qu'un snapshot ?** Capture instantanée de l'état d'un système à un moment précis. Permet de revenir à cet état initial en cas de problème.

- **Différence sauvegarde vs snapshot** :
  - Sauvegarde : analyse les changements et n'enregistre que les nouvelles modifications
  - Snapshot : capture totale d'un état, figé, conservé dans un fichier — retour en arrière immédiat

- **Avantages des backends de stockage** :
  - **BTRFS** : tolérance aux pannes, performance, flexibilité
  - **ZFS** : haute capacité, protection contre la corruption (checksums), snapshots rapides, compression intégrée
  - **LVM** : volumes logiques dynamiques, redimensionnement facile, souvent sans interruption de service

---
