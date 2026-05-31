# Partie 3 — Réseaux LXD

Dans cette partie on a exploré comment LXD gère le réseau de ses containers.Quand LXD crée un container, il crée automatiquement un bridge virtuel appelé lxdbr0 qui agit comme un switch logiciel — tous les containers s'y connectent. 

Chaque container reçoit une interface eth0 qui est en réalité une paire d'interfaces virtuelles appelée veth : une extrémité dans le container, l'autre attachée au bridge sur l'hôte.

```bash
lxc network show lxdbr0
ip route
brctl show
sudo apt install bridge-utils
ip a
ip addr show lxdbr0
```

<img width="945" height="1087" alt="image" src="https://github.com/user-attachments/assets/02f7b9c4-4dcf-4073-bd23-04e100ccbe6a" />


## Côté containeur :

<img width="945" height="603" alt="image" src="https://github.com/user-attachments/assets/3fcb91ea-fd28-46a6-bf9e-35b2f435e06c" />

**Concepts clés**

- **Rôle de lxdbr0** : bridge réseau créé par LXD, qui relie les conteneurs à l'hôte et leur fournit une connectivité.
- **Bridge réseau** : pont logiciel qui fonctionne comme un switch virtuel, reliant plusieurs interfaces réseau.
- **Interface veth** : paire d'interfaces virtuelles créées par le kernel — une extrémité est dans le conteneur, l'autre est attachée au bridge sur l'hôte.
- **Rôle du NAT dans LXD** : traduit les adresses privées des conteneurs en adresse publique de l'hôte, permettant aux conteneurs d'accéder à Internet.

**Questions/Réponses**

- **Quel est le rôle de lxdbr0 ?** Bridge réseau virtuel créé automatiquement par LXD, agit comme un switch logiciel interne, connecte les interfaces virtuelles des conteneurs (veth) à l'hôte, attribue des adresses IP privées aux conteneurs (souvent en 10.x.x.x).
- **Qu'est-ce qu'un bridge réseau ?** Pont logiciel qui relie plusieurs interfaces réseau comme le ferait un switch physique, permet aux conteneurs de communiquer entre eux.
- **Qu'est-ce qu'une interface veth ?** Paire d'interfaces virtuelles créées par le noyau Linux — ses 2 extrémités relient le conteneur au bridge.
- **Quel est le rôle du NAT dans LXD ?** Permet aux conteneurs d'utiliser une IP privée tout en accédant à Internet via l'IP publique de l'hôte. LXD configure automatiquement des règles de masquerading.
- **Pourquoi un container peut-il accéder à Internet ?** Car relié au bridge via interface veth, et le bridge est connecté à l'hôte. Le NAT traduit son IP privée en IP publique de l'hôte.

---
