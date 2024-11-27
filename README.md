# Nginx servera palaišanas pamācība
---
## Sistēmas prasības:

- **Operētājsistēma**: Ubuntu / Debian /  CentOS / Amazon Linux.
- **Nginx**: versija 1.26.2.
- **Certbot**: versija 3.0.1.
- **Jābūt root piekļuve serverim**.
---

## Instrukcijas soļi
## 1. DuckDNS konta izveide

### 1.1 Piereģistrēšanās DuckDNS

1. Dodies uz [DuckDNS](https://www.duckdns.org/index.jsp).
2. **Piereģistrējies**
3. Pēc veiksmīgas reģistrācijas lietotājs tiks pārcelts uz lapu kur spēs izveidot domēnu.

### 1.2 Izveido DuckDNS domēnu

1. **Izvēlies domēna nosaukumu** (piemēram, `kasparskusins`).
2. **Pievieno savu publisko IP adresi**, lai domēnu saistītu ar serveri.

### 2. Lejuplādēt Nginx

1. **Atsvaidzināt sistēmu**:
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```

2. **Lejuplādēt Nginx**:
    ```bash
    sudo apt install nginx -y
    ```

3. **Pārliecināties vai Nginx darbojas**:
    ```bash
    sudo systemctl status nginx
    ```

4. **Ieslēgt Nginx automātisko palaišanu**:
    ```bash
    sudo systemctl enable nginx
    ```

### 3. Lejuplādēt Certbot un Nginx pluginu

1. **Lejuplādēt Certbot un Nginx pluginu**:
    ```bash
    sudo apt install certbot python3-certbot-nginx -y
    ```

2. **Pārbaudīt Lejuplādi**:
    ```bash
    certbot --version
    ```

### 4. Konfigurēt Nginx serveri

1. **Atvērt un rediģēt Nginx konfigurācijas failu**:
    ```bash
    sudo nano /etc/nginx/sites-available/default
    ```

2. **Server_name nomainīt uz esošo domēnu (mūsu gadījumā `kasparskusins.duckdns.org`)**:
    ```nginx
    server {
        listen 80;
        listen [::]:80;
        server_name kasparskusins.duckdns.org;
        root /var/www/html;
        index index.html index.htm;
        location / {
            try_files $uri $uri/ =404;
        }
    }
    ```

3. **Pārbaudīt Nginx failu saturu - vai viss atbilst**:
    ```bash
    sudo nginx -t
    ```

4. **Restartēt Nginx, lai piemērotu izmaiņas**:
    ```bash
    sudo systemctl restart nginx
    ```

### 5. Iegūt SSL sertifikātu ar Certbot

1. **Palaist Certbot ar Nginx pluginu**:
    ```bash
    sudo certbot --nginx -d kasparskusins.duckdns.org
    ```

2. **Izvēlies iespēju (parasti "1")**, lai uzstādītu un instalētu SSL sertifikātu.

3. **Pārbaudīt, vai HTTPS ir pareizi konfigurēts**:
    Atver pārlūkprogrammu un dodies uz `https://kasparskusins.duckdns.org`. Ja viss ir pareizi, redzēsi drošu HTTPS savienojumu.

### 6. Automātiska sertifikāta atjaunošana

>Certbot jau automātiski iestatīs uzdevumu, lai atjaunotu sertifikātu katru dienu. 
---
# Secinājumi
**Šī pamācība palīdzēja apgūt un iemācīties šādas prasmes:**
* Certbot izmantošana: Iegūts SSL sertifikāts caur Certbot.
* AWS (Amazon Web Services) izmantošana: EC2 instance un tā apvienošana ar DuckDNS.


