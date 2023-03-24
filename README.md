

---
title: "[]{#_ytb6bo1r9nd9 .anchor}**RHV 4.3 to 4.4 Upgrade Test Case**"
---

**⇒ Storage domains remained the same or not after upgrade?**

→ storage domain does not remain the same. The name of the old storage
domain containing the hosted engine VM gets changed.

![image](file:///home/vaibhav/Videos/vertopal_3f93a0be9c81450daee5037b576777ab/media/image1.png){width="6.268055555555556in"
height="1.3472222222222223in"}

**⇒ Are you able to use the storage domain?**

→ The old storage domain is not usable anymore.

**⇒ Able to create vm?**

→ I am able to create Disks, Upload ISO images and Create VMs.

**⇒ Able to migrate vm?**

→ I am able to migrate new VMs but not the old VMs because the
Hypervisor running the old VMs is unable to enter the maintenance mode.

![](vertopal_3f93a0be9c81450daee5037b576777ab/media/image2.png){width="5.052083333333333in"
height="1.0729166666666667in"}

**⇒ During upgrade what happens with vms?**

→ After upgrading, I am unable to access the VMs.

![](vertopal_3f93a0be9c81450daee5037b576777ab/media/image3.png){width="6.268055555555556in"
height="1.4305555555555556in"}

**⇒Are existing vms working or not during upgrade or after upgrade?**

→ No, The VMs are not working after the update. RUN and CONSOLE buttons
are not visible and VM shows unknown state.

**⇒ What happens with the network?**

→ No major change was noticed in the network.

**⇒ Able to add a new storage domain?**

→ I am able to add a new storage domain.

**⇒ What are the precautions you have to take in production
environments?**

**→** After upgrade, We should not detach old storage domain as doing so
can cause Problems with the VMs.

**⇒ Any other negative possibility?**

→ After detaching the old Storage domain, One VM (VM2) disappeared. Only
the VM1 is left.

Before Upgrade:

![](vertopal_3f93a0be9c81450daee5037b576777ab/media/image4.png){width="6.268055555555556in"
height="1.2777777777777777in"}

After Upgrade:

![](vertopal_3f93a0be9c81450daee5037b576777ab/media/image5.png){width="6.268055555555556in"
height="1.0833333333333333in"}

Also, The RHEL 8.4 and 8.7 ISO images that I had uploaded earlier are
gone after upgrade:

![](vertopal_3f93a0be9c81450daee5037b576777ab/media/image6.png){width="6.268055555555556in"
height="2.263888888888889in"}

**RHV 4.3 to 4.4 Upgrade Summary**

**Collecting RHV-M details before upgrading:**

-   IP address and netmask

-   FQDN

-   Mac-address of Manager VM

-   Extra software and additional RPMs (eg: AD/ldap integration, etc)

-   Existing /etc/hosts details

**Upgrading RHV 4.3 to 4.4:**

\[root@vaibhav Downloads\]# ssh root@192.168.1.5

root@192.168.1.5\'s password:

Last login: Sat Mar 18 15:31:31 2023 from 192.168.1.11

**\[root@manager \~\]# systemctl stop ovirt-engine**

**\[root@manager \~\]# engine-backup \--scope=all \--mode=backup
\--file=backup.bck \--log=backuplog.log**

Start of engine-backup with mode \'backup\'

scope: all

archive file: backup.bck

log file: backuplog.log

Backing up:

Notifying engine

\- Files

\- Engine database \'engine\'

\- DWH database \'ovirt_engine_history\'

Packing into file \'backup.bck\'

Notifying engine

Done.

**\[root@manager \~\]# ls**

anaconda-ks.cfg backup.bck backuplog.log original-ks.cfg
ovirt-engine-answers

**\[root@manager \~\]# scp backup.bck backuplog.log
root@192.168.1.11:/home/manager/Downloads/rhv4.4/new**

The authenticity of host \'192.168.1.11 (192.168.1.11)\' can\'t be
established.

ECDSA key fingerprint is
SHA256:YS+9y/SQjs7hojaM4KdKzSE4dfHaYo869CmvqL/INw4.

ECDSA key fingerprint is
MD5:f8:b8:fa:8c:cc:c4:bf:be:24:96:59:fa:73:b4:2c:14.

Are you sure you want to continue connecting (yes/no)? yes

Warning: Permanently added \'192.168.1.11\' (ECDSA) to the list of known
hosts.

root@192.168.1.11\'s password:

backup.bck 100% 1280KB 35.6MB/s 00:00

backuplog.log 100% 3878 2.6MB/s 00:00

**\[root@manager \~\]# shutdown**

Shutdown scheduled for Mon 2023-03-20 01:27:39 IST, use \'shutdown -c\'
to cancel.

\[root@manager \~\]#

Broadcast message from root@manager.keenable.com (Mon 2023-03-20
01:26:39 IST):

