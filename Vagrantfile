# -*- mode: ruby -*-
# vi: set ft=ruby :

NOME1 = "pedro"
NOME2 = "joao"
MATRICULA_XX = "30" #(20222380030)
MATRICULA_YY = "13" #(20241380013)
# Para o MAC da VM 'app', usaremos a matrícula do primeiro integrante.
MATRICULA_XY_APP = "30" 

# Início da configuração do Vagrant
Vagrant.configure("2") do |config|

 # CONFIGURAÇÕES GLOBAIS PARA TODAS AS VMS

  # Define a imagem base para todas as VMs
  config.vm.box = "debian/bookworm64"

  # Desativa a inserção automática de chave SSH do Vagrant, pois o Ansible cuidará disso
  config.ssh.insert_key = false

  # Configurações específicas do provedor VirtualBox
  config.vm.provider "virtualbox" do |vb|
    # Usa "linked clones" para economizar espaço em disco
    vb.linked_clone = true
    # Desabilita a verificação dos "Guest Additions", conforme solicitado
    vb.customize ["modifyvm", :id, "--guestadditionsmode", "disabled"]
  end

  # Gatilho para desabilitar o servidor DHCP padrão do VirtualBox na rede vboxnet0
  # Isso garante que nosso servidor DHCP no 'arq' será o único a operar.
  config.trigger.before :up do |trigger|
    trigger.info = "Desabilitando o servidor DHCP do VirtualBox..."
    trigger.run_remote = {inline: "VBoxManage dhcpserver remove --netname HostInterfaceNetworking-vboxnet0 || true"}
  end

  # DEFINIÇÃO DAS MÁQUINAS VIRTUAIS
  
  # 1. Servidor de Arquivos (arq)
  config.vm.define "arq" do |arq|
    arq.vm.hostname = "arq.#{NOME1}.#{NOME2}.devops"
    arq.vm.network "private_network", ip: "192.168.56.1#{MATRICULA_XX}"
    
    arq.vm.provider "virtualbox" do |vb|
      vb.name = "arq"
      vb.memory = 512
      # Cria e anexa 3 discos de 10GB se eles não existirem
      ["disk1.vdi", "disk2.vdi", "disk3.vdi"].each_with_index do |disk, i|
        unless File.exist?(disk)
          vb.customize ['createhd', '--filename', disk, '--size', 10 * 1024]
        end
        vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', i + 1, '--device', 0, '--type', 'hdd', '--medium', disk]
      end
    end
  end

  # 2. Servidor de Banco de Dados (db)
  config.vm.define "db" do |db|
    db.vm.hostname = "db.#{NOME1}.#{NOME2}.devops"
    # IP via DHCP com MAC estático para receber o IP 192.168.56.113 do nosso servidor DHCP
    db.vm.network "private_network", type: "dhcp", mac: "0800270001#{MATRICULA_YY}"
    
    db.vm.provider "virtualbox" do |vb|
      vb.name = "db"
      vb.memory = 512
    end
  end

  # 3. Servidor de Aplicação (app)
  config.vm.define "app" do |app|
    app.vm.hostname = "app.#{NOME1}.#{NOME2}.devops"
    # IP via DHCP com MAC estático para receber o IP 192.168.56.130 do nosso servidor DHCP
    app.vm.network "private_network", type: "dhcp", mac: "0800270002#{MATRICULA_XY_APP}"
    
    app.vm.provider "virtualbox" do |vb|
      vb.name = "app"
      vb.memory = 512
    end
  end

  # 4. Host Cliente (cli)
  config.vm.define "cli" do |cli|
    cli.vm.hostname = "cli.#{NOME1}.#{NOME2}.devops"
    # IP via DHCP, vai pegar um IP da faixa 192.168.56.50-100
    cli.vm.network "private_network", type: "dhcp"
    
    cli.vm.provider "virtualbox" do |vb|
      vb.name = "cli"
      vb.memory = 1024
    end
  end

  # PROVISIONAMENTO COM ANSIBLE
  # Esta seção diz ao Vagrant para executar os playbooks do Ansible após criar as VMs.
  # O Vagrant gera automaticamente um inventário para o Ansible usar.
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/playbook-all.yml" # Executa em todas as VMs
    ansible.groups = {
        "arq" => ["arq"],
        "db" => ["db"],
        "app" => ["app"],
        "cli" => ["cli"],
        "devops:children" => ["arq", "db", "app", "cli"]
    }
    ansible.extra_vars = {
        nome1: NOME1,
        nome2: NOME2,
        matricula_db_yy: MATRICULA_YY,
        matricula_app_xy: MATRICULA_XY_APP
    }
  end
end

