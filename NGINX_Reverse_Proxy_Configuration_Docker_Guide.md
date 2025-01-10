
# Configuring NGINX as a Reverse Proxy for Flask API using Docker

## Step 1: Install Docker
Make sure Docker is installed on your system. You can install Docker by following the instructions from the [official Docker documentation](https://docs.docker.com/get-docker/).

## Step 2: Create a Docker Network (Optional but Recommended)
Create a Docker network for the Flask API and NGINX container to communicate:

```bash
docker network create flask_network
```

## Step 3: Dockerize Your Flask API

1. **Create a Dockerfile** for your Flask API if you don’t already have one. Here is an example `Dockerfile` for a Flask application:

   ```dockerfile
   # Use a base image with Python and Flask
   FROM python:3.8-slim

   # Set the working directory in the container
   WORKDIR /app

   # Copy the Flask application code to the container
   COPY . /app

   # Install dependencies
   RUN pip install -r requirements.txt

   # Expose the port Flask is running on
   EXPOSE 5000

   # Command to run your Flask app
   CMD ["python", "app.py"]
   ```

2. **Build the Flask API Docker image**:

   ```bash
   docker build -t flask-api .
   ```

3. **Run the Flask API container**:

   ```bash
   docker run --name flask-api --network flask_network -d flask-api
   ```

This command will run the Flask API on port 5000 inside the Docker network.

## Step 4: Configure NGINX for Reverse Proxy

1. **Create a NGINX Configuration File** (`nginx.conf`) with reverse proxy settings for your Flask API:

   ```nginx
   server {
       listen 80;

       server_name yourdomain.com;  # Replace with your domain or IP address

       location / {
           proxy_pass http://flask-api:5000;  # The Flask container name and port
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
   }
   ```

2. **Create a Dockerfile for the NGINX Container**:

   Create a new `Dockerfile` for NGINX, which will copy the configuration file into the container:

   ```dockerfile
   # Use the official NGINX image
   FROM nginx:latest

   # Copy the NGINX config file into the container
   COPY nginx.conf /etc/nginx/nginx.conf

   # Expose the port NGINX will use
   EXPOSE 80
   ```

3. **Build the NGINX Docker image**:

   ```bash
   docker build -t nginx-reverse-proxy .
   ```

4. **Run the NGINX container**:

   ```bash
   docker run --name nginx-proxy --network flask_network -p 80:80 -d nginx-reverse-proxy
   ```

This command runs the NGINX container, binding the host's port 80 to the container's port 80.

## Step 5: Test the Setup
After both containers are running, you can test the reverse proxy setup by navigating to `http://yourdomain.com` (or `http://localhost` if you are using localhost).

NGINX should proxy the requests to the Flask API running on port 5000.

## Step 6: Docker Compose Setup (Optional)
You can also use Docker Compose to manage the containers. Create a `docker-compose.yml` file:

```yaml
version: '3'
services:
  flask-api:
    build: .
    container_name: flask-api
    networks:
      - flask_network
    ports:
      - "5000:5000"

  nginx:
    build:
      context: ./nginx
    container_name: nginx-reverse-proxy
    networks:
      - flask_network
    ports:
      - "80:80"

networks:
  flask_network:
    driver: bridge
```

This will automatically link the Flask API and NGINX containers in the same network.

To run the setup with Docker Compose, use the following command:

```bash
docker-compose up -d
```

