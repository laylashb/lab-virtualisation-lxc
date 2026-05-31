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

<img width="945" height="342" alt="image" src="https://github.com/user-attachments/assets/bd8cd697-295a-46f3-926e-c52268f3d503" />

Pour générer de la charge dans le conteneur, on télécharge un outil de test, puis on lance une charge CPU et mémoire.

<img width="945" height="318" alt="image" src="https://github.com/user-attachments/assets/faa63834-99fe-40e1-851c-c924f2f40de3" />

---
