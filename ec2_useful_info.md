## Linux Commands
- **Update System Packages** `sudo apt update && sudo apt upgrade -y` 
- **Install Essential Packages** `sudo apt install -y vim htop wget curl `
---
### Python

- **Install Python** `sudo apt install python3`
- **Install Python-Pip**  `sudo apt install python3-pip`
- **Install App Requirements** `sudo pip3 install -r requirements.txt`
---
### NGINX

- **Install Nginx** `sudo apt install nginx` 
- **Start Nginx** `sudo systemctl start nginx` 
- **Stop Nginx** `sudo systemctl stop nginx` 
- **Restart Nginx** `sudo systemctl restart nginx` 
- **Disable Nginx at Server Start** `sudo systemctl disable nginx` 
- **Enable Nginx at Server Start** `sudo systemctl enable nginx` 
- **Change Ownership Ubuntu** `sudo chown -R ubuntu /etc/nginx`
---
### CERTBOT

- **Install Certbot** `sudo snap install --classic certbot`
- **Request Certificate** `sudo certbot certonly`
- **Renew Certificate** `sudo certbot renew`
- **Check Certificate** `sudo certbot certificates`
- **Check Certificate Auto Renewal** `sudo certbot renew --dry-run`
- **Change Ownership Ubuntu** `sudo chown -R ubuntu /etc/letsencrypt/live`
---
### Streamlit

-  **Run Streamlit Temporary**  `python3 -m streamlit run app.py`
-  **Run Streamlit Permanent**  `nohup python3 -m streamlit run app.py`
-  **Config file for EC2 Instance**  `sudo nano /home/ubuntu/.local/lib/python3.10/site-packages/streamlit/config.toml`

		[server]
		port=8501 # Default Streamlit uses 8501 port
		headless=true # This will eliminate automatically open browser
		# enableCORS=false
		# enableXsrfProtection=false
		# enableWebsocketCompression=false[browser]
		serverAddress = "domain.xyz" # Put your Local IP or Domain Name
		serverPort = 8501`
---
### NGINX Default config file with HTTPS settings

    server {
            listen 443 ssl;
            server_name  domain.xyz; # Put your Local IP or Domain Name
      
            ssl_certificate /etc/letsencrypt/live/domain.xyz/fullchain.pem; # Put Cerbot provide path
            ssl_certificate_key /etc/letsencrypt/live/domain.xyz/privkey.pem; # Put Cerbot provide path
      
            proxy_http_version 1.1; 
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $host;
    	
    	root /var/www/html;
    }
---

### NGINX Streamlit config file

    server {
            listen 443 ssl;
            server_name  domain.xyz; # Put your Local IP or Domain Name
      
            ssl_certificate /etc/letsencrypt/live/domain.xyz/fullchain.pem; # Put Cerbot path
            ssl_certificate_key /etc/letsencrypt/live/domain.xyz/privkey.pem; # Put Cerbot path
      
            proxy_http_version 1.1; 
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $host;
    	
    	root /var/www/html;
    
            # Streamlit specific: 
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_read_timeout 86400;
     
            location / {
                proxy_pass http://127.0.0.1:8501; # Streamlit Default Port 8501
            }
    }
