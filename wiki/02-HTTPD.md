# 02. Servidor web HTTPD

## 01. Arquitectura y requisitos

Apache publica la página de los integrantes en `http://web.gorel.lab` mediante el puerto **80/TCP**. Utilizamos `httpd` versión `2.4.63-18.el10.x86_64`. Archivos: `/etc/httpd/conf/httpd.conf` y `/var/www/html/index.html`; respaldos: `httpd.conf.bak` e `index.html.bak`.

## 02. Despliegue y configuración

```bash
sudo dnf install httpd -y
sudo cp /etc/httpd/conf/httpd.conf /root/backups_interciclo/httpd.conf.bak
sudo systemctl enable --now httpd
curl -I http://localhost
sudo cp /var/www/html/index.html /root/backups_interciclo/index.html.bak
sudo nano /var/www/html/index.html
```

Personalizamos la página con el título **GOREL NETWORK LAB**, los integrantes Anthony Gonzalez y Jorge Orellana, y los datos del servidor.

```html
<h1>GOREL NETWORK LAB</h1>
<h2>Examen Practico de Interciclo</h2>
<div class="caja">
  <h2>Integrantes</h2>
  <p>Anthony Gonzalez</p>
  <p>Jorge Orellana</p>
</div>
```

## 03. Aseguramiento: SELinux y firewall

Conservamos el contexto `httpd_sys_content_t` para los archivos web y autorizamos HTTP sin desactivar SELinux.

```bash
sudo restorecon -v /var/www/html/index.html
sudo ls -Z /var/www/html/index.html
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
sudo firewall-cmd --list-services
```

## 04. Pruebas de aceptación y solución de problemas

La petición `curl -I http://localhost` devolvió **HTTP/1.1 200 OK**. Desde Windows abrimos `http://web.gorel.lab` y comprobamos la página con los integrantes.

[Apache activo]()
<img width="700" height="491" alt="09-apache-activo" src="https://github.com/user-attachments/assets/1814d05d-fbbe-4365-829b-58daa7aee39a" />

[HTTP 200]()
<img width="637" height="257" alt="10-apache-http200" src="https://github.com/user-attachments/assets/213ae9f2-fc4e-4a3a-84a1-a8106845a926" />

[Página personalizada]()
<img width="747" height="856" alt="11-pagina-web" src="https://github.com/user-attachments/assets/a2a3f459-df14-4bad-a281-68df9f4b9c6c" />

**Incidente y solución en la segunda máquina:** el navegador mostró **403 Forbidden**. El registro de Apache reveló la falta del permiso de búsqueda en `/var/www/html/es-ES`; se corrigió esa carpeta con `chmod 755`. En el servidor principal no se produjo este fallo.
