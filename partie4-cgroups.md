# Partie 4 — cgroups et limitation de ressources

**Questions/Réponses**

- **Qu'est-ce qu'un cgroup ?** Collection de processus organisée hiérarchiquement afin de lancer des processus de manière automatisée et de limiter leurs ressources.
- **Différence namespaces vs cgroups** : namespaces = isolation des processus/réseau/fichiers ; cgroups = limitation des ressources.
- **Pourquoi limiter les ressources ?** Pour éviter qu'un seul conteneur consomme toutes les ressources (CPU, RAM, disque, réseau) de l'hôte, assurer une répartition équitable et prévenir les dénis de service internes.
- **Que se passe-t-il quand une limite mémoire est atteinte ?** Les processus qui utilisent trop de RAM peuvent s'arrêter ou planter.
- **Risques sans limitation** : déni de service, saturation disque/réseau/CPU, lenteur générale.

**Commandes appliquées**

```bash
lxc config set tp-alpine limits.cpu 1
lxc config set tp-alpine limits.memory 512MiB
lxc config show tp-alpine
```

<!-- CAPTURE : résultat de lxc config show tp-alpine avec les limites appliquées -->

Pour générer de la charge dans le conteneur, on télécharge un outil de test, puis on lance une charge CPU et mémoire.

<!-- CAPTURE : htop depuis le container montrant la limite à 512M -->

---