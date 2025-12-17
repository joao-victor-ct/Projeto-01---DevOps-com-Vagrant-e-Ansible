# -*- mode: ruby -*-
# vi: set ft=ruby :
# ===================================================================
# INFORMAÇÕES DOS INTEGRANTES
# ===================================================================
# Definindo os nomes e matrículas dos integrantes para personalização das VMs
NOME1 = "pedro"           # Nome do primeiro integrante
NOME2 = "joao"            # Nome do segundo integrante
MATRICULA_XX = "30"       # Matrícula do primeiro integrante
MATRICULA_YY = "13"       # Matrícula do segundo integrante
MATRICULA_XY_APP = "31"   # Matrícula associada à VM 'app'

Vagrant.configure("2") do |config|

  # ===================================================================
  # CONFIGURAÇÕES GLOBAIS
  # ===================================================================
  # Definindo a caixa base a ser utilizada pelas VMs
  config.vm.box = "debian/bookworm64"  # Usando uma imagem Debian Bookworm 64-bit
  config.vbguest.auto_update = false   # desabilita atualização automática 
  config.ssh.insert_key = false        # Desativando a inserção automática de chave SSH

  # Configuração do VirtualBox para todas as VMs
  config.vm.provider "virtualbox" do |vb|
    vb.linked_clone = true             # Utilizando clones vinculados para economizar espaço
  end

  # ===================================================================
  # CONFIGURAÇÃO DA VM ARQ
  # ===================================================================
  # Definindo a configuração específica para a VM chamada 'arq'
  config.vm.define "arq" do |arq|
    arq.vm.hostname = "arq.#{NOME1}.#{NOME2}.devops"  # Definindo o nome do host da VM com base nos nomes dos integrantes

    # Configuração do VirtualBox para a VM 'arq'
    arq.vm.provider "virtualbox" do |vb|
      vb.name = "arq"      # Nome da máquina virtual
      vb.memory = 512      # Memória alocada para a VM (512MB)

      # Adicionando discos rígidos à VM 'arq', caso não existam
      ["disk1.vdi", "disk2.vdi", "disk3.vdi"].each_with_index do |disk, i|
        unless File.exist?(disk)
          vb.customize ['createhd', '--filename', disk, '--size', 10240]  # Criação de disco virtual de 10GB
        end

        # Associando os discos à VM
        vb.customize [
          'storageattach', :id,
          '--storagectl', 'SATA Controller',  # Controlador de armazenamento
          '--port', i + 1,                   # Porta de conexão do disco
          '--device', 0,                     # Dispositivo de armazenamento
          '--type', 'hdd',                   # Tipo de armazenamento (HDD)
          '--medium', disk                   # Caminho do disco
        ]
      end
    end
  end

  # ===================================================================
  # CONFIGURAÇÃO DA VM DB
  # ===================================================================
  # Definindo a configuração específica para a VM chamada 'db'
  config.vm.define "db" do |db|
    db.vm.hostname = "db.#{NOME1}.#{NOME2}.devops"  # Definindo o nome do host da VM

    # Configuração do VirtualBox para a VM 'db'
    db.vm.provider "virtualbox" do |vb|
      vb.name = "db"        # Nome da máquina virtual
      vb.memory = 512       # Memória alocada para a VM (512MB)
    end
  end

  # ===================================================================
  # CONFIGURAÇÃO DA VM APP
  # ===================================================================
  # Definindo a configuração específica para a VM chamada 'app'
  config.vm.define "app" do |app|
    app.vm.hostname = "app.#{NOME1}.#{NOME2}.devops"  # Definindo o nome do host da VM

    # Configuração do VirtualBox para a VM 'app'
    app.vm.provider "virtualbox" do |vb|
      vb.name = "app"        # Nome da máquina virtual
      vb.memory = 512       # Memória alocada para a VM (512MB)
    end
  end

  # ===================================================================
  # CONFIGURAÇÃO DA VM CLI
  # ===================================================================
  # Definindo a configuração específica para a VM chamada 'cli'
  config.vm.define "cli" do |cli|
    cli.vm.hostname = "cli.#{NOME1}.#{NOME2}.devops"  # Definindo o nome do host da VM

    # Configuração do VirtualBox para a VM 'cli'
    cli.vm.provider "virtualbox" do |vb|
      vb.name = "cli"        # Nome da máquina virtual
      vb.memory = 1024      # Memória alocada para a VM (1024MB)
    end
  end

  
  # ===================================================================
  # PROVISIONAMENTO COM ANSIBLE
  # ===================================================================
  # Definindo o provisionamento das VMs utilizando o Ansible
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/playbook-all.yml"  # Caminho para o playbook Ansible a ser utilizado
    ansible.groups = {
      "arq" => ["arq"],                             # Grupo 'arq' com a VM 'arq'
      "db"  => ["db"],                              # Grupo 'db' com a VM 'db'
      "app" => ["app"],                             # Grupo 'app' com a VM 'app'
      "cli" => ["cli"],                             # Grupo 'cli' com a VM 'cli'
      "devops:children" => ["arq", "db", "app", "cli"] # Grupo 'devops' contendo todas as VMs
    }

    # Definindo variáveis extras que serão passadas ao Ansible
    ansible.extra_vars = {
      nome1: NOME1,                                 # Variável 'nome1' com o valor de NOME1
      nome2: NOME2,                                 # Variável 'nome2' com o valor de NOME2
      matricula_db_yy: MATRICULA_YY,               # Matrícula associada à VM 'db'
      matricula_app_xy: MATRICULA_XY_APP           # Matrícula associada à VM 'app'
    }
  end

end
