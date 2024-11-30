# Tailscale for Foundry VTT
Create a Tailscale overlay network to allow clients to connect to a Foundry VTT server.  Allows running the Foundry VTT server without opening up any firewall ports to the internet.  No static public IP's or DHCP reservations are needed.  The Tailscale overlay network handles the connection peering via "magic".  

## Prerequisites
- [Foundry VTT](https://foundryvtt.com/) is installed and using the default TCP port of 30000.

## Setup for Admin (person running the Foundry VTT server)
1. Create a Tailscale account on [tailscale.com](https://tailscale.com/).  
2. Copy and paste the contents of `tailscale_acl.hujson` in this repository into the "ACL" page of the Tailscale portal.
3. In the Tailscale portal, go to "Settings" > "Keys" and click "Generate auth key...".  In the "Tags" section, choose `foundry-vtt-server`, then click "Generate key".  Copy the key it displays.  
4. Install [Tailscale](https://tailscale.com/download) on the Foundry VTT server.
5. Open any terminal on Windows/Linux/MacOS.
6. In the terminal, copy and paste the following commands:
```bash
tailscale login --authkey=<key_that_was_copied_from_the_tailscale_portal> --hostname=foundry-server --unattended
tailscale set --auto-update
```
7. Start Foundry VTT on the server (if it's not already running).

## Setup for Clients
The steps below should be repeated for each user that will be remotely connecting to the Foundry server.
1. In the Tailscale portal, the admin should go to "Settings" > "Keys" and click "Generate auth key...".  In the "Tags" section, choose `foundry-vtt-client`, then click "Generate key".  Copy the key it displays.  
2. The admin will send these instructions, the server hostname the admin chose, and the key to each user.
3. On the user's computer, they should install [Tailscale](https://tailscale.com/download).
4. On the user's computer, they should open any terminal.
5. In the terminal, they should copy and paste the following commands:
```bash
tailscale login --authkey=<key_that_the_admin_generated> --hostname=<enter_desired_client_hostname> --unattended
tailscale set --auto-update
```
6. The user should test the connection to the server. On Windows the command (PowerShell) is `tnc foundry-server -Port 30000` with a response that includes the text "TcpTestSucceeded: True".  On Linux/MacOS the command is `nc -zv foundry-server 30000` with a response that includes the text similar to: "Connection to foundry-server (100.?.?.?) 30000 port [tcp/http] succeeded!".  
7. The user should now be able to open a web browser and connect to the Foundry VTT server at the address [http://foundry-server:30000](http://foundry-server:30000).   
