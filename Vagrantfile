# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  # JP Note: this box came from: https://cloud-images.ubuntu.com/vagrant/
  # JP Note: I first downloaded the image to my home folder
  config.vm.box = "~/wily-server-cloudimg-amd64-vagrant-disk1.box"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.

  # Set Windows rdp server port to 7000
  config.vm.network "forwarded_port", guest: 3389, host: 7000
  
  # Set apache2 web server port to 8000
  config.vm.network "forwarded_port", guest: 80, host: 8000

  # Set Tomcat web server prot to 9000
  config.vm.network "forwarded_port", guest: 8080, host: 9000

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
      # JP Note: 512 for simple server, 1024 for LAMP/Tomcat or simple GUI, 2048 for advanced GUI
      vb.memory = "2048"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.

  config.vm.provision "shell", inline: <<-SHELL

    echo '-----------------------------------------------------------------------------------------------'
    echo '---- UPDATING THE SYSTEM'
    echo '-----------------------------------------------------------------------------------------------'

    echo 'Starting shell script at:'
    date
    whoami
    pwd

    apt-get update
    # uncomment next line after you have this script working well
    # apt-get upgrade

    echo '---- SETTING HOST NAME'
    hostnamectl set-hostname csserver
    hostnamectl
    echo '--'

    echo '---- INSTALLING UTILITY PROGRAMS'
    apt-get install -y git bzip2 zip unzip screen
    echo '--'

    echo '---- SETTING PASSWORD FOR USER vagrant TO mucis'
    echo vagrant:mucis | sudo chpasswd
    echo '--'


    # Uncomment those features that you want to add:


    echo '-----------------------------------------------------------------------------------------------'
    echo '---- INSTALLING COMPILERS AND SERVERS'
    echo '-----------------------------------------------------------------------------------------------'

    echo '----PERL IS PREINSTALLED'
    perl --version
    echo '--'
 
    echo '---- PYTHON AND PYTHON3 ARE PREINSTALLED'
    python3 --version
    echo '--'

    echo '---- RUBY IS PREINSTALLED'
    ruby --version
    echo '--'

#    echo '---- INSTALLING C AND C++ COMPILERS'
#    apt-get install -y build-essential
#    gcc --version
#    g++ --version
#    echo '--'

#    echo '---- INSTALLING C# COMPILER'
#    apt-get install -y mono-complete
#    mono
#    echo '--'

#    echo '---- INSTALLING GO'
#    apt-get install -y golang
#    go version
#    echo '--'

    echo '---- INSTALLING JAVA 8 COMPILER'
    apt-get install -y openjdk-8-jdk
    javac -version
    java -version
    echo '--'

#    echo '---- INSTALLING CLOJURE'
#    apt-get install -y clojure1.6
#    echo '--'

#    echo '---- INSTALLING LEGACY COMPILERS'
#    echo '--'

#    echo '---- INSTALLING FORTRAN COMPILER'
#    apt-get install -y gfortran
#    gfortran --version
#    echo '--'

#    echo '---- INSTALLING COBOL COMPILER'
#    apt-get install -y open-cobol
#    cobc -V
#    echo '--'

