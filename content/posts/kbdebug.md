---
date: '2026-09-13T13:47:54+03:00'
draft: false
title: 'Ajazz K870T: Function Key Fix'
---

My keyboard, `Ajazz K870T`, had some incompability issues on Linux.
Especially, the function keys (F1, F2 ... F12) did not work. The 
solution is based on
[this comment](https://www.technopat.net/sosyal/konu/ajazz-k870t-pro.4083821/#post-30273887).

1. This keyboard uses `hid_apple` driver and that causes issues. Confirm it: `lsmod | grep hid_apple`.
If it returns anything, there is a high chance that the keyboard uses it.
2. Test the fix: `echo 2 | sudo tee /sys/module/hid_apple/parameters/fnmode`.
3. If function keys now work correctly, make it permanent: 
```
echo "options hid_apple fnmode=2" | sudo tee /etc/modprobe.d/hid_apple.conf
sudo update-initramfs -u
sudo reboot
```
