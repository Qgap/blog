
---
title: 砸壳与 class dump   

date: 2016-05-04 16:55:53   

---
 
##砸壳    

   
>
以前，总觉得，用的少的，没必要记录，因为你不知道下次用是什么时候。  
后来，才明白，越是用的少的，才有记录的必要，因为用的少，也很容易忘记，记录的也少，查起来也不是那么简单。  
这些步骤网上都有，而重写的目的是因为真实执行起来还是有些不同。所以还是予以记录，方便下次查阅这遍[砸壳](http://www.swiftyper.com/2016/05/02/iOS-reverse-step-by-step-part-1-class-dump/)文章记录的还挺详细。不过最后步骤有点繁琐。这里推荐比较简单的做法吧。    


1.dumpdecrypted
[源代码下载地址](https://github.com/stefanesser/dumpdecrypted/archive/master.zip) , 解压后,          

    $ cd /Users/gap/iOSRe/dumpdecrypted-master/
    $ make
    
 
或者从github 上clone     

    $git clone git://github.com/stefanesser/dumpdecrypted/ 
    $cd dumpdecrypted/ 
    $make

 
 上面的make命令执行完毕后，会在当前目录下生成一个`dumpdecrypted.dylib`文件，这就是我们等下砸壳所要用到的榔头。此文件生成一次即可，以后可以重复使用，下次砸壳时无须再重新编译。
 
 ---
 
上面准备了砸壳需要的工具，下面就是砸壳步骤了。在这，极力推荐使用`iFunBox`这个工具， 因为可视化的操作和基于USB 的操作，会很方便快捷。这里的步骤也是基于`iFunBox`搭配操作的，全代码的操作可以参考这个[iOSRe](http://bbs.iosre.com/t/dumpdecrypted-app/22)和上面提到的[砸壳](http://www.swiftyper.com/2016/05/02/iOS-reverse-step-by-step-part-1-class-dump/)     

---
1.如果你手机越狱成功，打开ifunbox，连接手机，会显示你的手机系统和手机上的应用程序。
![](/images/ifunbox.png)        


2.双击右侧出现的，你想砸壳的app ，会有Documents 文件夹，将mac 端上的`dumpdecrypted.dylib` 拖拽到Documents文件夹中。![wechat](/images/wechatfile.png)

3.打开terminal，通过ssh 连上你的手机     

       ＃ssh root@yourIP 
       ~ root# ps -e   (保持你需要砸壳的应用程序在前台)  
       10679 ??         0:14.58 /var/mobile/Containers/Bundle/Application/F6DB6407-4C2D-4C04-B5C2-9942DCC8870B/WeChat.app/WeChat
    10681 ??         0:00.43 /usr/libexec/misagent
    10687 ??         0:00.21 /usr/libexec/online-auth-agent
    10696 ??         0:11.96 /var/mobile/Containers/Bundle/Application/    2B97BE49-B373-4906-A8EC-B4BD2DF7683F/Twitter.app/Twitter
    10731 ??         0:00.44 /System/Library/PrivateFrameworks/SyncedDefaults.framework/Support/syncdefaultsd
    10734 ??         0:00.37 sshd: root@ttys000 
    10738 ??         0:00.20 /System/Library/Frameworks/UIKit.framework/Support/pasteboardd
  
  `/var/mobile/Containers/Bundle/Application/`字样的结果就是TargetApp可执行文件的全路径。如果像上图有多个，`/var/mobile/Containers/Bundle/Application/`,找准哪个是你需要砸壳的`app`就行，这里以`wechat`为例。
  

    ~ root# cd /var/mobile/Containers/Data/Application/19ECCADD-7DAE-4647-AD3A-A706056E54FD/Documents/  (图二上的路径)
    Documents#DYLD_INSERT_LIBRARIES=dumpdecrypted.dylib  /var/mobile/Containers/Bundle/Application/F6DB6407-4C2D-4C04-B5C2-9942DCC8870B/WeChat.app/WeChat (上面ps-e 命令找出来的路径)   
      
   最后会生成`.decrypted` 为扩展名的文件--(在图二中的`Documents`文件夹中)。把它拷贝到电脑上，使用Hopper Disassembler 软件打开，打开方式是tool 上的file ，然后通过read executable..导入，
         
    
    cltr +d 退出cycript 模式。
    
  
##class dump

  下载class dump [http://stevenygard.com/projects/class-dump/](http://stevenygard.com/projects/class-dump/)。
  然后将`class-dump` 复制到`/usr/bin`下，然而，这会报错`cp: /usr/bin/class-dump: Permission denied` ，可以参考[iosre论坛的这篇文章解决](http://bbs.iosre.com/t/10-11-usr-bin-class-dump/1936)。  
  安装好后，键入`class-dump`,会显示版本号和参数![classdump](/images/classdump.png)
    使用方法  
     
    class-dump -S -s -H path/xxx.decrypted -o path/Headers/ 
    
---
对逆向感兴趣的，建议多多翻阅[iOS逆向工程](https://www.amazon.cn/iOS%E5%BA%94%E7%94%A8%E9%80%86%E5%90%91%E5%B7%A5%E7%A8%8B-%E6%B2%99%E6%A2%93%E7%A4%BE/dp/B00VFDVY7E/ref=sr_1_1?ie=UTF8&qid=1462178712&sr=8-1&keywords=ios+%E9%80%86%E5%90%91%E5%B7%A5%E7%A8%8B)，入门的不二选择。    
  
  
 
  
  
  
  
  
