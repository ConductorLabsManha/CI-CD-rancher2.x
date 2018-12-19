# Provisionando máquinas AWS utilizando o Terraform e Ansible

Requisitos:
- [Terraform](https://learn.hashicorp.com/terraform/getting-started/install)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- AWS EC2 Account

Nessa Documentação vamos provisionar duas maquinas utilizando o AWS EC2 (https://aws.amazon.com/pt/ec2/?nc2=h_m1), uma contendo o Rancher, e outra com o Gitlab, de forma que um comando tudo seja provisionado automaticamente, utilizando os scripts do Terraform e do ansible.

Antes de tudo, faça o download do nosso repositório aqui (https://github.com/ConductorLabsManha/Terraform-Ansible.git), onde vai conter todos os scripts que vão provisionar as máquinas.

Depois de baixar o repositório, você deve criar as chaves de permissão para a sua conta na AWS EC2 e sua private key, onde você deverá mover sua private key para TerrAsible -> utils -> ssh dentro do repositório.

Abra o arquivo creator.tf dentro da pasta do Terraform, e modifique os campos marcados com os dados da sua conta, tipo de maquina selecionada, sua imagem e o nome da sua private key.

>creator.tf

![desktop screenshot 2018 12 15 - 16 50 17 58](https://user-images.githubusercontent.com/45598049/50046911-46832f00-008a-11e9-9696-98517828711a.png)

Com tudo ajustado, execute o comando:
   
    terraform apply
 
Depois, execute o comando para iniciar o processo de criação das maquinas no terraform.

    terraform init
    
O resultado será este:

![desktop screenshot 2018 12 15 - 17 06 27 85](https://user-images.githubusercontent.com/45598049/50046997-fad18500-008b-11e9-9b9f-4c09012abab5.png)

### Adaptando os Scripts do Ansible

Dentro do repositório, vai estar os scripts do Ansible, que executar qualquer comando que você desejar nas maquinas, basta criar o seu arquivo YML e salva-lo dentro da pasta do Ansible.

Porém se você necessitar executar o ansible junto com o terraform, você precisa renomear o seu arquivo novo do Ansible com algum número na frente de sua descrição, pois, possuimos um script em python que vai chamar seus playbooks de acordo com o número na sua descrição.
>run_playbooks.py

```
import os

playbooks =  os.listdir('../ansible')
playbooks.sort()

for playbook in playbooks:
  playml = playbook.split('.')
  catanything = playbook.split('-')
  
  
  if playml[1] == 'yml':
    if catanything[0] != '0':
      os.system('ansible-playbook -i ~/.ansible-config/hosts ../ansible/' + playbook)
```
Ainda podemos chamar os playbooks do Ansible sem precisar do Terraform, basta executar o comando:

    ansible-playbook -i <seu_host> <seu_playbook.yml>

Com isso podemos provisionar maquinas de forma rápida e totalmente automatizada.
