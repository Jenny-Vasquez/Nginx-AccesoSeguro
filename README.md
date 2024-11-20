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

3. **Credenciales de Usuario**:
   - Contraseña del usuario `vagrant` establecida en `jenny`.

### Archivos Necesarios

Los siguientes archivos deben estar en el directorio del proyecto para que la configuración sea exitosa:

- `taylorweb`: archivo de configuración de Nginx.
- `vsftpd.conf`: archivo de configuración del servidor FTP.

### Personalización

- Podemosmodificar el archivo `Vagrantfile` para cambiar configuraciones como la IP privada o las redes expuestas.
- Editaremos los archivos `taylorweb` y `vsftpd.conf` para ajustar las configuraciones según nuestras necesidades.
## IMAGENES DE LA CONFIGURACIÓN
![Imagen1](imagenes_configuracion/2.png)
![Imagen2](imagenes_configuracion/3.png)
![Imagen3](imagenes_configuracion/4.png)
![Imagen4](imagenes_configuracion/5.png)
![Imagen5](imagenes_configuracion/6.png)
![Imagen6](imagenes_configuracion/7.png)
![Imagen7](imagenes_configuracion/8.png)
