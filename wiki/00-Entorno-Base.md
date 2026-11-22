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

[Interfaces IP]()
<img width="711" height="547" alt="02-interfaces-ip" src="https://github.com/user-attachments/assets/b3e123c5-cf08-4404-afe8-6cfcb44d65e4" />

[10 ping hacia Internet]()

<img width="611" height="342" alt="03-ping-internet" src="https://github.com/user-attachments/assets/cd33df52-f265-4a73-a0ad-a753fe42f4b6" />


[Ping desde Windows]()
<img width="757" height="408" alt="04-ping-windows" src="https://github.com/user-attachments/assets/911fc007-3497-44a3-83dd-7a8232071699" />

[Cinco respaldos]()
<img width="607" height="162" alt="05-backups" src="https://github.com/user-attachments/assets/a862b963-c775-4711-9778-9acf14f74376" />

[Uploading 03-ping-internet.png…]()
<img width="611" height="342" alt="03-ping-internet" src="https://github.com/user-attachments/assets/b38102b8-bf57-44a3-8aef-7610c5139c63" />



**Incidente y solución:** la máquina virtual quedó bloqueada con mensajes sobre `udev-worker` y `vboxguest`. Reiniciamos VirtualBox, ingresamos de nuevo como `gorel-edu` y verificamos que el servicio DNS siguiera activo. Los archivos respaldados permanecieron disponibles.
