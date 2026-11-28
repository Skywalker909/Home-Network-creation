# Activity Report: Configuring a Fixed IP Address on a Home Network

## 1. Objective

To configure a Windows PC on a home WiFi network so that it always receives the same local IP address, while all other devices on the network continue to receive IP addresses automatically from the router's DHCP server.

## 2. Background

Home networks typically assign IP addresses to connected devices dynamically through DHCP (Dynamic Host Configuration Protocol), meaning a device's address can change each time it reconnects. For certain use cases, such as port forwarding, running a local server, remote access, or network printing, a device needs a predictable, unchanging address. This activity documents the process of achieving that without disrupting dynamic addressing for every other device on the network.

## 3. Equipment and Requirements

* A Windows PC connected to the home WiFi network
* Administrative access to the home router (admin login credentials)
* Command Prompt (for retrieving the PC's MAC address)
* A web browser (for accessing the router's admin interface)

## 4. Procedure

### Step 1: Identify the PC's MAC address

Command Prompt was opened and the following command was run:

```
ipconfig /all
```

The "Physical Address" value under the active network adapter (WiFi or Ethernet) was recorded. Windows presents this value in dash-separated format, for example:

```
00-1A-2B-3C-4D-5E
```

### Step 2: Access the router's admin interface

A browser was pointed at the router's admin address, typically one of:

```
192.168.0.1
192.168.1.1
```

Login was completed using the router's admin credentials.

### Step 3: Locate the address reservation feature

Depending on router brand, this feature may be labeled differently. Equivalent names encountered include:

* DHCP Reservation
* Address Reservation
* MAC IP Bind
* ARP Binding
* Static DHCP

All of these perform the same underlying function: binding a specific MAC address to a specific IP address.

### Step 4: Enter the MAC address and desired IP

The MAC address recorded in Step 1 was entered into the router's reservation field. Since most routers expect colon-separated MAC addresses rather than Windows' dash-separated format, the value was converted as follows:

| Windows format | Router format |
|---|---|
| 00-1A-2B-3C-4D-5E | 00:1A:2B:3C:4D:5E |

An IP address outside the router's dynamic DHCP pool range was chosen, to avoid any future conflict with addresses the router assigns automatically to other devices.

### Step 5: Save and reboot

The reservation was saved, and the PC was rebooted to apply the new fixed address.

## 5. Result

The PC now receives the same IP address every time it connects to the WiFi network, while all other connected devices continue to be assigned addresses dynamically by the router as before. No manual configuration was required on the PC itself, since the reservation is enforced at the router level.

## 6. Alternative Method Considered: Manual Static IP

A manual static IP, configured directly on the PC (Settings > Network and Internet > Edit IP settings > Manual), was considered as an alternative. This was not used as the primary method because it carries a risk: if an IP address is chosen that falls inside the router's dynamic pool, a future conflict could occur when the router assigns that same address to another device. The router-level reservation method avoids this risk entirely and was therefore preferred.

## 7. Conclusion

A router-based DHCP/address reservation was successfully used to assign a fixed IP address to a single device on the network, without affecting dynamic address assignment for any other device. This approach is recommended over a manually configured static IP, since it is managed centrally by the router and eliminates the risk of address conflicts.

## 8. Notes

* Router menu naming varies significantly by manufacturer (TP-Link, Tenda, MERCURY, Netgear, and others), so the exact steps may need slight adaptation depending on the specific router model used.
* The admin login address and credentials are often printed on a label on the router itself.

## 9. Follow-On Activity: Connecting to the SMB Server from Other Devices

With the PC now reachable at a fixed IP address, an SMB (file sharing) server was set up on it so that other devices on the home network could browse and stream its shared folders. The following steps document how each device type was connected.

### 9.1 iPhone (using the Files app)

1. Open the **Files** app.
2. Tap **Browse** at the bottom, then tap the **three-dot menu** (or the "..." icon) in the top right corner.
3. Select **Connect to Server**.
4. Enter the server address in the form:

   ```
   smb://<PC's fixed IP address>
   ```

   For example: `smb://192.168.1.50`
5. Tap **Connect**, then choose **Registered User** and enter the PC's Windows username and password (or **Guest**, if guest access was enabled on the share).
6. The shared folder now appears under **Shared** in the Files app sidebar, and can be browsed, previewed, or have media played directly from it.

### 9.2 Android (using a file manager that supports SMB/CIFS)

The default Google Files app has limited SMB support, so a file manager with network storage support (such as **CX File Explorer** or **Solid Explorer**) was used instead.

1. Install a file manager that supports SMB/CIFS from the Play Store.
2. Open the app and look for an option such as **Add Network Storage** or **New Connection**.
3. Select **SMB/CIFS** as the connection type.
4. Enter the following details:
   * **Server address:** the PC's fixed IP address (e.g. `192.168.1.50`)
   * **Share name:** the name of the shared folder on the PC
   * **Username / Password:** the PC's Windows login credentials (or leave blank for guest access, if enabled)
5. Save the connection. The shared folder now appears as a browsable network location within the app.

### 9.3 Android TV (streaming media via VLC or Kodi)

For streaming video and audio directly to the TV, a media player app with SMB support was used, since the built-in Android TV file browser does not support network shares natively.

**Using VLC for Android TV:**

1. Install **VLC** from the Google Play Store on the Android TV device.
2. Open VLC and select **Network** from the sidebar.
3. Choose **Local Network** to auto-detect the share, or manually enter:

   ```
   smb://<PC's fixed IP address>/<share name>
   ```
4. Enter the username and password when prompted.
5. Browse to the desired media file and select it to begin streaming.

**Using Kodi (alternative):**

1. Install **Kodi** on the Android TV device.
2. Go to **Settings > Media > Add Source**.
3. Select **Browse**, then **Windows network (SMB)**.
4. Enter the server IP address, share name, and credentials, or browse to the share directly.
5. Save the source, then access it from the Kodi home screen to play back media.

### 9.4 Result

All three device types (iPhone, Android phone, and Android TV) were successfully able to connect to the SMB share hosted on the fixed-IP PC, confirming that the fixed IP address set up in Sections 1 to 7 provided a stable, reliable address for other devices on the network to consistently reach the server.
