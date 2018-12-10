# CI-CD IN RANCHER 2.X

# CI GITLAB

Requisitos:
- Docker 
- Versão GitLab - v10.1
- Instalado o Docker-compose,disponíveil [aqui](https://github.com/NaturalHistoryMuseum/scratchpads2/wiki/Install-Docker-and-Docker-Compose-(Centos-7))

Primeiro passo, precisamos de um Runner rodando... 

humm.. o que seria um Runner?
O Runner é uma aplicação que roda separadamente e trabalha junto ao Gitlab CI-CD, executando os deploy e os build das aplicações identificadas. Ou seja, para que você possa efetuar todo workflow de CI/CD é necessária ao menos 1 instância do Gitlab CI/CD e 1 Gitlab Runner rodando em um server, no pc ou docker.

Ultilizamos o docker-compose (Entender sobre Docker-compose clique [aqui](https://www.concrete.com.br/2017/12/11/docker-compose-o-que-e-para-que-serve-o-que-come/)) para subir o container do gitlab_CI e gitlab_Runner. a baixo nosso docker-compose.yml

```version: '3'
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
  > docker-compose up -d
