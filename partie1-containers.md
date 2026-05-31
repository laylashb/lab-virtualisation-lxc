# Lab LXC/LXD — Administration avancée des containers Linux
*Container Alpine sur VM Debian*
 
**Layla SHABI** — 12/05/2026
 
---


# Partie 1 — Analyse d'un container Linux
 
Création d'un container Alpine via notre VM Debian.
 
```bash
lxc launch images:alpine/3.21 tp-alpine
```
 
Premiers tests du conteneur et comparaisons avec l'hôte : hostname, PID du shell courant, version du kernel.
 
<!-- CAPTURE : résultat des commandes hostname, echo $$, uname -r depuis l'hôte et depuis le container -->
 
> **Observation** : même version du kernel chez l'hôte et le container, mais hostname et PID du shell courant différents.
 
---

** Qu'est-ce qu'un container Linux ? **
 
Un conteneur Linux est un ensemble de processus isolés qui partagent le même noyau (kernel) que l'hôte. Il utilise des mécanismes comme les **namespaces** (pour l'isolation des processus, réseau, système de fichiers) et les **cgroups** (pour limiter les ressources). Contrairement à une VM, il ne simule pas un matériel complet : c'est une bulle légère qui permet d'exécuter des applications de manière isolée.
 
**Quelle est la différence entre un container et une machine virtuelle ?**
 
- **Conteneur** : partage le noyau de l'hôte, ne contient que les processus et bibliothèques nécessaires. Très léger, rapide à démarrer.

- **Machine virtuelle (VM)** : embarque son propre noyau et son propre système d'exploitation complet. Plus lourde, consomme plus de ressources, mais offre une isolation plus forte.

> En résumé : un conteneur est un service isolé sur le même kernel, une VM est une machine complète avec son propre kernel.
 
**Pourquoi le kernel est-il identique entre l'hôte et le container ?**

Parce qu'un conteneur ne virtualise pas le noyau : il dépend directement du kernel de l'hôte. Les conteneurs utilisent les fonctionnalités du noyau Linux (namespaces, cgroups, capabilities) pour créer l'isolation. Une VM, elle, embarque son propre kernel car elle simule un matériel complet.
 
**Quels avantages apporte le partage du kernel ?**
- **Rapidité** : démarrage quasi instantané
- **Légèreté** : pas besoin de charger un OS complet
- **Efficacité** : moins de consommation CPU/RAM/disque
- **Simplicité** : idéal pour déployer rapidement des services ou des environnements de test

---