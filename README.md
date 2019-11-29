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

### Gestion du temps

* Determiné la différence entre Local Time, Universal Time et , RTC Time

  * Le *Local Time* est l'heure actuel du systeme

  * L'universal Time est un standard de temps basé sur les rotation de la Terre

  * le RTC Time est un module de la carte mère auto-alimenter permettant de conserver l'heure.

* Timezones

  * Modifications du fuseaux horaires

```
[user@localhost ~]$ sudo timedatectl set-timezone Europe/Berlin
[user@localhost ~]$ sudo timedatectl
Local time: Fri 2019-11-29 16:19:15 CET
Universal time: Fri 2019-11-29 15:19:15 UTC
RTC time: Fri 2019-11-29 15:19:14
    Time zone: Europe/Berlin (CET, +0100)
    System clock synchronized: yes
    NTP service: active
    RTC in local TZ: no					   
```

* Utilisation NTP et désactivation du service ntp

```
[user@localhost ~]$ sudo timedatectl
Local time: Fri 2019-11-29 16:19:15 CET
Universal time: Fri 2019-11-29 15:19:15 UTC
RTC time: Fri 2019-11-29 15:19:14
    Time zone: Europe/Berlin (CET, +0100)
     System clock synchronized: yes
     NTP service: active
     RTC in local TZ: no
[user@localhost ~]$ sudo timedatectl set-ntp FALSE
[user@localhost ~]$ sudo timedatectl
Local time: Fri 2019-11-29 16:27:33 CET
Universal time: Fri 2019-11-29 15:27:33 UTC
RTC time: Fri 2019-11-29 15:27:32
    Time zone: Europe/Berlin (CET, +0100)
    System clock synchronized: yes
    NTP service: inactive
    RTC in local TZ: no	      
```

### Gestion des noms

* Expliquer la différence entre --static --transient --pretty
  * --transient set un transient name qui est le nom de l'hote maintenus par le kernel
  * --static set le nom de l'hote basique qu'habituellemnt l'utilisateur peux choisir
  * --pretty est une option qui combiner à une des deux précédentes permet de nettoyer le hostname en enlevant les caractère non UTF-8 et remplaçant les espaces par des "-"

Pour des machines de prod, on peut utiliser le "--pretty" et le "--static". Cela permet d'avoir un hostname propre avec que des caractères lisible.

### Gestion du réseaux