The system is going down for power-off at Mon 2023-03-20 01:27:39 IST!

Connection to 192.168.1.5 closed by remote host.

Connection to 192.168.1.5 closed.

**Note: SCP the backup files (backup.bck & backuplog.log), rhvm 4.4 ova
file (rhvm-appliance-4.4-20211117.1.el8ev.ova) and /etc/hosts file to
the deployment host.**

**\[root@host1 /\]# hosted-engine \--deploy
\--restore-from-file=backup.bck**

\[ INFO \] Stage: Initializing

\[ INFO \] Stage: Environment setup

During customization use CTRL-D to abort.

Continuing will configure this host for serving as hypervisor and will
create a local VM with a running engine.

The provided engine backup file will be restored there,

it\'s strongly recommended to run this tool on a host that wasn\'t part
of the environment going to be restored.

If a reference to this host is already contained in the backup file, it
will be filtered out at restore time.

The locally running engine will be used to configure a new storage
domain and create a VM there.

At the end the disk of the local VM will be moved to the shared storage.

The old hosted-engine storage domain will be renamed, after checking
that everything is correctly working you can manually remove it.

Other hosted-engine hosts have to be reinstalled from the engine to
update their hosted-engine configuration.

Are you sure you want to continue? (Yes, No)\[Yes\]: yes

It has been detected that this program is executed through an SSH
connection without using tmux.

Continuing with the installation may lead to broken installation if the
network connection fails.

It is highly recommended to abort the installation and run it inside a
tmux session using command \"tmux\".

Do you want to continue anyway? (Yes, No)\[No\]: yes

Configuration files:

Log file:
/var/log/ovirt-hosted-engine-setup/ovirt-hosted-engine-setup-20230320020922-7l1ean.log

Version: otopi-1.9.6 (otopi-1.9.6-2.el8ev)

\[ INFO \] DNF Updating Subscription Management repositories.

\[ INFO \] Stage: Environment packages setup

\[ INFO \] DNF Updating Subscription Management repositories.

\[ INFO \] Stage: Programs detection

\[ INFO \] Stage: Environment setup (late)

\[ INFO \] Stage: Environment customization

\--== STORAGE CONFIGURATION ==\--

\--== HOST NETWORK CONFIGURATION ==\--

Please indicate the gateway IP address \[192.168.1.1\]:

\[ INFO \] Checking available network interfaces:

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Execute just a
specific set of steps\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Force facts
gathering\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Detecting interface
on existing management bridge\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set variable for
supported bond modes\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get all active
network interfaces\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter bonds with
bad naming\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate output
list\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect interface
types\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for Team
devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get list of Team
devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect Team
devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter team
devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if only team
devices are available\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Search VLAN
devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for base
interface of VLAN devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get base interface
types of VLAN devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for bond as
base type of VLAN device\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check if bond base
interface of VLAN device is in supported mode\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect VLAN devices
with bad bond mode base interfaces\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate invalid
VLANs list\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Create list of
unsupported network devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect VLAN devices
naming convention pattern\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check VLAN devices
with bad naming\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect VLAN devices
with invalid naming convention\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter VLAN devices
with invalid naming convention\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if only VLAN
devices with invalid naming convention are available\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for base
interface of VLAN devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get base interface
types of VLAN devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for bond as
base type of VLAN device\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check if bond base
interface of VLAN device is in supported mode\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check VLAN devices
with bad bond mode base interfaces\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect unsupported
VLAN bonds\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter VLAN devices
with invalid bond mode base interface\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if only VLAN
devices with bad bond mode are available\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate list of all
unsupported VLAN devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate list of all
unsupported network devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter unsupported
interface types\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Failed if only
unsupported devices are available\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Validate selected
bridge interface if management bridge does not exist\]

\[ INFO \] skipping: \[localhost\]

Please indicate a nic to set ovirtmgmt bridge on (eno1) \[eno1\]:

Please specify which way the network connectivity should be checked
(ping, dns, tcp, none) \[dns\]: ping

\--== VM CONFIGURATION ==\--

Please enter the name of the data center where you want to deploy this
hosted-engine host.

Please note that if you are restoring a backup that contains info about
other hosted-engine hosts,

this value should exactly match the value used in the environment you
are going to restore.

Data center \[Default\]:

Please enter the name of the cluster where you want to deploy this
hosted-engine host.

Please note that if you are restoring a backup that contains info about
other hosted-engine hosts,

this value should exactly match the value used in the environment you
are going to restore.

Cluster \[Default\]:

Renew engine CA on restore if needed?

Please notice that if you choose Yes, all hosts will have to be later
manually reinstalled from the engine.

Renew CA if needed? (Yes, No)\[No\]:

Pause the execution after adding this host to the engine?

