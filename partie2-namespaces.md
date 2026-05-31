# Partie 2 — Namespaces Linux

Commandes utilisées :

```bash
lsns                        # afficher les namespaces présents sur l'hôte
ps -ef | grep tp-alpine     # identifier le PID du container
apk add iproute2            # installer les outils réseau dans le container
```

---

## 1. Namespaces présents sur l'hôte

```bash
sudo lsns
```

Résultat : 8 types de namespaces actifs, tous rattachés au PID 1 de l'hôte.

En filtrant sur le PID du container (14931) :

```bash
sudo lsns | grep 14931
```

```
4026532388 user   5 14931 1000000 /sbin/init
4026532389 mnt    5 14931 1000000 /sbin/init
4026532390 uts    5 14931 1000000 /sbin/init
4026532391 ipc    5 14931 1000000 /sbin/init
4026532392 pid    5 14931 1000000 /sbin/init
4026532393 cgroup 5 14931 1000000 /sbin/init
4026532395 net    5 14931 1000000 /sbin/init
```

Le container `tp-alpine` possède **7 namespaces isolés**, tous créés par son processus init (PID 14931 côté hôte).

<!-- CAPTURE : résultat de sudo lsns | grep 14931 -->

---

### 2. Namespaces associés au container

```bash
sudo ls -la /proc/14931/ns/
```

- `cgroup` → cgroup:[4026532393]
- `ipc` → ipc:[4026532391]
- `mnt` → mnt:[4026532389]
- `net` → net:[4026532395]
- `pid` → pid:[4026532392]
- `user` → user:[4026532388]
- `uts` → uts:[4026532390]

Chaque fichier est un lien symbolique vers l'identifiant unique du namespace. Ces IDs sont différents de ceux de l'hôte, ce qui confirme l'isolation.

<!-- CAPTURE : résultat de sudo ls -la /proc/14931/ns/ -->

---

### 3. Observation depuis l'hôte et depuis le container

**Processus**

Depuis l'hôte :
```bash
ps aux | grep 14931
```
Le container apparaît comme un simple processus (PID 14931, uid=1000000) parmi les 56+ processus du système.

Depuis le container :
```bash
ps aux
```
```
PID  USER  COMMAND
1    root  /sbin/init
282  root  /sbin/syslogd
310  root  /usr/sbin/crond
403  root  /sbin/udhcpc
421  root  /sbin/getty
441  root  sh
```

Le container ne voit que 6 processus. Son init est PID 1. C'est le **PID namespace** qui renumérote les processus : le PID 14931 de l'hôte devient le PID 1 à l'intérieur.

<!-- CAPTURE : ps aux depuis le container -->

**Interfaces réseau**

Depuis l'hôte : `ip a` → interfaces présentes : `lo`, `enp0s3` (192.168.11.177), `lxdbr0` (10.210.126.1), `vethf2a91323@if13`

Depuis le container : `ip a` → interfaces présentes : `lo`, `eth0@if14` (10.210.126.104)

Le container ne voit pas les interfaces de l'hôte. Son `eth0` est en réalité une paire veth : `vethf2a91323` côté hôte et `eth0` côté container sont les deux bouts d'un câble virtuel relié au bridge `lxdbr0`. C'est le **NET namespace** qui assure cette isolation.

**Hostname**

```bash
# Depuis l'hôte
hostname    # → debian13-server

# Depuis le container
hostname    # → tp-alpine
```

Les deux machines ont un hostname différent grâce au **UTS namespace** (ID 4026532390).

**Points de montage**

```bash
# Depuis l'hôte
cat /proc/14931/mounts | head -5
# → /dev/loop4 sur / de type btrfs, subvol=/containers/tp-alpine

# Depuis le container
mount
# → /dev/loop4 on / type btrfs (rw, subvol=/containers/tp-alpine)
```

Le container voit son propre arbre de montage. Son répertoire racine `/` correspond en réalité à un sous-volume btrfs sur l'hôte (`/containers/tp-alpine`). Le **MNT namespace** l'empêche de voir le reste du système de fichiers de l'hôte.

---

### 4. Tableau récapitulatif

| Élément | Hôte | Container |
|---|---|---|
| Namespaces | Namespaces globaux du noyau | 7 namespaces propres (user/mnt/uts/ipc/pid/cgroup/net) |
| Processus | 56+ processus, PID 14931 | 6 processus, PID 1 = init |
| Réseau | enp0s3, lxdbr0, veth | eth0 seul (10.210.126.104) |
| Hostname | debian13-server | tp-alpine |
| Montages | Système complet | Sous-volume btrfs /containers/tp-alpine |

---

### 5. Namespaces utilisés par LXC

| Namespace | Rôle |
|---|---|
| `user` | Isole les UIDs/GIDs (uid 0 dans le container = 1000000 sur l'hôte) |
| `mnt` | Isole les points de montage |
| `uts` | Isole le hostname |
| `ipc` | Isole la communication inter-processus |
| `pid` | Isole la numérotation des processus |
| `cgroup` | Isole la vue des cgroups |
| `net` | Isole les interfaces réseau |


**Questions/Réponses**

- **Qu'est-ce qu'un namespace Linux ?** Outil d'isolation puissant permettant de cloisonner des ressources spécifiques du système pour chaque application ou processus.

- **Rôle du PID namespace** : isoler les processus — chaque PID namespace possède son propre ensemble de processus avec des IDs uniques, évitant les collisions.

- **Rôle du NET namespace** : isolation réseau — crée des interfaces réseau, adresses IP et configurations virtuelles indépendantes.

- **Rôle du MNT namespace** : isolation du système de fichiers — donne à chaque environnement son propre espace de montage, permet de monter/démonter des volumes sans impacter le reste du système.

- **Rôle du UTS namespace** : isolation du nom d'hôte — permet de définir un hostname par environnement, donnant une identité à chaque namespace.

- **Pourquoi les PID sont-ils différents ?** LXC utilise un PID namespace. Le noyau maintient deux tables de correspondance : une pour l'hôte, une pour chaque container. Le PID 14931 de l'hôte devient le PID 1 à l'intérieur du container.

- **Un container possède-t-il son propre kernel ?** Non, il partage le kernel de son hôte Debian. Le container tp-alpine utilise ce même noyau pour tous ses appels système.

---
