# Restart services
systemctl restart sshd

# Updating installed software
apt update
apt list --upgradeable
apt upgrade

# Updating snap packages
sudo snap refresh --list

# List systemctl timers
systemctl list-timers

# Add firewall rules for Apache
ufw allow 'Apache Secure'
