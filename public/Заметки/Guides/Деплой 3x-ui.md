## Nginx
docker-compose для nginx:
```yml
services:
	nginx:
		image: nginx:1.29.5-alpine
		ports:  
			- "8443:8443"  
		volumes:
			- /opt/3x-ui/nginx/nginx.conf:/etc/nginx/nginx.conf:ro
			- /opt/3x-ui/certificates
		restart: unless-stopped
```

Конфигурация `nginx.conf`:
```

upstream 3x-ui-https {
	server 3xui
}

server {
	server_name localhost;
	port 8443;
	
	ssl_certificate ???;
	ssh_certificate_key ???;

	location / {
	    proxy_http_version 1.1;
	    proxy_set_header Upgrade $http_upgrade;
	    proxy_set_header Connection "upgrade";
	
	    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	    proxy_set_header X-Forwarded-Proto $scheme;
	    proxy_set_header Host $http_host;
	    proxy_set_header X-Real-IP $remote_addr;
	    proxy_set_header Range $http_range;
	    proxy_set_header If-Range $http_if_range;
	
	    proxy_redirect off;
	    proxy_pass http://3xui:2053;
	}
}
```

## 3X-UI
Конфигурация `docker-compose.yml`:
```yml
services:
  3xui:
    image: "ghcr.io/mhsanaei/3x-ui:v2.8.10"
    container_name: "3xui_app"
    volumes:
      - /opt/3x-ui/db/:/etc/x-ui/
      - /opt/3x-ui/cert/:/root/cert/ # ?
    environment:
      XRAY_VMESS_AEAD_FORCED: "false"
      XUI_ENABLE_FAIL2BAN: "true"
    tty: true
    restart: unless-stopped
    network_mode: host
```

