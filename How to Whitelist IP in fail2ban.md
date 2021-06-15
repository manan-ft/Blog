## How to Whitelist IP in fail2ban

[Detailed Description of how fail2ban works](https://www.digitalocean.com/community/tutorials/how-fail2ban-works-to-protect-services-on-a-linux-server)

**Symptoms**

1. Only some user(s) are not able to access the ERPNext instance, while others can
2. ERR_CONNECTION_REFUSED OR THE SITE CANNOT BE REACHED

**Issue**

fail2ban Bans certain IP temporarily for security reasons, need to whitelist the IP to avoid this issue

**Solution**

1. Find the IP of the user: Login via Admin ID to ERPNext instance > Go to User List > look for value in last_ip
1. Check if this IP was banned by fail to ban by running this command `sudo zgrep 'Ban' /var/log/fail2ban.log`
1. Whitelist the IP by adding this IP in `nano  /etc/fail2ban/jail.conf` , you will need to login as root to make changes to this file

Search for the following section.  It should be near the top of the file:

[DEFAULT]

`ignoreip = 127.0.0.1/8`

Add your IP address to the ignoreip line.  This line is space delimited.  so like this:

ignoreip = 127.0.0.1/8 192.168.0.1 8.8.8.8

4. Switch user to frappe and Restart fail2ban `service fail2ban restart`

### Long Term Fix

**Reason for failure**

[Fail2ban with Nginx](https://www.digitalocean.com/community/tutorials/how-to-protect-an-nginx-server-with-fail2ban-on-ubuntu-14-04)

Failure regex in following file /etc/fail2ban/filter.d/nginx-http-auth.conf

`failregex = ^ \[error\] \d+#\d+: \*\d+ user "(?:[^"]+|.*?)":? (?:password mismatch|was not found in "[^\"]*"), client: <HOST>, server: \S*, request: "\S+ \S+ HTTP/\d+\.\d+", host: "\S+"(?:, referrer:`

is banning the IP because of following entry in the log `sudo zgrep '103.82.158.98' /var/log/nginx/error.log`

`2020/08/13 14:38:28 [warn] 889#889: *721820 a client request body is buffered to a temporary file /var/lib/nginx/body/0000004311, client: 103.82.158.98, server: staging, request: "POST /api/method/upload_file HTTP/1.1", host: "erp.videohms.in", referrer: "https://erp.videohms.in/desk"`

It seems that a Client Request Body is buffered to a temp file by Nginx on loading the Desk, this warning is black listed by fail2ban regex

[Possible Solution:increase 
client_body_buffer_size 1M;
    client_max_body_size 1M;](https://serverfault.com/a/733742)
    
    
[Using Dynamic DNS Service](https://discuss.erpnext.com/t/instance-refuses-ip-address-at-intervals/38850/6?u=manan_shah)
