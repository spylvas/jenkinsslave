JENKINS_PUBLIC_KEY_FILE =  "jenkinskeys/id_rsa.pub"
HOSTNAME = "jenkins.slave"
HOST_IP = "10.10.10.10"


Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = HOSTNAME
  config.vm.network "private_network", ip: HOST_IP
  
   # Add the user's public key
  if File.exist?(File.expand_path("~/.ssh/id_rsa.pub"))
    public_key = File.read(File.expand_path("~/.ssh/id_rsa.pub"))
    config.vm.provision "shell", inline: "echo '#{public_key}' >> /home/vagrant/.ssh/authorized_keys"
  end
  
  # Add jenkins public key
  if File.exist?(File.expand_path(JENKINS_PUBLIC_KEY_FILE))
    jenkins_public_key = File.read(File.expand_path(JENKINS_PUBLIC_KEY_FILE))
    config.vm.provision "shell", inline: "echo '#{jenkins_public_key}' >> /home/vagrant/.ssh/authorized_keys"
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y docker
    apt-get install -y docker-compose
    sudo usermod -aG docker vagrant
    apt install -y openjdk-8-jdk openjdk-8-jre
    echo 'JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64' >> /etc/environment 
    echo 'JRE_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre' >> /etc/environment
  SHELL
end
