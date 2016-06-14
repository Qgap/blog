---
title: iOSRe (二) 配置THEOS环境  
date: 2016-05-05 14:37:26

---
基本配置

#####1.指定THEOS 的安装路径，  
      export THEOS=/opt/theos   

#####2.从Github 上下载Theos。 
      Gapyear:~ gap$ sudo git clone --recursive git://github.com/DHowett/theos.git $THEOS
 
#####3.配置ldid，从 http://joedj.net/ldid 下载ldid , 然后移至“/opt/theos/bin” 下，然后以下命令赋予可执行权限：   
    - Gapyear:~ gap$ sudo mv Downloads/ldid /opt/theos/bin/   
    - Gapyear:~ gap$ sudo chmod 777 /opt/theos/bin/ldid 

#####4.配置CydiaSubstrate
- 最简单的方法，将越狱后的手机，连上Mac ，然后使用打开iFunBox,将 “文件系统下的 `/Library/Frameworks/CydiaSubstrate.framework/CydiaSubstrate`“ 拷贝到OSX 中，
- 并且将其重命名为`libsubstrate.dylib` 后放到"`/opt/theos/lib/`" 中。     
  
#####5. 配置`dpkg－deb` ，这是我花最长时间配置的一步。先从 [github download dm.pl 文件](https://github.com/DHowett/dm.pl) ,然后将这里面的 `dm.pl` 重新命名为 `dpkg-deb` ， 这里，一定要记住，OS X 中默认的隐藏了文件的扩展属性，这里一定要注意扩展属性，是文件名全部替换，包括扩展后缀。  同样，将`dpkg-deb` 放到`/opt/theos/bin` 目录下   

    - Gapyear:~ gap$ sudo mv Downloads/dm.pl-master/dpkg-deb /opt/theos/bin/

    - Gapyear:~ gap$ sudo chmod 777 /opt/theos/bin/dpkg-deb 




----

以上是配置`THEOS` 过程，以下是参考资料

[Theos/Getting_Started#Setting_Up_Dependencies](http://iphonedevwiki.net/index.php/Theos/Getting_Started#Setting_Up_Dependencies)

[dpkg-bed Q](http://bbs.iosre.com/t/dpkg-deb/3321)