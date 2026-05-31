# Partie 9 — Publication d'une image personnalisée

## 1. Créer une image réutilisable

```bash
# Créer et préparer le container template
lxc launch images:alpine/3.21 alpine-template

# Installer un serveur web et outils système
lxc exec alpine-template -- apk update
lxc exec alpine-template -- apk add apache2 curl vim

# Lancer Apache
lxc exec alpine-template -- rc-update add apache2 default
lxc exec alpine-template -- service apache2 start

# Personnaliser la page web
echo "<h1>Bienvenue sur mon TP LXD avec Alpine</h1>" > /var/www/localhost/htdocs/index.html

# Vérifier
curl http://localhost
```

> ⚠️ **Erreur rencontrée** : j'ai essayé de modifier `/var/www/localhost/htdocs/index.html` avant d'installer Apache → le dossier n'existait pas encore. Toujours installer avant de configurer.

## 2. Publier le container comme image LXD

```bash
lxc stop alpine-template
lxc publish alpine-template --alias my-alpine-web
```

## 3. Créer un nouveau container à partir de l'image

```bash
lxc launch my-alpine-web alpine-from-image
lxc exec alpine-from-image -- curl http://localhost
```

<!-- CAPTURE : curl http://localhost retournant la page personnalisée -->

**Questions**

- **Qu'est-ce qu'une image LXD ?** Modèle de système de fichiers et de configuration servant de base pour créer des conteneurs. Contient un OS minimal enrichi de paquets et configurations. Équivalent d'un template prêt à être cloné.
- **Pourquoi créer ses propres images ?** Standardisation, gain de temps, automatisation, reproductibilité.
- **CI/CD** : déployer rapidement des environnements identiques en production et pour les tests.
- **Scalabilité** : lancer plusieurs conteneurs identiques en quelques secondes.
- **Portabilité** : partager l'image avec son équipe ou la publier dans un registre.
- **Sécurité** : contrôler exactement ce qui est installé, réduire les dépendances inutiles.

---