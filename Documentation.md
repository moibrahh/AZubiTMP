# Docker Setup Documentation

## Prerequisites

Before you begin, ensure you have the following installed on your system:
- Docker Engine (version 20.10.0 or later)
- Docker Compose (version 2.0.0 or later)
- Git (for cloning the repository)

## Setup Instructions

### 1. Clone the Repository
```bash
git clone https://github.com/moibrahh/AZubiTMP.git
cd AZubiTMP
```

### 2. Build and Run the Containers
To start all services (Frontend, Backend, and MongoDB):
```bash
docker-compose up --build
```

To run in detached mode (in the background):
```bash
docker-compose up -d --build
```

### 3. Accessing the Services
- Frontend: http://localhost:80
- Backend API: http://localhost:3000
- MongoDB: mongodb://localhost:27017

### 4. Stopping the Containers
To stop all services:
```bash
docker-compose down
```

To stop and remove volumes (this will delete all data):
```bash
docker-compose down -v
```

## Network and Security Configurations

### Port Mappings
- Frontend: 80:80 (Host:Container)
- Backend: 3000:3000 (Host:Container)
- MongoDB: 27017:27017 (Host:Container)

### Environment Variables
The following environment variables are configured in the docker-compose.yml:

#### Backend Service
- `MONGO_URI`: mongodb://mongo:27017
- `DB_NAME`: todos

### Network Configuration
- A custom bridge network `app-network` is created for inter-service communication
- All services are connected to this network
- Services can communicate using their service names as hostnames

### Data Persistence
- MongoDB data is persisted using a named volume `mongo-data`
- The volume is automatically created and managed by Docker

## Troubleshooting Guide

### Common Issues and Solutions

1. **Port Conflicts**
   - If you see errors about ports being in use, check if any of these ports (80, 3000, 27017) are already in use
   - Solution: Stop the conflicting service or modify the port mappings in docker-compose.yml

2. **Container Build Failures**
   - If the build fails, check the Docker logs:
   ```bash
   docker-compose logs
   ```
   - Common causes:
     - Missing dependencies in package.json
     - Network issues during npm install
     - Insufficient disk space

3. **MongoDB Connection Issues**
   - If the backend can't connect to MongoDB:
     - Ensure MongoDB container is running: `docker-compose ps`
     - Check MongoDB logs: `docker-compose logs mongo`
     - Verify the MONGO_URI environment variable is correct

4. **Frontend Not Accessible**
   - If you can't access the frontend:
     - Check if the frontend container is running
     - Verify nginx configuration
     - Check frontend logs: `docker-compose logs frontend`

### Useful Commands

1. **View Container Logs**
```bash
# All containers
docker-compose logs

# Specific service
docker-compose logs frontend
docker-compose logs backend
docker-compose logs mongo
```

2. **Check Container Status**
```bash
docker-compose ps
```

3. **Restart Services**
```bash
# Restart all services
docker-compose restart

# Restart specific service
docker-compose restart frontend
```

4. **Clean Up**
```bash
# Remove all containers and volumes
docker-compose down -v

# Remove unused images
docker image prune
```

### Debugging Tips

1. **Access Container Shell**
```bash
# Frontend container
docker-compose exec frontend sh

# Backend container
docker-compose exec backend sh

# MongoDB container
docker-compose exec mongo mongosh
```

2. **Check Network Connectivity**
```bash
# List networks
docker network ls

# Inspect network
docker network inspect app-network
```

3. **View Container Resource Usage**
```bash
docker stats
```

## Maintenance

### Regular Maintenance Tasks

1. **Update Dependencies**
   - Regularly update the base images in Dockerfiles
   - Keep npm packages updated in both frontend and backend

2. **Clean Up**
   - Periodically remove unused containers, images, and volumes
   - Monitor disk space usage

3. **Backup**
   - Regularly backup the MongoDB data volume
   - Consider implementing automated backup solutions

### Security Best Practices

1. **Environment Variables**
   - Never commit sensitive environment variables to version control
   - Use .env files or Docker secrets for sensitive data

2. **Network Security**
   - Only expose necessary ports
   - Use internal networks for service communication
   - Consider implementing network policies

3. **Image Security**
   - Regularly update base images to patch security vulnerabilities
   - Use specific version tags instead of 'latest'
