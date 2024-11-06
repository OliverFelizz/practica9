Vagrant.configure("2") do |config|
    # Configuración de la máquina web1
    config.vm.define "web1" do |web1|
      web1.vm.box = "ubuntu/jammy64"
      web1.vm.hostname = "web1"
      web1.vm.network "private_network", ip: "192.168.56.10"
      web1.vm.provider "virtualbox" do |vb|
        vb.memory = 512
        vb.cpus = 1
      end
      # Sincronización de carpetas entre el host y el servidor Apache
      web1.vm.synced_folder "./web1", "/var/www/html"
      # Script para instalar Apache
      web1.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        systemctl enable apache2
        systemctl start apache2
      SHELL
    end
  
    # Configuración de la máquina web2
    config.vm.define "web2" do |web2|
      web2.vm.box = "ubuntu/jammy64"
      web2.vm.hostname = "web2"
      web2.vm.network "private_network", ip: "192.168.56.11"
      web2.vm.provider "virtualbox" do |vb|
        vb.memory = 512
        vb.cpus = 1
      end
      # Sincronización de carpetas entre el host y el servidor Apache
      web2.vm.synced_folder "./web2", "/var/www/html"
      # Script para instalar Apache
      web2.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        systemctl enable apache2
        systemctl start apache2
      SHELL
    end
  
    # Configuración de la máquina loadbalancer
    config.vm.define "loadbalancer" do |lb|
      lb.vm.box = "ubuntu/jammy64"
      lb.vm.hostname = "loadbalancer"
      lb.vm.network "private_network", ip: "192.168.56.12"
      lb.vm.provider "virtualbox" do |vb|
        vb.memory = 512
        vb.cpus = 1
      end
      # Script para instalar NGINX y configurar el balanceador
      lb.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y nginx
        # Configuración del balanceador de carga en NGINX
        cat <<EOL > /etc/nginx/conf.d/load_balancer.conf
        upstream web_cluster {
            server 192.168.56.10;
            server 192.168.56.11;
        }
        server {
            listen 80;
            location / {
                proxy_pass http://web_cluster;
            }
        }
        EOL
        systemctl enable nginx
        systemctl restart nginx
      SHELL
    end
  end
  