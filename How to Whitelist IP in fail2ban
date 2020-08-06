## How to Whitelist IP in fail2ban

#Symptoms
1. Only some user(s) are not able to access the ERPNext instance, while others can
2. ERR_CONNECTION_REFUSED OR THE SITE CANNOT BE REACHED

#Issue
fail2ban Bans certain IP temporarily for security reasons, need to whitelist the IP to avoid this issue

#Solution
1. Find the IP of the user: Login via Admin ID to ERPNext instance > Go to User List > look for value in last_ip
1. Check if this IP was banned by fail to ban by running this command `sudo zgrep 'Ban' /var/log/fail2ban.log`
1. Whitelist the IP by adding this IP in `nano  /etc/fail2ban/jail.conf` , you will need to login as root to make changes to this file

Search for the following section.  It should be near the top of the file:

[DEFAULT]

# “ignoreip” can be an IP address, a CIDR mask or a DNS host. Fail2ban will not
# ban a host which matches an address in this list. Several addresses can be
# defined using space separator.
ignoreip = 127.0.0.1/8

Add your IP address to the ignoreip line.  This line is space delimited.  so like this:

ignoreip = 127.0.0.1/8 192.168.0.1 8.8.8.8

1. Switch user to frappe and Restart fail2ban `service fail2ban restart`

