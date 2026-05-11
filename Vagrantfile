Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.provision "shell", inline: <<-SHELL
    # Actualiza los paquetes del sistema
    apt-get update

    # Instala Docker (para ejecutar contenedores)
    apt-get install -y docker.io

    # Instala Docker Compose (para gestionar múltiples contenedores)
    apt-get install -y docker-compose

    # Instala Git (para clonar el repositorio del proyecto)
    apt-get install -y git

    # Activa Docker al inicio del sistema
    systemctl enable docker

    # Inicia el servicio Docker ahora mismo
    systemctl start docker

    # Crea la carpeta donde vivirá la aplicación dentro de la VM
    mkdir -p /home/vagrant/app

    # Clona el repositorio si no existe todavía
    if [ ! -d "/home/vagrant/app/.git" ]; then
      git clone https://github.com/Ilyass-9900/ProyectoAmpliaci-n /home/vagrant/app
    else
      # Si ya existe, actualiza el repositorio
      cd /home/vagrant/app
      git pull
    fi

    # Permisos para evitar el error "permission denied"
    usermod -aG docker vagrant
    chmod 666 /var/run/docker.sock
    
    # Entramos en la carpeta del proyecto
    cd /home/vagrant/app

    # Detenemos contenedores anteriores si existen (evita errores)
    docker-compose down || true

    # Levanta los contenedores definidos en docker-compose.yml
    docker-compose up -d

  SHELL

end
