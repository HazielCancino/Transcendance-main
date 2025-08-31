# Web Application Project

This is a Docker-based web application that includes a Django backend, React frontend, PostgreSQL database, Redis cache, and Nginx reverse proxy.

## Project Structure

```
.
├── Makefile          # Build and deployment commands
├── docker-compose.yml # Docker container configuration
├── .env              # Environment variables
├── src/
│   ├── backend/      # Django application
│   ├── frontend/     # Frontend application
│   └── nginx/        # Nginx configuration
└── .data/           # Persistent data storage
```

## Services

- **Backend**: Django web application (port 8000)
- **Frontend**: React application (port 3001)
- **Database**: PostgreSQL (port 5432)
- **Cache**: Redis (port 6378)
- **Reverse Proxy**: Nginx (port 3000)

## Prerequisites

- Docker
- Docker Compose

## Getting Started

### 1. Create Environment File

Create a `.env` file in the root directory with required environment variables:

```env
# Database credentials
DB_NAME=your_database_name
DB_USER=your_database_user
DB_PASSWORD=your_database_password
DB_HOST=db
DB_PORT=5432

# Django settings
SECRET_KEY=your_secret_key
DEBUG=True
```

### 2. Start the Application

```bash
make all
```

This will:
- Build all Docker images
- Start all containers in detached mode
- Run database migrations
- Show logs for the backend service

## Available Commands

```bash
make all        # Build, migrate, and start services
make build      # Build and start all containers
make logs       # Show backend service logs
make migrate    # Run Django migrations
make down       # Stop and remove containers
make re         # Restart the entire setup (down + up)
```

## Access the Application

- **Frontend**: http://localhost:3001
- **Backend API**: http://localhost:8000
- **Nginx (HTTPS)**: https://localhost:3000

## Development

### Backend Development

To access the Django shell:
```bash
docker-compose exec backend python home_api/manage.py shell
```

To run Django commands:
```bash
docker-compose exec backend python home_api/manage.py [command]
```

### Frontend Development

The frontend is mounted as a volume, so changes will be reflected immediately.

## Directory Structure

- `src/backend/` - Django application code
- `src/frontend/` - Frontend application code
- `src/nginx/` - Nginx configuration files
- `.data/postgres/` - Persistent PostgreSQL data directory

## Troubleshooting

### Common Issues

1. **Port conflicts**: Make sure ports 3000, 3001, 5432, and 6378 are available
2. **Database connection issues**: Check your `.env` file credentials
3. **Build failures**: Try `make down` then `make all` to rebuild

### Viewing Logs

```bash
# View all service logs
docker-compose logs -f

# View specific service logs
docker-compose logs -f backend
```

## Project Architecture

```
[Client] → [Nginx:443] → [Backend:8000]
                 ↓
          [Frontend:3001]
                 ↓
        [Database:5432] [Redis:6378]
```

The Nginx reverse proxy handles HTTPS termination and routes requests to the appropriate services.
```