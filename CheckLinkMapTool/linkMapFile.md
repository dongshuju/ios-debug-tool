
### 检测APP可执行文件的组成

iOS APP编译后，除了一些资源文件，剩下的就是一个可执行文件，有时候项目大了，引入的库多了，可执行文件很大，想知道这个可执行文件的构成是怎样，里面的内容都是些什么，哪些库占用空间较高，可以用以下方法勘察：

1. XCode开启编译选项 Write Link Map File
XCode -> Project -> Build Settings -> Write Link Map File -> 把Write Link Map File选项设为yes，

2. XCode设置LinkMap
XCode -> Project -> Build Settings -> Path to Link Map -> 自定义 Link Map File path

<br> 默认配置为：$(TARGET_TEMP_DIR)/$(PRODUCT_NAME)-LinkMap-$(CURRENT_VARIANT)-$(CURRENT_ARCH).txt
<br>
<br>在文件中的位置为：`/Users/dongshuju/Library/Developer/Xcode/DerivedData/YouProjectName-fgjtbsbtvccsajbajxzzwxvgyzqc/Build/Intermediates.noindex/YouProjectName.build/Debug-iphoneos/YouProjectName.build/YouProjectName-LinkMap-normal-arm64.txt`
<br>
<br>这个LinkMap里展示了整个可执行文件的全貌，列出了编译后的每一个.o目标文件的信息（包括静态链接库.a里的），以及每一个目标文件的代码段，数据段存储详情。
这个文件可以让你了解整个APP编译后的情况，也许从中可以发现一些异常，还可以用这个文件计算静态链接库在项目里占的大小，有时候我们在项目里链了很多第三方库，导致
APP体积变大很多，我们想确切知道每个库占用了多大空间，可以给我们优化提供方向。
<br>
<br>LinkMap里有了每个目标文件每个方法每个数据的占用大小数据，本脚本统计出了每个.o最后的大小，属于一个.a静态链接库的.o加起来，就是这个库在APP里占用的空间大小。

 使用方法：

 > 1. 下载脚本
 > 2. cd 到脚本所在路径，执行 'node linkMap.js path/LinkMap-normal-arm64.txt -hl

 输出：
 ```
XXXXXStream+XXXXXClass.o	246B
XXXXXConversation+XXXXClass.o	245B
XXXXXRelationship+CoreDataClass.o	245B
...
 ```
