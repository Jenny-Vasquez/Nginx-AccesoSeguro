# Nginx-Autentificación

## Configuración de Máquina Virtual con Debian, Nginx y FTPS

Hemos configurado automáticamente una máquina virtual basada en Debian utilizando **Vagrant**. La configuración incluye:

- Instalación de **Nginx** y configuración personalizada para un sitio web.
- Implementación de un servidor FTPS seguro con **vsftpd**.
- Clonación de un repositorio Git en nuestro caso https://github.com/Jenny-Vasquez/Practica2.git

Para este apartado de autentidicacion verificaremos si el paquete esta instalado. 
 ```bash
   dpkg -l | grep openssl
   ```
![Imagen21](imagenes_configuracion/21.png)

Creo los usarios con los siguientes comandos.
 ```bash
   sudo sh -c "echo -n 'jenny:' >> /etc/nginx/.htpasswd"
   ```
 ```bash
   sudo sh -c "openssl passwd -apr1 >> /etc/nginx/.htpasswd"
   ```
 ```bash
   sudo sh -c "echo -n 'vasquez:' >> /etc/nginx/.htpasswd"
   ```
 ```bash
   sudo sh -c "openssl passwd -apr1 >> /etc/nginx/.htpasswd"
   ```
Verifico que los usuarios se crearon correctamente
 ```bash
   cat /etc/nginx/.htpasswd
   ```
![Imagen22](imagenes_configuracion/22.png)
Modifico el archivo para añadir la autenticación y reinicio el servicio:
 ```bash
   sudo nano /etc/nginx/sites-available/taylorweb
   ```
![Imagen23](imagenes_configuracion/23.png)

 ```bash
   sudo nginx -t
   ```
![Imagen24](imagenes_configuracion/24.png)

Accedo a taylorweb y me sale la siguiente información:

![Imagen25](imagenes_configuracion/24.png)  ![Imagen27](imagenes_configuracion/24.png)

Ahora puedo visualizar la pagina.

A continuación compruebo los logs de erroy y acceso:
 ```bash
sudo cat /var/log/nginx/error.log | tail -2
sudo cat /var/log/nginx/access.log | tail -2
   ```
![Imagen28](imagenes_configuracion/28.png)  ![Imagen29](imagenes_configuracion/29.png)

Redirecciono la pagina para cuando acceda me aparezca la pagina de contact
 ![Imagen30](imagenes_configuracion/30.png)
  ![Imagen31](imagenes_configuracion/31.png)





# Nginx-Acceso Seguro

Previamente la configuracion de nuestra maquina debe estar correcta, para ello hemos realizado los siguientes pasos.

### 1. Instalación y Configuración de Nginx
1. **Vagrant** instalado en tu sistema.
2. **VirtualBox** u otro proveedor compatible con Vagrant.
3. Acceso a Internet para descargar la caja base y dependencias.
4. Un repositorio Git accesible para la clonación (en este caso: `https://github.com/Jenny-Vasquez/Practica2.git`).
5. La configuración del servidor se ajustó de la siguiente forma:

    ```nginx
   server{
       listen 80;
       listen [::]:80;
       server_name taylorweb;
   
       root /var/www/taylorweb/html/Practica2;
       index index.html index.htm index.nginx-debian.html;
   
       location / {
           try_files $uri $uri/ =404;  
       }
   }
    
6. Se verificó que la configuración estuviera correcta ejecutando:
   
   ```bash
   sudo nginx -t
   ```
![Imagen9](imagenes_configuracion/9.png)

### 2. Configuración del Cortafuegos

1. Se instaló y configuró el cortafuegos **UFW** para proteger la máquina virtual:
   ```bash
   sudo apt update
   sudo apt install ufw
   ```
2. Se realizaron las siguientes configuraciones:
   - Permitir SSH:  
     ```bash
     sudo ufw allow ssh
     ```
   - Permitir tráfico seguro por HTTPS (Nginx Full):  
     ```bash
     sudo ufw allow 'Nginx Full'
     ```
   - Bloquear tráfico no seguro:  
     ```bash
     sudo ufw delete allow 'Nginx HTTP'
     ```
     ![Imagen10](imagenes_configuracion/10.png) 
3. Se activó el cortafuegos:
   ```bash
   sudo ufw enable
   ```
   ![Imagen11](imagenes_configuracion/11.png) 
4. El estado se verificó con:
   ```bash
   sudo ufw status
   ```
   ![Imagen12](imagenes_configuracion/12.png) 

### 3. Habilitación de HTTPS con Certificado Autofirmado

1. Se generó un certificado SSL autofirmado:
   ```bash
   sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
   -keyout /etc/ssl/private/taylorweb.key \
   -out /etc/ssl/certs/taylorweb.crt
   ```
   ![Imagen13](imagenes_configuracion/13.png)
2. Se ajustaron los permisos de los archivos del certificado:
   ```bash
   sudo chmod 600 /etc/ssl/private/taylorweb.key
   sudo chmod 600 /etc/ssl/certs/taylorweb.crt
   ```
   ![Imagen14](imagenes_configuracion/14.png)

3. Se configuró Nginx para usar HTTPS en el puerto 443:
   ```nginx
   server {
       listen 443 ssl;  # Puerto HTTPS
       server_name taylorweb;

       root /var/www/taylorweb/html/Practica2;
       index index.html index.htm;

       ssl_certificate /etc/ssl/certs/taylorweb.crt; 
       ssl_certificate_key /etc/ssl/private/taylorweb.key;  

       ssl_protocols TLSv1.2 TLSv1.3; 

       location / {
           try_files $uri $uri/ =404;
       }
   }
   ```

4. Se verificó la configuración nuevamente:
   ```bash
   sudo nginx -t
   ```
 
![Imagen15](imagenes_configuracion/15.png)

## Resultados

El servidor está configurado para usar HTTPS, pero como el certificado SSL es **autofirmado**, los navegadores mostrarán una advertencia indicando que la conexión no es completamente segura. 

### Mensaje en el Navegador

Al acceder al sitio en un navegador mediante `https://taylorweb`, se muestra un mensaje indicando que la conexión no es completamente segura, lo que significa que los datos intercambiados entre el navegador y el sitio no están cifrados.
![Imagen16](imagenes_configuracion/16.png)

## IMAGENES DE LA CONFIGURACIÓN
![Imagen1](imagenes_configuracion/2.png)
![Imagen2](imagenes_configuracion/3.png)
![Imagen3](imagenes_configuracion/4.png)
![Imagen4](imagenes_configuracion/5.png)
![Imagen5](imagenes_configuracion/6.png)
![Imagen6](imagenes_configuracion/7.png)
![Imagen7](imagenes_configuracion/8.png)
