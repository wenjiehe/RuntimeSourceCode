# RuntimeSourceCode

## 环境介绍

* Xcode版本：11.2.1
* macOS版本：10.15.1
* runtime版本：objc4-750.1

## apple官方源码地址

* [源码下载](https://opensource.apple.com/release/macos-10141.html)

## 编译源码

1. 报错信息如下：

* Build target objc:The i386 architecture is deprecated. You should update your ARCHS build setting to remove the i386 architecture.
* Build target objc-trampolines:The i386 architecture is deprecated. You should update your ARCHS build setting to remove the i386 architecture.
   
```
解决方法：TARGETS->objc->Build Settings->Architectures->&(ARCHS_STANDARD_32_64_BIT)改为Standard Architectures(64-bit Intel)
同理，objc-trampolines也如此
```

* can't open order file: /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.15.sdk/AppleInternal/OrderFiles/libobjc.order
        
```
解决方法:TARGETS->objc->Build Settings->Linking->Order File->Debug/Release->/AppleInternal/OrderFiles/libobjc.order修改为$(SRCROOT)/libobjc.order
```
        
* library not found for -lCrashReporterClient

```
解决方法:TARGETS->objc->Build Settings->Linking->Other Linker Flags->Debug/Release->删除-lCrashReporterClient
```        
   
* Undefined symbol:_obj_opt_class

```
解决方法:TARGETS->objc->Build Phases->Link Binary With Libraries->添加libobjc.A.tbd
```

* SDK "macosx.internal" cannot be located

```
解决方法:TARGETS->objc->Build Phases->Run Script->将脚本里面的macosx.internal换成macosx
```

* no such public header file:'/tmp/objc.dst/usr/include/objc/ObjectiveC.apinotes'

```
解决方法:
TARGETS->objc->Build Settings->Text-Based API->Other Text-Based InstallAPI Flags->把里面的内容设置为空
TARGETS->objc->Build Settings->Text-Based API->Text-Based InstallAPI Verification Mode->Errors Only
```

* Static declaration of '_pthread_has_direct_tsd' follows non-static declaration
* Static declaration of '_pthread_getspecific_direct' follows non-static declaration
* Static declaration of '_pthread_setspecific_direct' follows non-static declaration

```
解决方法:由于重复定义注释,分别将pthread_machdep.h文件中的_pthread_has_direct_tsd、_pthread_setspecific_direct、_pthread_getspecific_direct注释即可
```

2. 缺少头文件

* 'sys/reason.h' file not found
* 'mach-o/dyld_priv.h' file not found
* 'os/tsd.h' file not found
* 'os/lock_private.h' file not found
* 'os/base_private.h' file not found
* 'pthread/tsd_private.h' file not found
* 'pthread/spinlock_private.h' file not found
* 'System/machine/cpu_capabilities.h' file not found
* 'System/pthread_machdep.h' file not found
* 'Block_private.h' file not found
* 'objc-shared-cache.h' file not found
* '_simple.h' file not found

```
解决方法：新建CommonHeaders文件夹，并把CommonHeaders添加到Header Search Paths
步骤:TARGETS->objc->Build Settings->Search Paths->Header Search Paths->Debug和Release中都加入$(SRCROOT)/CommonHeaders
```
        
* 'CrashReporterClient.h' file not found

```
解决方法:将CrashReporterClient.h文件放到CommonHeader目录下，并修改Preprocessor Macros
步骤:TARGETS->objc->Build Settings->Apple Clang-Preprocessing->Preprocessor Macros->Debug/Release添加LIBC_NO_LIBCRASHEPORTERCLIENT
```
        
* 'isa.h' file not found

```
解决方法:将runtime文件夹下的isa.h文件拷贝到CommonHeaders文件夹下
```

3. objc-runtime-new.mm编译错误

* use of undeclared identifier 'DYLD_MACOSX_VERSION_10_11'

```
解决方法:在dyld_priv.h文件头部添加宏

#define DYLD_MACOSX_VERSION_10_11 0x000A0B00
#define DYLD_MACOSX_VERSION_10_12 0x000A0C00
#define DYLD_MACOSX_VERSION_10_13 0x000A0D00
#define DYLD_MACOSX_VERSION_10_14 0x000A0E00
```

## 调试Runtime

1. 添加新的Target

> Xcode->File->New->Target,选择macOS->Command Line Tool

2. 运行 

> 选择新的target,比如runtime_test，运行，在objc-runtime.mm和objc-runtime-new.mm中打断点即可


