Vagrant.configure("2") do |config| 
    config.vm.define "debian" do |debian|
    config.vm.synced_folder "./roles", "/home/vagrant/roles"

        debian.vm.box = "debian/bookworm64" 
        debian.vm.provider "virtualbox" do |virtual| 
            virtual.gui = false
            virtual.name = "debian-track3"
            virtual.memory = 5024
        end
        
        debian.vm.provision "shell", inline: <<-SHELL
            apt-get update -y
            apt install -y curl 
            apt-get install -y apt-transport-https ca-certificates gnupg lsb-release
            curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
            echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
            apt-get update -y
            apt-get install -y docker-ce docker-ce-cli containerd.io
            sudo usermod -aG docker vagrant
            docker pull ealen/echo-server
            apt update && apt install -y podman
            ssh-keygen -t rsa -b 4096 -f /home/vagrant/.ssh/id_rsa_vagrant 
            sudo apt install -y apache2-utils
            sudo apt install -y ansible
        SHELL

        debian.vm.provision "ansible" do |ansible|
           ansible.playbook= "playbook.yaml"
        end

      
    end

end       
 