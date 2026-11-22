# Registro de evidencias

Las capturas de esta carpeta corresponden a las pruebas realizadas en el laboratorio. Se conservan los nombres originales del paquete entregado para evitar confusiones.

| Archivo | Evidencia | Página |
|---|---|---|
| `01-hostname.png` | Usuario y hostname `gorel-node` | 00. Entorno base |
| `02-interfaces-ip.png` | Interfaces NAT y Host-Only | 00. Entorno base |
| `03-ping-internet.png` | Diez ping a 8.8.8.8, 0 % pérdida | 00. Entorno base |
| `04-ping-windows.png` | Conectividad Windows–CentOS | 00. Entorno base |
| `05-backups.png` | Cinco respaldos confirmados | 00. Entorno base |
| `06-zona-dns-ok.png` | Validación de zona DNS, resultado OK | 01. DNS |
| `07-named-activo.png` | Servicio BIND activo | 01. DNS |
| `08-dns-windows.png` | Resolución desde Windows | 01. DNS |
| `09-apache-activo.png` | Servicio HTTPD activo | 02. HTTPD |
| `10-apache-http200.png` | Respuesta HTTP 200 | 02. HTTPD |
| `11-pagina-web.png` | Página GOREL NETWORK LAB en navegador | 02. HTTPD |
| `12-ssh-conexion.png` | Acceso desde PowerShell por puerto 2222 | 03. SSH-SCP |
| `13-scp-subida.png` | Transferencia Windows a CentOS | 03. SSH-SCP |
| `14-scp-recibido.png` | Archivo recibido en CentOS | 03. SSH-SCP |
| `15-scp-descarga.png` | Transferencia CentOS a Windows | 03. SSH-SCP |
| `16-ssh-solo2222.png` | Puerto SSH personalizado único | 03. SSH-SCP |
| `17-selinux-enforcing.png` | SELinux activo | 03. SSH-SCP |
| `18-firewall-final.png` | Reglas del firewall | 03. SSH-SCP |
| `19-dns-error-companero.png` | Incidencia DNS en el segundo equipo | 01. DNS |
| `20-web-error403-companero.png` | Incidencia HTTP 403 en el segundo equipo | 02. HTTPD |
| `21-web-solucion-companero.png` | Resolución del error web en el segundo equipo | 02. HTTPD |

> Las capturas del segundo equipo están identificadas como incidencias de la práctica en pareja. No deben confundirse con las pruebas del servidor principal (`192.168.56.101`).

**Estado de imágenes:** deben añadirse los archivos PNG originales a esta carpeta. Los documentos ya incorporan las referencias para que se muestren automáticamente una vez publicados.
