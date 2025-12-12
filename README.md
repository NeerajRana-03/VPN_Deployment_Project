🔐 CloudGate — Manual WireGuard VPN Deployment on AWS

CloudGate is a fully manual, step-by-step WireGuard VPN deployment on an
AWS EC2 instance.
No scripts. No automation.
Every configuration (server + client + routing + firewall) is done
manually to demonstrate real cloud, networking, and Linux skills.

This project is ideal for showcasing knowledge in VPNs, AWS, Linux
administration, firewall security, routing, and troubleshooting.

------------------------------------------------------------------------

🚀 Project Overview

This project sets up a WireGuard VPN server on AWS EC2 to provide
secure, encrypted remote access to cloud resources.

Key capabilities:

-   Manual WireGuard installation & configuration
-   Manual server key generation
-   Manual peer (client) configuration
-   Manual routing & forwarding
-   Manual firewall setup using UFW / iptables
-   Understanding of network flows and encryption

------------------------------------------------------------------------

🧩 Architecture Overview

Client Device | | Encrypted VPN Tunnel (UDP 51820) | [ AWS EC2 Ubuntu
Server Running WireGuard ] | |–> Access to Private AWS Subnets /
Resources

------------------------------------------------------------------------

🛠️ Requirements

-   AWS EC2 instance (Ubuntu recommended)
-   Port 51820/udp open in EC2 Security Group
-   Basic Linux CLI knowledge
-   WireGuard installed on client device

------------------------------------------------------------------------

⚙️ Manual Setup — Step by Step

1️⃣ Update System

sudo apt update && sudo apt upgrade -y

------------------------------------------------------------------------

2️⃣ Install WireGuard

sudo apt install wireguard -y

------------------------------------------------------------------------

3️⃣ Generate Server Keys (MANUALLY)

wg genkey | tee server_privatekey | wg pubkey > server_publickey

------------------------------------------------------------------------

4️⃣ Create Server Configuration File

/etc/wireguard/wg0.conf

[Interface] Address = 10.0.0.1/24 ListenPort = 51820 PrivateKey =

PostUp = ufw route allow in on wg0 out on eth0 PostDown = ufw route
delete allow in on wg0 out on eth0

------------------------------------------------------------------------

5️⃣ Enable IP Forwarding

Edit /etc/sysctl.conf:

net.ipv4.ip_forward=1

Apply:

sudo sysctl -p

------------------------------------------------------------------------

6️⃣ Start WireGuard

sudo wg-quick up wg0
sudo systemctl enable wg-quick@wg0

------------------------------------------------------------------------

👤 Manual Client (Peer) Setup

7️⃣ Generate Client Keys

wg genkey | tee client_privatekey | wg pubkey > client_publickey

------------------------------------------------------------------------

8️⃣ Add Client to Server Configuration

Add to /etc/wireguard/wg0.conf:

[Peer] PublicKey = AllowedIPs = 10.0.0.2/32

------------------------------------------------------------------------

9️⃣ Create Client Config File

client.conf:

[Interface] PrivateKey = Address = 10.0.0.2/24 DNS = 1.1.1.1

[Peer] PublicKey = Endpoint = :51820 AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25

------------------------------------------------------------------------

🔥 Firewall Rules (Manual Setup)

sudo ufw allow 51820/udp
sudo ufw allow OpenSSH
sudo ufw enable

------------------------------------------------------------------------

🧪 Testing Commands

sudo wg show
ping 10.0.0.2
sudo journalctl -u wg-quick@wg0

------------------------------------------------------------------------

🔒 Security Notes

-   Rotate keys manually
-   Remove unused peers
-   Restrict AllowedIPs
-   Disable root SSH login
-   Limit port exposure

------------------------------------------------------------------------

🎯 Why This Manual Project Matters

Shows real understanding of:

-   Linux internals
-   WireGuard fundamentals
-   Routing & NAT
-   Firewall logic
-   VPN troubleshooting

Perfect for Cloud, Cybersecurity, DevOps roles.

------------------------------------------------------------------------

📬 Contact

Author: Neeraj Rana
GitHub: https://github.com/NeerajRana-03
Email: neerajranaa116@gmail.com
