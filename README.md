### to view package details
sudo apt-cache show package_name

--------------------------------------------------------------------------------

### installing all dependancies for ryu

sudo apt-get install python-pbr python-py python-six python-oslo.config python-eventlet python-lxml python-netaddr python-paramiko python-routes python-webob python-sphinx python-pip
sudo apt-get install git python-pip python-dev libxml2-dev libxslt1-dev -y


source : http://ewen.mcneill.gen.nz/blog/entry/2014-08-31-ryu-on-ubuntu-14-04/

--------------------------------------------------------------------------------

### to change date and time at one go

sudo date -s '11 JUL 2017 01:01:30'

--------------------------------------------------------------------------------

### Install a software over multiple hosts


parallel-ssh -i -h host_list.txt sudo apt-get -y install vim

--------------------------------------------------------------------------------

### Running Synergy client before login in Ubuntu MATE 16.04


Edit the file  `/etc/lightdm/lightdm.conf` and add the following at the end of the file


[SeatDefaults]
greeter-setup-script=/usr/bin/synergyc <OPTIONS> <SERVER HOSTNAME>


In my case, options was blank. I just added the server IP.
Do not try to edit the rc.local or any files. Sounds good, doesn't work.

--------------------------------------------------------------------------------

### Run cvlc as root


sed -i 's/geteuid/getppid/' /usr/bin/vlc

--------------------------------------------------------------------------------

#### execute a command in a separate terminal


xterm -hold -e python archive.py 600 &

--------------------------------------------------------------------------------

when you want a VM to be discoverable from outside and have access to the 
internet, add NAT and bridged networking (Two network interfaces)

--------------------------------------------------------------------------------

### controlling rate for a particular port
tc qdisc del dev eth0 root
tc qdisc add dev eth0 root handle 1:0 htb default 10
tc class add dev eth0 parent 1:0 classid 1:10 htb rate 1024kbps ceil 2048kbps prio 0
iptables -A OUTPUT -t mangle -p tcp --sport 80 -j MARK --set-mark 10
tc filter add dev eth0 parent 1:0 prio 0 protocol ip handle 10 fw flowid 1:10

--------------------------------------------------------------------------------

### disable NTP at startup

sudo update-rc.d -f ntp remove

### stop NTP service

sudo service ntp stop

--------------------------------------------------------------------------------

### switch from Ubuntu GUI to CUI after reboot
systemctl set-default multi-user.target

### switch from Ubuntu CUI to GUI after reboot
systemctl set-default graphical.target

##### works for Debian as reported over 
here [https://unix.stackexchange.com/questions/264393/how-to-disable-x-server-autostart-in-debian-jessie]
##### works on Ubuntu Mate (Personally checked)

--------------------------------------------------------------------------------

### when closing laptop lid, do nothing

sudo vim /etc/UPower/UPower.conf
set `IgnoreLid=true`

--------------------------------------------------------------------------------

### creating a user in linux

useradd tmp_user            # no home directory created
useradd -m tmp_user         # home directory created
passwd tmp_user             # creating a password for the user
usermod -aG sudo tmp_user   # adding user to sudo
chsh -s /bin/bash tmp_user  # changing default shell of user to bash

--------------------------------------------------------------------------------

### mapping graphic tablet to one monitor

* Check the wacom device name using the following command
	xinput --list
* Rotate the orientation if required
	xsetwacom set "Wacom Bamboo One S Pen stylus" rotate HALF
* Map to a screen if required
	xsetwacom set "Wacom Bamboo One S Pen stylus" MapToOutput DP2

--------------------------------------------------------------------------------

## Arch Linux Stuff

### install a package
sudo pacman -S $package

### update repo
sudo pacman -Syy

### upgrade
sudo pacman -Su

## Things to keep in mind while installing Antergos in IIT KGP

While installing Antergos in IIT KGP, make sure to do the following

* Make sure to have internet
* Start with live boot, don't touch other options
* After starting the live boot, set the proxy in the following areas
	* In .bashrc. Make sure to add http_proxy and https_proxy
	* In pacman proxy file. Run the following commands.
 
    	`sudo visudo`
    	`Defaults env_keep += "https_proxy http_proxy"`

    Source (https://ketansingh.net/setting-proxy-for-archlinux/) 
	* In chchi, when it asks for proxy
* When it asks for download repo lists, click default, don't go for recommended. The installer hangs.
