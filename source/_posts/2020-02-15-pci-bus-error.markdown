---
layout: post
title: "PCI Bus Error"
date: 2020-02-15 19:37:37 -0300
comments: true
categories: linux
---

The CPU communicates with the PCIe bus controller through transaction layer packets (TLPs). The hardware detects when failure occurred, and the Linux kernel reports that as messages.
The mentioned error ***pcieport XXXX:YY:ZZ.Z: AER: Corrected error received***, is one of those cases... <!--more-->

<p style="border:3px; border-style:solid; border-color:#eead2d; padding: 1em;"><strong>Note about PCI-id</strong><br><br>
The four first hexadecimal id xxxx represents the Vendor ID and the four last hexadecimal id, represents the Device ID. Actually there is also
some sub-vendor-id, sub-vendor-id(to identify the computer/vendor implementation), pci function and class...(https://wiki.debian.org/HowToIdentifyADevice/PCI#references).
</p>

<h3 style="color:navy;">The lspci Tool</h3>

The lspci(list Peripheral Component Interconnect) utility, show us the devices connected to any pci compatible bus. e.g:

{% highlight bash %}
$ lspci -tv
-[0000:00]-+-00.0  Intel Corporation Xeon E3-1200 v6/7th Gen Core Processor Host Bridge/DRAM Registers
+-02.0  Intel Corporation HD Graphics 620
  +-04.0  Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem
     +-14.0  Intel Corporation Sunrise Point-LP USB 3.0 xHCI Controller
       +-14.2  Intel Corporation Sunrise Point-LP Thermal subsystem
          +-16.0  Intel Corporation Sunrise Point-LP CSME HECI #1
              +-17.0  Intel Corporation Sunrise Point-LP SATA Controller [AHCI mode]
                 +-1c.0-[01]--
                  +-1c.5-[02]----00.0  Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller
     	             +-1d.0-[03]----00.0  Realtek Semiconductor Co., Ltd. RTL8723BE PCIe Wireless Network Adapter
	                +-1f.0  Intel Corporation Sunrise Point LPC Controller/eSPI Controller
	                   +-1f.2  Intel Corporation Sunrise Point-LP PMC
	    	              +-1f.3  Intel Corporation Sunrise Point-LP HD Audio
		                 \-1f.4  Intel Corporation Sunrise Point-LP SMBus
{% endhighlight %}

According output command above, we can get the same(even then, Realtek Semiconductor Co., Ltd. RTL8723BE PCIe Wireless Network Adapter) id device that
causing the error of PCI Bus output log messages. i.e:

***[ 5315.986588] pcieport 0000:00:1c.0: AER: Corrected error received: id=01d0***

The workaround to solve that problem is adding such these kernel parameters:

**pci=nomsi**
<br>
**pci=noaer**

where **nomsi** meaning **No Message Signaled Interrupts** and **noaer** meaning **No Advanced Error Reporting**.

In short, both disable and stop the messages from showing output. The following example, show us how to configure theses parameters.

Edit the /etc/default/grub file and add pci=noaer and pci=nomsi to the line starting with the GRUB_CMDLINE_LINUX_DEFAULT:

{% highlight bash %}
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash reboot=efi,pci pci=nomsi,noaer pcie_aspm=off"
{% endhighlight %}


<p style="border:3px; border-style:solid; border-color:#eead2d; padding: 1em;"><strong>Note:</strong><br><br>
pci=nomsi and noaer, do not remove the error but just hide it into the error log. In this particular case, we add <mark>pcie_aspm=off</mark> option. This will increase the PCIe Active State
Power Management consumption of machine as it disables the power savings. More details(https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/power_management_guide/aspm).
</p>

So, update the grub with the new parameters:

{% highlight bash %}
$ update grub 
{% endhighlight %}

next, reboot the SO.

If it do not works, keep the kernel's parameters setting previously and run the following command:

{% highlight bash %}
$ grub-mkconfig -o /boot/grub/grub.cfg
$ update-grub
$ reboot
{% endhighlight %}


Remember yourself check logs files size in /var/log directory, such as messages.log file, kern.log file, and syslog file. Because the related error PCI generated
logs, these files can become increase pretty bigger.

You can fix them:


{% highlight bash %}
$ df -h
Filesystem	Size	Used	Available 	Use%	Mounted on
/dev/sda1        37G    37G   	0   		100%		/var
{% endhighlight %}

{% highlight bash %}
$ echo "" > /var/log/kern.log
$ echo "" > /var/log/syslog
$ echo "" > /var/log/messages
{% endhighlight %}

