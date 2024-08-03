# Nginx
Here's a comprehensive and detailed list of Nginx CLI commands.

### **Nginx CLI Commands**

#### **1. Basic Nginx Commands**

- **Start Nginx:**
  ```sh
  sudo systemctl start nginx
  ```

- **Stop Nginx:**
  ```sh
  sudo systemctl stop nginx
  ```

- **Restart Nginx:**
  ```sh
  sudo systemctl restart nginx
  ```

- **Reload Nginx without dropping connections:**
  ```sh
  sudo systemctl reload nginx
  ```

- **Check the status of Nginx:**
  ```sh
  sudo systemctl status nginx
  ```

- **Enable Nginx to start at boot:**
  ```sh
  sudo systemctl enable nginx
  ```

- **Disable Nginx from starting at boot:**
  ```sh
  sudo systemctl disable nginx
  ```

#### **2. Configuration and Management**

- **Test Nginx configuration for syntax errors:**
  ```sh
  sudo nginx -t
  ```

- **Show the version of Nginx:**
  ```sh
  nginx -v
  ```

- **Show the version and configuration options of Nginx:**
  ```sh
  nginx -V
  ```

- **Start Nginx with a specific configuration file:**
  ```sh
  sudo nginx -c /path/to/nginx.conf
  ```

- **Get help and list Nginx options:**
  ```sh
  nginx -h
  ```

#### **3. Managing Nginx Logs**

- **View the Nginx access log:**
  ```sh
  sudo tail -f /var/log/nginx/access.log
  ```

- **View the Nginx error log:**
  ```sh
  sudo tail -f /var/log/nginx/error.log
  ```

- **Rotate Nginx logs:**
  ```sh
  sudo logrotate /etc/logrotate.d/nginx
  ```

#### **4. Security and Access Control**

- **Set up basic HTTP authentication:**
  1. Install `apache2-utils` if not already installed:
     ```sh
     sudo apt-get install apache2-utils
     ```
  2. Create a password file and add a user:
     ```sh
     sudo htpasswd -c /etc/nginx/.htpasswd username
     ```

- **Restrict access by IP address:**
  ```nginx
  location /admin {
      allow 192.168.1.1;
      deny all;
  }
  ```

- **Enable SSL/TLS for secure connections:**
  ```nginx
  server {
      listen 443 ssl;
      server_name example.com;

      ssl_certificate /etc/nginx/ssl/example.com.crt;
      ssl_certificate_key /etc/nginx/ssl/example.com.key;

      location / {
          # Configuration
      }
  }
  ```

#### **5. Load Balancing and Reverse Proxy**

- **Configure Nginx as a reverse proxy:**
  ```nginx
  server {
      listen 80;
      server_name example.com;

      location / {
          proxy_pass http://backend_server;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
      }
  }
  ```

- **Set up load balancing:**
  ```nginx
  upstream backend {
      server backend1.example.com;
      server backend2.example.com;
  }

  server {
      listen 80;
      server_name example.com;

      location / {
          proxy_pass http://backend;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          proxy_set_header X-Forwarded-Proto $scheme;
      }
  }
  ```

#### **6. Caching and Performance Optimization**

- **Enable caching:**
  ```nginx
  location / {
      proxy_cache my_cache;
      proxy_cache_valid 200 1h;
      proxy_cache_use_stale error timeout updating http_500 http_502 http_503 http_504;
      proxy_pass http://backend;
  }
  ```

- **Set up gzip compression:**
  ```nginx
  gzip on;
  gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
  ```

- **Rate limiting:**
  ```nginx
  http {
      limit_req_zone $binary_remote_addr zone=mylimit:10m rate=1r/s;

      server {
          location / {
              limit_req zone=mylimit burst=5;
          }
      }
  }
  ```

#### **7. Advanced Techniques and Best Practices**

- **Use Nginx with Docker:**
  ```sh
  docker run --name nginx -p 80:80 -v /my/custom/nginx.conf:/etc/nginx/nginx.conf:ro nginx
  ```

- **Set up Nginx as a mail proxy:**
  ```nginx
  mail {
      server_name mail.example.com;
      auth_http   localhost:9000/cgi-bin/auth;
      proxy_pass_error_message on;

      server {
          listen     25;
          protocol   smtp;
          smtp_auth  login plain;
      }
  }
  ```

- **Serve static files:**
  ```nginx
  server {
      listen 80;
      server_name static.example.com;

      location / {
          root /var/www/static;
      }
  }
  ```

- **Prevent DDOS attacks with Nginx:**
  ```nginx
  http {
      limit_conn_zone $binary_remote_addr zone=conn_limit_per_ip:10m;

      server {
          limit_conn conn_limit_per_ip 10;

          location / {
              # Configuration
          }
      }
  }
  ```

- **Use geo-blocking:**
  ```nginx
  http {
      geo $geo {
          default 1;
          127.0.0.1 0;
      }

      server {
          if ($geo) {
              return 403;
          }

          location / {
              # Configuration
          }
      }
  }
  ```

This comprehensive list of Nginx CLI commands covers basic operations, configuration management, security measures, load balancing, caching, performance optimization, and advanced techniques to ensure you can manage an Nginx server effectively.
