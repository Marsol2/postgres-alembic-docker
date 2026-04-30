# PostgreSQL + Alembic + Docker

Projecto de exemplo que demonstra como configurar um container **PostgreSQL** via **Docker Compose** e gerir as migrations de base de dados com **Alembic** e **SQLAlchemy**.

## Tecnologias

| Tecnologia | Versão | Finalidade |
|---|---|---|
| PostgreSQL | 15 | Base de dados relacional |
| Docker Compose | v2+ | Orquestração do container |
| Alembic | 1.18.4 | Gestão de migrations |
| SQLAlchemy | 2.0.49 | ORM e mapeamento de modelos |
| psycopg2-binary | 2.9.12 | Driver PostgreSQL para Python |

## Estrutura do Projecto

```
postgres-alembic-docker/
├── alembic/
│   ├── versions/
│   │   └── <hash>_create_users_table.py   # Migration gerada automaticamente
│   ├── env.py                              # Configuração do ambiente Alembic
│   ├── README
│   └── script.py.mako                     # Template de migration
├── alembic.ini                             # Configuração principal do Alembic
├── docker-compose.yml                      # Definição do container PostgreSQL
├── models.py                               # Modelos SQLAlchemy (tabelas)
├── requirements.txt                        # Dependências Python
└── README.md
```

## Pré-requisitos

- [Docker](https://docs.docker.com/get-docker/) e [Docker Compose](https://docs.docker.com/compose/) instalados
- Python 3.11+

## Como Usar

### 1. Clonar o repositório

```bash
git clone https://github.com/Marsol2/postgres-alembic-docker.git
cd postgres-alembic-docker
```

### 2. Criar e activar o ambiente virtual Python

```bash
python3 -m venv venv
source venv/bin/activate   # Linux/macOS
# ou
venv\Scripts\activate      # Windows
```

### 3. Instalar as dependências

```bash
pip install -r requirements.txt
```

### 4. Subir o container PostgreSQL

```bash
docker compose up -d
```

O PostgreSQL ficará disponível em `localhost:5432` com as seguintes credenciais:

| Parâmetro | Valor |
|---|---|
| Host | `localhost` |
| Porta | `5432` |
| Utilizador | `user` |
| Palavra-passe | `password` |
| Base de dados | `mydatabase` |

### 5. Aplicar as migrations

```bash
alembic upgrade head
```

Este comando aplica todas as migrations pendentes, criando as tabelas definidas em `models.py`.

### 6. Verificar as tabelas criadas

```bash
docker exec postgres_db psql -U user -d mydatabase -c "\dt"
```

## Gerir Migrations

### Criar uma nova migration manualmente

```bash
alembic revision -m "descricao_da_alteracao"
```

### Criar uma migration com autogenerate (detecta alterações nos modelos)

```bash
alembic revision --autogenerate -m "descricao_da_alteracao"
```

### Reverter a última migration

```bash
alembic downgrade -1
```

### Ver o histórico de migrations

```bash
alembic history
```

### Ver a versão actual aplicada

```bash
alembic current
```

## Modelos Disponíveis

### Tabela `users`

| Coluna | Tipo | Restrições |
|---|---|---|
| `id` | INTEGER | PRIMARY KEY, AUTO INCREMENT |
| `name` | VARCHAR | INDEX |
| `email` | VARCHAR | UNIQUE, INDEX |

## Parar o Container

```bash
docker compose down
```

Para remover também o volume de dados:

```bash
docker compose down -v
```

## Referências

- [Documentação Alembic](https://alembic.sqlalchemy.org/)
- [Documentação SQLAlchemy](https://docs.sqlalchemy.org/)
- [Imagem oficial PostgreSQL no Docker Hub](https://hub.docker.com/_/postgres)
