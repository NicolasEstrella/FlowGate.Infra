# FlowGate.Infra

Infraestrutura local do projeto FlowGate via Docker Compose.

## Servicos

| Servico     | URL                   |
|-------------|-----------------------|
| Frontend    | http://localhost:4200 |
| Backend API | http://localhost:8080 |
| PostgreSQL  | localhost:5432        |

## Como usar

### Subir tudo
```bash
docker compose up -d
```

### Ver logs
```bash
docker compose logs -f
```

### Parar tudo
```bash
docker compose down
```

### Resetar banco de dados
```bash
docker compose down -v
```

## Credenciais locais (dev only)

| Campo    | Valor      |
|----------|------------|
| Database | flowgatedb |
| User     | flowgate   |
| Password | FlowGate2025 |

Connection String:
```
Host=localhost;Port=5432;Database=flowgatedb;Username=flowgate;Password=FlowGate2025
```

MCP Postgres (Copilot):
```
postgresql://flowgate:FlowGate2025@localhost:5432/flowgatedb
```

## Documentacao
Consulte a documentacao completa no Notion do projeto.
