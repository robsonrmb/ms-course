=========================================================================================================================================
PASSOS PARA SUBIR A APLICAÇÃO COM O USO DO DOCKER
=========================================================================================================================================

1. CRIAR UMA REDE NO DOCKER
	- docker network create hr-net
	- docker network ls (para listar as redes existentes).
	
-----------------------------------------------------------------------------------------------------------------------------------------	
2. CRIAR UMA IMAGE NO DOCKER

	2.1 BAIXAR A IMAGE DO POSTGRES
	- docker pull postgres:15-alpine
	- docker images (para listar as images existentes).
	
	2.2 CRIAR AS IMAGES DOS MICROSERVICES
	ESTA CRIAÇÃO É FEITA APARTIR DO ARQUIVO DOCKERFILE NA RAIZ DA APLICAÇÃO.
	OU SEJA, DEVE ENTRAR NA PASTA ONDE ESTÁ O DOCKERFILE DE CADA PROJETO E EXECUTAR O COMANDO ABAIXO:
	
	** docker build -t NOME_DA_APLICACAO:NOME_DA_TAG
	
	- docker build -t hr-config-server:v1
	- docker build -t hr-eureka-server:v1
	- docker build -t hr-api-gateway-zuul:v1
	- docker build -t hr-worker:v1
	- docker build -t hr-user:v1
	- docker build -t hr-payroll:v1
	- docker build -t hr-oauth:v1
	
-----------------------------------------------------------------------------------------------------------------------------------------
3. SUBIR OS CONTAINERS DE MICROSERVICES NO DOCKER
   TODO O ITEM 3 ESTÁ AUTOMATIZADO ATRAVÉS DO DOCKER-COMPOSE LISTADO NO ITEM 4.

	3.1 RODANDO OS CONTAINERS DE BANCO DE DADOS - POSTGRES
	- docker run -p 5432:5432 --name hr-worker-pg15 --network hr-net -e POSTGRES_PASSWORD=1234567 -e POSTGRES_DB=db_hr_worker postgres:15-alpine
	- docker run -p 5433:5432 --name hr-user-pg15 --network hr-net -e POSTGRES_PASSWORD=1234567 -e POSTGRES_DB=db_hr_user postgres:15-alpine
	
	3.2 RODANDO OS CONTAINERS DOS MICROSERVIÇOS - JAVA/SPRINGBOOT
	- docker run -p 8888:8888 --name hr-config-server --network hr-net -e GITHUB_USER=robsonrmb -e GITHUB_PASS= hr-config-server:v1
	- docker run -p 8761:8761 --name hr-eureka-server --network hr-net hr-eureka-server:v1
	- docker run -p 8765:8765 --name hr-app-gateway-zuul --network hr-net hr-app-gateway-zuul:v1
	- docker run -P --network hr-net hr-worker:v1
	- docker run -P --network hr-net hr-user:v1
	- docker run -P --network hr-net hr-payroll:v1
	- docker run -P --network hr-net hr-oauth:v1
	
-----------------------------------------------------------------------------------------------------------------------------------------
4. SUBIR CONTAINERS APARTIR DO DOCKER-COMPOSE

	4.1 EXECUTANDO O DOCKER COMPOSE 
	O comando abaixo inicializa todos os containers e deve ser rodado pelo terminal na pasta onde se encontra o arquivo docker-compose.yml.
	- docker compose up
