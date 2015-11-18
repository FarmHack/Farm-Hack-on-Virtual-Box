# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # https://docs.vagrantup.com.
  config.vm.box = "debian/jessie64"

  config.vm.hostname = "farmhack"

  config.vm.define "farmhack" do |farmhack|
  end

  config.vm.provider "virtualbox" do |vb|
    vb.name = "farmhack"
  end

  config.vm.network "forwarded_port", guest: 80, host: 8888

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    debconf-set-selections <<< 'mysql-server mysql-server/root_password password password'
    debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password password'
    apt-get install -y mysql-server
    sudo aptitude install php5 php5-mysql mysql-client -y
    sudo apt-get install php5-gd -y
    sudo apt-get install git -y
    curl -sS https://getcomposer.org/installer | php
    sudo mv composer.phar /usr/local/bin/composer
    sudo ln -s /usr/local/bin/composer /usr/bin/composer
    sudo git clone https://github.com/drush-ops/drush.git /usr/local/src/drush
    sudo ln -s /usr/local/src/drush/drush /usr/bin/drush
    cd /usr/local/src/drush
    sudo composer install
    cd /vagrant
    git clone https://github.com/FarmHack/FarmHack.org.git
    cd FarmHack.org
    mysql -uroot -ppassword -e "create database farmhack"
    sudo drush si standard -y --db-url=mysql://root:password@localhost/farmhack --site-name=local.farmhack.org
    cd /var/www
    sudo mv html html_old
    sudo ln -s /vagrant/FarmHack.org html
    sudo a2enmod rewrite
    sudo cp /vagrant/000-default.conf /etc/apache2/sites-available/
    sudo service apache2 reload
    cd /var/www/html
    cat dump.sql | drush sql-cli
  SHELL
end
