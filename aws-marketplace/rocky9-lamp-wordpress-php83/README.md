# Rocky Linux 9 LAMP WordPress (PHP 8.3) - Usage Instructions

## What this AMI includes
- OS: Rocky Linux 9.x
- Apache httpd, PHP-FPM (PHP 8.3), MariaDB (version)
- WordPress installed at: /var/www/wordpress
- phpMyAdmin installed (local-only)

## Quickstart (5 minutes)
1) Launch EC2
   - Security Group: allow 80/443 from your IP (or 0.0.0.0/0 for testing)
   - For phpMyAdmin: do NOT open 80/443 publicly if you intend to use it; use SSH tunnel
2) Access
   - http://<public-ip>/  (redirects to setup if not configured)
   - https://<public-ip>/ (self-signed by default; browser warning is expected)
3) Credentials
   - Credentials file: /home/rocky/credentials.toml
   - Created on first boot

## phpMyAdmin access (recommended: SSH tunnel)
- ssh -L 8080:localhost:80 rocky@<public-ip>
- Open: http://127.0.0.1:8080/phpmyadmin
- Login: DB user shown in credentials.toml (wordpress / password)

## HTTPS / Certificates
- Default: self-signed (for “it works out of the box”)
- Recommended: use ACM + ALB, or certbot/your own cert
- Replace certificate paths:
  - /etc/pki/tls/certs/localhost.crt
  - /etc/pki/tls/private/localhost.key

## SELinux notes
- This AMI sets contexts so WordPress works with SELinux enforcing.
- If you customize paths, update contexts accordingly.

## Updates / Maintenance
- OS update / package update procedure
- WordPress update procedure (WP-CLI or UI)
- Backup guidance (EBS snapshot + DB dump)

## Troubleshooting
- httpd not running: systemctl status httpd ; journalctl -u httpd -b
- 443 not listening: check Listen 443 + cert paths + mod_ssl
- Permission/SELinux: ausearch -m AVC -ts recent

