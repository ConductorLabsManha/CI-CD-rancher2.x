# CI-CD IN RANCHER 2.X

# CI GITLAB IN DOCKER

Requisitos:
- Versão GitLab - v10.1
- Instalado o Docker-compose [a Download aqui](https://github.com/NaturalHistoryMuseum/scratchpads2/wiki/Install-Docker-and-Docker-Compose-(Centos-7))

Primeiro passo, precisamos de um Runner rodando... 

humm.. o que seria um Runner?
O Runner é uma aplicação que roda separadamente e trabalha junto ao Gitlab CI-CD, executando os deploy e os build das aplicações identificadas. Ou seja, para que você possa efetuar todo workflow de CI/CD é necessária ao menos 1 instância do Gitlab CI/CD e 1 Gitlab Runner rodando em um server, no pc ou docker.
