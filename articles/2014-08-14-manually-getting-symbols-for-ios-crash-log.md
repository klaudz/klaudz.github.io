---
title: "手动还原iOS Crash Log"
date: "2014-08-14"
---

很多时候需要手动还原iOS上的crash日志， 假如你已经弄到了符号文件（.dSYM）和crash log（.crash或.plist）， 假如你懂得怎么打开Terminal， 那我将为你介绍以下两种方法。

# 测试环境

下面例子中，KLMyApp是我用于测试的程序， 我把所有文件置于“~/KLMyApp”目录中，其目录结构如下：

\[code light="true"\] ~/KLMyApp |--KLMyApp.app.dSYM（符号文件） |--KLMyApp\_klaudz-iPhone-5.plist（crash log） \[/code\]

其中，crash log的部分片段如下：

\[objc\] Exception Type: EXC\_CRASH (SIGABRT) Exception Codes: 0x0000000000000000, 0x0000000000000000 Triggered by Thread: 0

Last Exception Backtrace: (0x30738e7e 0x3aa956c2 0x3066eff4 0x40488 0x32ef1d9e 0x32ef1d3a 0x32ef1d0e 0x32edd73e 0x32ef1756 0x32ef1420 0x32eec44c 0x32ec1d74 0x32ec0564 0x30703f1a 0x307033e2 0x30701bd2 0x3066c46c 0x3066c24e 0x353a62e6 0x32f21840 0x404d6 0x40234)

Thread 0 Crashed: 0 libsystem\_kernel.dylib 0x3b0451fc 0x3b032000 + 78332 1 libsystem\_pthread.dylib 0x3b0aca4e 0x3b0a9000 + 14926 2 libsystem\_c.dylib 0x3aff6028 0x3afac000 + 303144 3 libc++abi.dylib 0x3a44498a 0x3a444000 + 2442 4 libc++abi.dylib 0x3a45d6e2 0x3a444000 + 104162 5 libobjc.A.dylib 0x3aa95936 0x3aa8d000 + 35126 6 libc++abi.dylib 0x3a45b1b0 0x3a444000 + 94640 7 libc++abi.dylib 0x3a45ad12 0x3a444000 + 93458 8 libobjc.A.dylib 0x3aa9580a 0x3aa8d000 + 34826 9 CoreFoundation 0x3066c4e2 0x30664000 + 34018 10 CoreFoundation 0x3066c24e 0x30664000 + 33358 11 GraphicsServices 0x353a62e6 0x3539f000 + 29414 12 UIKit 0x32f21840 0x32eb2000 + 456768 13 KLMyApp 0x000404d6 0x3a000 + 25814 14 KLMyApp 0x00040234 0x3a000 + 25140

Thread 1: 0 libsystem\_kernel.dylib 0x3b032838 0x3b032000 + 2104 1 libdispatch.dylib 0x3af810d0 0x3af79000 + 32976 2 libdispatch.dylib 0x3af7b61e 0x3af79000 + 9758 \[/objc\]

# 检查UUID

**（如果你确认手中的crash log和符号文件是相对应的，则可跳过此步检查。）**

每次编译程序，其可执行文件都会产生一个唯一的UUID。 因此，只要检查crash log上的UUID和符号文件中的可执行文件的UUID是否对应得上。

1. 查看crash log的UUID
    
    \[bash\] grep "KLMyApp arm" KLMyApp\_klaudz-iPhone-5.plist # KLMyApp对应app可执行文件的名字 # 或 # grep --after-context=1 "Binary Images:" KLMyApp\_klaudz-iPhone-5.plist
    
    \## 回显 ## 0x3a000 - 0x41fff KLMyApp armv7s <4308010235fd33eda80ecbfc5e009e09> /var/mobile/Applications/89E8671A-CF63-42CD-8E6E-60942BE3BBDC/KLMyApp.app/KLMyApp \[/bash\]
    
    其UUID为“4308010235fd33eda80ecbfc5e009e09”。
2. 查看app的可执行文件的UUID
    
    \[bash\] dwarfdump --uuid KLMyApp.app/KLMyApp
    
    \## 回显 ## UUID: 709C2B2E-DAD9-388B-B710-A6A8D2A4FBBD (armv7) KLMyApp.app/KLMyApp UUID: 43080102-35FD-33ED-A80E-CBFC5E009E09 (armv7s) KLMyApp.app/KLMyApp UUID: 145920FD-F6D0-3CDE-A1A8-2AFC09E1E43B (arm64) KLMyApp.app/KLMyApp \[/bash\]
    
    可以看到可执行文件对应不同CPU架构的UUID，其中armv7s的UUID为“43080102-35FD-33ED-A80E-CBFC5E009E09”，与crash log配对。

# 开始还原

