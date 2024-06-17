### Docker Compose File

```yaml
services:
  feature-flags-frontend:
    container_name: feature-flags-frontend
    image: shiv4nk4r/devflush-features:latest
    ports:
      - "4444:8080"
    environment:
      - VITE_BACKBACKEND_URL=http://localhost:3001
  feature-flags-backend:
    container_name: feature-flags-backend
    image: shiv4nk4r/devflush-features-backend:latest
    ports:
      - "3001:3000"
    environment:
      - MONGO_URL=mongodb://mongodb:27017/features-flag
      - ADMIN_USER=shivankar
      - ADMIN_PASS=securepass
  mongodb:
    image: mongo:6-jammy
    ports:
      - "27017:27017"
    volumes:
      - dbdata:/data/db
volumes:
  dbdata:
```
