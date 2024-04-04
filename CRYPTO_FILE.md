# Criptografando arquivos localmente
---

É possível cryptografar os arquivos localmente, de modo que para que alguém possa ler o conteúdo inicial é necessário uma senha para descriptografar usando o GPG.

O GPG (GNU Privacy Guard) é uma ferramenta de criptografia e assinatura digital de código aberto, que protege a privacidade e autenticidade dos seus dados. Com o GPG, você pode:

- Criar pares de chaves pública e privada para criptografar e descriptografar dados.
- Assinar digitalmente arquivos para verificar sua autenticidade.
- Estabelecer identidades associadas às suas chaves para identificação.
- Gerenciar confiança em chaves públicas de outras pessoas, assinando suas chaves.
- Proteger a comunicação digital e garantir a segurança dos dados transmitidos.


1. Instale o pacote gnupg
    - 1.1 Para macOs
        `brew install gnupg`
    - 1.2 Para linux ubuntu/debian
        `sudo apt-get update`
        `sudo apt-get install gnupg`

2. Crie seu arquivo no local desejado
    - 1.1 `> <FILENAME/>.txt`
    - 1.2 ctrl + c
    - 1.3 edite o arquivo usando vim 
        `vim <FILENAME/>.txt`
    - É possível cryptografar pastas inteiras, basta gerar um .zip
        `zip <DESTINO/>.zip <ORIGEM/>.txt`

3. Criptografe o arquivo
    `gpg -c <FILENAME/>.txt`
    - uma senha e sua confirmação será solicitada, guarde-a para decriptografar
    - o arquivo ganhará o .gpg após o .txt, ficando `<FILENAME>.txt.gpg`
    - ao tentar abrir nao será mais possível ler o conteúdo, o arquivo terá apenas simbolos
    - se preferir pode apagar o arquivo que originou a cryptografia

4. Lendo novamente o arquivo
    `gpg --output <DESTINO/>.txt --decrypt <ORIGEM>.txt.gpg`
    `vim <DESTINO/>.txt`
