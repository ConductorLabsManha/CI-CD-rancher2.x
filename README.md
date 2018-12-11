# CI-CD IN RANCHER 2.X

# CI GITLAB

Requisitos:
- [Docker](https://docs.docker.com/install/)
- Versão GitLab - v10.1.4
- Instalado o [Docker-compose](https://github.com/NaturalHistoryMuseum/scratchpads2/wiki/Install-Docker-and-Docker-Compose-(Centos-7))
- Versão Runner - v10.1.0

Crie uma pasta e dentro dela adicione um arquivo que será nosso docker-compose (Entender sobre Docker-compose [aqui](https://www.concrete.com.br/2017/12/11/docker-compose-o-que-e-para-que-serve-o-que-come/)) para subir o container do gitlab_CI e gitlab_Runner.

precisamos de um **Runner** rodando. humm.. o que seria um Runner?
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

  
  2. Agora que já acessamos nosso Gitlab, precisamos configurar o Gitlab Runner
  
                         imagem aqui
  
  3. Não temos nenhum runner configurado ainda, vamos configurar por comandos via docker
  
    docker exec -i -t Gitlab_Runner sudo gitlab-runner register
  
