---
id: Perso.archlinux
title: Perso.archlinux
desc: install ArchLinux & Omarchy
updated: 0
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

```bash
sudo pacman -S dhcpcd`
sudo dhcpcd
```
