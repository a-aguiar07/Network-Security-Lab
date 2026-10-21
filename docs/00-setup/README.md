---

## 4. Objetivo vulnerable: Metasploitable 2

**Objetivo:** desplegar una máquina vulnerable a propósito dentro de `SecLab`
como objetivo de las prácticas.

**Configuración de red:**

| Parámetro | Valor |
|-----------|-------|
| Adaptador | NAT Network |
| Red | SecLab |

![Red de MS2](../../evidence/00-setup/evidence00-setup-06-ms2-network-settings.png)

> ⚠️ La VM nunca se conecta en modo puente: es vulnerable a propósito y no debe
> exponerse a redes reales.

**Dirección IP:**

```bash
ifconfig eth0
```

![IP de MS2](../../evidence/00-setup/evidence00-setup-07-ms2-ip.png)

**Verificación de conectividad desde Kali:**

```bash
ping -c 3 <IP-MS2>
```

![Ping Kali a MS2](../../evidence/00-setup/08-kali-ping-ms2.png)

**Escaneo de servicios:**

```bash
nmap -sV -oN ms2-nmap-sv.txt <IP-MS2>
```

![Nmap parte 1](../../evidence/00-setup/09-nmap-part1.png)
![Nmap parte 2](../../evidence/00-setup/10-nmap-part2.png)

Salida completa: [`ms2-nmap-sv.txt`](../../evidence/00-setup/ms2-nmap-sv.txt)

**Resultado:** MS2 respondió al ping con 0% de pérdida y Nmap detectó
**23 puertos abiertos** (977 cerrados) en un escaneo de 17 segundos.
Entre los servicios expuestos hay dos servidores FTP (vsftpd 2.3.4 y
ProFTPD 1.3.1), SSH (OpenSSH 4.7p1), Telnet, SMTP (Postfix), DNS (BIND 9.4.2),
HTTP (Apache 2.2.8 y Tomcat), Samba, NFS, MySQL 5.0.51a, PostgreSQL 8.3,
VNC y UnrealIRCd, todos con versiones obsoletas. Destaca el puerto 1524, que
expone una shell de root sin autenticación (*bindshell*).

**Servicios más relevantes:**

| Puerto | Servicio | Versión | Por qué importa |
|--------|----------|---------|-----------------|
| 21 | FTP | vsftpd 2.3.4 | Versión con backdoor conocido |
| 23 | Telnet | Linux telnetd | Tráfico sin cifrar |
| 1524 | bindshell | Metasploitable root shell | Acceso root sin autenticación |
| 3306 | MySQL | 5.0.51a | Versión obsoleta |
| 6667 | IRC | UnrealIRCd | Versión con backdoor conocido |

**Snapshot:** `MS2 limpio`.