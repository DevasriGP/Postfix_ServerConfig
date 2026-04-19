# Change Request: Postfix Relay Configuration using SendGrid

## Objective

Configure Postfix to route outbound emails via SendGrid SMTP relay to improve external email delivery and reliability.

---

## Change Type

Standard Change – Configuration Update

---

## Target System

* Service: Postfix
* Server: <your-server-name>

---

## Pre-Implementation Tasks

### 1. Snapshot Backup

* Coordinate with Wintel team to take a full VM snapshot
* Confirm snapshot completion before proceeding

---

### 2. Backup Existing Configuration

```bash
cp -pr /etc/postfix/main.cf /etc/postfix/main.cf_bkp_<date>
```

---

## Implementation Steps

### 3. Update Postfix Configuration

```bash
vim /etc/postfix/main.cf
```

Append:

```
relayhost = [smtp.sendgrid.net]:587
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
sender_canonical_maps = pcre:/etc/postfix/sender_canonical.regexp
sender_canonical_classes = envelope_sender
```

---

### 4. Configure SMTP Authentication

Edit:

```bash
/etc/postfix/sasl_passwd
```

Add:

```
[smtp.sendgrid.net]:587 apikey:<SendGrid_API_Key>
```

Run:

```bash
postmap /etc/postfix/sasl_passwd
chmod 600 /etc/postfix/sasl_passwd*
```

---

### 5. Restart Postfix Service

```bash
systemctl restart postfix
```

---

## Validation Steps

### 6. Verify Service Status

```bash
systemctl status postfix
```

---

### 7. Send Test Email

```bash
echo "Test mail" | mail -s "Test" user@example.com
```

---

### 8. Check Logs

```bash
tail -f /var/log/maillog
```

Confirm:

* Mail sent successfully
* No authentication or relay errors

---

## Post-Implementation Checks

* Application team validates email functionality
* No errors in logs
* Stable mail flow

---

## Rollback Plan

If any issue occurs:

1. Restore backup:

```bash
cp -pr /etc/postfix/main.cf_bkp_<date> /etc/postfix/main.cf
```

2. Restart service:

```bash
systemctl restart postfix
```

3. Validate mail flow

---

## Risks & Mitigation

| Risk                         | Mitigation              |
| ---------------------------- | ----------------------- |
| Incorrect SMTP configuration | Validate before restart |
| Authentication failure       | Verify API key          |
| Service disruption           | Keep backup ready       |

---

## Expected Outcome

* Postfix successfully relays emails via SendGrid
* Improved email delivery reliability
* No service disruption

