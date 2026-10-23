# Fase 02: Segmentación con pfSense

## Objetivo

Implementar en VirtualBox la misma segmentación diseñada en la Fase 01
(Packet Tracer), usando pfSense como firewall con estado y las máquinas reales
del laboratorio (Kali y Metasploitable 2).

## Diseño

| Zona | Red interna (VirtualBox) | Subred | Equipo |
|------|--------------------------|--------|--------|
| WAN | NAT | DHCP de VirtualBox | pfSense |
| LAN | `intnet-lan` | 192.168.10.0/24 | Cliente |
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

## 2. Instalación de pfSense CE

Se utilizó el instalador de Netgate (`netgate-installer v1.2`), que descarga
pfSense durante el proceso.

1. Asignación de interfaces: `em0` como WAN (adaptador NAT) y `em1` como LAN
   (`intnet-lan`).
2. El instalador ofreció pfSense Plus (de pago); se eligió **Install CE**, la
   edición gratuita.
3. Instalación en el disco virtual `ada0` (10 GB).

Las interfaces DMZ (`em2`) y ATACANTE (`em3`) se asignan tras la instalación,
desde la consola de pfSense.

---

## Problemas y soluciones

| Problema | Causa | Solución |
|----------|-------|----------|
| VirtualBox marcaba error en la ruta del ISO | 7-Zip creó una carpeta con el nombre `.iso` y el archivo real estaba dentro, así que la ruta salía duplicada | Mover el `.iso` un nivel arriba y seleccionarlo desde el explorador |
| Tras instalar, la consola solo mostraba la WAN | El instalador solo configura WAN y LAN | Asignar todas las interfaces con la opción 1 de la consola |

---

## 3. Asignación de interfaces y direccionamiento

Desde la consola de pfSense se asignaron las interfaces y las IPs, usando las
mismas subredes del diseño en Packet Tracer:

| Interfaz | NIC | Red interna | IP | DHCP |
|----------|-----|-------------|----|------|
| WAN | em0 | NAT | 10.0.2.15/24 (DHCP) | n/a |
| LAN | em1 | `intnet-lan` | 192.168.10.1/24 | .100 a .200 |
| OPT1 (DMZ) | em2 | `intnet-dmz` | 192.168.20.1/24 | No |
| OPT2 (ATACANTE) | em3 | `intnet-atacante` | 192.168.30.1/24 | .100 a .200 |

![Consola de pfSense](../../evidence/02-pfsense/03-pfsense-interfaces.png)