You will be able to connect to the restored engine in order to manually
review and remediate its configuration.

This is normally not required when restoring an up to date and coherent
backup.

Pause after adding the host? (Yes, No)\[No\]:

If you want to deploy with a custom engine appliance image, please
specify the path to the OVA archive you would like to use.

Entering no value will use the image from the rhvm-appliance rpm,
installing it if needed.

Appliance image path \[\]: /rhvm-appliance-4.4-20211117.1.el8ev.ova

\[ INFO \] Checking OVF archive content (could take a few minutes
depending on archive size)

\[ INFO \] Checking OVF XML content (could take a few minutes depending
on archive size)

Please specify the number of virtual CPUs for the VM. The default is the
appliance OVF value \[4\]:

Please specify the memory size of the VM in MB. The default is the
maximum available \[14797\]:

\[ INFO \] Detecting host timezone.

\[ INFO \] Using Engine VM FQDN manager.keenable.com from backup file.

\[ INFO \] Using domain name from backup file.

Enter root password that will be used for the engine appliance:

Confirm appliance root password:

You may provide an SSH public key, that will be added by the deployment
script to the authorized_keys file of the root user in the engine
appliance.

This should allow you passwordless login to the engine machine after
deployment.

If you provide no key, authorized_keys will not be touched.

SSH public key \[\]: ssh-rsa
AAAAB3NzaC1yc2EAAAADAQABAAABgQC4dZhGDOiq9/UUaQhqKv0lVlGbMM075PCp2/3wkI/cwmnx95AlBqVZhGKDcmolScf796ulFp/mzLPU4X7wBkjdjTi80OiXjiVs8PkS64I9igUJONDE7/Sw+ccrxlky+ltuMoNtZSdFM84vaCsQqlYokiuBpZytfSgGbUKKyrx7LLmUz9mjIMNcL3UtK1GE8ahjRKxsLNnnB5GNqUkOS1lWWn3p1Md5Asa9Ls6O7vFanrZu6epW6v1Nj3WYsshEVaVzX3UOifP0e5XKWaiAACsDdCrdOYNmRevLb1JX3quABy7g0p/M6EHVUdhxz5doz+TqdyOxAvLTdRDhZahWYmpDCNPaBdXSnerbyI29sO1N6vgpiSxwpgmhfdJwpKK76z5RbDMCIrAAzcCsKvKq1q9c0zRjcMyXKdhd9nN3J22QUWZCp9SaF1DUXvUU3irtG//iocA+KBx3LQJda22ACOZNuTIdbTnnZt/BabVQ5PQ7chHhfZ0vufXaf24rEhyPMTk=
root@host1.keenable.com

Do you want to enable ssh access for the root user? (yes, no,
without-password) \[yes\]:

Do you want to apply a default OpenSCAP security profile? (Yes, No)
\[No\]:

Do you want to enable FIPS? (Yes, No) \[No\]:

Please specify a unicast MAC address for the VM, or accept a randomly
generated default \[00:16:3e:38:33:1f\]: 00:16:3e:74:32:45

How should the engine VM network be configured? (DHCP, Static)\[DHCP\]:
static

Please enter the IP address to be used for the engine VM \[\]:
192.168.1.5

\[ INFO \] The engine VM will be configured to use 192.168.1.5/24

Please provide a comma-separated list (max 3) of IP addresses of domain
name servers for the engine VM

Engine VM DNS (leave it empty to skip) \[8.8.8.8\]:

Add lines for the appliance itself and for this host to /etc/hosts on
the engine VM?

Note: ensuring that this host could resolve the engine VM hostname is
still up to you.

Add lines to /etc/hosts? (Yes, No)\[Yes\]:

\--== HOSTED ENGINE CONFIGURATION ==\--

Please provide the name of the SMTP server through which we will send
notifications \[localhost\]:

Please provide the TCP port number of the SMTP server \[25\]:

Please provide the email address from which notifications will be sent
\[root@localhost\]:

Please provide a comma-separated list of email addresses which will get
notifications \[root@localhost\]:

Enter engine admin password:

Confirm engine admin password:

\[ INFO \] Stage: Setup validation

Please provide the hostname of this host on the management network
\[host1.keenable.com\]:

\[WARNING\] Failed to resolve host1.keenable.com using DNS, it can be
resolved only locally

\[ INFO \] Stage: Transaction setup

\[ INFO \] DNF Updating Subscription Management repositories.

\[ INFO \] Stage: Misc configuration (early)

\[ INFO \] Stage: Package installation

\[ INFO \] Stage: Misc configuration

\[ INFO \] Stage: Transaction commit

\[ INFO \] Stage: Closing up

