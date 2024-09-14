# Project Name

## Overview

This project is a multi-tier application using Docker Compose, consisting of a proxy server, a backend service, and a MySQL database. The services are designed to run on separate networks to ensure modularity and security.

## Architecture

![Docker_Project](https://github.com/user-attachments/assets/9eb2fb71-9547-4328-8d3e-82ad7e45ef6d)

## Output
![Screenshot 2024-09-14 174119](https://github.com/user-attachments/assets/c740d8e0-eb3e-4212-be15-84d37917db46)



## Services

### 1. Proxy
![image](https://github.com/user-attachments/assets/613c10e7-214a-45bf-ab23-0a4989b3f398)

![image](https://github.com/user-attachments/assets/9cc9d276-0923-477f-862b-3c712f40942d)


1. **`FROM nginx:alpine`**: 
   - **Purpose**: Sets the base image for the Docker container.
   - **Details**: Uses the official Nginx image based on Alpine Linux, which is known for its small size and efficiency.

2. **`COPY ./nginx.conf /etc/nginx/nginx.conf`**: 
   - **Purpose**: Copies a custom Nginx configuration file into the container.
   - **Details**: The local `nginx.conf` file is placed in the container's Nginx configuration directory, allowing you to customize the server settings.

3. **`CMD ["nginx", "-g", "daemon off;"]`**: 
   - **Purpose**: Specifies the command to run when the container starts.
   - **Details**: Runs Nginx with the `daemon off` option, which keeps Nginx running in the foreground. This is necessary for Docker to manage the process.

4. **`EXPOSE 80`**: 
   - **Purpose**: Exposes port 80 on the container.
   - **Details**: Allows external access to the web server running inside the container.

- **Image**: `nginx_proxy`
- **Build Context**: `./nginx`
- **Ports**: 
  - `8080:8080`
- **Networks**: 
  - `proxy-network`
  - `shared-network`
- **Dependencies**: 
  - `backend`

**Description**: This service uses Nginx as a reverse proxy. It routes requests to the backend service.

### 2. Backend

![image](https://github.com/user-attachments/assets/2bb4fcfb-a275-4c39-b376-0a5fc2a2a201)

- **Image**: `backend`
- **Build Context**: `./backend`
- **Ports**:
  - `8000:8000`
- **Networks**:
  - `backend-network`
  - `shared-network`
- **Dependencies**:
  - `db`
- **Secrets**:
  - `db-password`

**Description**: This service is the application backend, which processes data and communicates with the database.

### 3. Database (MySQL)

- **Image**: `mysql:8.0`
- **Environment**:
  - `MYSQL_ROOT_PASSWORD_FILE`: `/run/secrets/db-password`
  - `MYSQL_DATABASE`: `example`
- **Volumes**:
  - `db_data:/var/lib/mysql`
- **Secrets**:
  - `db-password`
- **Networks**:
  - `db-network`
  - `shared-network`

**Description**: This service provides a MySQL database for storing application data.

## Docker-compose file

![image](https://github.com/user-attachments/assets/87e3a32d-66b8-4e81-87d5-7cdec704ab77)


## Networks

- **proxy-network**: Used for communication between the proxy and other services.
- **backend-network**: Used for communication between the backend service and other services.
- **db-network**: Used for communication between the database service and other services.
- **shared-network**: Allows communication between services across different networks.

## Secrets

- **db-password**: The password for the MySQL root user, stored securely.

## Volumes

- **db_data**: Persistent storage for MySQL data.

