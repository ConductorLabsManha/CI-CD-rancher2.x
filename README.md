# CI-CD-rancher2.x

# CI in gitlab

Requisitos 
- Versão GitLab - v10.1
- Instalado o Docker-compose

Primeiro passo, precisamos de um Runner rodando... o que é run ner?

O Runner é uma aplicação que roda separadamente e trabalha junto ao Gitlab CI-CD, executando os deploy e os build das aplicações identificadas. Ou seja, para que você possa efetuar todo workflow de CI/CD é necessária ao menos 1 instância do Gitlab CI/CD e 1 Gitlab Runner rodando em um server, no pc ou docker.
