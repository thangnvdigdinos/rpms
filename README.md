rpms
====

* Reference
    * [nginxとredisのrpmを作ってみた(fedora16 x86_64用)](http://spring-mt.tumblr.com/post/23156957111/nginx-redis-rpm-fedora16-x86-64)
    * [redisのrpmを作ってみた(centos 6.2編)](http://spring-mt.tumblr.com/post/26409034144/redis-rpm-centos-6-2)

## libyaml
* version
    * 0.1.3
    * OS
        * centos6.2 x86_64
    * source
        * http://ftp.jaist.ac.jp/pub/Linux/Fedora/epel/6/SRPMS/

## nginx
* version
    * 1.2.0
    * OS
        * fedora16 x86_64
    * source
        * http://nginx.org/en/download.html

## redis
* version
    * 2.4.13
    * OS
        * fedora16 x86_64
    * source
        * http://redis.io/download
* version
    * 2.4.15
    * OS
        * centos6.2 x86_64

## td-agent
* version
    * 0.10.6
* OS
    * fedora16 x86_64
* source 
    * http://packages.treasure-data.com/redhat/source/

~~~~
# wget http://packages.treasure-data.com/redhat/source/td-agent-1.1.6-0.src.rpm
# rpm -ivh td-agent-1.1.6-0.src.rpm 
   1:td-agent               warning: user build does not exist - using root
warning: group build does not exist - using root
warning: user build does not exist - using root8%)
warning: group build does not exist - using root
warning: user build does not exist - using root
warning: group build does not exist - using root
########################################### [100%]

# cd /root/rpmbuild/SOURCES/
# wget https://github.com/downloads/fluent/fluentd/fluentd-0.10.6.tar.gz
# cd /root/rpmbuild/SPECS/
# vim td-agent.spec 
一応確認したが、何も修正はしてない
# rpmbuild -bb td-agent.spec 
error: Failed build dependencies:
	libtool is needed by td-agent-1.1.6-0.fc16.x86_64
	readline-devel is needed by td-agent-1.1.6-0.fc16.x86_64
# yum install libtool readline-devel
# rpmbuild -bb td-agent.spec 
Executing(%prep): /bin/sh -e /var/tmp/rpm-tmp.odAwVg
+ umask 022
+ cd /root/rpmbuild/BUILD
+ cd /root/rpmbuild/BUILD
+ rm -rf td-agent-1.1.6
・
・
Executing(%clean): /bin/sh -e /var/tmp/rpm-tmp.7IcAUq
+ umask 022
+ cd /root/rpmbuild/BUILD
+ cd td-agent-1.1.6
+ rm -rf /root/rpmbuild/BUILDROOT/td-agent-1.1.6-0.fc16.x86_64
+ exit 0

所々下記エラーwarningがでたが、libyamlの問題か。。。このままでも大丈夫かな
It seems your ruby installation is missing psych (for YAML output).
To eliminate this warning, please install libyaml and reinstall your ruby.

# cd ../RPMS/x86_64/
# rpm -ivh td-agent-1.1.6-0.fc16.x86_64.rpm
error: Failed dependencies:
	td-libyaml is needed by td-agent-1.1.6-0.fc16.x86_64
# vim /etc/yum.repos.d/td.repo
[treasuredata]
name=TreasureData
baseurl=http://packages.treasure-data.com/redhat/$basearch
gpgcheck=0

# yum install td-libyaml
# rpm -ivh td-agent-1.1.6-0.fc16.x86_64.rpm 
Preparing...                ########################################### [100%]
   1:td-agent               ########################################### [100%]
adding 'td-agent' group...
adding 'td-agent' user...
Installing default conffile  ...
prelink detected. Installing /etc/prelink.conf.d/td-agent-ruby.conf ...
Configure td-agent to start, when booting up the OS...
# /etc/init.d/td-agent start
Starting td-agent (via systemctl):                         [  OK  ]
# rpm -e td-agent-1.1.6-0.fc16.x86_64
一応削除しておく

~~~~