\[ INFO \] Cleaning previous attempts

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Execute just a
specific set of steps\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Force facts
gathering\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Install oVirt Hosted
Engine packages\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : System configuration
validations\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Detecting interface
on existing management bridge\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set variable for
supported bond modes\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get all active
network interfaces\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter bonds with
bad naming\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate output
list\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect interface
types\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for Team
devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get list of Team
devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect Team
devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter team
devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if only team
devices are available\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Search VLAN
devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for base
interface of VLAN devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get base interface
types of VLAN devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for bond as
base type of VLAN device\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check if bond base
interface of VLAN device is in supported mode\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect VLAN devices
with bad bond mode base interfaces\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate invalid
VLANs list\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Create list of
unsupported network devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect VLAN devices
naming convention pattern\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check VLAN devices
with bad naming\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect VLAN devices
with invalid naming convention\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter VLAN devices
with invalid naming convention\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if only VLAN
devices with invalid naming convention are available\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for base
interface of VLAN devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get base interface
types of VLAN devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for bond as
base type of VLAN device\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check if bond base
interface of VLAN device is in supported mode\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check VLAN devices
with bad bond mode base interfaces\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect unsupported
VLAN bonds\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter VLAN devices
with invalid bond mode base interface\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if only VLAN
devices with bad bond mode are available\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate list of all
unsupported VLAN devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate list of all
unsupported network devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter unsupported
interface types\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Failed if only
unsupported devices are available\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Validate selected
bridge interface if management bridge does not exist\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if he_force_ip4
and he_force_ip6 are set at the same time\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Prepare getent key\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get full hostname\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set hostname
variable if not defined\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Define host address
variable if not defined\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if he_force_ip4
and he_force_ip6 are set at the same time\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Prepare getent key\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get host address
resolution\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check address
resolution\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Parse host address
resolution\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if host\'s ip
is empty\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Avoid localhost\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Ensure host address
resolves locally\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get target address
from selected interface (IPv4)\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get target address
from selected interface (IPv6)\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check the resolved
address resolves on the selected interface\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for alias\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter resolved
address list\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Ensure the resolved
address resolves only on the selected interface\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Avoid localhost\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get engine FQDN
resolution\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check engine he_fqdn
resolution\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Parse engine he_fqdn
resolution\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Ensure engine
he_fqdn doesn\'t resolve locally\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check http/https
proxy\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get domain name\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set
he_cloud_init_domain_name\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Define
he_cloud_init_host_name\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get uuid\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_vm_uuid\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get uuid\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_nic_uuid\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get uuid\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_cdrom_uuid\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : get timezone\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_time_zone\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if Data Center
name format is incorrect\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Validate Cluster
name\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check firewalld
status\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Enforce firewalld
status\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get default gateway
IPv4\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get default gateway
IPv6\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_gateway\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if there is no
gateway\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate unicast MAC
address\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_vm_mac_addr\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if MAC address
structure is incorrect\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get free memory\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get cached memory\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set Max memory\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : set he_mem_size_MB
to max available if not defined\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if available
memory is less then the minimal requirement\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if user chose
less memory then the minimal requirement\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if user chose
more memory then the available memory\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if
he_disk_size_GB is smaller then the minimal requirement\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if
he_network_test is not valid\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if
he_tcp_t\_address is not defined\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if
he_tcp_t\_port is not defined\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if
he_tcp_t\_port is no integer\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Populate service
facts\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if the service
is masked or not running\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : get max cpus\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_maxvcpus\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_vcpus to
maximum amount if not defined\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check number of
chosen CPUs\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Stop libvirt
service\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Drop vdsm config
statements\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Drop VNC encryption
config statements\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Restore initial abrt
config files\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Restart abrtd
service\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Drop libvirt sasl2
configuration by vdsm\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Stop and disable
services\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Restore initial
libvirt default network configuration\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Start libvirt\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for leftover
local Hosted Engine VM\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Destroy leftover
local Hosted Engine VM\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for leftover
defined local Hosted Engine VM\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Undefine leftover
local engine VM\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for leftover
defined Hosted Engine VM\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Undefine leftover
engine VM\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove eventually
entries for the local VM from known_hosts file\]

\[ INFO \] ok: \[localhost -\> localhost\]

