# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define :apollo do |apollo|
    apollo.vm.box = "centos65"
    apollo.vm.box_url="https://github.com/2creatives/vagrant-centos/releases/download/v6.5.3/centos65-x86_64-20140116.box"
    apollo.vm.provider :virtualbox do |vb|
      apollo.vm.network :private_network, ip: "192.168.10.100"
      vb.customize ["modifyvm", :id, "--memory", "512"]
    end
  end

  config.vm.define :apollo_aws do |apollo_aws|
    ssh_username = "root"
    aws_access_key = ENV['AWS_ACCESS_KEY_ID']
    aws_secret_key = ENV['AWS_SECRET_ACCESS_KEY']
    aws_keypair_name = ENV['AWS_KEYPAIR_NAME']
    private_key_path = ENV['AWS_PRIVATE_KEY_PATH']
    apollo_aws.vm.box = "aws"
    apollo_aws.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"

    apollo_aws.vm.provider :aws do |aws, override|
      aws.tags = {
        'Name' => 'APOLLO'
      }
      aws.region = 'ap-northeast-1'
      aws.instance_type = 'm1.small'
      aws.ami = 'ami-05197104'
      aws.security_groups = ['apollo']

      aws.access_key_id = aws_access_key
      aws.secret_access_key = aws_secret_key
      aws.keypair_name = aws_keypair_name
      override.ssh.username = ssh_username
      override.ssh.private_key_path = private_key_path
    end
  end

  config.vm.provision :shell, :inline => <<-EOT
    rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
    yum -y upgrade

    yum -y install wget
    yum -y install java-1.7.0-openjdk

    if [ ! -e /usr/local/apollo.tar.gz ]; then
      wget 'http://repository.apache.org/service/local/artifact/maven/redirect?r=snapshots&g=org.apache.activemq&a=apache-apollo&v=99-trunk-SNAPSHOT&e=tar.gz&c=unix-distro' -O /usr/local/apollo.tar.gz
    fi
    if [ ! -e /usr/local/apache-apollo ]; then
      tar xvfz /usr/local/apollo.tar.gz -C /usr/local
      ln -s /usr/local/apache-apollo-99-trunk-SNAPSHOT /usr/local/apache-apollo
    fi
    if [ ! -e /var/lib/apollo ]; then
      /usr/local/apache-apollo/bin/apollo create /var/lib/apollo
    fi
    sed -i.bak 's/127.0.0.1/0.0.0.0/g' /var/lib/apollo/etc/apollo.xml
    if [ ! -e /etc/init.d/apollo-broker-service ]; then
      ln -s "/var/lib/apollo/bin/apollo-broker-service" /etc/init.d/
      chkconfig --add /etc/init.d/apollo-broker-service
    fi
    /etc/init.d/apollo-broker-service start

    yum -y install nodejs npm
    npm install mqtt
  EOT
end
