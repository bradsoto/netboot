# netboot
Very small linux system for capturing and deploying Windows ESD images.
### Requirements
NFSv4 and dnsmasq
### How to Build and Install
Edit initramfs/init NFS IP to your server IP. Then do these commands
```
cd netboot
sudo mkdir -p /srv/tftp
sudo cp tftp/undionly.kpxe /srv/tftp/
sudo mkdir -p /srv/netboot/images/efi
sudo cp srv/nfs/images/efi/* /srv/netboot/images/efi/
sudo mkdir -p /srv/netboot/capture
sudo chmod 777 /srv/netboot/capture
wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.19.5.tar.xz
tar -xJf linux-4.19.5.tar.xz
cp kernel_config linux-4.19.5/.config
cd linux-4.19.5
make
sudo cp arch/x86/boot/bzImage /srv/tftp/netboot.efi
cd ../
sudo cp etc/dnsmasq.conf /etc/dnsmasq.conf
sudo cp etc/exports /etc/exports
```
Restart nfs server and dnsmasq. You should be up and running! 
### Capturing a Windows Image
Make sure you have at least 8GB on the machine you run capture. Once it finishes move the image from /srv/nfs/capture to /srv/nfs/images. Now you can deploy it to thousands of machines over NFS.