#    echo '---- INSTALLING PL/I COMPILER'
#    wget http://www.iron-spring.com/pli-0.9.9.tgz
#    tar -xvzf pli-0.9.9.tgz
#    cd pli-0.9.9
#    make install
#    cd ..
#    rm -f pli-0.9.9.tgz
#    plic -V
#    echo '--'


    echo '-----------------------------------------------------------------------------------------------'
    echo '---- INSTALLING APACHE2, MYSQL, AND TOMCAT SERVERS'
    echo '-----------------------------------------------------------------------------------------------'
    
    apt-get install -y apache2
    echo '--'

    echo '---- INSTALLING PHP5'
    apt-get install -y php5 php5-mysql libapache2-mod-php5 php5-gd php5-imagick
    echo '--'

    echo '---- INSTALLING MYSQL WITH NO ROOT PASSWORD'
    DEBIAN_FRONTEND=noninteractive apt-get -y install mysql-server
    echo '--'

    echo '---- INSTALLING TOMCAT JAVA WEB SERVER'
    apt-get install -y tomcat8 tomcat8-docs tomcat8-admin tomcat8-examples
    service tomcat8 status
    echo '--'


    echo '-----------------------------------------------------------------------------------------------'
    echo '---- CONFIGURE APACHE2 AND TOMCAT WORKSPACES'
    echo '-----------------------------------------------------------------------------------------------'
    
    echo '---- SET DEFAULT APACHE2 WEB DIRECTORY TO /vagrant/html'
    echo '---- VIEW WEB PROGRAMS IN BROWSER AT localhost:8000'
    service apache2 stop
    rm -rf /var/www/html
    mkdir /vagrant/html
    ln -s /vagrant/html /var/www/html
    echo '<html><body><h1>Hello world from Apache2</h1></body></html>' > /vagrant/html/testhtml.html
    service apache2 start
    echo '--'

    echo '---- SET THE TOMCAT ADMIN USER: vagrant'
    echo '---- SET THE TOMCAT ADMIN PASSWORD: mucis'
    echo '---- UPLOAD AND RUN JSP PROGRAMS AT BROWSER URL OF: localhost:9000'
    sed -i 's/<\\/tomcat-users>/  <user username="vagrant" password="mucis" roles="manager-gui,admin-gui"\\/><\\/tomcat-users>/' /etc/tomcat8/tomcat-users.xml
    service tomcat8 restart
    echo '--'


    echo '-----------------------------------------------------------------------------------------------'
    echo '---- INSTALLING GUI ENVIRONMENT'
    echo '---- BRING UP GUI ENVIRONMENT USING: vagrant rdp'
    echo '---- OR LAUNCH SEPARATE REMOTE DESKTOP FROM WINDOWS OR OS X USING: localhost:7000'
    echo '-----------------------------------------------------------------------------------------------'

    echo '---- INSTALLING REMOTE DESKTOP AND MATE GUI'
    apt-get install -y xrdp
    sed -i 's/.*\/etc\/X11\/Xsession/mate-session/' /etc/xrdp/startwm.sh

    apt-get install -y mate-desktop-environment-core
    apt-get install -y mate-themes ubuntu-mate-wallpapers-utopic ubuntu-mate-wallpapers-vivid
    apt-get install -y fonts-inconsolata fonts-dejavu fonts-droid fonts-liberation fonts-ubuntu-font-family-console
    apt-get install -y xterm vim-gnome gdebi-core pluma 
    apt-get install -y firefox
    wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
    gdebi -n google-chrome-stable_current_amd64.deb
    rm -f google-chrome-stable_current_amd64.deb
    echo '--'

    echo '---- INSTALLING PYTHON3 TK GRAPHICS LIBRARY'
    apt-get install -y python3-tk
    echo '--'


    echo '-----------------------------------------------------------------------------------------------'
    echo '---- INSTALLING GUI IDEs'
    echo '-----------------------------------------------------------------------------------------------'

#    echo '---- INSTALLING C, C++, C# Mono IDE'
#    apt-get install -y monodevelop
#    echo '--'

#    echo '---- INSTALLING SPRING STS / ECLIPSE IDE'
#    wget http://dist.springsource.com/release/STS/3.7.2.RELEASE/dist/e4.5/spring-tool-suite-3.7.2.RELEASE-e4.5.1-linux-gtk-x86_64.tar.gz
#    tar xvzf spring-tool-suite-3.7.2.RELEASE-e4.5.1-linux-gtk-x86_64.tar.gz
#    rm -f spring-tool-suite-3.7.2.RELEASE-e4.5.1-linux-gtk-x86_64.tar.gz
#    echo 'RUN AS: ~/sts-bundle/sts-3.7.2.RELEASE/STS'
#    echo '--'

    echo '---- INSTALLING NETBEANS IDE (as of 20160118 installling older version 8.0.2)'
    apt-get install -y netbeans
    echo '--'

    echo 'Ending shell script at:'
    date

    echo '-----------------------------------------------------------------------------------------------'
    echo '---- AFTER FIRST vagrant up BE SURE TO vagrant halt AND THEN vagrant up AGAIN TO FINISH INSTALL'
    echo '-----------------------------------------------------------------------------------------------'

SHELL
end