# Fase 03: Análisis de tráfico con Wireshark

## Objetivo

Observar cómo se ve en la red el tráfico del atacante hacia la DMZ, para
entender qué paquetes genera cada acción y preparar las reglas de detección
de la Fase 04.

**Punto de captura:** interfaz `eth0` de Kali (atacante), `192.168.30.100`.

---

## 1. Handshake TCP (conexión correcta)

```bash
curl -I http://192.168.20.10
```

Filtro: `tcp.port == 47052`

![Handshake TCP](../../evidence/03-wireshark/01-handshake-tcp.png)

| Paquete | Descripción |
|---------|-------------|
| `[SYN]` | Kali solicita abrir la conexión |
| `[SYN, ACK]` | MS2 acepta |
| `[ACK]` | Handshake completo |
| `HEAD / HTTP/1.1` | Petición del `curl` |
| `HTTP/1.1 200 OK` | Respuesta de Apache |
| `[FIN, ACK]` | Cierre ordenado |

La trama del atacante lleva como MAC de destino la interfaz del firewall
(`em3`), no la de MS2: todo el tráfico entre zonas pasa por pfSense.