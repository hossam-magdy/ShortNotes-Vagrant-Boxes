
# How to initialize `ubuntu/trusty64` Virtual Machine (via Vagrant & VirtualBox)


## 1- Install Vagrant & VirtualBox:
- Vagrant:
    The software that configures the VM and lets you share files between your host computer and the VM's filesystem
    [https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html)
    - Windows: [https://releases.hashicorp.com/vagrant/1.9.4/vagrant_1.9.4.msi](https://releases.hashicorp.com/vagrant/1.9.4/vagrant_1.9.4.msi) 
    - Ubuntu: [https://releases.hashicorp.com/vagrant/1.9.4/vagrant_1.9.4_x86_64.deb](https://releases.hashicorp.com/vagrant/1.9.4/vagrant_1.9.4_x86_64.deb) 
- VirtualBox:
    The software that actually runs the VM.
    [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads) 
    - Windows: [http://download.virtualbox.org/virtualbox/5.1.22/VirtualBox-5.1.22-115126-Win.exe](http://download.virtualbox.org/virtualbox/5.1.22/VirtualBox-5.1.22-115126-Win.exe) 
    - Ubuntu: [https://www.virtualbox.org/wiki/Linux_Downloads](https://www.virtualbox.org/wiki/Linux_Downloads) 



## 2- Download the `.box` file of `ubuntu/trusty64`:

#### - Manual download:
[trusty-server-cloudimg-amd64-vagrant-disk1.box](http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box) 

OR

[versions/20170422.0.0/providers/virtualbox.box](https://atlas.hashicorp.com/ubuntu/boxes/trusty64/versions/20170422.0.0/providers/virtualbox.box) (to download a specific version)

[ubuntu/boxes/trusty64](https://atlas.hashicorp.com/ubuntu/boxes/trusty64) (to specify a version)

#### - [OR] Existing OVA backup (pay attestion to UPPER_CASE variables)
If you have `.ova` backup of `ubuntu/trusty64`, run the following commands:

`
VBoxManage import PATH_TO_LOAD_OVA_FILE --vsys 0 --vmname "Ubuntu_64_Trusty" --eula accept
`

`
VBoxManage list vms
`

Output: `"Ubuntu_64_Trusty" {UID_OF_MACHINE}`

Then run:

`
vagrant package --base UID_OF_MACHINE --output PATH_TO_SAVE_BOX_FILE
`

#### - [OR (not recommended)] Auto-download by vagrant
At each host a vagrant `ubuntu/trusty64` is initialized, the box file will be downloaded.


## 3- Set the ".box" file as the box of "ubuntu/trusty64" machines:
Run command:
`
vagrant box add PATH_TO_BOX_FILE --name "ubuntu/trusty64"
`


## 4- Get the "vagrantfile" or initialize new one
In terminal:
- cd the directory containing the `vagrantfile`
- [OR] cd a new directory & run: `vagrant init ubuntu/trusty64`


## 5- (for NTFS partitions on Ubuntu)
Add the following line before the **end** of the vagrant file:

`
config.ssh.insert_key=false
`

to avoid errors regarding owner & permissions of ssh private key file


## 6- Launch/TurnOn the VM

To turn on the machine, run:

`vagrant up`

To turn off / halt, run:

`vagrant halt`

To check whether it is turned on/off, run:

`vagrant status`

To connect to the machine via SSH, run:

`vagrant ssh`

## 7- Enjoy ;)

