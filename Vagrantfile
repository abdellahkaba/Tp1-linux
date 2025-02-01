Vagrant.configure("2") do |config|

  config.vm.define "server" do |webserver|

    webserver.vm.box = "ubuntu/focal64"  # Utiliser Ubuntu 20.04
    webserver.vm.box_check_update = false
    webserver.vm.boot_timeout = 800

    # Forwarder les ports pour Nginx et MySQL
    webserver.vm.network "forwarded_port", guest: 80, host: 8081   # Nginx
    webserver.vm.network "forwarded_port", guest: 3306, host: 3307  # MySQL

    # Réseau privé avec IP statique
    webserver.vm.network "private_network", type: "static", ip: "192.168.0.10"
    webserver.vm.synced_folder "C:/Users/Kaba/Documents/Angular/start-angular/dist/front-app/browser", "/home/vagrant/angular-app"


    webserver.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.name = "nginx-mysql"
      vb.memory = "1024"
      vb.cpus = 1
    end

    # Provision pour installer Nginx et MySQL
    webserver.vm.provision "shell", inline: <<-SHELL

      sudo apt-get update
      sudo apt-get -f install -y

      # Installer Nginx
      sudo apt-get install -y nginx
      sudo systemctl start nginx  

      # Pré-configurer MySQL avec un mot de passe root
      echo "mysql-server mysql-server/root_password password password" | sudo debconf-set-selections
      echo "mysql-server mysql-server/root_password_again password password" | sudo debconf-set-selections

      # Installer MySQL
      sudo apt-get install -y mysql-server
      sudo systemctl start mysql  # Utiliser 'systemctl'

    SHELL

  end
end

