# CI-CD IN RANCHER 2.X

# CI GITLAB

Requisitos:
- [Docker](https://docs.docker.com/install/)
- Versão GitLab - v10.1.4
- Instalado o [Docker-compose](https://github.com/NaturalHistoryMuseum/scratchpads2/wiki/Install-Docker-and-Docker-Compose-(Centos-7))
- Versão Runner - v10.1.0

Crie uma pasta e dentro dela adicione um arquivo que será nosso docker-compose (Entender sobre Docker-compose [aqui](https://www.concrete.com.br/2017/12/11/docker-compose-o-que-e-para-que-serve-o-que-come/)) para subir o container do gitlab_CI e gitlab_Runner.

Precisamos de um **Runner** rodando. humm.. o que seria um Runner?
O Runner é uma aplicação que roda separadamente e trabalha junto ao Gitlab CI-CD, executando os deploy e os build das aplicações identificadas. Ou seja, para que você possa efetuar todo workflow de CI/CD é necessária ao menos 1 instância do Gitlab CI/CD e 1 Gitlab Runner rodando em um server, no pc ou docker.

> docker-compose.yml

```
version: '3'
services:
 Gitlab_CI:
  container_name: Gitlab_CI
  image: 'gitlab/gitlab-ce:10.1.4-ce.0'
  networks: 
   - 'DockerLAN'
  restart: always
  hostname: 'gitlab.docker'
  ports:
   - '80:80'
  volumes:
   - '/srv/gitlab/config:/etc/gitlab'
   - '/srv/gitlab/logs:/var/log/gitlab'
   - '/srv/gitlab/data:/var/opt/gitlab'
 Gitlab_Runner:
  container_name: Gitlab_Runner
  image: 'gitlab/gitlab-runner:v10.1.0'
  networks:
   - 'DockerLAN'
  restart: always
  volumes:
   - '/var/run/docker.sock:/var/run/docker.sock'
   - '/srv/gitlab-runner/config:/etc/gitlab-runner'
networks:
 DockerLAN:
  driver: bridge
  ````
  Depois de criado, ainda na pasta digite esse comando
  
    docker-compose up -d
  
  **Acesso ao gitlab:**
  1. Com o IP público da MV para acessar o gitlab apontando para porta 80. No primeiro acesso ele vai solicitar a **nova senha** para o usuário “root”.
  
  ![image](https://user-images.githubusercontent.com/45598049/49802444-ac3e8680-fd2b-11e8-8486-92af5b9b261d.png)

  
  2. Agora que já acessamos nosso Gitlab, precisamos configurar o Gitlab Runner, clique em admin
  
  ![image](https://user-images.githubusercontent.com/45598049/49811259-a94e9080-fd41-11e8-932e-52f07f4ca246.png)
  
    Depois vá em Runner, vamos configurar nosso Runner. 
   
   ![image](https://user-images.githubusercontent.com/45598049/49811432-05b1b000-fd42-11e8-9a2b-1f5397ff8a67.png)
   
    Vejamos que temos nenhum Runner configurado, certo?
   
   ![image](https://user-images.githubusercontent.com/45598049/49811654-7789f980-fd42-11e8-90c5-7fbe9a295864.png)
    
   3. Com comandos docker, abra o terminal e cole esse comando, irá regristar nosso primeiro runner :) 
    
    1. docker exec -i -t Gitlab_Runner sudo gitlab-runner register
    
   - Primeiro passo, é informar a URL do Gitlab
   - Prómixo passo, é informar o Token que tambem esta na descrição do Gitlab
   - Depois, irá solicitar a tag para o Runner que estamos configurando. como por exemplo: **runner01**. Essa Tag que você        vai utilizar nos jobs de suas pipelines para que eles funcionem corretamente. Pode usar a opção para que elas funcionem      sem ela, mas vamos utilizá-la, já que é um processo mais detalhado e é bem legal conhecer.
   - Agora, vamos configurar para que os jobs sem Tags não sejam executados e deixar o runner compartilhado, assim todos           os projetos que tiverem com a Tag runner01 configurada serão executados.
   - Para concluir o registro, vamos indicar qual executor do Runner vamos usar e a imagem padrão a ser utilizada. Neste           caso vamos usar o executor **Docker** e uma imagem do **alpine:3.5**.
       
   ![image](https://user-images.githubusercontent.com/45598049/49935194-2b0ffc80-feaf-11e8-9e96-9702eb5facd3.png)
     
   Pronto, temos nosso runner configurado !! uuh uuh
   
   # CD RANCHER
   
   O Rancher uma tecnologia de software livre que fornece uma plataforma de gerenciamento de contêiner criada para              organizações que implantam contêineres na produção, no Rancher 2.x facilita a execução do Kubernetes em todos os lugares.
   
   Requisitos:
   - Docker
   - Hardware 4GB
    
   1. Em outra maquina virtual (MV), instale com o docker.
   Obs: utilizamos outra MV para utlizar o rancher por motivo de memória.
     
   2. Agora, faça a instalação do Rancher 2.x 
   
    docker run -d --restart=unless-stopped -p 80:80 -p 443:443 rancher/rancher
  
 
   
  
