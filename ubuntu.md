# Restart services
systemctl restart sshd

# List packages that need upgrading
apt list --upgradeable

# List systemctl timers
systemctl list-timers

# Add firewall rules for Apache
ufw allow 'Apache Secure'
