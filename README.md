
## Instalação

1º - Faça o Clone do Repositório.

2º - Navegue até a pasta do projeto: docker-php

3º - Navegue até o diretório para gerar um certificado SSL.

```bash
    cd .docker/apache/ssl
```

4º - Execute o comando para gerar o certificado.

```bash
    openssl req -x509 -new -out mycert.crt -keyout mycert.key -days 365 -newkey rsa:4096 -sha256 -nodes
```
    
5º - Informe as configurações do Certificado ou tecle ENTER para avançar.

```bash
    Country Name (2 letter code) [AU]:
    State or Province Name (full name) [Some-State]: 
    Locality Name (eg, city) []:
    Organization Name (eg, company) [Internet Widgits Pty Ltd]:
    Organizational Unit Name (eg, section) []:
    Common Name (e.g. server FQDN or YOUR name) []:
    Email Address []:
```

6º - Altere o endereço (dev.localhost.com.br) nos arquivos abaixo para um de sua preferência.

```bash
   .docker\apache\000-default.conf
```
e também

```bash
 .docker-php-mysql/.htaccess
```

7º - Navegue até a raiz e execute.

```bash
 .docker compose up -d
```



