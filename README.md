# Chatwoot Self-Hosted (Docker) — Stack pronta para produção

Este repositório disponibiliza uma stack completa para rodar o **Chatwoot self-hosted** com Docker, focada em **deploy rápido**, **segurança básica**, **persistência de dados** e **setup simplificado**.

A ideia é você clonar, configurar variáveis de ambiente e subir o ambiente com poucos comandos.

---

## O que vem neste repositório

- Chatwoot (Web + Worker)
- PostgreSQL (banco de dados)
- Redis (filas/cache)
- Persistência via volumes
- Estrutura recomendada para produção (com boas práticas de configuração)

> Se você quiser adicionar Nginx/Traefik, SSL, backup automático e monitoramento, dá para acoplar facilmente.

---

## Requisitos

- Docker instalado
- Docker Compose (v2+)
- Servidor Linux recomendado (Ubuntu/Debian)
- Porta(s) liberada(s) no firewall (ex.: 80/443 se usar proxy reverso)

---

## Instalação rápida

1) Clone o repositório:
```bash
git clone https://github.com/SEU_USUARIO/SEU_REPO.git
cd SEU_REPO
````

2. Copie o arquivo de ambiente:

```bash
cp .env.example .env
```

3. Edite o `.env` e configure pelo menos:

* `FRONTEND_URL`
* `SECRET_KEY_BASE`
* `POSTGRES_PASSWORD`
* `REDIS_PASSWORD` (se aplicável)
* SMTP (para envio de e-mails) — opcional, mas recomendado

4. Suba os containers:

```bash
docker compose up -d
```

5. Execute as migrações (primeira execução):

```bash
docker compose exec chatwoot bundle exec rails db:chatwoot_prepare
```

6. Acesse:

* Web: `http://SEU_DOMINIO_OU_IP`

---

## Configuração do `.env`

Exemplo (ajuste para o seu ambiente):

* `FRONTEND_URL=https://seu-dominio.com`
* `SECRET_KEY_BASE=gerar_uma_key_segura`
* `POSTGRES_HOST=postgres`
* `POSTGRES_USERNAME=postgres`
* `POSTGRES_PASSWORD=uma_senha_forte`
* `REDIS_URL=redis://redis:6379`

### Gerando `SECRET_KEY_BASE`

Você pode gerar uma chave forte usando:

```bash
openssl rand -hex 64
```

---

## Volumes e persistência

Esta stack utiliza volumes para garantir persistência de:

* Banco de dados (PostgreSQL)
* Redis (se necessário)
* Uploads e arquivos do Chatwoot

---

## Atualização

Para atualizar para a versão mais recente do Chatwoot:

```bash
docker compose pull
docker compose up -d
docker compose exec chatwoot bundle exec rails db:migrate
```

---

## Segurança (recomendado)

Para uso em produção:

* Use **proxy reverso** (Nginx/Traefik/Caddy) com **SSL**
* Configure **SMTP** para envio confiável de e-mails
* Use senhas fortes e variáveis seguras
* Restrinja acesso às portas internas (Postgres/Redis)
* Configure backups do Postgres

---

## Troubleshooting

### Ver logs

```bash
docker compose logs -f
```

### Reiniciar serviços

```bash
docker compose restart
```

### Recriar do zero (cuidado: apaga volumes)

```bash
docker compose down -v
docker compose up -d
```

---

## Licença e créditos

* Chatwoot é um projeto open source mantido pela comunidade e pela equipe do Chatwoot.
* Este repositório organiza uma stack de deploy e não substitui a licença/termos do projeto original.

---

## Suporte

Se você encontrar algum problema:

* Abra uma **Issue** no repositório com logs e contexto do ambiente (SO, Docker/Compose, etc.).
* Descreva o passo a passo para reproduzir.

```

Se você me mandar:
1) o **conteúdo do seu `docker-compose.yml`** (ou sua stack),  
2) se vai usar **Traefik/Nginx** e **domínio**,  
eu adapto o README para ficar 100% fiel ao seu projeto (com comandos exatos, variáveis reais e seção de deploy em produção).
```
