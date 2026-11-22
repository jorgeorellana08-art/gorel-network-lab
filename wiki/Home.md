# GOREL NETWORK LAB
## Implementación de servicios de red en CentOS Stream 10

**Examen práctico de interciclo — Sistemas Operativos**  
**Modalidad:** En parejas  
**Integrantes:** Anthony Gonzalez y Jorge Orellana

## Introducción
En esta práctica configuramos un servidor CentOS Stream 10 en VirtualBox. Instalamos y verificamos DNS mediante BIND, una página web con Apache y acceso remoto con SSH. También transferimos archivos entre Windows y CentOS mediante SCP.

## Datos del laboratorio

| Parámetro | Valor |
|---|---|
| Hostname | `gorel-node` |
| Usuario | `gorel-edu` |
| IP Host-Only | `192.168.56.101` |
| Dominio | `gorel.lab` |
| Cliente | Windows |
| Puerto SSH | `2222/tcp` |

## Documentación
- [00. Entorno base](00-Entorno-Base) — IP, conectividad y respaldos.
- [01. Servidor DNS](01-DNS) — BIND, zona DNS y consultas.
- [02. Servidor HTTPD](02-HTTPD) — Apache y página personalizada.
- [03. SSH y SCP](03-SSH-SCP) — acceso remoto y transferencias.

## Resultados
El DNS resolvió el dominio interno desde Windows; la página GOREL NETWORK LAB abrió mediante `web.gorel.lab`; SSH funcionó en el puerto 2222; SCP permitió enviar y recuperar archivos; se comprobó SELinux en modo `Enforcing` y se respaldaron los archivos intervenidos.

**Video demostrativo:** pendiente de incorporar el enlace público con la participación de ambos integrantes.

---
Anthony Gonzalez — Jorge Orellana
