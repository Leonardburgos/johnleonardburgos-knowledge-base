### Create a MySQL Image on `docker-compose.yml`
```yaml
services:
  mysql:
    image: mysql:8.0
    container_name: mysql
    ports:
      - "3308:3306" # Map MySQL container port 3306 to host port 3308
    environment:
      MYSQL_ROOT_PASSWORD: "yourpassword"
      MYSQL_DATABASE: mydatabasename
      MYSQL_USER: yourusername
      MYSQL_PASSWORD: "yourpassword"
    volumes:
      - ./mysql_data:/var/lib/mysql
    healthcheck:
      test: ["CMD", "mysql", "-h", "localhost", "-u", "yourusername", "-p<yourpassword>", "-e", "SELECT 1;"]
      retries: 5
      interval: 10s
      start_period: 10s
      timeout: 10s
volumes:
  mysql_data:
```

## Create Redis Image on Docker
```yaml
services:

  redis:
    image: redis/redis-stack-server:latest
    container_name: redis
    ports:
      - "6390:6379"
    healthcheck:
      test: ["CMD", "redis-cli", "--raw", "incr", "ping"]
```