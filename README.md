# Ansible #
## Visão geral ##
Ansible é uma simples e poderosa ferramenta de automatização criada para gerenciar múltiplas máquinas de uma vez. Além disso, é uma engine que permite executar Ansible Playbooks.
Uma das principais características que faz o Ansible ser atrativo para uso em relação à automação de serviços é a sua linguagem bastante simples, ao ponto de ser “humanamente legível”, isto é, não precisando ter um notável conhecimento técnico para entender o que está sendo feito. Pelo fato da linguagem ser de fácil entendimento, é possível começar a criar serviços de automação de forma fácil e rápida. E para auxiliar ainda mais o entendimento do que está sendo processado, as tasks são executadas na ordem em que são escritas.
Outras duas características interessantes do Ansible são: Ele é simples de começar a utilizar, pois apenas utiliza SSH para se conectar com os servidores e executar as tasks; e o Ansible utiliza do princípio da idempotência, isto é, seus módulos não executarão uma ação que não mudarão o estado do sistema.
Por fim, o Ansible funciona em cima de uma arquitetura sem a presença de agentes (agentlesss) e não necessita de uma infraestrutura de segurança customizada, fazendo com que o processo de automação seja mais eficiente e mais seguro.
Playbook
Playbooks são a forma pelo qual o Ansible consegue configurar uma política ou passos de um processo de configuração. São feitos para serem fáceis de ler e podem realizar desde deploys de máquinas remotas até delegar ações com diferentes hosts através da interação com servers de monitoramento.
 
### Play ###
Um Playbook pode contar várias plays que nada mais são que uma espécie de introdução para as tasks. Isto é, uma play contém várias tasks e define as propriedades que serão utilizadas por elas. Que podem ser nome de hosts, permissões de acesso, portas http. As configurações para as tasks são definidas aqui.

### Task ###
As tasks são onde o trabalho vai ser efetivamente realizado. Elas contém as definições do que será instalado ou qual arquivo será copiado para o servidor que está sendo configurado, por exemplo. As tasks contém módulos, que efetivamente vão realizar o trabalho de automatização.
### Módulo ###
As tasks são o local onde o trabalho ocorrerá, mas quem efetivamente o realiza são os modules. São parecidos com os resources do Chef, onde você pode definir diversas atividades, como iniciar um serviço, alterar aquivos com base em um template e outra infinidade de coisas. Por exemplo, há modulos responsáveis por instalar pacotes (apt), adicionar um repositório via ppa (apt_repository), entre outros.

### Handler ###
São opcionais, sendo estruturas que são ativadas por tasks e são executadas quando são notificadas por uma task. Por exemplo, após a instalação e configuração de um serviço, talvez seja necessário que você o reinicie. Essa é uma das ocasiões em que o uso de um handler é aconselhado.
Uma característica interessante dos handlers, que é fortemente ligada ao princípio da idempotência, é que, caso mais de uma task notifique a execução de um handler,este só será executado uma vez ao fim do bloco de tasks.
 
### Roles: ###
As roles são boas para organizar várias Tarefas relacionadas e encapsular dados necessários para realizar essas Tarefas. Por exemplo, a instalação do Nginx pode envolver a inclusão de um repositório de pacotes, a instalação do pacote e a configuração.
A parte de configuração geralmente requer dados extras, como variáveis, arquivos, tarefas, handlers e muito mais. Essas ferramentas podem ser usadas com o Playbooks, mas podemos melhorar imediatamente organizando tarefas e dados relacionados em uma estrutura coerente: uma role.
As roles têm uma estrutura de diretórios como esta:
role
 - files
 - handlers
 - meta
 - templates
 - tasks
 - vars

Dentro de cada diretório, o Ansible irá procurar e ler qualquer arquivo Yaml chamado main.yml automaticamente.

### Meta ###

O arquivo main.yml dentro do diretório meta contém metadados de função/role, incluindo dependências.
Se essa função dependesse de outra função/role, poderíamos definir isso aqui. Por exemplo, eu tenho a função Nginx que depende da função/role SSL, que instala certificados SSL.

\---

dependencies:
  - { role: ssl }
 
Se eu chamei a role"nginx", ele tentaria primeiro executar o papel "ssl".
Caso contrário, podemos omitir esse arquivo ou definir a função como não tendo dependências:

\---

dependencies: []
 
### Template ###
Os arquivos de templates podem conter variáveis de template, com base no mecanismo de modelos Jinja2 do Python. Os arquivos aqui devem terminar com a extensão .j2, mas podem ter qualquer nome. Semelhante aos arquivos, não encontraremos um arquivo main.yml no diretório de templates.

### Plugins ###
Plugins são pedaços de código que aumentam a funcionalidade principal do Ansible. Ansible vem com uma série de plugins úteis, e você pode facilmente escrever o seu próprio.

### Tags ###
Se você tiver um playbook grande, pode ser útil poder executar uma parte específica da configuração sem executar todo o playbook.

### group_vars ###
Contém um conjunto de variáveis, por exemplo, nome de usuário e senha do banco de dados.
 
### Facts ###
Antes de executar qualquer tarefa, o Ansible coletará informações sobre o sistema que está provisionando. Eles são chamados de Facts e incluem uma ampla variedade de informações do sistema, como o número de núcleos de CPU, redes ipv4 e ipv6 disponíveis, discos montados, distribuição Linux e muito mais.

Os Facts costumam ser úteis em configurações de Tarefas ou Templates.
Por exemplo, o Nginx é comumente configurado para usar como qualquer processador de trabalho, pois há núcleos de CPU. Sabendo disso, você pode escolher configurar seu template do arquivo nginx.conf da seguinte forma:\
user www-data www-data;\
worker_processes {{ ansible_processor_cores }};\
pid /var/run/nginx.pid;
### E outras configurações ... ###
 
Ou se você tiver um servidor com múltiplas CPUs, você pode usar:
 
user www-data www-data;\
worker_processes {{ ansible_processor_cores * ansible_processor_count }};\
pid /var/run/nginx.pid;
### E outras configurações ... ###
 
Todos os facts possíveis começam com ansible_ e estão disponíveis globalmente para uso em qualquer lugar que possa ser usado: files, vars, tasks e templates.
 

### Demonstração: ###
![image](https://user-images.githubusercontent.com/34744444/51430272-f6a30500-1bff-11e9-8d40-a00f99fb4679.png)


OBS: É importante notar que arquivos “yml” são indentados com espaços e nunca com TABS. Vai salvar seu tempo.

## Nota final ##
Todos os meus playbooks serão hospedados neste repositório. 
Começarei com algo básico, como algum playbook que criei para configurar minha maquina pessoal. 

Para executar o playbook é simples. Basta executar a seguinte instrução:
ansible-playbook playbook.yml -i arquivo_hosts -e "distro=ubuntu".
Ou: ansible-playbook playbook.yml -i arquivo_hosts all

Fiquem a vontade para fazer pull requests, criticar, elogiar... Enfim, espero que gostem. 
