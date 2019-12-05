# RuntimeSourceCode

## 环境介绍
* Xcode版本：11.2.1
* macOS版本：10.15.1
* runtime版本：objc4-750.1

[源码下载](https://opensource.apple.com/release/macos-10141.html)


## 编译源码

1. 报错信息如下：

        Build target objc:The i386 architecture is deprecated. You should update your ARCHS build setting to remove the i386 architecture.
        Build target objc-trampolines:The i386 architecture is deprecated. You should update your ARCHS build setting to remove the i386 architecture.
    
        解决方法：TARGETS->objc->Build Settings->Architectures->&(ARCHS_STANDARD_32_64_BIT)改为Standard Architectures(64-bit Intel)
        同理，objc-trampolines也如此

2. 缺少头文件

        'sys/reason.h' file not found objc-os.h

        解决方法：
