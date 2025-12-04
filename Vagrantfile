# -*- mode: ruby -*-
# vi: set ft=ruby :

# ===================================================================
# INFORMAÇÕES DOS INTEGRANTES
# ===================================================================
NOME1 = "pedro"
NOME2 = "joao"
MATRICULA_XX = "30"
MATRICULA_YY = "13"
MATRICULA_XY_APP = "30"

Vagrant.configure("2") do |config|


  # CONFIGURAÇÕES GLOBAIS

  config.vm.box = "debian/bookworm64"
  config.ssh.insert_key = false

  config.vm.provider "virtualbox" do |vb|
    vb.linked_clone = true
  end


  # VM ARQ

  config.vm.define "arq" do |arq|
    arq.vm.hostname = "arq.#{NOME1}.#{NOME2}.devops"

    arq.vm.provider "virtualbox" do |vb|
      vb.name = "arq"
      vb.memory = 512

      ["disk1.vdi", "disk2.vdi", "disk3.vdi"].each_with_index do |disk, i|
        unless File.exist?(disk)
          vb.customize ['createhd', '--filename', disk, '--size', 10240]
        end

        vb.customize [
          'storageattach', :id,
          '--storagectl', 'SATA Controller',
          '--port', i + 1,
          '--device', 0,
          '--type', 'hdd',
          '--medium', disk
        ]
      end
    end
  end


  # VM DB

  config.vm.define "db" do |db|
    db.vm.hostname = "db.#{NOME1}.#{NOME2}.devops"

    db.vm.provider "virtualbox" do |vb|
      vb.name = "db"
      vb.memory = 512
    end
  end


  # VM APP

  config.vm.define "app" do |app|
    app.vm.hostname = "app.#{NOME1}.#{NOME2}.devops"

    app.vm.provider "virtualbox" do |vb|
      vb.name = "app"
      vb.memory = 512
    end
  end


  # VM CLI

  config.vm.define "cli" do |cli|
    cli.vm.hostname = "cli.#{NOME1}.#{NOME2}.devops"

    cli.vm.provider "virtualbox" do |vb|
      vb.name = "cli"
      vb.memory = 1024
    end
  end

  
  # PROVISIONAMENTO COM ANSIBLE
  
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/playbook-all.yml"
    ansible.groups = {
      "arq" => ["arq"],
      "db"  => ["db"],
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
