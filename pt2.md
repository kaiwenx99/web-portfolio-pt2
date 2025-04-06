## Prep: All 3 AWS EC2 Instances Setup

- Ubuntu
- t2.micro
- security group: allow port 22 and 80

## Step 1: Nginx Reverse Proxy Setup for HTTPS Backend

### Goal:

Configure an EC2 instance running Nginx to reverse proxy requests to an external HTTPS backend: `https://portfolio.play.kaiwenx.xyz/`

### Steps:

1. Install Nginx

   ```bash
   sudo apt update
   sudo apt install -y nginx
   ```

2. Create Reverse Proxy Config

   ```bash
   sudo vim /etc/nginx/sites-available/reverse_proxy.conf
   ```

3. Paste the following config:

   ```nginx
   server {
       listen 80;

       location / {
           proxy_pass https://portfolio.play.kaiwenx.xyz/;
           proxy_ssl_server_name on;
           proxy_ssl_verify off;

           proxy_set_header Host portfolio.play.kaiwenx.xyz;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
   }
   ```

4. Enable the config

   ```bash
   sudo ln -s /etc/nginx/sites-available/reverse_proxy.conf /etc/nginx/sites-enabled/
   sudo rm /etc/nginx/sites-enabled/default
   ```

5. Test and restart Nginx

   ```bash
   sudo nginx -t
   sudo systemctl restart nginx
   ```

6. Test the reverse proxy
   ```bash
   curl http://localhost
   ```

### Expected result:

The portfolio page content returned by `curl`

## Step 2: HAProxy Load Balancer Setup for Two Nginx Reverse Proxies

### Goal:

Set up HAProxy on a third EC2 instance to load balance traffic between two existing Nginx reverse proxy servers.

### Steps:

1. **Install HAProxy**

   ```bash
   sudo apt update
   sudo apt install -y haproxy
   ```

2. **Edit the HAProxy config**

   ```bash
   sudo nano /etc/haproxy/haproxy.cfg
   ```

3. **Replace or append the config with:**

   ```haproxy
   frontend http_front
       bind *:80
       default_backend aws_backends

   backend aws_backends
       balance roundrobin
       server nginx1 172.31.29.80:80 check
       server nginx2 172.31.26.80:80 check
   ```

4. **Restart HAProxy**

   ```bash
   sudo systemctl restart haproxy
   ```

5. **Test the Load Balancer**

   ```bash
   curl http://localhost
   curl http://<HAProxy_Public_IP>
   ```
