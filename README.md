# CI-CD IN RANCHER 2.X

# CI GITLAB IN DOCKER

Requisitos:
- Docker 
- Versão GitLab - v10.1
- Instalado o Docker-compose,disponíveil [aqui](https://github.com/NaturalHistoryMuseum/scratchpads2/wiki/Install-Docker-and-Docker-Compose-(Centos-7))

Primeiro passo, precisamos de um Runner rodando... 

humm.. o que seria um Runner?
O Runner é uma aplicação que roda separadamente e trabalha junto ao Gitlab CI-CD, executando os deploy e os build das aplicações identificadas. Ou seja, para que você possa efetuar todo workflow de CI/CD é necessária ao menos 1 instância do Gitlab CI/CD e 1 Gitlab Runner rodando em um server, no pc ou docker.

Ultilizamos o docker-compose (Entender sobre Docker-compose [clique aqui](https://www.concrete.com.br/2017/12/11/docker-compose-o-que-e-para-que-serve-o-que-come/)) para subir container do gitlab_CI e gitlab_Runner      
