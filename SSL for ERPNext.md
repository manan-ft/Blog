# SSL Setup for ERPNext

To get SSL working you need two things

1. Private Key
2. Certificate

Both of these can be generated using [Lets
Encrypt](https://www.linode.com/docs/security/ssl/install-lets-encrypt-to-create-ssl-certificates/).
Key thing to do before is ensure that Nginx is stopped.

Use `sudo /etc/init.d/nginx stop` to turnoff Nginx. Perform
following steps

1. `sudo git clone https://github.com/letsencrypt/letsencrypt /opt/letsencrypt`
1. `cd /opt/letsencrypt`
1. `sudo certbot --nginx -d your_domain -d your_domain`
    1. Make sure to replace `example.com` with valid domain-subdomain you want
1. This will generate following important files
    1. Private Key: `/etc/letsencrypt/live/example.com/privkey.pem`
    1. Chained Certificate: `/etc/letsencrypt/live/example.com/fullchain.pem`

Once these files have been generated follow the [Digital Ocean Article](https://www.digitalocean.com/community/tutorials/how-to-install-an-ssl-certificate-from-a-commercial-certificate-authority) link above and make
following changes to Nginx `nginx.conf` file

1. Change listen block to ` listen 443 ssl;`
1. Add following lines
    ```
    server_name example.com;
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    ```
1. Ensure we're using secure connection
    ```
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    ssl_ciphers 'EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH';
    ```
1. Redirect all HTTP traffic to HTTPs
    ```
    server {
        listen 80;
        server_name example.com;
        rewrite ^/(.*) https://example.com/$1 permanent;
    }
    ```
1. Also install following package `sudo apt-get install libssl1.0-dev` {to solve the WKHTMLTOPDF's Print Issue}
1. Test Nginx's configration and restart
    ```
    bench restart
    sudo nginx -t
    sudo /etc/init.d/nginx restart
    ```

## Steps to Renew SSL

```sh
    sudo /etc/init.d/nginx stop
    cd /opt/letsencrypt/
    sudo -H ./letsencrypt-auto certonly --standalone --renew-by-default -d gtlerp.globaltradelinks.biz
    sudo /etc/init.d/nginx start
```
