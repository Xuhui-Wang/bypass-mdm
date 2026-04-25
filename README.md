# 🚀 Bypass MDM for macOS (Up to Sequoia)

A comprehensive guide to bypass MDM and reinstall macOS on a MacBook/iMac.
Follow the steps below to ensure a successful setup.

---

## 📥 Tools & Resources
- **[IPSW Files](https://ipsw.me/product/Mac)**: Download macOS firmware.
- **[Apple Configurator 2](https://apps.apple.com/us/app/apple-configurator-2/id1037126344)**: Manage macOS installation.

---

## ⚙️ Automated Script

Use the script below to bypass MDM automatically:

```bash
curl https://raw.githubusercontent.com/Xuhui-Wang/bypass-mdm/refs/heads/main/mdm.sh -o bypass.sh && chmod +x ./bypass.sh && ./bypass.sh
```

---

## 🖥️ I. Reinstall macOS Using IPSW

1. **Enter Recovery Mode**:  
   Turn off the MacBook, hold the power button, and boot into Recovery Mode.

2. **Erase Disk**:  
   Open **Disk Utility**, select "Macintosh HD", and erase it.

3. **Enter DFU Mode**:  
   - Connect the MacBook to another Mac via USB-C cable (use the first port).  
   - Power off the MacBook.  
   - Press **Control (L) + Option (L) + Shift (R) + Power** for 10 seconds.  
   - Release all keys except Power and hold it for another 10 seconds.  

4. **Restore macOS**:  
   - Open **Apple Configurator 2** on the second Mac.  
   - Drag and drop the IPSW file into it.  
   - Wait ~10 minutes for installation to complete.  
   - The MacBook will restart into macOS.

---

## 🔒 II. Bypass Network in all macOS Versions Setup Assistant

1. **Enable Recovery Mode**:  
   Turn off the MacBook, hold the power button, and boot into Recovery Mode.

2. **Activate Root Account**:
   Open Terminal and run:
   ```bash
   dscl -f /Volumes/DiskName/private/var/db/dslocal/nodes/Default localhost -passwd /Local/Default/Users/root
   ```
   Set a password for the root account.
   
   **Note**: Adjust the DiskName based on the macOS installation location.

4. **Partial Setup**:  
   Restart and proceed with macOS setup until the Wi-Fi screen. **Do not connect to Wi-Fi**.

5. **Access Terminal**:  
   Press **Command + Option + Control + T** to open Terminal.

6. **Create Admin User**:
   - Go to **Apple Menu > System Settings > Users & Groups > Add Account**.
   - Authenticate as root.  
   - Create a new admin account.

7. **Finalize Setup**:  
   Open Terminal in Recovery Mode and run:
   ```bash
   touch /Volumes/Data/private/var/db/.AppleSetupDone
   ```

8. **Disable Root**:  
   After setup, disable root:
   ```bash
   dsenableroot -d
   ```

---

## 🚫 III. Block DEP Notifications

1. **Initial Setup Without Wi-Fi**.  
   Complete macOS setup without connecting to Wi-Fi.

2. **Block MDM Hosts**:
   Open Terminal and run:
   ```bash
   sudo -i
   echo "0.0.0.0 iprofiles.apple.com" >> /etc/hosts
   echo "0.0.0.0 mdmenrollment.apple.com" >> /etc/hosts
   echo "0.0.0.0 deviceenrollment.apple.com" >> /etc/hosts
   ```

3. **Restart and Connect**:  
   Reboot, connect to Wi-Fi, and use your Mac as normal.

---

## 🔄 IV. Update macOS on MDM MacBook

1. **Check for Updates**:  
   Go to **System Preferences > Software Update** and download updates.

2. **Disconnect Wi-Fi**:  
   Forget the Wi-Fi or block the MacBook's MAC address.

3. **Reboot to Install**:  
   Reboot and install updates. Skip network connection during the setup.

4. **Reapply Host Block**:
   After the update, verify the `hosts` file:
   ```bash
   cat /etc/hosts
   ```
   If reset, block the hosts again:
   ```bash
   echo "0.0.0.0 iprofiles.apple.com" >> /etc/hosts
   echo "0.0.0.0 mdmenrollment.apple.com" >> /etc/hosts
   echo "0.0.0.0 deviceenrollment.apple.com" >> /etc/hosts
   ```

5. **Reconnect Wi-Fi**:  
   Reconnect and enjoy using your Mac.

---

## 📬 Questions?

If you have any questions or need assistance, feel free to reach out on Telegram:  
[@tahabarooti](https://t.me/tahabarooti)
