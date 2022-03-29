# docker-proxy
Web Proxy usando Docker, NGINX e Let's Encrypt

Configurar seu servidor com vários sites usando um único proxy NGINX para gerenciar suas conexões, automatizando o contêiner de aplicativos (portas 80 e 443) para renovar automaticamente seus certificados SSL com o Let´s Encrypt.

Com essa configuração, você poderá iniciar um ambiente de produção em alguns segundos.

## Pré requisitos

1. docker (https://docs.docker.com/engine/installation/)
2. docker-compose (https://docs.docker.com/compose/install/)


## Como utilizar

1. Clone o repositório:

```bash
git clone https://github.com/leguass7/docker-proxy.git
```

2. Faça uma cópia do arquivo `.env.sample` e renomeie para `.env`:

Atualize o arquivo `.env` com suas configurações.

3. Rode o script para iniciar
 > *Atenção para as permissões do arquivo `start.sh`*
 > `chmod 755 start.sh`

```bash
./start.sh
```

Pronto o proxy está funcionando!

## Iniciando um web container

Depois de seguir as etapas acima, você pode iniciar novos contêineres da web com a porta 80 aberta.
basta adicionar a variável de ambiente `VIRTUAL_HOST=your.domain.com` para que o proxy gere automaticamente o script reverso no NGINX Proxy para encaminhar novas conexões ao seu contêiner de aplicativos/web:

```bash
docker run -d -e VIRTUAL_HOST=your.domain.com \
              --network=webproxy \
              --name my_app \
              httpd:alpine
```

Para ter SSL em seu aplicativo / web, basta adicionar a variável de ambiente `LETSENCRYPT_HOST=your.domain.com`:

```bash
docker run -d -e VIRTUAL_HOST=your.domain.com \
              -e LETSENCRYPT_HOST=your.domain.com \
              -e LETSENCRYPT_EMAIL=your.email@your.domain.com \
              --network=webproxy \
              --name my_app \
              httpd:alpine
```

> Você não precisa abrir a porta *443* no seu contêiner, a validação do certificado é gerenciada pelo proxy da web.


## Scripts de teste adicionais

```bash
./test_start_ssl.sh your.domain.com
```

```bash
./test_stop.sh
```

## Créditos

Os créditos vão para:
- nginx-proxy [@jwilder](https://github.com/jwilder/nginx-proxy)
- docker-gen [@jwilder](https://github.com/jwilder/docker-gen)
- docker-letsencrypt-nginx-proxy-companion [@JrCs](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion)

- https://github.com/evertramos/docker-compose-letsencrypt-nginx-proxy-companion
- https://github.com/evertramos/nginx-proxy-automation
