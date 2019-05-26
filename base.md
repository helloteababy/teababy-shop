## Install base OS ##

*Regarding the operating system, stability is first priority, as we do not have much time to take care of this part, so even so many options available like FreeBSD/Debian etc, CentOS 7.4 is chosen.*

** Turn on Google BBR**
> [Google TCP BBR](https://cloud.google.com/blog/products/gcp/tcp-bbr-congestion-control-comes-to-gcp-your-internet-just-got-faster) achives higher bandwidths and lower latencies for internet traffic.

That's the first job should be done, this will help to speed your website finally. There is a good guide on Vultr - [How to deploy BBR on CentOS 7](https://www.vultr.com/docs/how-to-deploy-google-bbr-on-centos-7), below is a brief instruction.

###Step 1: Upgrade the kernel###
Replace Kernel as the default is too old that do not contain BBR that version need up to 4.9

```
sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org (external link)
sudo rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
sudo yum --enablerepo=elrepo-kernel install kernel-ml -y
sudo egrep ^menuentry /etc/grub2.cfg | cut -f 2 -d \
sudo grub2-set-default 0  # Indexing starts at 0, choose the one you just installed
sudo reboot
```
[Some hosting company like Digital Ocean needs to comfigure the VPS to be kernel customizable or new settings of grub2 will not go into effect.](https://blog.csdn.net/weixin_42405070/article/details/82383847)

As CentOS will try to downgrade the kernel to default one, `nano /etc/yum.conf` add the following line into [main] sector to prohibit any changes to the kernel.

> exclude=kernel*
> exclude=centos-release*

###Step 2: Enable BBR###

```
modprobe tcp_bbr
echo "tcp_bbr" >> /etc/modules-load.d/modules.conf
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
```

Excute commands below and if bbr show up which means it's on.

```
sysctl net.ipv4.tcp_available_congestion_control
sysctl net.ipv4.tcp_congestion_control
```

## Add Swap partition ##
Some do not have swap set by default, swap is similar to RAM but slower however can increase the OS performance a lot, the size of swap could be the same as RAM.

```
dd if=/dev/zero of=/etc/swapfile bs=1M count=1024  # 1024=1G, change it according to your needs
chmod 0600 /etc/swapfile
mkswap /etc/swapfile
swapon /etc/swapfile
```

`free -m` # Added swap should show up

`nano /etc/fstab` # Add following line to let swap on when system rebooting

> /etc/swapfile	swap	swap	default	0	0

### Update the whole system ###

`sudo yum update`
