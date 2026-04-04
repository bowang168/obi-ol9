# Oracle Linux 9 OBI

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-support-yellow?style=flat-square&logo=buy-me-a-coffee)](https://buymeacoffee.com/bowang168)
[![Sponsor](https://img.shields.io/badge/GitHub%20Sponsors-sponsor-ea4aaa?style=flat-square&logo=github-sponsors)](https://github.com/sponsors/bowang168)

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

[OBI kickstart URL removed]

```bash
sudo dd if=obi-ng-kickstart-OL9-x86_64.iso of=/dev/disk4 bs=512k
```


### OBI Setup

Internal documentation reference (removed)

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
sudo cryptsetup status luks-<your-luks-uuid>
```


### Download Cisco VPN Client Manually - Optional

[Oracle Internal VPN Portal]

### OS Compliant with OBI

Download the OBI compliance script from your internal portal and run it:

```bash
bash obi-ng-<version>.run
```


### LUKS key escrow

```bash
# If the OBI script failed to complete the LUKS key escrow,
# re-run the escrow step per your internal documentation.
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
sudo systemctl disable chronyd.service

# Install rpms
dnf install gcc make iotop xclip wmctrl powerline-fonts xdotool zsh ltrace \
    whois nmap tcpdump google-noto-sans-fonts

# Remove rpms
dnf remove fprintd fprintd-pam audit rpm-plugin-audit avahi selinux-policy \
    elinux-policy-targeted setroubleshoot setroubleshoot-plugins setroubleshoot-server \
    rpm-plugin-selinux flatpak-selinux evolution-* PackageKit cockpit-packagekit cups

```

## Support This Project

[![Buy Me A Coffee](https://img.shields.io/badge/Buy%20Me%20A%20Coffee-support-yellow?style=flat-square&logo=buy-me-a-coffee)](https://buymeacoffee.com/bowang168)
[![Sponsor](https://img.shields.io/badge/GitHub%20Sponsors-sponsor-ea4aaa?style=flat-square&logo=github-sponsors)](https://github.com/sponsors/bowang168)
