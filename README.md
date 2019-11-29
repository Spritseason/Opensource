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

* Pour affiché les infos DHCP récupéré par le NetworkManager sur une interface disposant d'un DHCP est la suivante :

```
[user@localhost ~]$ sudo nmcli -f DHCP4 con show enp0s8
DHCP4.OPTION[1]:                        dhcp_lease_time = 1200
DHCP4.OPTION[2]:                        dhcp_rebinding_time = 1050
DHCP4.OPTION[3]:                        dhcp_renewal_time = 600
DHCP4.OPTION[4]:                        dhcp_server_identifier = 10.0.0.100
DHCP4.OPTION[5]:                        expiry = 1575044792
DHCP4.OPTION[6]:                        ip_address = 10.0.0.104
DHCP4.OPTION[7]:                        requested_broadcast_address = 1
DHCP4.OPTION[8]:                        requested_dhcp_server_identifier = 1
DHCP4.OPTION[9]:                        requested_domain_name = 1
DHCP4.OPTION[10]:                       requested_domain_name_servers = 1
DHCP4.OPTION[11]:                       requested_domain_search = 1
DHCP4.OPTION[12]:                       requested_host_name = 1
DHCP4.OPTION[13]:                       requested_interface_mtu = 1
DHCP4.OPTION[14]:                       requested_ms_classless_static_routes = 1
DHCP4.OPTION[15]:                       requested_nis_domain = 1
DHCP4.OPTION[16]:                       requested_nis_servers = 1
DHCP4.OPTION[17]:                       requested_ntp_servers = 1
DHCP4.OPTION[18]:                       requested_rfc3442_classless_static_routes = 1
DHCP4.OPTION[19]:                       requested_root_path = 1
DHCP4.OPTION[20]:                       requested_routers = 1
DHCP4.OPTION[21]:                       requested_static_routes = 1
DHCP4.OPTION[22]:                       requested_subnet_mask = 1
DHCP4.OPTION[23]:                       requested_time_offset = 1
DHCP4.OPTION[24]:                       requested_wpad = 1
DHCP4.OPTION[25]:                       subnet_mask = 255.255.255.0
```