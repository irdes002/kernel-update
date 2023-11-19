1. Название задания:
Vagrant-стенд для обновления ядра и создания образа системы

2. Текст задания:
1) Обновить ядро ОС из репозитория ELRepo
2) Создать Vagrant box c помощью Packer
3) Загрузить Vagrant box в Vagrant Cloud

3. Описание команд и их вывод:
root@ubuntu:~# vagrant box add generic/centos8s
==> box: Loading metadata for box 'generic/centos8s'
    box: URL: https://vagrantcloud.com/api/v2/vagrant/generic/centos8s
This box can work with multiple providers! The providers that it
can work with are listed below. Please review the list and choose
the provider you will be working with.

1) hyperv
2) libvirt
3) parallels
4) virtualbox
5) vmware_desktop

Enter your choice: 4
==> box: Adding box 'generic/centos8s' (v4.3.6) for provider: virtualbox (amd64)
    box: Downloading: https://vagrantcloud.com/generic/boxes/centos8s/versions/4.3.6/providers/virtualbox/amd64/vagrant.box
    box: Calculating and comparing box checksum...
==> box: Successfully added box 'generic/centos8s' (v4.3.6) for 'virtualbox (amd64)'!
root@ubuntu:~# cd /opt/vm/
root@ubuntu:/opt/vm# vagrant init
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
root@ubuntu:/opt/vm# vim Vagrantfile 
root@ubuntu:/opt/vm# vagrant up
Bringing machine 'kernel-update' up with 'virtualbox' provider...
==> kernel-update: Importing base box 'generic/centos8s'...
==> kernel-update: Matching MAC address for NAT networking...
==> kernel-update: Checking if box 'generic/centos8s' version '4.3.6' is up to date...
==> kernel-update: Setting the name of the VM: vm_kernel-update_1699792364252_56859
==> kernel-update: Clearing any previously set network interfaces...
==> kernel-update: Preparing network interfaces based on configuration...
    kernel-update: Adapter 1: nat
==> kernel-update: Forwarding ports...
    kernel-update: 22 (guest) => 2222 (host) (adapter 1)
==> kernel-update: Running 'pre-boot' VM customizations...
==> kernel-update: Booting VM...
==> kernel-update: Waiting for machine to boot. This may take a few minutes...
    kernel-update: SSH address: 127.0.0.1:2222
    kernel-update: SSH username: vagrant
    kernel-update: SSH auth method: private key
    kernel-update: 
    kernel-update: Vagrant insecure key detected. Vagrant will automatically replace
    kernel-update: this with a newly generated keypair for better security.
    kernel-update: 
    kernel-update: Inserting generated public key within guest...
    kernel-update: Removing insecure key from the guest if it's present...
    kernel-update: Key inserted! Disconnecting and reconnecting using new SSH key...
==> kernel-update: Machine booted and ready!
==> kernel-update: Checking for guest additions in VM...
==> kernel-update: Setting hostname...
root@ubuntu:/opt/vm# vagrant ssh
[vagrant@kernel-update ~]$ sudo -i
[root@kernel-update ~]# uname -r
4.18.0-519.el8.x86_64
[root@kernel-update ~]# yum install -y https://www.elrepo.org/elrepo-release-8.el8.elrepo.noarch.rpm
CentOS Stream 8 - AppStream                                                                                                                          3.3 MB/s |  34 MB     00:10    
CentOS Stream 8 - BaseOS                                                                                                                             3.0 MB/s |  53 MB     00:17    
CentOS Stream 8 - Extras                                                                                                                             8.7 kB/s |  18 kB     00:02    
CentOS Stream 8 - Extras common packages                                                                                                             4.4 kB/s | 6.9 kB     00:01    
Extra Packages for Enterprise Linux 8 - x86_64                                                                                                       694 kB/s |  16 MB     00:23    
Extra Packages for Enterprise Linux 8 - Next - x86_64                                                                                                 80 kB/s | 368 kB     00:04    
elrepo-release-8.el8.elrepo.noarch.rpm                                                                                                               6.2 kB/s |  13 kB     00:02    
Dependencies resolved.
=====================================================================================================================================================================================
 Package                                      Architecture                         Version                                          Repository                                  Size
=====================================================================================================================================================================================
Installing:
 elrepo-release                               noarch                               8.3-1.el8.elrepo                                 @commandline                                13 k

Transaction Summary
=====================================================================================================================================================================================
Install  1 Package

Total size: 13 k
Installed size: 5.0 k
Downloading Packages:
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                             1/1 
  Installing       : elrepo-release-8.3-1.el8.elrepo.noarch                                                                                                                      1/1 
  Verifying        : elrepo-release-8.3-1.el8.elrepo.noarch                                                                                                                      1/1 

