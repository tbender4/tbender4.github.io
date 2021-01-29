---

title: "Finding vSphere Profiles and Updating to vSphere 6.7 Easily"
categories: [general, vmware, coding]
tags: [general, bash, coding, vmware, esxi, vsphere, notes]


---

### VMware Makes the Easiest Thing Confusing
----

I've been tinkering with ESXi at home for both Linux and Windows virtual machines. I have a love-hate relationship with it.

VMware has a convoluted approach to deploying vSphere updates but we can make it painless using `esxicli`.

This post will be broken down to two parts.
1. Finding vSphere Profiles available to find the latest version.
2. How to Update to vSphere 6.7

### Finding the latest vSphere Profile
----
`esxcli` can print the current profile of ESXi installed with `esxi software profile get`. The output will give spew a LOT of information. Thankfully we can use `sed` and `cut` to just get the first line:
```
$ esxcli software profile get | sed '1!d' | cut -d' ' -f2
ESXi-6.7.0-8169922-standard
```

There's a neat one-liner to find all available official profiles of ESXi. It can be found with:

```
esxcli software sources profile list --depot=https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml
```

[*Credits to tinkertry for this line*](https://tinkertry.com/easy-update-to-esxi-67)

Let's do more piping sort this list and then grab the latest official profile.
```
esxcli software sources profile list \
	--depot=https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml \
	| sort -k4 \
	| tail -2 | head -1 \
	| sed \
	-e 's/VMware.*$//'
```
I've written a shell script that'll output both for easy comparison.
[grab the script here](https://github.com/tbender4/Easy-ESXi-Host-Updater)

### How to Update to vSphere 6.7
----

**Optional:** Enable swap. By default vSphere will not enable a swap partition. Attempting to update without swap will be met with `[Errno 28] No space left on device`. This was solved by setting a swap partition.

[Thanks to /r/vmware for having this solution.](https://www.reddit.com/r/vmware/comments/6q4akd/error_trying_to_update_an_esxi_65_host_to_the/)

Enable shell access and log into your host. Check the previous section as a recap.

Update our host to vSphere 6.7:
```
esxcli software profile update -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml -p ESXi-6.7.0-8169922-standard
```

It'll notify if the installation is successful with the following message:

```
Update Result
   Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective.
   Reboot Required: true
```

Reboot your machine with:
```
reboot
```

That's it! Once the machine is online, you can verify your vSphere version by visiting the WebGUI:

[comment]: <> (![image]({{ "assets/images/2018-04-19-finding-vsphere-profiles-and-updating-over-esxcli/imagename.png" | absolute_url }}) )

### Creating a Reusable Script
----

I've created a script that will compare the currently installed profile to the latest version found on VMware's production depot. If a newer profile is found, it will ask the user if they want to update the ESXi host.

The script and its installation instructions are found obtained [here](https://github.com/tbender4/Easy-ESXi-Host-Updater).

Here's what to expect from the script:

```

$ ./update-host.sh
Current version:
ESXi-6.5.0-20170301001-standard
Latest version of ESXI:
ESXi-6.7.0-8169922-standard
Update host from ESXi-6.5.0-20170301001-standard to ESXi-6.7.0-8169922-standard? (y/N) y
Updating to ESXi-6.7.0-8169922-standard...

$ ./update-host.sh
Current version:
ESXi-6.7.0-8169922-standard
Latest version of ESXi:
ESXi-6.7.0-8169922-standard
Up to date
```

This script can be reused whenever a new version of vSphere is available by merely running the script and rebooting the host.
