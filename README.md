# Vagrant-CentOS6.5-Nginx-PHP5.5-MySQL
MacOS X (10.8) Vagrant + ChefでLaravel環境構築
## 環境
 * CentOS 6.5
 * Nginx
 * PHP5.5
 * MySQL 5.1

##  Vagrantインストール
下記、参考URLの通り
[参考URL](http://weblabo.oscasierra.net/install-vagrant-onto-macosx/ )

## VirtualBoxインストール
下記、参考URLの通り
[参考URL](http://weblabo.oscasierra.net/install-virtualbox-into-macosx/) 

## Box作成
1. VirualBoxのVagrant用ディレクトリをローカルに作成し、CentOS用のディレクトリを作成します  
```
$ cd /Users/yano/  
$ mkdir -P Vagrant/centos65_64-laravel
```  
~/Vagrant  
 - centos65  
 - ubunts  
 みたいに階層分けを想定してます。

1. Vagrant CentOSのイメージを追加します  
```
$ cd Vagrant/centos65_64-laravel
```
  * 64bit  
 ```
$ vagrant box add cent65-64 https://github.com/2creatives/vagrant-centos/releases/download/v6.5.3/centos65-x86_64-20140116.box
```
  * 32bit  
 ```
$ vagrant box add cent65-64 https://dl.dropbox.com/s/3fgr7lbvcpn51py/centos_6-5_i386.box
```

1. initを実行、CentOSを起動します  
```
$ vagrant init cent65-64
```  
```
$ vagrant up //起動
```
1. 仮想環境のCentOSへログインができます  
```
$ vagrant ssh
```

1. 毎回Vagrant/centos65_64-laravelに移動が面倒なので何処のディレクトリでもログインできるようにします  
```
$ vagrant ssh-config --host laravel >> ~/.ssh/config
```  
```
$ ssh laravel
```

1. VagrantFileの編集
ホストのディレクトリをホストのディレクトリをVMから参照できるように設定します。  
例）Gitからcloneするディレクトリを /Users/yano/Sites/lion-uとして想定した場合  
```
$ vim VagrantFile
```  
```
config.vm.synced_folder "/Users/yano/Sites/lion-u", "/vagrant_data"
```  
  * 設定を変更したらreloadします  
```
$ vagrant reload
```  
  * 確認します  
```
$ ssh laravel
```  
```
$ cd /vagrant_data
```  
## ミドルウェアインストール (Nginx, PHP, MySQL,)
仮想環境へNginx, PHP, MySQL, PHPMyAdminをインストールします。  

 1. Chefで作成したレシピを実行する為、knife-soloをインストールします  
```
$ gem install knife-solo
```

 1. centos65_64-laravel.zip を/Users/yano/Vagrant/centos65_64-laravelに回答し設置します  
 1. Cookを実行します  
```
$ knife solo cook laravel
```  
時間掛かります。。。
 1. ローカル環境からSeverNameでアクセスできるようの/etc/hostsを設定します  
```
$ vim /etc/hosts
```  
```  
192.168.33.10 lion-u.dev.vm
```  
```
192.168.33.10 lion-u.dev.mysql
```
 1. ローカルから仮想環境のURLにアクセスできれば完了です
  * [ライオン屋法人サイト] (http://lion-u.dev.vm/)
  * [PHPMyAdmin] (http://lion-u.dev.mysql/)

