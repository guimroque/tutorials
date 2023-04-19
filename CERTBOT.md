# Instalando o certificado no ec2 container
---

1. Acesse o route53 na aws
    - 1.1: Se já tiver um domínio configurado
        - 1.1.1: Adicione um novo record dentro do domínio já configurado
        - 1.1.2: Esse novo record deve ser do tipo A, e como valor deve ter o <IP_elastico/> vinculado ao container
        - 1.1.3: Salve e acesse pelo navegador o endereço do record que acabou de ser criado
    - 1.2: Se precisar configurar um novo domínio
        - 1.2.1: Se o seu domínio foi comprado fora da aws, voce precisa criar um novo domínio no route53
        - 1.2.2: Ao criar o record, serão gerados 4 records do tipo NS, você precisa SUBSTITUIR onde comprou o domínio os novos endereços de DNS
        - 1.2.3: Para gerar um certificado, acesse o serviço Certificate Manager e solicite um novo certificado, será gerado um registro do tipo CNAME que voce deve adicionar como record no domínio pretendido do route53 para a aws validar o certificado solicitado (Recomendo que solicite um certificado coringa do tipo ` *.domain.com`, para que voce possa usar o mesmo certificado mais de uma vez).
        - 1.2.4: Siga as etapas 1.1 desse tutorial
2. Acesse sua máquina via ssh
    - 2.1: Instale o certbot: https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal 
        - 2.1.1 Até o comando `sudo certbot --nginx`
        - 2.1.2 Verifique a renovação `sudo certbot renew --dry-run`
    - 2.2: Execute
        ````
        sudo apt install python3-certbot-nginx
        ````
3. Configure nginx:
    - 3.1: Abra o arquivo `/etc/nginx/sites-enabled/default`
        ````
        sudo vim /etc/nginx/sites-enabled/default
        ````
    - 3.2: Comente as linas:
        ````
        # index index.html index.htm index.nginx-debian.html;
        ````
        ```
        #location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                #try_files $uri $uri/ =404;
        #}
        ```
    - 3.3: Adicione um novo bloco location, apontando a key `proxy_pass` para a porta que sua api está executando pelo docker
        ```
        location / {
                proxy_set_header        Host $host;
                proxy_set_header        X-Real-IP $remote_addr;
                proxy_set_header        X-Forwarded-For   $proxy_add_x_forwarded_for;
                proxy_set_header        X-Forwarded-Proto $scheme;
                proxy_intercept_errors  on;
                # The port needs to be equal the api port on the machine
                proxy_pass              http://localhost:3000;
        }
        ```
4. Reinicie a máquina
    ````
    sudo reboot 0
    ````




