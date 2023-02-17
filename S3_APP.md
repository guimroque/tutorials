# Criando um front pelo S3 com certificado SSH
---

1. Crie um bucket pelo s3 [Sugestão de nome: `<sistema/> - ui`]
    - Selecione a região pretendida [Lembre-se que o cloudfront precisa estar na mesma localizaçao]
    - Habilite a opção ACL's -> Object Writer [útil para automatizarmos o processo de deploy pelo gitActions]
    - Remova o bloqueio ao acesso público
    - Confirme
2. Altere as permissões de acesso do bucket
    - <bucket/> -> permissions
    ````
    {
        "Version": "2008-10-17",
        "Id": "Policy1380877762691",
        "Statement": [
            {
                "Sid": "Stmt1380877761162",
                "Effect": "Allow",
                "Principal": {
                    "AWS": "*"
                },
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::<bucket/>/*"
            }
        ]
    }
    ````
3. Acesse o serviço Cloudfront e crie uma nova distribuição
    - O cloudfront funciona como um delivery dos arquivos do s3 (controla quais e quando os arquivos são entregues aos usuários pelo navegador)
    - Adicione a origem dos arquivos [seu bucket recém criado]
    - domínio CNAME [ endereço pretendido para seu sistema, o endereço precisa ser compativel ao seu certificado ]
    - anexe o certificado curinga
    - Adicione a prop `Default root object`o valor `index.html`
    - Confirme

4. Acesse o cloudfront que acabou de ser criado para terminar de configurar
    - Adicione `error pages` para os HTTPS 400, 403 e 404
        - Costomize resposta de erro [`true`]
        - Page response [`/index.html`]
        - HTTP response code [200:OK]

5. Acesse o `Route53` na distribuição do certificado que voce usou para o cloudfront
    - Crie uma nova config do tipo A
        - Record name: valor adicionado ao domónio pretendido no cloudfront
        - Value: claudfront domain name da distribuição cloud que acabou de criar [<cod/>.cloudfront.net] por exemplo.

6. Acesse a url que a nova distribuição do route53 aponta.