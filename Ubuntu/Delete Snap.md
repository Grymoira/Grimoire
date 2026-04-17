> This instruction is based on Ubuntu 24.04 LTS System

Uninstall and banned the snap completely, you need to perform the following step by step.

```bash
   snap list
   
   sudo snap remove --purge appnames
   sudo apt autoremove --purge snapd
```

*ATTENTION* : You should delete the items in the snap list one by one.

```bash
   rm -rf ~/snap 
   sudo rm -rf /var/snap 
   sudo rm -rf /var/snap 
   sudo rm -rf /var/lib/snapd
   sudo nano /etc/apt/preferences.d/nosnap.pref
```

And if you want to install and use the `firefox` again, you can:

```bash
sudo add-apt-repository ppa:mozillateam/ppa
sudo apt update
sudo apt install firefox
```

