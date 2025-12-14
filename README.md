Projeto-01 — DevOps com Vagrant e Ansible

Projeto de Administração de Sistemas Abertos utilizando Vagrant e Ansible

Equipe: João Victor Coelho Trigueiro e Pedro Henrique Cardoso Teixeira de Paula

🧠 Sobre o Projeto

Este projeto tem como objetivo demonstrar a automação de infraestrutura usando Vagrant para criar ambientes virtuais e Ansible para configurar e administrar serviços nas máquinas provisionadas.

Com ele, você pode criar um ambiente reproduzível de servidores sem intervenção manual, exemplificando conceitos de Infraestrutura como Código (IaC) e automação, que são pilares da cultura DevOps.

📁 Estrutura do Repositório


📦Projeto-01---DevOps-com-Vagrant-e-Ansible

ansible/   
Vagrantfile     
README.md

                      
🛠️ Ferramentas Utilizadas

Vagrant - Gerencia ambientes de máquinas virtuais portáteis

VirtualBox - Provider padrão para executar Vagrant

Ansible - Provisionamento e configuração automatizada


🚀 Pré-requisitos

Antes de começar, você precisa ter instalado:
Vagrant

VirtualBox (ou outro provider compatível com Vagrant)

Ansible

Após instalar essas ferramentas, você conseguirá rodar todo o projeto localmente.


🧪 Como Executar

1. Clone o repositório

git clone https://github.com/joao-victor-ct/Projeto-01---DevOps-com-Vagrant-e-Ansible.git

cd Projeto-01---DevOps-com-Vagrant-e-Ansible

2. Suba as máquinas com Vagrant

vagrant up

3. Provisionar com Ansible

O provisionamento pode ser executado automaticamente pelo Vagrant (quando configurado no Vagrantfile), ou manualmente:

ansible-playbook -i ansible/hosts ansible/playbook.yml

4. Acessar as VMs

vagrant ssh <nome_da_vm>

📌 O que o Ansible faz aqui

O Ansible foi utilizado para:

Instalar pacotes e dependências dentro das VMs

Configurar serviços

Aplicar definições de sistema automaticamente

Garantir que o estado final da VM esteja conforme o desejado

Isso demonstra como é possível fazer provisionamento declarativo e automatizado de infraestrutura com poucos comandos.


📝 Boas práticas

🔹 Use vagrant provision sempre que alterar os playbooks ou as definições de configuração.

🔹 Verifique os logs de Ansible para debug com -vvv (verbose).

🔹 Mantenha seus playbooks idempotentes (sem afetar estado repetidas vezes).

👥 Autores

João Victor Coelho Trigueiro

Pedro Henrique Cardoso Teixeira de Paula

📄 Licença

Este projeto é aberto e pode ser utilizado livremente para estudo e demonstração de conceitos de DevOps.

Sinta-se livre para adaptar para seus próprios estudos/projetos.
