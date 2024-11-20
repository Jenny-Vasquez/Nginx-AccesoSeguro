# -*- mode: ruby -*-
# vi: set ft=ruby :

# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Definir la imagen de Debian
  config.vm.box = "debian/bookworm64"
  # Configuración de red
  config.vm.network "forwarded_port", guest: 80, host: 8081
  config.vm.network "private_network", ip: "192.168.56.11"
  # Aprovisionamiento con shell script
  config.vm.provision "shell", name: "nginx", inline: <<-SHELL
    # Actualizar el sistema e instalar Nginx y vsftpd para FTPS
    apt-get update
    apt-get install -y nginx git
    # Crea un directorio /var/www/nginxS/html si no existe. 
    # El -p asegura que también se creen subdirectorios necesarios.
    mkdir -p /var/www/taylorweb/html
    # Cambia al directorio para continuar con las siguientes operaciones.
    cd /var/www/taylorweb/html

    # Clona el repositorio, esto descargará los archivos del repositorio

    git clone https://github.com/Jenny-Vasquez/Practica2.git

    #3Cambia el propietario y grupo de todos los archivos dentro de /var/www/nginxS/html a www-data, 
    # que es el usuario y grupo bajo el que corre el servidor Nginx.
    chown -R www-data:www-data /var/www/taylorweb/html
    # Cambia los permisos de los archivos en el directorio /var/www/nginxS para dar permisos de lectura, 
    # escritura y ejecución al propietario, y de lectura y ejecución a los demás usuarios.
    chmod -R 755 /var/www/taylorweb
    # Copia un archivo de configuración Nginx llamado nginxS desde /vagrant/ al directorio /etc/nginx/sites-available/,
    # donde se almacenan las configuraciones de los sitios en Nginx.
    cp -v /vagrant/taylorweb /etc/nginx/sites-available/
    # Crea un enlace simbólico desde /etc/nginx/sites-available/nginxS a /etc/nginx/sites-enabled/, habilitando la configuración del sitio.
    # Nginx utiliza este enlace para saber qué sitios debe servir.
    ln -s /etc/nginx/sites-available/taylorweb /etc/nginx/sites-enabled
    # Elimina el archivo de configuración por defecto de Nginx que viene con la instalación
    rm /etc/nginx/sites-enabled/default
    # Reinicia el servicio Nginx para aplicar los cambios de configuración.
    systemctl restart nginx
  SHELL

  # Establece la contraseña para el usuario vagrant en la máquina virtual
  config.vm.provision "shell", name: "set_password", inline: <<-SHELL
    echo "vagrant:jenny" | sudo chpasswd
  SHELL
  #  Instala y configura un servidor FTP seguro utilizando vsftpd
  config.vm.provision "shell", name: "vsftpd", inline: <<-SHELL
    # Establecer permisos y propiedad
    mkdir /home/vagrant/ftp
    chown vagrant:vagrant /home/vagrant/ftp
    chmod -R 755 /home/vagrant/ftp
    # Actualizar los paquetes y instalar vsftpd
    apt-get update
    apt-get install -y vsftpd
    # Generar un certificado SSL para FTP seguro (FTPS)
    openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt -subj "/C=ES/ST=./L=./O=./OU=./CN=nginxS/emailAddress=."
    # Copiar archivo de configuración de vsftpd
    cp -v /vagrant/vsftpd.conf /etc/
    # Ajustar permisos del directorio privado del certificado
    sudo chmod 755 /etc/ssl/private
    # Reiniciar el servicio vsftpd
    sudo systemctl restart vsftpd
  SHELL
end


