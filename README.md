# Linux Administration – Postfix SendGrid Integration

## Overview

This project demonstrates a production-style change implementation for configuring Postfix to relay emails using SendGrid SMTP.

It includes structured steps, validation, and rollback planning.

---

## Project Structure

postfix-sendgrid-integration/
└── change-request.md

---

## What This Project Covers

* Postfix SMTP relay configuration
* Secure authentication setup
* Service restart and validation
* Rollback planning

---

## Implementation Summary

Configured Postfix to route outbound emails via SendGrid:

* Updated `main.cf` with relay configuration
* Configured SMTP authentication using API key
* Restarted service and validated mail flow
* Verified logs for successful delivery

---

## Skills Demonstrated

* Linux System Administration
* Postfix Configuration
* Change Management
* Troubleshooting & Validation
* Backup and Recovery

---

## Tools Used

* Linux
* Postfix
* Git

---

## Outcome

* Email relay successfully configured
* No service disruption
* Verified mail delivery

---

## Author

Devasri GP

