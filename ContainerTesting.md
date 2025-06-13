# Container Testing Documentation


### 1. Container Status Verification

```bash
# Check if all containers are running
docker-compose ps

# Expected output should show all three services (frontend, backend, mongo) as "Up"
```

### 2. Frontend Verification

```bash
# Test frontend accessibility
curl http://localhost:80

# Expected output should show HTTP 200 OK
# If using a browser, visit http://localhost:80

# Check frontend container logs
docker-compose logs frontend

# Check nginx configuration
docker-compose exec frontend nginx -t
```

### 3. Backend Verification

```bash
# Test backend API endpoint
curl http://localhost:3000


# Check backend container logs
docker-compose logs backend

# Test backend container shell access
docker-compose exec backend sh
```

### 4. MongoDB Verification

```bash
# Check MongoDB container logs
docker-compose logs mongo

# Connect to MongoDB and verify database
docker-compose exec mongo mongosh

# Once in MongoDB shell, run these commands:
use todos
show collections
db.todos.find()  # Should show existing todos if any

# Test MongoDB connection from backend
docker-compose exec backend node -e "
const mongoose = require('mongoose');
mongoose.connect('mongodb://mongo:27017/todos')
  .then(() => console.log('MongoDB connection successful'))
  .catch(err => console.error('MongoDB connection error:', err));
"
```

### 5. Network Connectivity Tests

```bash
# Test frontend to backend communication
docker-compose exec frontend curl http://backend:3000


# Test network connectivity between containers
docker-compose exec backend ping mongo
docker-compose exec frontend ping backend
```


### 6. Resource Usage Verification

```bash
# Check container resource usage
docker stats

# Check container logs for any errors
docker-compose logs --tail=100
```

### 7. Common Verification Issues and Solutions

1. **Frontend Not Responding**
   ```bash
   # Check nginx configuration
   docker-compose exec frontend nginx -t
   
   # Restart frontend container
   docker-compose restart frontend
   ```

2. **Backend API Not Responding**
   ```bash
   # Check backend logs
   docker-compose logs backend
   

   ```

3. **MongoDB Connection Issues**
   ```bash
   # Check MongoDB logs
   docker-compose logs mongo
   
   # Verify MongoDB is accepting connections
   docker-compose exec mongo mongosh --eval "db.serverStatus()"
   ```


```
