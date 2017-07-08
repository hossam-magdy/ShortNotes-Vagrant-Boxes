
# How to initialize any Ubuntu `{box}` Virtual Machine (via Vagrant & VirtualBox)

{box} can be: `trusty64`, `xenial32`, `xenial64`, ... [choices](https://atlas.hashicorp.com/ubuntu/boxes/)

## 1- Install Vagrant & VirtualBox:
- Vagrant:
    The software that configures the VM and lets you share files between your host computer and the VM's filesystem
    [https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)
    - Windows: [vagrant_1.9.4.msi](https://releases.hashicorp.com/vagrant/1.9.4/vagrant_1.9.4.msi) 
    - Ubuntu: [vagrant_1.9.4_x86_64.deb](https://releases.hashicorp.com/vagrant/1.9.4/vagrant_1.9.4_x86_64.deb) 
- VirtualBox:
    The software that actually runs the VM
    [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads) 
    - Windows: [VirtualBox-5.1.22-115126-Win.exe](http://download.virtualbox.org/virtualbox/5.1.22/VirtualBox-5.1.22-115126-Win.exe) 
    - Ubuntu: [Linux_Downloads](https://www.virtualbox.org/wiki/Linux_Downloads) 



## 2- Download the `.box` file:

#### - Manual download (recommended):

Download the box file only once & use it whenever you like on many machines:

Choose ubuntu box (trusty/xenial/...etc.) and version (v20170619.0.0, ...etc.)
[https://atlas.hashicorp.com/ubuntu/boxes/](https://atlas.hashicorp.com/ubuntu/boxes/)

Example download links:
- For `trusty64` (14.04.5) `v20170619.0.0`:
[ubuntu/boxes/trusty64/versions/20170619.0.0/providers/virtualbox.box](https://atlas.hashicorp.com/ubuntu/boxes/trusty64/versions/20170619.0.0/providers/virtualbox.box) OR [trusty-server-cloudimg-amd64-vagrant-disk1.box](http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box) 
- For `xenial64` (16.04.2) `v20170706.0.0`:
[ubuntu/boxes/xenial64/versions/20170706.0.0/providers/virtualbox.box](https://atlas.hashicorp.com/ubuntu/boxes/xenial32/versions/20170706.0.0/providers/virtualbox.box)
- For `bento/ubuntu-16.04-i386` `v2.3.6`:
[bento/boxes/ubuntu-16.04-i386/versions/2.3.6/providers/virtualbox.box](https://atlas.hashicorp.com/bento/boxes/ubuntu-16.04-i386/versions/2.3.6/providers/virtualbox.box)
- For `laravel-homestead` `v2.1.0`
[laravel/boxes/homestead/versions/2.1.0/providers/virtualbox.box](https://atlas.hashicorp.com/laravel/boxes/homestead/versions/2.1.0/providers/virtualbox.box)


#### - [OR] Existing OVA backup (pay attention to UPPER_CASE variables):
If you have `.ova` backup, run the following commands:

`VBoxManage import PATH_TO_OVA_FILE --vsys 0 --vmname "Ubuntu_BOXNAME" --eula accept`

`VBoxManage list vms`

Outputs: `"Ubuntu_BOXNAME" {UID_OF_MACHINE}`

Copy the `UID_OF_MACHINE`, then run:

`vagrant package --base UID_OF_MACHINE --output PATH_TO_SAVE_BOX_FILE`

#### - [OR] Auto-download by vagrant (not recommended):
At each host a vagrant `ubuntu/{box}` is initialized, the box file will be downloaded.


## 3- Set the ".box" of "ubuntu/{box}" machines:
Run command:

`vagrant box add PATH_TO_BOX_FILE --name "ubuntu/{box}" --force`


## 4- Get the "vagrantfile" or initialize new one:
In terminal:
- cd the directory containing the `vagrantfile`
- [OR] cd a new directory & run: `vagrant init ubuntu/trusty64`


## 5- (for NTFS partitions on Ubuntu):
Add the following line before the **end** of the vagrant file:

`config.ssh.insert_key=false`

to avoid errors regarding owner & permissions of ssh private key file


## 6- Launch/TurnOn the VM, connect via SSH:
To turn on the machine, run:

`vagrant up --provision`

To turn off / halt, run:

`vagrant halt`

To check whether it is turned on/off, run:

`vagrant status`

To connect to the machine via SSH, run:

`vagrant ssh`

## 7- Enjoy ;)


# Possible Errors:

### - Error 1: Vagrant can not see the provider `virtualbox`:

> No usable default provider could be found for your system.

*OR when running: `vagrant up --provider=virtualbox`*

>The provider 'virtualbox' that was requested to back the machine
'default' is reporting that it isn't usable on this system. The
reason is shown below:

> Vagrant has detected that you have a version of VirtualBox installed
that is not supported by this version of Vagrant. Please install one of
the supported versions listed below to use Vagrant:

**Cause**: VirtualBox version is not compatible with Vagrant version.

**Solution**: Make sure you installed latest version of both. (If installing \*.deb on Ubuntu, use `sudo dpkg -i PATH_OF_DEB`)

### - Error 2: Can not run any `vagrant` command:

> ... 'pwd': No such file or directory - getcwd (Errno::ENOENT)

**Cause**: Your terminal is cd'ed to a non-existing directory.

**Solution**: Make sure you cd the directory where `vagrantfile` exists.

### - Error 3: SSH file has invalid permissions => should be 0600:

> The private key to connect to this box via SSH has invalid permissions
set on it. The permissions of the private key should be set to 0600, otherwise SSH will
ignore the key. Vagrant tried to do this automatically for you but failed. Please set the
permissions on the following file to 0600 and then try running this command again

**Cause**:
- The vagrant folder is on NTFS partition mounted in Ubuntu (NTFS filesystem doesn't support `chmod`).

**Solution**:
- Move the vagrant folder to `ext` partition. If problem persists, remove the `.vagrant` folder.

### - Error 4: SSH file is not owned by the current user:

> The private key to connect to the machine via SSH must be owned
by the user running Vagrant. This is a strict requirement from
SSH itself. Please fix the following key to be owned by the user
running Vagrant:

**Cause**:
- User created the machine (first `vagrant up`) is not the same user trying to `vagrant up` it.
- {OR} The vagrant folder is on NTFS partition mounted in Ubuntu (on NTFS the owner of files & folders is always `root`).

**Solution**:
- Move the vagrant folder to `ext` partition. If problem persists, remove the `.vagrant` folder.
- {OR} Either use `sudo` with `vagrant` commands => `sudo vagrant up`.

### - Error 5: Port already in use:

> Vagrant cannot forward the specified ports on this VM, since they
would collide with some other application that is already listening
on these ports. The forwarded port to 8000 is already in use
on the host machine.

**Cause**: Another process is listening to the port assigned in `vagrantfile` (~ by default: 8000)

**Solution**:
- Either find out which process is using this port: `sudo netstat -peant | grep ":8000 "` & kill it.
- {OR} Change the port assigned in the `vagrantfile`.

