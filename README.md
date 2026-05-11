# FlowGate.Infra

Infraestrutura local do projeto FlowGate via Docker Compose. Esta stack sobe os servicos base da arquitetura para desenvolvimento integrado do backend e do portal.

## Servicos

| Servico | URL / Porta | Observacao |
|---------|-------------|------------|
| Gateway NGINX | http://localhost:8088 | Entrada recomendada para navegar no portal e acessar `/api/*` |
| Portal | http://localhost:4200 | Acesso direto ao container do front para troubleshooting |
| Backend API | http://localhost:8080 | Acesso direto ao ASP.NET Core |
| Swagger via gateway | http://localhost:8088/api/swagger/index.html | Verificacao rapida do roteamento do gateway |
| PostgreSQL | localhost:5432 | Banco principal do desenvolvimento local |
| RabbitMQ | localhost:5672 | Broker AMQP |
| RabbitMQ Management | http://localhost:15672 | Console web do broker |
| Redis | localhost:6379 | Cache e armazenamento efemero |
| MinIO API | http://localhost:9000 | API S3 compativel |
| MinIO Console | http://localhost:9001 | Console web do storage |

## Arquivos importantes

- `docker-compose.yml`: orquestracao da stack local completa.
- `.env.example`: contrato de variaveis de ambiente e portas padrao.
- `nginx.gateway.conf`: roteamento do gateway entre portal e API.

## Bootstrap

1. Copie `.env.example` para `.env` na pasta `FlowGate.Infra`.
2. Revise as portas e credenciais se houver conflito local.
3. Suba toda a stack:

```bash
docker compose --env-file .env up -d --build
```

4. Verifique o estado dos servicos:

```bash
docker compose ps
docker compose logs -f gateway backend frontend
```

## Operacoes comuns

### Ver a configuracao resolvida

```bash
docker compose --env-file .env config
```

### Parar tudo

```bash
docker compose down
```

### Resetar totalmente o ambiente

```bash
docker compose down -v --remove-orphans
```

### Subir somente a infraestrutura base

```bash
docker compose --env-file .env up -d postgres rabbitmq redis minio
```

## Credenciais locais

| Recurso | Usuario | Senha |
|---------|---------|-------|
| PostgreSQL | flowgate | FlowGate2025 |
| RabbitMQ | flowgate | FlowGate2025 |
| MinIO | flowgate | FlowGate2025 |

Connection string padrao do backend:

```text
Host=localhost;Port=5432;Database=flowgatedb;Username=flowgate;Password=FlowGate2025
```

MCP Postgres:

```text
postgresql://flowgate:FlowGate2025@localhost:5432/flowgatedb
```

## Troubleshooting

### O backend nao sobe

- Rode `docker compose ps` e confirme se `postgres`, `rabbitmq`, `redis` e `minio` estao `healthy`.
- Inspecione `docker compose logs backend` para falhas de conexao.
- Verifique se a porta `8080` esta livre na maquina host.

### O gateway responde, mas a API falha

- Teste `http://localhost:8080/swagger/index.html` diretamente.
- Teste `http://localhost:8088/api/swagger/index.html` para confirmar o proxy.
- Rode `docker compose logs gateway backend` para validar o upstream.

### O portal nao carrega

- Teste `http://localhost:4200` diretamente.
- Rode `docker compose logs frontend gateway` para validar build e roteamento.

### Quero recriar tudo do zero

- Execute `docker compose down -v --remove-orphans`.
- Confirme que os volumes nomeados foram removidos com `docker volume ls | findstr flowgate`.
- Suba novamente com `docker compose --env-file .env up -d --build`.
