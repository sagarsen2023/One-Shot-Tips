# Creating your very own VPS hosting server

## Pre requisites
- A local pc/laptop where Ubuntu is installed. You cam install Ubuntu Server or Ubuntu Desktop on your choice.
- A wifi connection with static IP.
- And a lot of excitement.

## Setup
We are not going to discuss installation of ubuntu. This guide is for post intstalation of Ubuntu.
- Run the following commands one by one in your terminal in your local computer.
  1. `sudo apt update`
  2. `sudo apt install openssh-server`
  3. `sudo systemctl enable ssh`
  4. `sudo adduser <your_username_here>` The username that you will connect through SSH. And follow the guide from the terminal for creating the user.
  5. `sudo usermod -aG sudo <your_username_here>`
  6. `ssh-keygen -t ed25519 -c "your_email@domain.com"`
  7. You can check status by using `sudo systemctl status ssh`
  8. To check the port exposed enter `ip adder show`. Under the `inet` you will see a local IP where the SSH is connected.
 
- Our setup for the local SSH remote access is now set up. If you open terminal and type `ssh <your_username_here>@<port_from_inet>` from another fully independent PC/laptop you will be connected to your local Ubuntu server.
- If you want to access your local server from a WAN you have to do **PORT FORWARDING**.
- Steps to do **PORT FORWARDING**:
  1. Open your router local IP: Mostly it stays `192.168.1.1` (you can check it for your router in its user manual)
  2. Navigate to that IP from your browser and login.
  3. Search for `**NAT FORWARDING** OR **PORT FORWARFING**`.
     If asked for **Service type**, select **HTTP**
     Enter the IP you want to forward: Here type the local IP taken from INET.
     Select Protocol **TCP** and for PORT enter **22**.

Your setup is done.
Now from any computer type `ssh <your_username_here>@<your_public_static_ip>`
Type your password and enjoy...
