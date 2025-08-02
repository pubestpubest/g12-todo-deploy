# G12 Todo - Docker Deployment

A containerized deployment setup for the G12 Todo application using Docker Compose. This project provides a complete development and production-ready environment with PostgreSQL database, backend API, and frontend web application.

## 🏗️ Architecture

The application follows a three-tier architecture:

- **Database Layer**: PostgreSQL 17 (Alpine) for data persistence
- **Backend Layer**: API service (Go/Node.js/Python - depending on your backend implementation)
- **Frontend Layer**: Web application served via Nginx

## 📋 Prerequisites

- [Docker](https://docs.docker.com/get-docker/) (version 20.10 or higher)
- [Docker Compose](https://docs.docker.com/compose/install/) (version 2.0 or higher)

## 🚀 Quick Start

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd g12-todo-deploy
   ```

2. **Create environment file**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

3. **Start the application**
   ```bash
   docker-compose up -d
   ```

4. **Access the application**
   - Frontend: http://localhost:3000 (or your configured FRONTEND_PORT)
   - Backend API: Available through the frontend proxy

## ⚙️ Configuration

### Environment Variables

Create a `.env` file in the root directory with the following variables:

```env
# Project Configuration
PROJECT_NAME=g12-todo

# Database Configuration
POSTGRES_PASSWORD=your_postgres_password
POSTGRES_USER=postgres
POSTGRES_DB=g12_todo_db
POSTGRES_APP_USER=g12_app_user
POSTGRES_APP_PASSWORD=your_app_password

# Backend Configuration
BACKEND_IMAGE_NAME=your-backend-image:latest
BACKEND_PORT=8080

# Frontend Configuration
FRONTEND_IMAGE_NAME=your-frontend-image:latest
FRONTEND_PORT=3000
```

### Service Configuration

- **PostgreSQL**: Runs on port 5432 (internal)
- **Backend**: Runs on port 8080 (internal)
- **Frontend**: Runs on port 3000 (external, configurable)

## 🐳 Docker Services

### PostgreSQL Database
- **Image**: `postgres:17-alpine`
- **Container**: `${PROJECT_NAME}-db`
- **Features**:
  - Automatic initialization with custom user setup
  - Persistent data storage
  - Network isolation

### Backend API
- **Container**: `${PROJECT_NAME}-backend`
- **Features**:
  - Depends on PostgreSQL service
  - Environment-based configuration
  - Automatic restart on failure

### Frontend Application
- **Container**: `${PROJECT_NAME}-frontend`
- **Features**:
  - Nginx reverse proxy to backend
  - Configurable external port
  - Depends on backend service

## 📁 Project Structure

```
g12-todo-deploy/
├── docker-compose.yaml    # Main deployment configuration
├── _entrypoint/
│   └── init.sh           # Database initialization script
├── .env                  # Environment variables (not in git)
├── .gitignore           # Git ignore rules
└── README.md            # This file
```

## 🔧 Database Setup

The PostgreSQL database is automatically initialized with:
- Custom application user with restricted permissions
- Proper schema access for ORM frameworks (like GORM)
- Secure default configuration

## 🛠️ Development Commands

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop all services
docker-compose down

# Rebuild and start
docker-compose up -d --build

# Remove all containers and volumes
docker-compose down -v

# Access PostgreSQL
docker-compose exec postgres psql -U postgres -d g12_todo_db
```

## 🔍 Troubleshooting

### Common Issues

1. **Port conflicts**: Ensure the configured ports are not in use by other services
2. **Database connection**: Check that the database credentials in `.env` match your backend configuration
3. **Image not found**: Ensure your backend and frontend Docker images are built and available

### Logs and Debugging

```bash
# View all service logs
docker-compose logs

# View specific service logs
docker-compose logs backend
docker-compose logs frontend
docker-compose logs postgres

# Follow logs in real-time
docker-compose logs -f
```

## 🔒 Security Considerations

- Database passwords should be strong and unique
- The `.env` file is excluded from version control
- Application user has restricted database permissions
- Services are isolated in a custom Docker network

## 📝 Notes

- The frontend service acts as a reverse proxy to the backend
- Database data persists across container restarts
- All services automatically restart on failure
- The setup is designed for both development and production use

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test the deployment
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.