1. ## symbolicatecrash
    
    1. 首先定位symbolicatecrash的路径。 假如Xcode的安装路径为“/Applications/Xcode.app”。 那么，在Xcode < 4.3，路径为
        
        \[bash\] /Developer/Platforms/iPhoneOS.platform/Developer/Library/PrivateFrameworks/DTDeviceKit.framework/Versions/A/Resources/symbolicatecrash \[/bash\]
        
        在Xcode >= 4.3，路径为
        
        \[bash\] /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/PrivateFrameworks/DTDeviceKitBase.framework/Versions/A/Resources/symbolicatecrash \[/bash\]
        
        鉴于不同版本的Xcode对应的symbolicatecrash的路径不同，可以在对应目录搜索一下，例如：
        
        \[bash\] find /Applications/Xcode.app -name symbolicatecrash -type f # 其中“/Applications/Xcode.app”为搜索目录 \[/bash\]
        
    2. 设置developer\_dir环境变量
        
        \[bash\] export DEVELOPER\_DIR="/Applications/Xcode.app/Contents/Developer" \[/bash\]
        
        如果不设置可能会得到“Error: "DEVELOPER\_DIR" is not defined at ./symbolicatecrash line 60.”的错误。
    3. 还原crash log cd到symbolicatecrash目录，然后执行还原
        
        \[bash\] # 先'cd'到symbolicatecrash目录 ./symbolicatecrash ~/KLMyApp/KLMyApp\_klaudz-iPhone-5.plist ~/KLMyApp/KLMyApp.app.dSYM \[/bash\]
        
        还原结果的片段如下：
        
        \[objc collapse="true"\] Exception Type: EXC\_CRASH (SIGABRT) Exception Codes: 0x0000000000000000, 0x0000000000000000 Triggered by Thread: 0
        
        Last Exception Backtrace: 0 CoreFoundation 0x30738e7e \_\_exceptionPreprocess + 126 1 libobjc.A.dylib 0x3aa956c2 objc\_exception\_throw + 34 2 CoreFoundation 0x3066eff4 -\[\_\_NSArrayI objectAtIndex:\] + 172 3 KLMyApp 0x00040488 -\[KLViewController raiseCrashing\] (KLViewController.m:51) 4 UIKit 0x32ef1d9e -\[UIApplication sendAction:to:from:forEvent:\] + 86 5 UIKit 0x32ef1d3a -\[UIApplication sendAction:toTarget:fromSender:forEvent:\] + 34 6 UIKit 0x32ef1d0e -\[UIControl sendAction:to:forEvent:\] + 42 7 UIKit 0x32edd73e -\[UIControl \_sendActionsForEvents:withEvent:\] + 370 8 UIKit 0x32ef1756 -\[UIControl touchesEnded:withEvent:\] + 590 9 UIKit 0x32ef1420 -\[UIWindow \_sendTouchesForEvent:\] + 524 10 UIKit 0x32eec44c -\[UIWindow sendEvent:\] + 828 11 UIKit 0x32ec1d74 -\[UIApplication sendEvent:\] + 192 12 UIKit 0x32ec0564 \_UIApplicationHandleEventQueue + 7112 13 CoreFoundation 0x30703f1a \_\_CFRUNLOOP\_IS\_CALLING\_OUT\_TO\_A\_SOURCE0\_PERFORM\_FUNCTION\_\_ + 10 14 CoreFoundation 0x307033e2 \_\_CFRunLoopDoSources0 + 202 15 CoreFoundation 0x30701bd2 \_\_CFRunLoopRun + 626 16 CoreFoundation 0x3066c46c CFRunLoopRunSpecific + 520 17 CoreFoundation 0x3066c24e CFRunLoopRunInMode + 102 18 GraphicsServices 0x353a62e6 GSEventRunModal + 134 19 UIKit 0x32f21840 UIApplicationMain + 1132 20 KLMyApp 0x000404d6 main (main.m:16) 21 KLMyApp 0x00040234 start + 36
        
        Thread 0 Crashed: 0 libsystem\_kernel.dylib 0x3b0451fc \_\_pthread\_kill + 8 1 libsystem\_pthread.dylib 0x3b0aca4e pthread\_kill + 54 2 libsystem\_c.dylib 0x3aff6028 abort + 72 3 libc++abi.dylib 0x3a44498a abort\_message + 70 4 libc++abi.dylib 0x3a45d6e2 default\_terminate\_handler() + 250 5 libobjc.A.dylib 0x3aa95936 \_objc\_terminate() + 190 6 libc++abi.dylib 0x3a45b1b0 std::\_\_terminate(void (\*)()) + 76 7 libc++abi.dylib 0x3a45ad12 \_\_cxa\_rethrow + 98 8 libobjc.A.dylib 0x3aa9580a objc\_exception\_rethrow + 38 9 CoreFoundation 0x3066c4e2 CFRunLoopRunSpecific + 638 10 CoreFoundation 0x3066c24e CFRunLoopRunInMode + 102 11 GraphicsServices 0x353a62e6 GSEventRunModal + 134 12 UIKit 0x32f21840 UIApplicationMain + 1132 13 KLMyApp 0x000404d6 main (main.m:16) 14 KLMyApp 0x00040234 start + 36
        
        Thread 1: 0 libsystem\_kernel.dylib 0x3b032838 kevent64 + 24 1 libdispatch.dylib 0x3af810d0 \_dispatch\_mgr\_invoke + 228 2 libdispatch.dylib 0x3af7b61e \_dispatch\_mgr\_thread + 34 \[/objc\]
        
