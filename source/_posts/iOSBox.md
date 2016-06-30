---
title: iOS 开发中遇到的问题及解决方法。  

date: 2016-06-20 23:10:17

---

> 上周看完《程序员45个习惯》，中间有那么一条，是记录开发日志，所以本文主要是记录总结 自己在iOS 开发中遇到的问题和解决方法。方便日后查看

---
 `NSString`    
 去掉 `NSString`中的空格和换行，`stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]` 但是这只能去掉头部和尾部的空格和换行符，中间的没法去掉。   
 
如果去掉两端的空格和换行后，需要将中间的去掉，结合下面的方法。
 
    NSArray *components = [string  componentsSeparatedByCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]];
    
    string = [components componentsJoinedByString:@""];
 
 参考[nshipster NSCharacter​Set](http://nshipster.cn/nscharacterset/)
 
 
  替换`NSString` 中指定的字符，    
  
      [string stringByReplacingOccurrencesOfString:@"-" withString:@""];
 
 ----
 
 `NSNotificationCenter`
 
 `NSNotificationCenter`  使用顺序，`addObserver`在先， 而后`postNotificationName` ；
 可能crash 的原因，   
 1. 使用后没有`removeObserver`，在升级`xcode 7.3`后遇到过，从而导致crash；   
 2. `post`操作的线程和`addObserver`的线程不一致而引发的。[南峰子博客 Notification与多线程](http://southpeak.github.io/blog/2015/03/14/nsnotificationyu-duo-xian-cheng/)；  
 
    
 
          dispatch_async(dispatch_get_main_queue(), ^{
                [[NSNotificationCenter defaultCenter] postNotificationName:AFNetworkingTaskDidCompleteNotification object:task userInfo:userInfo];
            });
 
 
---

 
  
 
 
 
 
