# Partie 5 — Profils LXD

```bash
lxc profile create web-profile
lxc profile edit web-profile
lxc launch images:alpine/3.19 web-container -p default -p web-profile
lxc config show web-container
sudo apt install stress-ng
lxc exec alpine323-container -- sh
lxc config show alpine323-container
```

<!-- CAPTURE : lxc exec alpine323-container -- sh, ls /mnt/webdata et cat test.txt -->

<!-- CAPTURE : lxc config show alpine323-container avec le profil web-profile appliqué -->

**Profil LXD** : modèle de configuration réutilisable qui définit des paramètres (limites de ressources, périphériques, réseau, volumes) pour les conteneurs.

Intérêts :
- Standardiser les configurations
- Gagner du temps lors de la création de conteneurs
- Éviter les erreurs de configuration répétitives

Avantages en production :
- **Homogénéité** : tous les conteneurs d'un même rôle ont la même config
- **Scalabilité** : déploiement rapide de plusieurs conteneurs identiques
- **Maintenance simplifiée** : modification du profil appliquée aux nouveaux conteneurs
- **Sécurité et performance** : limites de ressources et règles réseau cohérentes

---