Installed:
  elrepo-release-8.3-1.el8.elrepo.noarch                                                                                                                                             

Complete!
[root@kernel-update ~]# yum --enablerepo elrepo-kernel install kernel-ml -y
ELRepo.org Community Enterprise Linux Repository - el8                                                                                                80 kB/s | 248 kB     00:03    
ELRepo.org Community Enterprise Linux Kernel Repository - el8                                                                                        686 kB/s | 2.1 MB     00:03    
Dependencies resolved.
=====================================================================================================================================================================================
 Package                                        Architecture                        Version                                         Repository                                  Size
=====================================================================================================================================================================================
Installing:
 kernel-ml                                      x86_64                              6.6.1-1.el8.elrepo                              elrepo-kernel                              117 k
Installing dependencies:
 kernel-ml-core                                 x86_64                              6.6.1-1.el8.elrepo                              elrepo-kernel                               38 M
 kernel-ml-modules                              x86_64                              6.6.1-1.el8.elrepo                              elrepo-kernel                               34 M

Transaction Summary
=====================================================================================================================================================================================
Install  3 Packages

Total download size: 72 M
Installed size: 114 M
Downloading Packages:
(1/3): kernel-ml-6.6.1-1.el8.elrepo.x86_64.rpm                                                                                                        72 kB/s | 117 kB     00:01    
(2/3): kernel-ml-core-6.6.1-1.el8.elrepo.x86_64.rpm                                                                                                  2.3 MB/s |  38 MB     00:16    
(3/3): kernel-ml-modules-6.6.1-1.el8.elrepo.x86_64.rpm                                                                                               472 kB/s |  34 MB     01:14    
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                                987 kB/s |  72 MB     01:15     
ELRepo.org Community Enterprise Linux Kernel Repository - el8                                                                                        1.6 MB/s | 1.7 kB     00:00    
Importing GPG key 0xBAADAE52:
 Userid     : "elrepo.org (RPM Signing Key for elrepo.org) <secure@elrepo.org>"
 Fingerprint: 96C0 104F 6315 4731 1E0B B1AE 309B C305 BAAD AE52
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-elrepo.org
Key imported successfully
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                                                             1/1 
  Installing       : kernel-ml-core-6.6.1-1.el8.elrepo.x86_64                                                                                                                    1/3 
  Running scriptlet: kernel-ml-core-6.6.1-1.el8.elrepo.x86_64                                                                                                                    1/3 
  Installing       : kernel-ml-modules-6.6.1-1.el8.elrepo.x86_64                                                                                                                 2/3 
  Running scriptlet: kernel-ml-modules-6.6.1-1.el8.elrepo.x86_64                                                                                                                 2/3 
  Installing       : kernel-ml-6.6.1-1.el8.elrepo.x86_64                                                                                                                         3/3 
  Running scriptlet: kernel-ml-core-6.6.1-1.el8.elrepo.x86_64                                                                                                                    3/3 
dracut: Disabling early microcode, because kernel does not support it. CONFIG_MICROCODE_[AMD|INTEL]!=y
dracut: Disabling early microcode, because kernel does not support it. CONFIG_MICROCODE_[AMD|INTEL]!=y

  Running scriptlet: kernel-ml-6.6.1-1.el8.elrepo.x86_64                                                                                                                         3/3 
  Verifying        : kernel-ml-6.6.1-1.el8.elrepo.x86_64                                                                                                                         1/3 
  Verifying        : kernel-ml-core-6.6.1-1.el8.elrepo.x86_64                                                                                                                    2/3 
  Verifying        : kernel-ml-modules-6.6.1-1.el8.elrepo.x86_64                                                                                                                 3/3 

Installed:
  kernel-ml-6.6.1-1.el8.elrepo.x86_64                    kernel-ml-core-6.6.1-1.el8.elrepo.x86_64                    kernel-ml-modules-6.6.1-1.el8.elrepo.x86_64                   

Complete!
[root@kernel-update ~]# grub2-mkconfig -o /boot/grub2/grub.cfg
Generating grub configuration file ...
done
[root@kernel-update ~]# grub2-set-default 0
[root@kernel-update ~]# reboot
[root@kernel-update ~]# uname -r
6.6.1-1.el8.elrepo.x86_64

4. Особенности проектирования и реализации решения
Изучив методичку, пришла к выводу, что не имеет большого смысла использование packer для решения задачи, если можно просто добавить исполнение того же скрипта в Vagrantfile. Поэтому здесь присутствует именно такое решение.
