---
title: Fix USB Modem
date: 2025-12-09
draft: false
tags:
- linux
- hardware
---

We have a USB stick with a SIM in it, for sending SMS. When pluging in the new stick, it always was showing up as a CD ROM instead of a usb-serial. This is what we did to solve it:

```bash
dnf install -y usbutils usb_modeswitch python3-pip git

# Setup USB Modem
lsusb
# > Bus 001 Device 004: ID 3566:2001 Mobile Mobile

echo "#!/bin/bash

# Fix USB Mode
/usr/sbin/usb_modeswitch -v 0x3566 -p 0x2001 -X

# Wait 3 seconds than read that stick for usb serials
sleep 3
echo '3566 2001 ff' | sudo tee /sys/bus/usb-serial/drivers/option1/new_id" > /usr/local/sbin/fix-huawei-stick.sh
chmod +x /usr/local/sbin/fix-huawei-stick.sh

echo 'ATTRS{idVendor}=="3566", ATTRS{idProduct}=="2001", RUN+="/usr/local/sbin/fix-huawei-stick.sh"' > /etc/udev/rules.d/99-huawei.rules

udevadm control --reload-rules
udevadm trigger


echo "usbserial
option" > /etc/modules-load.d/sms.conf


reboot
```