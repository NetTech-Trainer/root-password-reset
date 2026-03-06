# root-password-reset
---

# Reset Root Password using GRUB (RHEL / CentOS / Rocky / AlmaLinux)

This guide explains how to **reset the root password from the GRUB boot menu** when the root password is forgotten.

The process involves **booting into single-user mode**, changing the password, and then relabeling SELinux.

⚠️ Requires **physical or console access to the server/VM**.

---

# Step 0: Enable GRUB Menu

Edit the GRUB configuration file.

```bash
sudo vi /etc/default/grub
```

Find or add the following lines:

```ini
GRUB_TIMEOUT=5
GRUB_TIMEOUT_STYLE=menu
```

This ensures the **GRUB boot menu appears during startup**.

---

# Step 1: Update GRUB Configuration

Apply the configuration changes.

```bash
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

---

# Step 2: Disable Hidden GRUB Menu

Run the following command to make sure the GRUB menu is visible at boot.

```bash
sudo grub2-editenv - unset menu_auto_hide
```

---

# Step 3: Enter GRUB Edit Mode

1. Reboot the system
2. When the **GRUB menu appears**, press **`e`** to edit the boot entry.

Find the line starting with:

```
linux
```

At the end of the line, add:

```
rw init=/bin/bash
```

This boots the system into a **root shell without requiring authentication**.

---

# Step 4: Boot the System

Press:

```
Ctrl + X
```

The system will boot into a **root shell environment**.

---

# Step 5: Change Root Password

Run:

```bash
passwd root
```

Enter the new password:

```
New password:
Retype new password:
```

---

# Step 6: Trigger SELinux Relabel

Create the relabel file:

```bash
touch /.autorelabel
```

This ensures proper **SELinux context restoration after reboot**.

---

# Step 7: Exit from Bash Shell

Start the system normally:

```bash
exec /sbin/init
```

---

# Step 8: Reboot the System

```bash
reboot
```

The system will reboot and **SELinux relabeling will start automatically**.

⚠️ This process may take several minutes depending on the system.

---

# Important Notes

* Requires **physical or console access**
* SELinux relabeling is necessary to prevent permission issues
* Works on:

  * RHEL
  * CentOS
  * Rocky Linux
  * AlmaLinux

---

# Quick Command Summary

| Step             | Command                                       |
| ---------------- | --------------------------------------------- |
| Edit GRUB config | `sudo vi /etc/default/grub`                   |
| Update GRUB      | `sudo grub2-mkconfig -o /boot/grub2/grub.cfg` |
| Show GRUB menu   | `sudo grub2-editenv - unset menu_auto_hide`   |
| Boot into bash   | add `rw init=/bin/bash`                       |
| Change password  | `passwd root`                                 |
| SELinux relabel  | `touch /.autorelabel`                         |
| Start system     | `exec /sbin/init`                             |
| Reboot           | `reboot`                                      |

---

# Author

**Sagar**

Linux | AWS | DevOps | Cloud Enthusiast

---
