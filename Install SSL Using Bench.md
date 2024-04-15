## Point Sub Domain to Instance IP Address
1. Take the IP Address of the instance, let us say it is http://167.71.237.6/
2. Login to the Domain Service providers Login like GoDaddy or BigRock
3. Go to Manage Domain and add A Record for the SubDomain
4. Type = A, Host=`erp` (or any sub-domain that you want to use like erpnext.harnaamzippers.com), Points to=`IP Address mentioned in Step 1`, TTL=`Custom`,`600` and Save the A Record
5. Once the Mapping is done, after 2-5 mins, check from the Browser if the new subdomain is redirecting to the original IP. For example `erpnext.harnaamzippers.com` should redirect to the ERPNext instance's IP without any issues.

##[Add Custom-Domain to Site](https://frappeframework.com/docs/user/en/bench/guides/adding-custom-domains)
1. `bench setup add-domain <your-domain.your-website.com>`
2. Enter the Site Name when prompted
3. [Setup Multi-tenancy](https://frappeframework.com/docs/user/en/bench/guides/setup-multitenancy#dns-based-multitenancy)
  a. `bench use <your_site_name>`
  b. `bench config dns_multitenant on`
4. [Install CertBot](https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal)
  a. `sudo apt update`
  b. `sudo apt install snapd`
  c. `sudo apt-get remove certbot`
  d. `sudo snap install --classic certbot`
5. [Install SSL using Bench](https://frappeframework.com/docs/user/en/bench/guides/lets-encrypt-ssl-setup#custom-domains)
  a. `sudo -H bench setup lets-encrypt [site-name] --custom-domain [your-domain.your-website.com]`
''' 
Running this will stop the nginx service temporarily causing your sites to go offline
Do you want to continue? [y/N]: y
$ sudo systemctl stop nginx
$ /snap/bin/certbot  --config /etc/letsencrypt/configs/[your-domain.your-website.com].cfg certonly
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): [your-email-address]

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.4-April-3-2024.pdf. You must agree in
order to register with the ACME server. Do you agree?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: N
Account registered.
Requesting a certificate for [your-domain.your-website.com]

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/[your-domain.your-website.com]/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/[your-domain.your-website.com]/privkey.pem
This certificate expires on 2024-07-dd.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
nginx.conf already exists and this will overwrite it. Do you want to continue? [y/N]: y
$ sudo systemctl start nginx
Setting Up cron job to Renew lets-encrypt every month

'''
