# Lab LXC/LXD — Administration avancée des containers Linux
*Container Alpine sur VM Debian*
 
**Layla SHABI** — 12/05/2026
 
---

Dans ce lab, nous mettons en place une infrastructure de virtualisation sur une VM Debian 13. L'objectif est de comprendre et manipuler les containers Linux via LXC/LXD, depuis la création d'un container jusqu'à sa sécurisation, en passant par la gestion des ressources, du réseau et du stockage.
LXC/LXD est une technologie de containerisation système. Contrairement à Docker qui isole des applications, LXC isole des environnements complets — chaque container a son propre réseau, ses propres processus, son propre hostname, tout en partageant le kernel de l'hôte Debian.

---

## Environnement

- Hôte : VM Debian 13 sous VirtualBox
- Technologie : LXC/LXD
- Image utilisée : Alpine Linux 3.21

---

## Contenu

| Fichier | Thème |
|---|---|
| [partie1-containers.md](partie1-containers.md) | Création d'un container Alpine, comparaison hôte/container |
| [partie2-namespaces.md](partie2-namespaces.md) | Isolation par namespaces Linux (PID, NET, MNT, UTS...) |
| [partie3-reseaux-lxd.md](partie3-reseaux-lxd.md) | Bridge lxdbr0, interfaces veth, NAT |
| [partie4-cgroups.md](partie4-cgroups.md) | Limitation des ressources CPU et mémoire |
| [partie5-profils.md](partie5-profils.md) | Profils LXD réutilisables |
| [partie6-stockage-snapshots.md](partie6-stockage-snapshots.md) | Backend BTRFS, snapshots et restauration |
| [partie7-securite.md](partie7-securite.md) | Containers non privilégiés, UID mapping, AppArmor |
| [partie8-container-vs-vm.md](partie8-container-vs-vm.md) | Comparaison container / machine virtuelle |
| [partie9-publication-image.md](partie9-publication-image.md) | Créer et publier une image LXD personnalisée |
| [partie10-supervision.md](partie10-supervision.md) | Monitoring et diagnostic avec lxc monitor et journalctl |
| [partie11-challenge-final.md](partie11-challenge-final.md) | Déploiement complet : web, ressources, snapshot, image |

---

## Ce que ce lab démontre

**Isolation** : un container partage le kernel de l'hôte mais vit dans sa propre bulle — son propre réseau, ses propres processus, son propre hostname. C'est les namespaces qui rendent ça possible.

**Contrôle des ressources** : sans limitation, un container peut consommer toutes les ressources de l'hôte et bloquer les autres. Les cgroups permettent de définir des plafonds CPU et mémoire par container.

**Sécurité** : un container non privilégié mappe son root (UID 0) vers un UID élevé sur l'hôte (1000000). Concrètement, même si quelqu'un s'échappe du container, il n'est pas root sur la machine hôte.

**Reproductibilité** : via les profils LXD et la publication d'images personnalisées, on peut déployer plusieurs containers identiques en quelques secondes — c'est la base du principe CI/CD.

---

## Lancer un container de test

```bash
# Installer LXD
sudo snap install lxd
lxd init

# Lancer un container Alpine
lxc launch images:alpine/3.21 mon-container

# Entrer dedans
lxc exec mon-container -- sh

# Lister les containers
lxc list
```

---

lab-virtualisation-lxc/
├── README.md

├── partie1-containers.md

├── partie2-namespaces.md

├── partie3-reseaux-lxd.md

├── partie4-cgroups.md

├── partie5-profils.md

├── partie6-stockage-snapshots.md

├── partie7-securite.md

├── partie8-container-vs-vm.md

├── partie9-publication-image.md

├── partie10-supervision.md

└── partie11-challenge-final.md

---

*Projet réalisé dans le cadre de la prépa Mastère Cybersécurité — IPSSI 2026*
