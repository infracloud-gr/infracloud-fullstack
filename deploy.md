
# Deploy application step by step

# 1. Setup infrastructure

Go inside @infra and go to ansible folder and run command

```
./run.sh
```

## 2. Build frontend

Go inside @infra-ui and run command

```
npm run build
```

Then copy `dist/**`, `deploy.sh` folder to deployment server

```
scp -r disk/** <username>@<ip>:<folder>
ssh <username>@<ip> "chmod +x deploy.sh && ./deploy.sh"
```

***deploy.sh**

```
#!/bin/bash

apt update
sudo ufw allow 80/tcp
apt install nginx -y
cd /var/www/html
sudo git clone https://github.com/novnc/noVNC.git novnc

cat > /etc/nginx/sites-available/default <<'EOF'
upstream infra-backend {
    server 127.0.0.1:8080;
}

server {
    listen 80;

    # server_name 192.168.10.128; # Your Nginx IP
    location /infra/ {
        alias /usr/share/nginx/html/;

        index index.html;

        try_files $uri $uri/ /index.html;
    }

    # 1. Serve the noVNC UI files
    location /novnc/ {
        root /var/www/html;
        index vnc.html;
    }
# 2. Internal block to ask Python for the VM's Compute IP
    location = /auth-vnc {
        internal;
        # Use the custom variable we saved below
        proxy_pass http://infra-backend/api/vnc/v1/validate?vmid=$saved_vmid&userid=$saved_userid;

        proxy_pass_request_body off;
        proxy_set_header Content-Length "";
        proxy_set_header X-Original-URI $request_uri;
    }

    # 3. The public WebSocket endpoint your noVNC client connects to
    location /websockify {
        # SAVE the parameter into memory before the subrequest happens!
        set $saved_vmid $arg_vmid;
        set $saved_userid $arg_userid;

        # Trigger the authentication sub-request to the Python backend
        auth_request /auth-vnc;

        # Extract the compute node IP from the backend's response header
        auth_request_set $compute_target $upstream_http_x_compute_target;

        set $args "token=$saved_vmid";
        # Proxy the WebSocket connection dynamically to the internal Compute IP
        proxy_pass http://$compute_target;

        # Standard headers required to proxy WebSockets
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Host $host;

        proxy_read_timeout 3600s;
        proxy_send_timeout 3600s;
    }
}
EOF

systemctl restart nginx
```

## 3. Build backend

Inside deployment server you need login docker by using this command:

```
docker login
```

then pass your username and password

then you need use this command to pull backend image to your server

```
docker pull irohas2004/infracloud:latest
```

Then run application by docker-compose which file located in parent folder

***Docker file***

```
version: '3.8'

services:
  database:
    image: mysql:8.0
    container_name: infra-db
    restart: always
    environment:
      MYSQL_DATABASE: ${DB_NAME}
      MYSQL_USER: ${DB_USER}
      MYSQL_PASSWORD: ${DB_PASSWORD}
      MYSQL_ROOT_PASSWORD: ${DB_ROOT_PASSWORD}
    volumes:
      - db_data:/var/lib/mysql

  backend:
    build:
      context: ./infra
      dockerfile: Dockerfile
    container_name: infra-backend
    restart: always
    depends_on:
      - database
    environment:
      - SPRING_PROFILES_ACTIVE=${SPRING_PROFILES_ACTIVE}
      - SPRING_DATASOURCE_URL=${SPRING_DATASOURCE_URL}
      - SPRING_DATASOURCE_USERNAME=${SPRING_DATASOURCE_USERNAME}
      - SPRING_DATASOURCE_PASSWORD=${SPRING_DATASOURCE_PASSWORD}
      - JWT_SECRET_KEY=${JWT_SECRET_KEY}
      - JWT_ACCESS_TOKEN=${JWT_ACCESS_TOKEN}
      - JWT_REFRESH_TOKEN=${JWT_REFRESH_TOKEN}
      - APP_FE_BASE_URL=${APP_FE_BASE_URL}
      - CEPH_MONITORS=${CEPH_MONITORS}
      - SPRING_MAIL_USERNAME=${SPRING_MAIL_USERNAME}
      - SPRING_MAIL_PASSWORD=${SPRING_MAIL_PASSWORD}
    ports:
      - "8080:8080"
    volumes:
      - ./infra/uploads:/app/uploads

volumes:
  db_data:
```

```
docker-compose up -d
```