# Criando uma máquina ec2
---

1. Selecione uma AMI do tipo ubuntu
2. Baixe o .pem para conectar via ssh no container alocado
3. Crie um IP elástico e o vincule ao container (Lembre-se, só é possivel alocar 5 IP's fixos por região)
4. Acesse a máquina
    ````
    ssh -i "<filename/>.pem" ubuntu@<IP_addres/>
    ````
5. Instale os requisitos necessários na máquina
    ````
    sudo apt update
    sudo apt install git docker docker-compose nginx
    ````
6. Clone o repositório do git
7. Dentro do repositório devem ter os arquivos docker
    - 7.1 Dockerfile: Deve conter instruções de execução e variáveis de ambiente
    ````
    FROM node:12

    # Create app directory
    WORKDIR api
    ADD . /api

    # Install NPM dependencies
    RUN yarn

    # Env
    ENV PORT=3333

    # Start prod version
    CMD ["yarn", "run", "start"]
    ````

    - 7.2 docker-compose [ a api deve estar rodando na portas 3000 ou 3333, obrigatóriamente]: 
    ````
    version: '2.1'
    services:
    api:
        container_name: <container_api_name/>
        ports:
        - 3333:3333
        build:
        dockerfile: Dockerfile
        context: .
        working_dir: /api
        volumes:
        - ./uploads:/api/uploads
        environment:
        - NODE_ENV=production
    ````
8. Suba o container
    ````
        sudo docker-compose -f <docker-compose_filename/>.yml up --build -d
    ````
