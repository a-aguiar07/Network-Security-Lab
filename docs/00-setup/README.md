# Fase 00: Laboratorio base

## Objetivo

Montar un entorno virtual aislado con una máquina atacante (Kali Linux) y un
objetivo vulnerable (Metasploitable 2), sobre el que se construirán las
siguientes fases del proyecto.

## Entorno

- Hipervisor: Oracle VirtualBox
- Red: NAT Network `SecLab`

---

## 1. Creación de la red NAT "SecLab"

**Objetivo:** crear una red virtual donde las VMs se comuniquen entre sí y
tengan salida a internet sin exponerse a la red local.

**Configuración:**

| Parámetro | Valor |
|-----------|-------|
| Nombre | SecLab |
| Prefijo IPv4 | 10.0.2.0/24 |
| DHCP | Habilitado |
| IPv6 | Deshabilitado |

![Configuración de la red SecLab](c:\Users\adria\OneDrive\Pictures\Screenshots\Network-Security-Lab\evidence00-setup01-natnetwork-seclab.png.png)

**Resultado:** red `SecLab` creada y lista para conectar las VMs.