# Partie 11 — Challenge Final

Créer un conteneur dédié au service web et limiter ses ressources.

```bash
# Créer le container et limiter ses ressources
lxc launch images:alpine/3.21 web-final
lxc config set web-final limits.cpu 1
lxc config set web-final limits.memory 512MB

# Installer et configurer Apache
lxc exec web-final -- apk update
lxc exec web-final -- apk add apache2 curl
lxc exec web-final -- rc-update add apache2 default
lxc exec web-final -- service apache2 start
```

> ⚠️ **Problème port 8080 déjà occupé** : `sudo ss -tulnp | grep 8080` → c'est LXD lui-même qui utilisait ce port. Solution : utiliser le port **8081**.

```bash
# Corriger le port d'écoute
lxc config device add web-final myport80 proxy listen=tcp:0.0.0.0:8081 connect=tcp:127.0.0.1:80
```

<!-- CAPTURE : curl http://192.168.x.x:8081 retournant la page web -->

```bash
# Snapshot et restauration
lxc snapshot web-final snap1
lxc restore web-final snap1

# Publier en image
lxc stop web-final
lxc publish web-final --alias web-image-final
lxc launch web-image-final web-copy
```

<!-- CAPTURE : lxc list montrant web-final, web-copy en RUNNING -->

---

*Fin du TP LXC/LXD — Layla SHABI*