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

<img width="945" height="571" alt="image" src="https://github.com/user-attachments/assets/d5a61717-5709-43a4-b4cc-0867e89a72ff" />

<img width="945" height="197" alt="image" src="https://github.com/user-attachments/assets/d9b471ab-8a5b-46fb-b790-224818296cea" />

<img width="945" height="525" alt="image" src="https://github.com/user-attachments/assets/ccefee11-6c44-4e05-bee1-1665115b7779" />


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
