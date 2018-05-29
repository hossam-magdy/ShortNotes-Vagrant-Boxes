# `vagrant init` creates new `Vagrantfile` with guide

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.box_check_update = false

  # Forward/map ports
  # https://www.vagrantup.com/docs/networking/forwarded_ports.html
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 3306, host: 3306

  config.vm.network "private_network", ip: "192.168.33.10"

  # Custom synced/mapped folder, other than /vagrant
  # https://www.vagrantup.com/docs/synced-folders/basic_usage.html
  # config.vm.synced_folder "/home/userX/www", "/var/www"

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    
    # git
    sudo apt-get install -y git
    
    # node & npm
    curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    sudo apt-get install -y nodejs
    sudo apt-get install -y build-essential
    sudo npm install -g npm

    # MySQL: suppress password prompt + grant all prev to root
    sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password password root'
    sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password root'
    sudo apt-get install -y mysql-server
    sudo sed -i "s/^bind-address/#bind-address/" /etc/mysql/mysql.conf.d/mysqld.cnf
    sudo mysql -u root -proot -e "GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION; FLUSH PRIVILEGES; SET GLOBAL max_connect_errors=10000;"
    sudo /etc/init.d/mysql restart
    
    # cleanup
    sudo apt-get update
    sudo apt-get upgrade -y
    sudo apt-get autoremove -y
  SHELL
end
