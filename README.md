# rpmbuild
定制src.rpm包

```
rpmbuild -bb /root/rpmbuild/SPECS/nginx.spec
解释spec文件，在/var/tmp/下生成各段可执行shell，执行解释生成后shell脚本
可能的输出：
[root@localhost ~]# rpmbuild -bb /root/rpmbuild/SPECS/nginx.spec
Executing(%prep): /bin/sh -e /var/tmp/rpm-tmp.WSCOO4
+ umask 022
+ cd /root/rpmbuild/BUILD
+ exit 0
Executing(%build): /bin/sh -e /var/tmp/rpm-tmp.uBphhV
+ umask 022
+ cd /root/rpmbuild/BUILD
+ cd nginx-1.6.2
++ pcre-config --cflags
+ ./configure --prefix=/etc/nginx 
```

# spec文件内容

```
%prep                         #
%setup -q                     #解压源码包，屏蔽此指令可避免每次重复解压生成源码目录

%build
#cd %{_builddir}/%{name}-%{version}      #由于不屏蔽解压源码包指令后，build段脚本将不包括进入“源码”目录的指令，使得下列configure命令无法执行，可在build段加入定位目录的指令
./configure                              #执行配置编译

%install
#cd %{_builddir}/%{name}-%{version}      #同样的原因，make命令将在%{_builddir}目录执行，需要添加此指令修正目录
%{__rm} -rf $RPM_BUILD_ROOT
%{__make} DESTDIR=$RPM_BUILD_ROOT install
```

# spec文件中可以引用的系统宏变量定义：
/usr/lib/rpm/macros
/usr/lib/rpm/rpmrc

```
rpm --showrc
rpm --eval "%{_bindir}"
```
