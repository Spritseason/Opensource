# TP Systemd Léo

## Systemd-basics

### First steps

* Pour s'assurer que systemd est PID 1, on peut utiliser htop et faire une recherche par PID.
On remarque alors la ligne de PID 1 avec comme utilisateur "root" et processus "/usr/lib/systemd/systemd".

* Afin de vérifier tous les autres processus système, il est possible d'utiliser la commande ```ps -eo pid,user,cmd | grep -v -E '\[.+\]'``` afin d'obtenir tous les processus suivant :

| Processus     |  Description   |
|:------------|:-------------|
| /usr/lib/systemd/systemd-journald | Récolte et stock les logs de connexions |
| /usr/lib/systemd/systemd-udevd  | Démon chargé de géré les événement liées aux périphériques  |
| /usr/bin/dockerd | Démon docker |
| /usr/lib/systemd/systemd-logind | Manager de connexion des utilisateurs |
| /usr/sbin/chronyd | Démon chargé de la synchronisation de l'horloge du système |


