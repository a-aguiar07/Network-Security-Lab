# Fase 01: Diseño de red segmentada en Cisco Packet Tracer

## Objetivo

Diseñar y validar la segmentación de la red con VLANs, routing entre VLANs y
ACLs antes de implementarla con pfSense en la siguiente fase.

## Diseño

| Zona | VLAN | Subred | Equipo |
|------|------|--------|--------|
| LAN (usuarios) | 10 | 192.168.10.0/24 | PC-LAN |
| DMZ (servidores) | 20 | 192.168.20.0/24 | SRV-DMZ |
| Atacante | 30 | 192.168.30.0/24 | PC-Kali |

---

## 1. Topología

Router (R1) conectado a un switch (SW1) que agrupa tres equipos, uno por zona.

![Topología](../../evidence/01-packet-tracer/01-topologia.png)

| Equipo | Puerto del switch | VLAN |
|--------|-------------------|------|
| PC-LAN | Fa0/10 | 10 (LAN) |
| SRV-DMZ | Fa0/20 | 20 (DMZ) |
| PC-Kali | Fa0/24 | 30 (ATACANTE) |
| R1 | Gig0/1 | Trunk (pendiente) |

**Estado inicial de las interfaces** (antes de configurar VLANs, todos los
puertos en VLAN 1):

```
show interfaces status
```

![Estado de interfaces](../../evidence/01-packet-tracer/02-show-interfaces-status.png)

**Resultado:** los tres equipos aparecen como `connected` en los puertos
previstos (Fa0/10, Fa0/20 y Fa0/24). El enlace con el router queda como
`notconnect` porque la interfaz de R1 aún está apagada.

---

## 2. Creación de VLANs en SW1

```
hostname SW1
vlan 10
 name LAN
vlan 20
 name DMZ
vlan 30
 name ATACANTE

interface FastEthernet0/10
 switchport mode access
 switchport access vlan 10
interface FastEthernet0/20
 switchport mode access
 switchport access vlan 20
interface FastEthernet0/24
 switchport mode access
 switchport access vlan 30
```

- `vlan X` y `name` crean la VLAN con una etiqueta legible.
- `switchport mode access` fija el puerto como de usuario (una sola VLAN).
- `switchport access vlan X` asigna el puerto a su VLAN.

Verificación:

```
show vlan brief
```

![VLANs](../../evidence/01-packet-tracer/03-show-vlan-brief.png)

**Resultado:** tres VLANs creadas (10 LAN, 20 DMZ, 30 ATACANTE), cada equipo
asignado a la suya. Los puertos restantes permanecen en la VLAN `default`.

---

## Problemas y soluciones

| Problema | Causa | Solución |
|----------|-------|----------|
| `% Invalid input detected` al usar `show` | El prompt estaba en modo configuración (`config-if`) | Escribir `end` para volver al modo privilegiado |
| `Error copying nvram:yes` al guardar | Se escribió `yes` en vez de pulsar Enter en `Destination filename` | Pulsar Enter y aceptar el nombre por defecto |
| El switch 2960 no tiene el puerto Fa0/30 | Error en el plan inicial: solo hay Fa0/1 a Fa0/24 | Usar Fa0/24 para el equipo atacante |

## Próximos pasos

- Configurar el trunk SW1 → R1 y las subinterfaces con `encapsulation dot1Q`.
- Asignar direccionamiento IP y gateways por VLAN.
- Implementar ACLs según la política de seguridad y validarlas con pruebas.