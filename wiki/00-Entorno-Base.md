# 00. Entorno base

## 01. Arquitectura y requisitos

| Elemento | Configuración |
|---|---|
| Sistema | CentOS Stream 10 en Oracle VirtualBox |
| Usuario y hostname | `gorel-edu` / `gorel-node` |
| NAT | `enp0s3`: `10.0.2.15/24` |
| Host-Only | `enp0s8`: `192.168.56.101/24` |
| Cliente Windows | `192.168.56.1/24` |
| Directorio de respaldos | `/root/backups_interciclo` |

## 02. Despliegue y configuración

Identificamos el usuario, asignamos el nombre del equipo, verificamos las interfaces y creamos la carpeta para respaldar los archivos antes de modificarlos.

```bash
whoami
sudo hostnamectl set-hostname gorel-node
hostname
ip a
ping -c 10 8.8.8.8
sudo mkdir -p /root/backups_interciclo
sudo ls -lh /root/backups_interciclo
```

Al finalizar, se verificaron estos cinco respaldos: `named.conf.bak`, `httpd.conf.bak`, `index.html.bak`, `sshd_config.bak` y `10-examen-puertos.conf.bak`.

## 03. Aseguramiento: SELinux y firewall

Mantenemos una red Host-Only para la comunicación Windows-CentOS y la salida NAT para Internet. Verificamos que SELinux permanezca activo y revisamos las reglas del firewall.

```bash
getenforce
sudo firewall-cmd --list-all
```

## 04. Pruebas de aceptación y solución de problemas

El comando `ping -c 10 8.8.8.8` registró **10 paquetes enviados, 10 recibidos y 0 % de pérdida**. Desde Windows, `ping 192.168.56.101` recibió cuatro respuestas.

**Evidencias a consultar:**

![Interfaces IP](../evidencias/02-interfaces-ip.png)
![10 ping hacia Internet](../evidencias/03-ping-internet.png)
![Ping desde Windows](../evidencias/04-ping-windows.png)
![Cinco respaldos](../evidencias/05-backups.png)

**Incidente y solución:** la máquina virtual quedó bloqueada con mensajes sobre `udev-worker` y `vboxguest`. Reiniciamos VirtualBox, ingresamos de nuevo como `gorel-edu` y verificamos que el servicio DNS siguiera activo. Los archivos respaldados permanecieron disponibles.