\[ INFO \] Starting local VM

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Execute just a
specific set of steps\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Force facts
gathering\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Install oVirt Hosted
Engine packages\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : System configuration
validations\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Detecting interface
on existing management bridge\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set variable for
supported bond modes\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get all active
network interfaces\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter bonds with
bad naming\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate output
list\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect interface
types\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for Team
devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get list of Team
devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect Team
devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter team
devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if only team
devices are available\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Search VLAN
devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for base
interface of VLAN devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get base interface
types of VLAN devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for bond as
base type of VLAN device\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check if bond base
interface of VLAN device is in supported mode\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect VLAN devices
with bad bond mode base interfaces\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate invalid
VLANs list\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Create list of
unsupported network devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect VLAN devices
naming convention pattern\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check VLAN devices
with bad naming\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect VLAN devices
with invalid naming convention\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter VLAN devices
with invalid naming convention\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if only VLAN
devices with invalid naming convention are available\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for base
interface of VLAN devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get base interface
types of VLAN devices\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for bond as
base type of VLAN device\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check if bond base
interface of VLAN device is in supported mode\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check VLAN devices
with bad bond mode base interfaces\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect unsupported
VLAN bonds\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter VLAN devices
with invalid bond mode base interface\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if only VLAN
devices with bad bond mode are available\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate list of all
unsupported VLAN devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate list of all
unsupported network devices\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter unsupported
interface types\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Failed if only
unsupported devices are available\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Validate selected
bridge interface if management bridge does not exist\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if he_force_ip4
and he_force_ip6 are set at the same time\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Prepare getent key\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get full hostname\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set hostname
variable if not defined\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Define host address
variable if not defined\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if he_force_ip4
and he_force_ip6 are set at the same time\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Prepare getent key\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get host address
resolution\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check address
resolution\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Parse host address
resolution\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if host\'s ip
is empty\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Avoid localhost\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Ensure host address
resolves locally\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get target address
from selected interface (IPv4)\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get target address
from selected interface (IPv6)\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check the resolved
address resolves on the selected interface\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for alias\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Filter resolved
address list\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Ensure the resolved
address resolves only on the selected interface\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Avoid localhost\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get engine FQDN
resolution\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check engine he_fqdn
resolution\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Parse engine he_fqdn
resolution\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Ensure engine
he_fqdn doesn\'t resolve locally\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check http/https
proxy\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get domain name\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set
he_cloud_init_domain_name\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Define
he_cloud_init_host_name\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get uuid\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_vm_uuid\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get uuid\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_nic_uuid\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get uuid\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_cdrom_uuid\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : get timezone\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_time_zone\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if Data Center
name format is incorrect\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Validate Cluster
name\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check firewalld
status\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Enforce firewalld
status\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get default gateway
IPv4\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get default gateway
IPv6\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_gateway\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if there is no
gateway\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate unicast MAC
address\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_vm_mac_addr\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if MAC address
structure is incorrect\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get free memory\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get cached memory\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set Max memory\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : set he_mem_size_MB
to max available if not defined\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if available
memory is less then the minimal requirement\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if user chose
less memory then the minimal requirement\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if user chose
more memory then the available memory\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if
he_disk_size_GB is smaller then the minimal requirement\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if
he_network_test is not valid\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if
he_tcp_t\_address is not defined\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if
he_tcp_t\_port is not defined\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if
he_tcp_t\_port is no integer\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Populate service
facts\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if the service
is masked or not running\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : get max cpus\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_maxvcpus\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set he_vcpus to
maximum amount if not defined\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check number of
chosen CPUs\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Stop libvirt
service\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Drop vdsm config
statements\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Drop VNC encryption
config statements\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Restore initial abrt
config files\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Restart abrtd
service\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Drop libvirt sasl2
configuration by vdsm\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Stop and disable
services\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Restore initial
libvirt default network configuration\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Start libvirt\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for leftover
local Hosted Engine VM\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Destroy leftover
local Hosted Engine VM\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for leftover
defined local Hosted Engine VM\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Undefine leftover
local engine VM\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for leftover
defined Hosted Engine VM\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Undefine leftover
engine VM\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove eventually
entries for the local VM from known_hosts file\]

\[ INFO \] ok: \[localhost -\> localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check IPv6\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get IPv4 route\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check if route
exists\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get IPv6 route\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check if route
exists\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Parse libvirt
default network configuration\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove IPv4
configuration\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Configure libvirt
default network as a forward network\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Edit libvirt default
network configuration, enable NAT\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Edit libvirt default
network configuration, enable NAT IPv6\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Edit libvirt default
network configuration, set IPv6 address\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Edit libvirt default
network configuration, set IPv6 prefix\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Edit libvirt default
network configuration, enable DHCPv6\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Edit libvirt default
network configuration, set DHCPv6 range\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Edit libvirt default
network configuration, change default address\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Edit libvirt default
network configuration, change DHCP start range\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Edit libvirt default
network configuration, change DHCP end range\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Update libvirt
default network configuration, destroy\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Update libvirt
default network configuration, undefine\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Update libvirt
default network configuration, define\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Activate default
libvirt network\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Start libvirt\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Activate default
libvirt network\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get libvirt
interfaces\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get routing rules,
IPv4\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get routing rules,
IPv6\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Save bridge name\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Wait for the bridge
to appear on the host\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Accept IPv6 Router
Advertisements for virbr0\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Refresh network
facts\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch IPv4 CIDR for
virbr0\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch IPv6 CIDR for
virbr0\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add IPv4 outbound
route rules\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add IPv4 inbound
route rules\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add IPv6 outbound
route rules\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add IPv6 inbound
route rules\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get host unique id\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Create directory for
local VM\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set local vm dir
path\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fix local VM
directory permission\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Register appliance
PATH\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check available
space on local VM directory\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check appliance
size\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Ensure we have
enough space to extract the appliance\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Extract appliance to
local VM directory\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Find the local
appliance image\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set
local_vm_disk_path\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get appliance disk
size\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Parse qemu-img
output\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Hash the appliance
root password\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Create cloud init
user-data and meta-data files\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Create ISO disk\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Create local VM\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get local VM IP\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove leftover
entries in /etc/hosts for the local VM\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Create an entry in
/etc/hosts for the local VM\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Wait for SSH to
restart on the local VM\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set the name for
add_host\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch the value of
HOST_KEY_CHECKING\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get the username
running the deploy\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Register the engine
FQDN as a host\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Wait for the local
VM\]

