# 01. Servidor DNS (BIND)

## 01. Arquitectura y requisitos

El servicio DNS resuelve los nombres internos de `gorel.lab` a la dirección del servidor `192.168.56.101` mediante el puerto **53 TCP/UDP**. Instalamos `bind` y `bind-utils`. Los archivos intervenidos fueron `/etc/named.conf` y `/var/named/gorel.lab.zone`, con respaldo `named.conf.bak`.

| Registro | IP |
|---|---|
| `ns1.gorel.lab` | `192.168.56.101` |
| `web.gorel.lab` | `192.168.56.101` |
| `ssh.gorel.lab` | `192.168.56.101` |
| `admin.gorel.lab` | `192.168.56.101` |

## 02. Despliegue y configuración

```bash
sudo dnf install bind bind-utils -y
sudo cp /etc/named.conf /root/backups_interciclo/named.conf.bak
sudo nano /etc/named.conf
```

En `/etc/named.conf` configuramos:

```conf
listen-on port 53 { 127.0.0.1; 192.168.56.101; };
allow-query { localhost; 192.168.56.0/24; };

zone "gorel.lab" IN {
    type master;
    file "gorel.lab.zone";
    allow-update { none; };
};
```

Creamos `/var/named/gorel.lab.zone`:

```text
$TTL 86400
@ IN SOA ns1.gorel.lab. admin.gorel.lab. (
    2026112101
    3600
    1800
    604800
    86400
)
@       IN NS ns1.gorel.lab.
ns1     IN A 192.168.56.101
web     IN A 192.168.56.101
ssh     IN A 192.168.56.101
admin   IN A 192.168.56.101
```

Validamos los archivos e iniciamos el servicio:

```bash
sudo named-checkconf
sudo named-checkzone gorel.lab /var/named/gorel.lab.zone
sudo systemctl enable --now named
sudo systemctl status named
```

## 03. Aseguramiento: SELinux y firewall

Limitamos consultas a la red Host-Only, impedimos actualizaciones dinámicas y conservamos el contexto `named_zone_t`.

```bash
sudo restorecon -v /var/named/gorel.lab.zone
sudo ls -Z /var/named/gorel.lab.zone
sudo firewall-cmd --permanent --add-service=dns
sudo firewall-cmd --reload
getenforce
```

## 04. Pruebas de aceptación y solución de problemas

`named-checkzone` devolvió **OK**; `named` quedó `active (running)`. Desde PowerShell comprobamos:

```powershell
nslookup web.gorel.lab 192.168.56.101
nslookup web.gorel.lab
```

Ambas consultas devolvieron `192.168.56.101` después de configurar DNS en Windows.

[Validación DNS]()
<img width="723" height="222" alt="06-zona-dns-ok" src="https://github.com/user-attachments/assets/3f9dcbce-3f8c-456c-bd28-1ab244cb7287" />

[Servicio named activo]()
<img width="665" height="510" alt="07-named-activo" src="https://github.com/user-attachments/assets/ca2062da-4e95-4887-bc18-90578988e72e" />

[Consulta desde Windows]()
<img width="532" height="196" alt="08-dns-windows" src="https://github.com/user-attachments/assets/8b7fddfc-4d17-42a4-976e-0e679d4c1238" />

**Incidente y solución de la práctica en pareja:** en el equipo secundario BIND no iniciaba por escribir `allow-update { node; };`, lo que generó `undefined ACL 'node'`. Se corrigió a `none`. También se ajustó `allow-query` a la red del segundo equipo al recibir `Query refused`. Este incidente no corresponde al servidor principal.
