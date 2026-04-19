# Backup configuration
cp -pr /etc/postfix/main.cf /etc/postfix/main.cf_bkp_<date>

# Edit main configuration
vim /etc/postfix/main.cf

# Add below lines
relayhost = [smtp.sendgrid.net]:587
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
sender_canonical_maps = pcre:/etc/postfix/sender_canonical.regexp
sender_canonical_classes = envelope_sender

# Configure authentication
vim /etc/postfix/sasl_passwd

# Add credentials
[smtp.sendgrid.net]:587 apikey:<API_KEY>

# Secure and hash
postmap /etc/postfix/sasl_passwd
chmod 600 /etc/postfix/sasl_passwd*

# Restart service
systemctl restart postfix

# Validation
systemctl status postfix
echo "Test mail" | mail -s "Test" user@example.com
tail -f /var/log/maillog

# Rollback
cp -pr /etc/postfix/main.cf_bkp_<date> /etc/postfix/main.cf
systemctl restart postfix