\[ INFO \] ok: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add an entry for
this host on /etc/hosts on the local VM\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set FQDN\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Force the local VM
FQDN to temporary resolve on the natted network address\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Reconfigure IPv6
default gateway\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Restore sshd reverse
DNS lookups\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add lines to
answerfile\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add lines to
answerfile\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add lines to
answerfile\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get appliance
distribution\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set appliance
distribution variables\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Enable FIPS on the
engine VM\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Initialize OpenSCAP
variables\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set OpenSCAP
datastream path\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Verify OpenSCAP
datastream\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set default OpenSCAP
profile\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Apply OpenSCAP
profile\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Reset
PermitRootLogin for sshd\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Reboot the engine VM
to ensure that FIPS is enabled\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check if FIPS mode
is enabled\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Enforce openscap
profile on CentOS\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Enforce FIPS mode\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Include before
engine-setup custom tasks files for the engine VM\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Copy the backup file
to the engine VM for restore\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Run engine-backup\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove backup file\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove previous
hosted-engine VM\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Update dynamic data
for VMs on the host used to redeploy\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Update dynamic data
for VMs migrating to the host used to redeploy\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove host used to
redeploy\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Rename previous HE
storage domain to avoid name conflicts\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Save original
DisableFenceAtStartupInSec\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Update
DisableFenceAtStartupInSec to prevent host fencing during the recovery\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add lines to
engine-setup answerfile for PKI renewal\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : remove version lock
from the engine\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : recreate versionlock
empty file\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Perform pre-install checks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Gather facts on installed
packages\]

\[ INFO \] ok: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Fail when firewall manager
is not installed\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Install required packages
for oVirt Engine deployment\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Install oVirt Engine
package\]

\[ INFO \] ok: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Check if rhevm package is
installed\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Install RHV package\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Install rest of the packages
required for oVirt Engine deployment\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Run engine setup\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Set answer file path\]

\[ INFO \] ok: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Use the default answerfile\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Copy custom answer file\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Use remote\'s answer file\]

\[ INFO \] ok: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Update setup packages\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Copy yum configuration
file\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Set \'best\' to false\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Update all packages\]

\[ INFO \] ok: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Remove temporary yum
configuration file\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Set offline parameter if
variable is set\]

