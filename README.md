# Home-Network-creation
A system of file sharing between my devices at home.


Fixed IP for One Device on a Shared WiFi Network

This guide explains how to give one device (such as a PC) a permanent, unchanging IP address on your local network, while every other device on the same WiFi continues to receive IP addresses automatically as normal.

Why do this

Some situations call for a device to always have the same local IP address. Examples include running a home server, port forwarding for gaming or remote access, setting up network printers, or any setup where other devices or scripts need to reliably find a particular machine on the network.

The cleanest way to achieve this is through your router, not by manually configuring the PC itself.

Method 1: Router DHCP Reservation (Recommended)

This method lets the router keep assigning addresses automatically to everything else, while always giving your chosen device the same address.

Step 1: Find the device's MAC address

On Windows, open Command Prompt and run:

ipconfig /all

Look for "Physical Address" under your active network adapter (Ethernet or WiFi). It will look something like:

00-1A-2B-3C-4D-5E

Step 2: Log into your router

Open a browser and go to your router's admin page. Common addresses are:

192.168.0.1
192.168.1.1

Check the sticker on the router itself if unsure. Log in with the admin credentials (also often printed on the router).

Step 3: Locate the reservation setting

Depending on your router brand, this setting may appear under different names:

	•	DHCP Reservation
	•	Address Reservation
	•	MAC IP Bind
	•	ARP Binding
	•	Static DHCP

They all do the same job: tie a MAC address to a fixed IP.

Step 4: Enter the details

	•	MAC address: paste the address from Step 1. Note that Windows separates it with dashes (00:1A:2B:3C:4D:5E uses colons, 00-1A-2B-3C-4D-5E uses dashes), but most routers expect colons. Convert if needed; many router fields accept either format or strip the separator automatically.
	•	IP address: choose an address outside your router's dynamic DHCP pool (for example, something like 192.168.1.50 if your pool starts higher).

Step 5: Save and reboot

Save the setting, then restart the device whose MAC address you just entered. It will now always receive the same IP, while other devices continue getting addresses dynamically as usual.