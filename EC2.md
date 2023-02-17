# Criando uma máquina ec2
---

1. Selecione uma AMI do tipo ubuntu
    - Selecione um grupo de segurança que possuí as seguintes portas livres para todos os IP's (IPV6 -> ::/0 | IPV4 -> 0.0.0.0/0)
    - As portas devem ser do protocolo tipo TCP, e acessíveis para endereços IPV6 e IPV4 (cadastre a mesma porta duas vezes, uma para IPV6 e outra IPV4)
    - Portas necessárias:
        - `80`
        - `443`
        - `22`
        - `5472`[Se voce pretender usar um BD rodando pelo docker dentro do container, para que voce consiga acessar esse BD sem acessar a máquina via ssh]
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
    - 5.1: Nessa etapa voce consegue acessar a máquina e ver a mensagem de boas vindas do nginx
            - 5.1.1: acesse no navegador pelo IP elastico vinculado a máquina
            - 5.1.2: também é possivel pingar a máquina pelo próprio terminal, e voce deve receber um html com as boas vindas do nginx
                ````
                    curl http://<IP_elastico>:80
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
