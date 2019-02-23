####

Crie um arquivo com o nome do ambiente dentro do diretório onde se encontra o playbook.
Exemplo de nome do arquivo: dev
Dentro do arquivo dev, adicione o seguinte conteúdo:

[loadbalancer]
lb01

[webserver]
app01
app02

[database]
db01

[control]
control ansible_connection=local


Depois crie no mesmo diretório um arquivo chamado ansible.cfg e adicione o seguinte conteúdo:
[defaults]
inventory = ./dev

####

Depois dos procedimentos acima, você poderá rodar comandos mais simplificados dentro do diretório criado.
Exemplo:
$ ansible --list-hosts all
$ ansible --list-hosts "*"
$ ansible -m ping database:control
$ ansible -m ping webserver[0]
$ ansible -m ping app0*
$ ansible --list-hosts  \!webserver[0]
$ ansible --list-hosts  \!webserver
$ ansible -m command -a "date" webserver
$ ansible-playbook hostname.yml -e "env=webserver[0]" # Somente quando usar variável no hosts
$ ansible-playbook playbooks/full.yml --limit webserver

##### Para criptofrar variaveis no ansible, use ansible-vault. #####
$ ansible-vault create vault

##### Add a seguinte linha no arquivo: vault_db_password: alyson #####
##### A variável vault_db_password será criptofada e receberá o valor "alyson" #####

##### Para editar suas variáveis criptofadas #####
$ ansible-vault edit vault

##### Para executar o playbook com ansible-vault #####
$ ansible-playbook playbook.yml --ask-vault-pass

##### O ansible pergunta com (Y/N/C) quais tarefas você deseja executar #####
$ ansible-playbook playbook.yml --step

##### Listando tags #####
ansible-playbook playbooks/full.yml --list-tags

##### Executa a task pela descrição. #####
# OBS: O Ansible irá executar a task daí pra frente.
ansible-playbook playbooks/full.yml --start-at-task "Copiar demo.wsgi"

##### Checando syntax do seu playbook #####
ansible-playbook playbooks/full.yml --syntax-check