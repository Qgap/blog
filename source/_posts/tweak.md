---
title: iOSRe(tweak)  
date: 2016-05-06 20:49:58

---

创建tweak 文件
虽然这个步骤已经被我实践过很多遍，但还是不免会忘记，毕竟好记性不如烂笔头。  

    1.$THEOS/bin/nic.pl          
       
    Mac-mini:~ mac$ /opt/theos/bin/nic.pl 
    NIC 2.0 - New Instance Creator
     ------------------------------
    [1.] iphone/activator_event
    [2.] iphone/application_modern
    [3.] iphone/cydget
    [4.] iphone/flipswitch_switch
    [5.] iphone/framework
    [6.] iphone/ios7_notification_center_widget
    [7.] iphone/library
    [8.] iphone/notification_center_widget
    [9.] iphone/preference_bundle_modern
    [10.] iphone/tool
    [11.] iphone/tweak
    [12.] iphone/xpc_service
    Choose a Template (required): 11
    Project Name (required): gap
    Package Name [com.yourcompany.gap]: com.gapyear.gap
    Author/Maintainer Name [Mac]: Mac
    [iphone/tweak] MobileSubstrate Bundle filter [com.apple.springboard]: com.apple.springboard
    [iphone/tweak] List of applications to terminate upon installation (space-separated, '-' for none) [SpringBoard]: SpringBoard
    Instantiating iphone/tweak in gap/...
    Done.
    
    
 这时，会在你执行`$THEOS/bin/nic.pl ` 命令下的目录中生成一个gap 的文件夹，文件中包括`control`,      
 `gap.plist`(如果是App，则为app 的`bundle id`,即第一步输入的`MobileSubstrate Bundle filter [com.apple.springboard]`),   
 `Makefile`,这里，需要在第一行添加`THEOS_DEVICE_IP = Your IP Address.`    
 `Tweak.xm` ,重头戏，这里写你需要`%hook`的方法。


Making Project
---
    make package
    make install
    

改完配置， 编写完代码。打开terminal，切换到`gap` 文件夹，执行`make package`,如果一切 正常，会生成一个`debs`的文件夹，里面有个`.deb`的文件（然而，更多情况是有错误。例如，有时可能报告`include $(THEOS)/makefiles/common.mk`这句， 找不到`common.mk`文件，这里可以将`$(THEOS)`换成`/opt/theos`）。   
成功编译生成`deb` 文件后，执行`make install`，这里会要求输入iPhone ssh password，然后会重启，(重启是因为在    `[iphone/tweak] List of applications to terminate upon installation (space-separated, '-' for none) [SpringBoard]`这步键入了一个`SpringBoard`,选择iphone 安装tweak 后的操作)。

