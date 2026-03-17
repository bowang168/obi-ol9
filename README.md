# Oracle Linux 9 OBI

## Table of Contents

- [Download Full OS image](#Download-Full-OS-image)
- [Download OBI Kickstart OL9 image](#Download-OBI-Kickstart-OL9-image)
- [OBI Setup](#OBI-Setup)
- [Encrypt /home and / with luks1 version](#Encrypt-home-and--with-luks1-version)
- [Download Cisco VPN Client Manually - Optional](#Download-Cisco-VPN-Client-Manually---Optional)
- [OS Compliant with OBI](#OS-Compliant-with-OBI)
- [LUKS key escrow](#LUKS-key-escrow)
- [Customization](#Customization)

### Download Full OS image

[https://www.oracle.com/linux/technologies/oracle-linux-downloads.html](https://www.oracle.com/linux/technologies/oracle-linux-downloads.html)

### Download OBI Kickstart OL9 image

[https://[REDACTED]/ks-ol9](https://[REDACTED]/ks-ol9)

```bash
sudo dd if=obi-ng-kickstart-OL9-x86_64.iso of=/dev/disk4 bs=512k
```


### OBI Setup

[https://[REDACTED]/docs/oracle-linux-workstation/](https://[REDACTED]/docs/oracle-linux-workstation/)

### Encrypt `/home` and `/` with `luks1` version

> During the partitioning phase, set the encryption scheme for the root `/` and `/home` volumes to `luks1`

```bash
# If already luks2, do the following to convert luks2 to luks1, in rescue mode:
sudo cryptsetup luksConvertKey --pbkdf pbkdf2 /dev/nvme0n1p3
sudo cryptsetup convert --type luks1 /dev/nvme0n1p3
sudo cryptsetup luksAddKey /dev/nvme0n1p3
sudo cryptsetup luksKillSlot /dev/nvme0n1p3 1

# after reboot
lsblk --fs
sudo cryptsetup status luks-5e799d14-d726-49ad-b48d-bfd95d697e86
```


### Download Cisco VPN Client Manually - Optional

[https://[REDACTED]](https://[REDACTED])

### OS Compliant with OBI

Download script [https://[REDACTED]/download](https://[REDACTED]/download)

```bash
bash obi-ng-23.1.1.446.run
```


### LUKS key escrow

```bash
# If the obi-ng script failed to complete the LUKS key escrow, then run again:
sudo /opt/desktop/obi-ng.sh -l
```


### Customization

```bash
# sudo without password (replace <your-username> with your actual username)
sudo su -
chmod +w /etc/sudoers
cat >>/etc/sudoers<<EOF
<your-username> ALL=(ALL) NOPASSWD: ALL
EOF
chmod -w /etc/sudoers

# Optional
sudo systemctl disable ipv6
sudo systemctl disable selinux
sudo systemctl disable REDACTED_SERVICE.service
sudo systemctl disable REDACTED_SERVICE.service
sudo systemctl disable chronyd.service

# Install rpms
dnf install gcc make iotop xclip wmctrl powerline-fonts xdotool zsh ltrace \
    whois nmap tcpdump google-noto-sans-fonts

# Remove rpms
dnf remove fprintd fprintd-pam audit rpm-plugin-audit avahi selinux-policy \
    elinux-policy-targeted setroubleshoot setroubleshoot-plugins setroubleshoot-server \
    rpm-plugin-selinux flatpak-selinux evolution-* PackageKit cockpit-packagekit cups

# Installing the Oracle Redwood Branding Fonts and InputMono Fonts
# Download from: https://[REDACTED]:11443/otmm/ux-html/?p=collection/url_selection/40093030
# Download from: https://input.djr.com/download/

sudo cp -R ~/Downloads/oracle_fonts/Fonts/REDACTED_FONT /usr/share/fonts/
sudo fc-cache -f /usr/share/fonts

```
