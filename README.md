# GOREL NETWORK LAB

**Implementación de servicios de red en CentOS Stream 10**  
Examen práctico de interciclo · Sistemas Operativos · Modalidad en parejas  
**Integrantes:** Anthony Gonzalez y Jorge Orellana

## Introducción
En esta práctica configuramos un servidor CentOS Stream 10 en VirtualBox para proporcionar DNS con BIND, una página web con Apache y acceso remoto mediante SSH. También realizamos transferencias de archivos SCP en los dos sentidos desde Windows. Documentamos los procedimientos, las medidas de seguridad y las pruebas efectuadas.

## Entorno y servicios

| Elemento | Configuración |
|---|---|
| Servidor | `gorel-node` · CentOS Stream 10 |
| Usuario | `gorel-edu` |
| Red de cliente | Windows `192.168.56.1` |
| Red del servidor | `192.168.56.101` (Host-Only) |
| Salida a Internet | `10.0.2.15` (NAT) |
| Dominio interno | `gorel.lab` |
| DNS | BIND · puerto 53 |
| HTTP | Apache · puerto 80 |
| SSH / SCP | OpenSSH · puerto 2222 |
| Seguridad | SELinux Enforcing y firewalld |

## Documentación técnica
Cada componente contiene las cuatro secciones exigidas: **Arquitectura y requisitos; Despliegue y configuración; Seguridad SELinux/firewall; Pruebas y solución de problemas**.

1. [00. Entorno base](wiki/00-Entorno-Base.md)
2. [01. Servidor DNS](wiki/01-DNS.md)
3. [02. Servidor web HTTPD](wiki/02-HTTPD.md)
4. [03. Acceso SSH y SCP](wiki/03-SSH-SCP.md)

La carpeta [evidencias](evidencias/) reúne las capturas originales del laboratorio. **Las imágenes se incorporarán en una carga separada** y no deben considerarse publicadas hasta que aparezcan en el repositorio.

## Resultados
Se comprobó conectividad mediante diez ping sin pérdida, resolución de `web.gorel.lab` desde Windows, acceso a la página personalizada, conexión SSH por el puerto 2222 y dos transferencias SCP completadas al 100 %. Se verificó SELinux en `Enforcing` y se conservaron cinco respaldos de configuración.

## Entregables
- Documentación técnica: [páginas del repositorio](wiki/Home.md).
- Wiki pública del profesor: pendiente de trasladar estas páginas a la pestaña **Wiki**.
- Video demostrativo con ambos integrantes: **pendiente de añadir enlace público**.

---
Anthony Gonzalez · Jorge Orellana
