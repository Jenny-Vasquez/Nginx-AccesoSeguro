# Nginx-Autentificacion

Previamente la confiracion de nuestra maquina debe estar correcta, para ello hemos realizado los siguientes pasos.

## Configuración de Máquina Virtual con Debian, Nginx y FTPS

Hemos configurado automáticamente una máquina virtual basada en Debian utilizando **Vagrant**. La configuración incluye:

- Instalación de **Nginx** y configuración personalizada para un sitio web.
- Implementación de un servidor FTPS seguro con **vsftpd**.
- Clonación de un repositorio Git en un directorio específico.

### Requisitos Previos

1. **Vagrant** instalado en tu sistema.
2. **VirtualBox** u otro proveedor compatible con Vagrant.
3. Acceso a Internet para descargar la caja base y dependencias.
4. Un repositorio Git accesible para la clonación (en este caso: `https://github.com/Jenny-Vasquez/Practica2.git`).

#### Aprovisionamiento

1. **Nginx**:
   - Descarga y configuración.
   - Clonación del repositorio especificado en `/var/www/taylorweb/html`.
   - Configuración de permisos y propietarios para Nginx.
   - Enlace simbólico a la configuración de Nginx habilitada.

2. **vsftpd**:
   - Instalación y configuración para FTPS.
   - Generación de certificados SSL para un servidor seguro.
   - Configuración de un directorio FTP en `/home/vagrant/ftp`.

Como ya tenemos el archivo del servido configurado de la siguiente manera:
server {
    listen 80;
    listen [::]:80;
    server_name taylorweb;

    root /var/www/taylorweb/html/Practica2;
    index index.html index.htm index.nginx-debian.html;

    location / {
        try_files $uri $uri/ =404;  # Si no se encuentra el archivo, devuelve 404
    }
}


Comprobaremos que nuestro servido funciona correctamente con 
"sudo nginx -t"
![Imagen9](imagenes_configuracion/9.png)

Ahora porcederemos a configurar el cortafuegos previamente deberemos actualizar e installar Uncomplicated Firewall
sudo apt update
sudo apt install ufw 

sudo ufw status

sudo ufw allow ssh

sudo ufw allow 'Nginx Full'

sudo ufw delete allow 'Nginx HTTP'
![Imagen10](imagenes_configuracion/10.png) 
Activo el cortafuegos 
sudo ufw enable
![Imagen11](imagenes_configuracion/11.png) 

## IMAGENES DE LA CONFIGURACIÓN
![Imagen1](imagenes_configuracion/2.png)
![Imagen2](imagenes_configuracion/3.png)
![Imagen3](imagenes_configuracion/4.png)
![Imagen4](imagenes_configuracion/5.png)
![Imagen5](imagenes_configuracion/6.png)
![Imagen6](imagenes_configuracion/7.png)
![Imagen7](imagenes_configuracion/8.png)
