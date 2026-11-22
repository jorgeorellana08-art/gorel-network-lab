# 03. SSH y SCP

## 01. Arquitectura y requisitos

Usamos `openssh-server` en `gorel-node` para acceso seguro desde Windows al dominio `ssh.gorel.lab`, por el puerto **2222/TCP**. Los archivos de configuración fueron `/etc/ssh/sshd_config` y `/etc/ssh/sshd_config.d/10-examen-puertos.conf`; se respaldaron ambos.

## 02. Despliegue y configuración

```bash
rpm -q openssh-server
sudo cp /etc/ssh/sshd_config /root/backups_interciclo/sshd_config.bak
sudo cp /etc/ssh/sshd_config.d/10-examen-puertos.conf /root/backups_interciclo/10-examen-puertos.conf.bak
```

En el archivo complementario quedó solo:

```conf
Port 2222
```

Comprobamos la configuración y reiniciamos el servicio:

```bash
sudo sshd -t
sudo sshd -T | grep '^port '
sudo systemctl restart sshd
sudo ss -tlnp | grep sshd
```

El resultado final mostró `port 2222` y las direcciones `0.0.0.0:2222` y `[::]:2222`, sin el puerto 22.

## 03. Aseguramiento: SELinux y firewall

Comprobamos `ssh_port_t` con el puerto 2222, abrimos `2222/tcp` y retiramos la regla genérica `ssh` (22) solo después de probar la conexión. SELinux siguió en `Enforcing`.

```bash
sudo semanage port -l | grep ssh_port_t
sudo firewall-cmd --permanent --add-port=2222/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --permanent --remove-service=ssh
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
getenforce
```

## 04. Pruebas de aceptación y solución de problemas

Conexión desde PowerShell:

```powershell
ssh -p 2222 gorel-edu@ssh.gorel.lab
```

Subida Windows a CentOS:

```powershell
echo "Prueba SCP GOREL NETWORK LAB" > prueba.txt
scp -P 2222 prueba.txt gorel-edu@ssh.gorel.lab:/home/gorel-edu/
```

Archivo de retorno creado desde CentOS:

```bash
echo "Archivo enviado desde CentOS GOREL" > servidor.txt
```

Descarga CentOS a Windows:

```powershell
scp -P 2222 gorel-edu@ssh.gorel.lab:/home/gorel-edu/servidor.txt .
```

Ambas transferencias finalizaron al **100 %** y comprobamos que `prueba.txt` existía en el servidor.

![SSH desde Windows](../evidencias/12-ssh-conexion.png)
![SCP subida](../evidencias/13-scp-subida.png)
![Archivo recibido](../evidencias/14-scp-recibido.png)
![SCP descarga](../evidencias/15-scp-descarga.png)
![SSH solo puerto 2222](../evidencias/16-ssh-solo2222.png)
![SELinux Enforcing](../evidencias/17-selinux-enforcing.png)
![Firewall final](../evidencias/18-firewall-final.png)

**Incidente y solución:** inicialmente `sshd -T` mostró `port 22` y `port 2222`. Identificamos `10-examen-puertos.conf`, lo respaldamos, quitamos `Port 22`, comprobamos la sintaxis y reiniciamos SSH. Confirmamos la conexión por 2222 antes de retirar la regla del puerto 22.
