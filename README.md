### Install Wireguard vpn on vps server

1. Connect to your VPS server via SSH using a terminal or SSH client. You will need to have root or sudo access to the server.
2. Update the server packages and install the necessary dependencies by running the following commands:
```sh
sudo apt update
sudo apt upgrade
sudo apt install wireguard
```
3. Generate the WireGuard private and public keys using the wg command:

```sh
sudo wg genkey | sudo tee /etc/wireguard/privatekey | wg pubkey | sudo tee /etc/wireguard/publickey
```
This command generates a private key and saves it to /etc/wireguard/privatekey and a public key to /etc/wireguard/publickey.

4. Create a new WireGuard configuration file. You can name this file whatever you like, but it should have a .conf file extension. For example, you can create a file called wg0.conf by running:
```sh
sudo nano /etc/wireguard/wg0.conf
```
This command will open the Nano text editor where you can create the configuration file.
5. Inside the configuration file, add the following lines:

```sh
[Interface]
PrivateKey = <private key>
Address = 10.0.0.1/24
ListenPort = 51820

[Peer]
PublicKey = <client public key>
AllowedIPs = 10.0.0.2/32
```
Replace <private key> with the private key generated in step 3 and <client public key> with the public key of the client device you want to connect to the VPN server. You can add multiple [Peer] sections for different clients.
6. Enable IP forwarding on the server by running the following command:
```sh
sudo sysctl -w net.ipv4.ip_forward=1
```
This command allows the server to forward traffic between the client and the internet.

7. Start the WireGuard service and enable it to run on startup:

```sh
sudo systemctl enable --now wg-quick@wg0
```
This command starts the WireGuard service using the configuration file you created in step 4.

#### That's it! You have now successfully installed and configured WireGuard VPN on your VPS server. You can now connect to the server from a client device using the public IP address of the server and the private key you generated in step 3.