\[ INFO \] ok: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Restore engine from file\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Run engine-setup with
answerfile\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Make sure \`ovirt-engine\`
service is running\]

\[ INFO \] ok: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Run engine-config\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Restart engine after
engine-config\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Check if Engine health page
is up\]

\[ INFO \] ok: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[redhat.rhv.engine_setup : Clean temporary files\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Include after
engine-setup custom tasks files for the engine VM\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Wait for the engine
to reach a stable condition\]

\[ INFO \] ok: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Configure LibgfApi
support\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Save original
OvfUpdateIntervalInMinutes\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set OVF update
interval to 1 minute\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Saving original
value\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Adding new
SSO_ALTERNATE_ENGINE_FQDNS line\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Restart ovirt-engine
service for changed OVF Update configuration and LibgfApi support\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Mask cloud-init
services to speed up future boot\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Wait for
ovirt-engine service to start\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Open a port on
firewalld\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Expose engine VM
webui over a local port via ssh port forwarding\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Evaluate temporary
bootstrap engine VM URL\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] The bootstrap engine is temporarily accessible via
https://host1.keenable.com:6900/ovirt-engine/

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Detect VLAN ID\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set Engine public
key as authorized key without validating the TLS/SSL certificates\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Obtain SSO token
using username/password credentials\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Ensure that the
target datacenter is present\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Ensure that the
target cluster is present in the target datacenter\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check actual cluster
location\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Enable GlusterFS at
cluster level\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set VLAN ID at
datacenter level\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get active list of
active firewalld zones\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Configure libvirt
firewalld zone\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add host\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Include
after_add_host tasks files\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Always revoke the
SSO token\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Obtain SSO token
using username/password credentials\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Wait for the host to
be up\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Notify the user
about a failure\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : set_fact\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect error events
from the Engine\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate the error
message from the engine events\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] The host has been set in non_operational status, deployment
errors: code 120: Migration failed due to an Error: Error creating the
requested VM (VM: external-HostedEngineLocal, Source:
host1.keenable.com, Destination: host2.keenable.com)., code 519: Host
host1.keenable.com does not comply with the cluster Default networks,
the following networks are missing on host: \'ovirtmgmt\', code 9000:
Failed to verify Power Management configuration for Host
host1.keenable.com., code 9027: Host host1.keenable.com is not
responding. Host cannot be fenced automatically because power management
for the host is disabled., code 10802: VDSM host1.keenable.com command
Get Host Capabilities failed: Recovering from crash or Initializing,

\[ INFO \] skipping: \[localhost\]

\[ INFO \] You can now connect to
https://host1.keenable.com:6900/ovirt-engine/ and check the status of
this host and eventually remediate it, please continue only when the
host is listed as \'up\'

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Create temporary
lock file\]

\[ INFO \] changed: \[localhost -\> localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Pause execution
until /tmp/ansible.573d6tqx_he_setup_lock is removed, delete it once
ready to proceed\]

\[ INFO \] ok: \[localhost -\> localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Always revoke the
SSO token\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Obtain SSO token
using username/password credentials\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check if the host is
up\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : set_fact\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Collect error events
from the Engine\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate the error
message from the engine events\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail with error
description\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail with generic
error\]

\[ INFO \] skipping: \[localhost\]

Please specify the storage you would like to use (glusterfs, iscsi, fc,
nfs)\[nfs\]:

Please specify the nfs version you would like to use (auto, v3, v4,
v4_0, v4_1, v4_2)\[auto\]:

Please specify the full shared storage connection path to use (example:
host:/path): 192.168.1.11:/home/storage2

If needed, specify additional mount options for the connection to the
hosted-engine storagedomain (example: rsize=32768,wsize=32768) \[\]:

\[ INFO \] Creating Storage Domain

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Execute just a
specific set of steps\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Force facts
gathering\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Wait for the storage
interface to be up\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check local VM dir
stat\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Enforce local VM dir
existence\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Obtain SSO token
using username/password credentials\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch host facts\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch cluster ID\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch cluster
facts\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch Datacenter
facts\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch Datacenter
ID\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch Datacenter
name\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add NFS storage
domain\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add glusterfs
storage domain\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add iSCSI storage
domain\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add Fibre Channel
storage domain\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get storage domain
details\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Find the appliance
OVF\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Parse OVF\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get required size\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove unsuitable
storage domain\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check storage domain
free space\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Activate storage
domain\]

\[ INFO \] changed: \[localhost\]

Please specify the size of the VM disk in GiB: \[59\]:

\[ INFO \] Creating Target VM

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Execute just a
specific set of steps\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Force facts
gathering\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get full hostname\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set hostname
variable if not defined\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Define host address
variable if not defined\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if he_force_ip4
and he_force_ip6 are set at the same time\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Prepare getent key\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get host address
resolution\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check address
resolution\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Parse host address
resolution\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if host\'s ip
is empty\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Obtain SSO token
using username/password credentials\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get local VM IP\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set the name for
add_host\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch the value of
HOST_KEY_CHECKING\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get the username
running the deploy\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Register the engine
FQDN as a host\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch host facts\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch Cluster ID\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch Cluster
facts\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch Datacenter
facts\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch Cluster name\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch Datacenter
ID\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch Datacenter
name\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Parse Cluster
details\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get server CPU
list\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get cluster emulated
machine list\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Prepare for parsing
server CPU list\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Parse server CPU
list\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Convert CPU model
name\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Parse
emulated_machine\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get storage domain
details\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add HE disks\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Register disk
details\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set default graphics
protocols\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check if FIPS is
enabled\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Select graphic
protocols\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add VM\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Register external
local VM uuid\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Create a temporary
directory for ansible as postgres user\]

\[ INFO \] ok: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Update target VM
details at DB level\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Insert Hosted Engine
configuration disk uuid into Engine database\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch host SPM_ID\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Parse host SPM_ID\]

\[ INFO \] ok: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Restore original
DisableFenceAtStartupInSec\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove
DisableFenceAtStartupInSec temporary file\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Restore original
OvfUpdateIntervalInMinutes\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove
OvfUpdateIntervalInMinutes temporary file\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Removing temporary
value\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Restoring original
value\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove temporary
directory for ansible as postgres user\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Configure
PermitRootLogin for sshd to its final value\]

\[ INFO \] ok: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Clean cloud-init
configuration\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove cloud-user
user\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove cloud-init
file from /etc/sudoers.d\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove cloud-user
from /etc/sudoers file\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove cloud-init
package\]

\[ INFO \] changed: \[localhost -\> 192.168.222.71\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fail if he_force_ip4
and he_force_ip6 are set at the same time\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Prepare getent key\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Trigger hosted
engine OVF update and enable the serial console\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Wait until OVF
update finishes\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Parse OVF_STORE disk
list\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check OVF_STORE
volume status\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Wait for OVF_STORE
disk content\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Prepare images\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch Hosted Engine
configuration disk path\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch Hosted Engine
virtio disk path\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch Hosted Engine
virtio metadata path\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Shutdown local VM\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Wait for local VM
shutdown\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Undefine local VM\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Update libvirt
default network configuration, destroy\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Update libvirt
default network configuration, undefine\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Detect
ovirt-hosted-engine-ha version\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set ha_version\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Create configuration
templates\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Create configuration
archive\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Create
ovirt-hosted-engine-ha run directory\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Copy configuration
files to the right location on host\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Copy configuration
archive to storage\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Initialize metadata
volume\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Find the local
appliance image\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set
local_vm_disk_path\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate DHCP
network configuration for the engine VM\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate static
network configuration for the engine VM, IPv4\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Generate static
network configuration for the engine VM, IPv6\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Inject network
configuration with guestfish\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Extract /etc/hosts
from the Hosted Engine VM\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Clean /etc/hosts for
the Hosted Engine VM for Engine VM FQDN\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add an entry on
/etc/hosts for the Hosted Engine VM for the VM itself\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Clean /etc/hosts for
the Hosted Engine VM for host address\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Inject /etc/hosts
with guestfish\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Copy local VM disk
to shared storage\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Verify copy of VM
disk\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove temporary
entry in /etc/hosts for the local VM\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set the name for
add_host\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch the value of
HOST_KEY_CHECKING\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get the username
running the deploy\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Register the engine
FQDN as a host\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Start
ovirt-ha-broker service on the host\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Initialize lockspace
volume\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Workaround for
ovirt-ha-broker start failures\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Initialize lockspace
volume\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Start ovirt-ha-agent
service on the host\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Exit HE maintenance
mode\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check engine VM
health\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get target engine VM
address\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Reconfigure OVN
central address\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Obtain SSO token
using username/password credentials\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Check for the local
bootstrap engine VM\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Make the engine
aware that the external VM is stopped\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Wait for the local
bootstrap engine VM to be down at engine eyes\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove bootstrap
external VM from the engine\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove
ovirt-engine-appliance rpm\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Include custom tasks
for after setup customization\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Include Host vars\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set Engine public
key as authorized key without validating the TLS/SSL certificates\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add additional
gluster hosts to engine\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Add additional
glusterfs storage domains\]

\[ INFO \] skipping: \[localhost\]

\[ INFO \] Stage: Clean up

\[ INFO \] Cleaning temporary resources

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Execute just a
specific set of steps\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Force facts
gathering\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set the name for
add_host\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch the value of
HOST_KEY_CHECKING\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Get the username
running the deploy\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Register the engine
FQDN as a host\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Sync on engine
machine\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Fetch logs from the
engine VM\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set destination
directory path\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Create destination
directory\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Find the local
appliance image\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Set
local_vm_disk_path\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Give the vm time to
flush dirty buffers\]

\[ INFO \] ok: \[localhost -\> localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Copy engine logs\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : include_tasks\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove local vm
dir\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Remove temporary
entry in /etc/hosts for the local VM\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Clean local storage
pools\]

\[ INFO \] ok: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Destroy local
storage-pool localvmpr9e66ll\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Undefine local
storage-pool localvmpr9e66ll\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Destroy local
storage-pool 7cc8e180-070f-4883-b71c-2bac20083d94\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] TASK \[ovirt.ovirt.hosted_engine_setup : Undefine local
storage-pool 7cc8e180-070f-4883-b71c-2bac20083d94\]

\[ INFO \] changed: \[localhost\]

\[ INFO \] Generating answer file
\'/var/lib/ovirt-hosted-engine-setup/answers/answers-20230320025740.conf\'

\[ INFO \] Generating answer file
\'/etc/ovirt-hosted-engine/answers.conf\'

\[ INFO \] Stage: Pre-termination

\[ INFO \] Stage: Termination

\[ INFO \] Hosted Engine successfully deployed

\[ INFO \] Other hosted-engine hosts have to be reinstalled in order to
update their storage configuration. From the engine, host by host,
please set maintenance mode and then click on the reinstall button
ensuring you choose DEPLOY in the hosted engine tab.

\[ INFO \] Please note that the engine VM ssh keys have changed. Please
remove the engine VM entry in ssh known_hosts on your clients.
