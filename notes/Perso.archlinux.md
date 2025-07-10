---
id: Perso.archlinux
title: Perso.archlinux
desc: install ArchLinux & Omarchy
updated: 1752134368756
created: 0
---
# configuration du réseau

en utilisant `wpa_supplicant`.

wpa.conf : 
```
network={
   ssid="Livebox-XXXXX"
   psk="<PASSWORD>"
}
```

puis activation du réseau

```bash
wpa_supplicant -i <NIC_ID> -c wpa.conf
```

le `<NIC_ID>` peut être découvert avec `ip link`


# activation IPv4 après install

Si l'install omarchy échoue sur le clone github c'est que IPv4 n'est pasconfiguré. la solution est de forcer la re-configuration DHCP.

```bash
sudo pacman -S dhcpcd` # si pas déjà installé
sudo dhcpcd
```

# wpa_supplicant at boot 

[wpa_supplicant at boot with dhcpcd hook](https://wiki.archlinux.org/title/Dhcpcd#10-wpa_supplicant)
