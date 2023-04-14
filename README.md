# Iniciando com o Ansible

## Instalar o ansible em um host - virtualbox ou outros.
Aqui na raiz do repo vou deixar o Vagrantfile para facilitar a vida, nele consta o nome das maquinas, vms com 1gb de ram(cada)

Crie um diretório para este projeto em uma pasta de sua preferencia, clone o repositório dentro da sua pasta: git clone <REPO>. Em seguida entre na pasta do projeto e verifique se o Vagrantfile está lá.
Se sim, basta começar a festa com o comando:
```console
vagrant up
```
Ele vai subir todas e, tcharam, todas estaram visiveis com o comando:
```console
vagrant status
```
Acessar o host que será o servidor do ansible:
```console
vagrant ssh ansible
```
Instalar o ansible com os seguintes comandos:
```console
yum install epel-release
yum install vim ansible
ansbile --version
```
O diretório padrão do ansible é:
```console
/etc/ansible
```
OBS: nesse diretório que será trabalhado todos os arquivos, step-by-step

Há um arquivo que pode ser definido conforme a necessidade nesse diretório:
```console
ansible.cfg
```
Na raiz do diretório /ansible iremos criar os seguintes diretórios:
```console
mkdir roles
mkdir group_vars
mkdir host_vars
mkdir playbooks
```

Há outro arquivo que será usado no primeiro momento que é o "hosts", o meu ficou definido assim:
```bash
[webapp] // grupo de hosts a serem chamados em uma task do playbook
web ansible_host=192.168.99.11 // nome_do_host + ansible_host=<IP> 

[appdocker] 
app ansible_host=192.168.99.10

[dbapp]
db ansible_host=191.168.99.12
```

Após a criação destes, acesse o diretório /roles e executar os seguintes comandos, para facilitar a estruturação dos diretórios que serão usados. O comando possui a seguinte sintaxe: "ansible-galaxy init nome_aplicação"
```console
ansible-galaxy init nginx
ansible-galaxy init wordpress
ansible-galaxy init mysql
```
Esse é especifico do curso, esse "install" vai buscar um template de um repositório do galaxy na internet: [link do repositório](https://galaxy.ansible.com/alvarobacelar/install_docker)
```console
ansible-galaxy install alvarobacelar.install_docker
```
Depois você pode renomear, sem problemas:
```console
mv alvarobacelar.install_docker install_docker
```
OBS: Pode ser que o diretório vá para o /etc/ então é só executar esse comando estando dentro do diretório /roles
```console
sudo mv /root/.ansible/roles/alvarobacelar.install_docker/ .
```

# Ambiente configurado, hora de #HANDSONTHECODE

Criando as "tasks" do playbook:
```console
vim roles/nginx/tasks/main.yml
```
O conteudo do arquivo está aqui nesse repo, em: [arquivo](https://gitlab.com/labsan1/lab-ansible/-/blob/master/arquivos-ansbile/roles/nginx/tasks/main.yml)

Feito isso temos alguns outros passos, um deles é o de colocar o arquivo "nginx.conf" no servidor para ser substituido e criar um backup do original, como está descrito na task, sendo assim, crie o arquivo dentro do diretório roles/nginx/files.
Crie o arquivo com o comando:
```console
touch nginx.conf && vim nginx.conf
```

O conteudo do arquivo está aqui nesse repo, em: [arquivo](https://gitlab.com/labsan1/lab-ansible/-/blob/master/arquivos-ansbile/roles/nginx/files/nginx.conf)

Definido as tasks, vamos ajustar as variaveis:
```console
vim group_vars/webapp
```
O conteudo do arquivo está aqui nesse repo, em: [arquivo](https://gitlab.com/labsan1/lab-ansible/-/blob/master/arquivos-ansbile/group_vars/webapp)

Feito isso, vamos criar um arquivo de template que vai chamar essas variaveis e aplicar no location do "conf.d" no nosso nginx.
O nome do arquivo deve possui a extensão ".j2" que é uma extensão utilizada pelo ansible para tratar as variaveis dentro do playbook para esse template.
```console
touch roles/nginx/templates/location.j2 && vim roles/nginx/templates/location.j2
```
No caso, o arquivo esta como descrito na task do template do playbook "location.j2".
O conteudo do arquivo está aqui nesse repo, em: [arquivo](https://gitlab.com/labsan1/lab-ansible/-/blob/master/arquivos-ansbile/roles/nginx/templates/location.j2)

Agora vamos criar os handlers para os notifys que foram definidos na task:
```console
vim roles/nginx/handlers/main.yml
```
O conteudo do arquivo está aqui nesse repo, em: [arquivo](https://gitlab.com/labsan1/lab-ansible/-/blob/master/arquivos-ansbile/roles/nginx/handlers/main.yml)

Feito tudo isso, podemos executar nosso playbook tranquilamente para ver a mágica acontecer no servidor "webapp":
```console
ansible-playbook playbooks/install-nginx.yml
```
