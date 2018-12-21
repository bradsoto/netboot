# netboot
Very small Linux system for capturing and deploying Windows ESD images.
### Requirements
nfs-server and dnsmasq
### How to Install
```
cd netboot
sudo cp -r srv/* /srv/
sudo cp etc/dnsmasq.conf /etc/dnsmasq.conf
sudo cp etc/exports /etc/exports
```
Restart nfs server and dnsmasq. You should be up and running! 
### Capturing a Windows Image
Make sure you have at least 8GB on the machine you run capture. Once it finishes move the image from /srv/nfs/netboot/capture/current.esd to /srv/nfs/netboot/images/current.esd. Then deploy it to thousands of machines using NFS.
### Rebuilding iPXE for custom directories
ROM-o-matic (https://rom-o-matic.eu/) is the simplest way to customize iPXE. This is the embedded script we use for compatibility with modern NFS servers.
```
#!ipxe

:retry_dhcp
dhcp && isset ${next-server} || goto retry_dhcp
kernel nfs://${next-server}/srv/nfs/netboot.efi root=/dev/nfs rootfstype=nfs nfsroot=${next-server}:/srv/nfs/netboot,vers=3,tcp ip=dhcp rw quiet
boot
```
When you build iPXE for EFI systems make sure to deselect Linux bzImage image support and select EFI image support. Do the opposite when you build iPXE for BIOS systems.
