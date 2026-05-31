# Partie 8 — Container vs Machine Virtuelle

| Critère | Container | VM |
|---|---|---|
| Kernel | Partagé avec l'hôte | Propre kernel |
| Démarrage | Quasi instantané | Plusieurs secondes/minutes |
| Poids | Léger (MB) | Lourd (GB) |
| Isolation | Partielle (namespaces) | Complète |
| Usage | Services, microservices | Applications lourdes, OS complet |

- **Pourquoi une VM est-elle plus lourde ?** Car elle embarque son propre OS complet avec émulation des périphériques matériels.
- **Avantages des containers** : légèreté, rapidité, isolation par service, plusieurs équipes peuvent travailler en parallèle sans impacter les autres conteneurs.
- **Quand utiliser une VM ?** Pour des applications lourdes, une isolation complète, du développement interactif avec installation manuelle de logiciels.
- **Quand utiliser un container ?** Pour des services légers, une itération rapide, un faible coût de stockage.
- **Pourquoi les containers démarrent-ils plus vite ?** Parce qu'ils partagent le kernel de l'hôte et ne chargent pas un OS complet.

---
