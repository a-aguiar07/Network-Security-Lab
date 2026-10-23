# Fase 02: Segmentación con pfSense

## Objetivo

Implementar en VirtualBox la misma segmentación diseñada en la Fase 01
(Packet Tracer), usando pfSense como firewall con estado y las máquinas reales
del laboratorio (Kali y Metasploitable 2).

## Diseño

| Zona | Red interna (VirtualBox) | Subred | Equipo |
|------|--------------------------|--------|--------|
| WAN | NAT | DHCP de VirtualBox | pfSense |
| LAN | `intnet-lan` | 192.168.10.0/24 | Cliente (pendiente) |
| DMZ | `intnet-dmz` | 192.168.20.0/24 | Metasploitable 2 |
| ATACANTE | `intnet-atacante` | 192.168.30.0/24 | Kali |

---

## 1. VM de pfSense

**Recursos:** 1 GB de RAM, 1 CPU, disco de 10 GB, FreeBSD (64-bit).

**Adaptadores de red:**

| Adaptador | Tipo | Nombre | Función |
|-----------|------|--------|---------|
| 1 | NAT | n/a | WAN |
| 2 | Red interna | `intnet-lan` | LAN |
| 3 | Red interna | `intnet-dmz` | DMZ |
| 4 | Red interna | `intnet-atacante` | ATACANTE |

![Adaptadores de red de pfSense](../../evidence/02-pfsense/01-pfsense-vm-network.png)

> Se usó el instalador de Netgate (`netgate-installer`), que descarga pfSense CE
> durante la instalación, por lo que la VM necesita salida a internet por el
> adaptador NAT.

---

## Problemas y soluciones

| Problema | Causa | Solución |
|----------|-------|----------|
| VirtualBox marcaba error en la ruta del ISO | 7-Zip creó una carpeta con el nombre `.iso` y el archivo real estaba dentro, así que la ruta salía duplicada | Mover el `.iso` un nivel arriba y seleccionarlo desde el explorador |