2. ## atos
    
    先cd到测试目录“~/KLMyApp”。
    
    方法有如下两种：
    1. 基于可执行文件的base address（基址）
        
        \[bash\] atos -o executable -arch architecture address ... \[/bash\]
        
        首先，要取得可执行文件的base address：
        
        \[bash highlight="11"\] # otool -arch armv7s -l KLMyApp.app.dSYM/Contents/Resources/DWARF/KLMyApp | grep -B 1 -A 10 "LC\_SEGM" | grep -B 3 -A 8 "\_\_TEXT" otool -arch armv7s -l KLMyApp.app.dSYM/Contents/Resources/DWARF/KLMyApp \\ | grep -B 1 -A 10 "LC\_SEGM" \\ | grep -B 3 -A 8 "\_\_TEXT"
        
        \## 回显 ## Load command 3 cmd LC\_SEGMENT cmdsize 532 segname \_\_TEXT vmaddr 0x00004000 # 此为base address vmsize 0x00008000 fileoff 0 filesize 0 maxprot 0x00000005 initprot 0x00000005 nsects 7 flags 0x0 \[/bash\]
        
        其中，vmaddr对应的**0x00004000**即为base address。
        
        然后，计算出函数在可执行文件的地址。 以Thread 0第13行的地址为例。
        
        \[objc highlight="4"\] Thread 0 Crashed: ...... 12 UIKit 0x32f21840 0x32eb2000 + 456768 13 KLMyApp 0x000404d6 0x3a000 + 25814 14 KLMyApp 0x00040234 0x3a000 + 25140 \[/objc\]
        
        函数在可执行文件的地址addressInExecutable的计算方法为：
        
        \[code light="true"\] addressInExecutable = baseAddress + offset = 0x00004000 + 25814 = 0x0000a4d6 \[/code\]
        
        最后进行还原：
        
        \[bash\] xcrun atos -o KLMyApp.app.dSYM/Contents/Resources/DWARF/KLMyApp -arch armv7s
        
        \## 输入 ## # 输入计算得出的（可执行文件中的）函数地址 0x0000a4d6
        
        \## 回显 ## main (in KLMyApp) (main.m:16) \[/bash\]
        
    2. 基于内存的load address（加载入口地址）
        
        \[bash\] atos -o executable -arch architecture -l loadAddress address ... \[/bash\]
        
        值得注意，crash log上的地址“0x000404d6 0x3a000 + 25814”是运行时的地址，是经过[ASLR](http://theiphonewiki.com/wiki/ASLR "ASLR - The iPhone Wiki")处理的。 “0x3a000”是运行时的load address，并非可执行文件的base address。 load address由系统随机分配，因此，“0x000404d6”也是运行时的且可变的，不能用于第一种方法的还原。
        
        此方法更简便些，无需取得base address，也无需进行地址计算，只需要直接使用load address和基于load address偏移的地址即可。
        
        还是以方法一的地址进行说明。 还原方法如下：
        
        \[bash\] xcrun atos -o KLMyApp.app.dSYM/Contents/Resources/DWARF/KLMyApp -arch armv7s -l 0x3a000 # 参数-l后接load address
        
        \## 回显 ## got symbolicator for KLMyApp.app.dSYM/Contents/Resources/DWARF/KLMyApp, base address 4000
        
        \## 输入 ## # 直接输入crash log上对应的（运行时的）函数地址 0x000404d6
        
        \## 回显 ## main (in KLMyApp) (main.m:16) \[/bash\]
        
        还原成功，其结果与方法一是一致的。
        
        又如， Exception Stack只有函数地址，没有函数偏移值，使用方法一要进行运算才可还原。 而用方法二显然会更快捷。
        
        \[objc highlight="3"\] Last Exception Backtrace: (0x30738e7e 0x3aa956c2 0x3066eff4 0x40488 0x32ef1d9e 0x32ef1d3a 0x32ef1d0e 0x32edd73e 0x32ef1756 0x32ef1420 0x32eec44c 0x32ec1d74 0x32ec0564 0x30703f1a 0x307033e2 0x30701bd2 0x3066c46c 0x3066c24e 0x353a62e6 0x32f21840 0x404d6 0x40234) \[/objc\]
        
        \[bash\] xcrun atos -o KLMyApp.app.dSYM/Contents/Resources/DWARF/KLMyApp -arch armv7s -l 0x3a000
        
        \## 输入 ## 0x40488
        
        \## 回显 ## -\[KLViewController raiseCrashing\] (in KLMyApp) (KLViewController.m:51) \[/bash\]
