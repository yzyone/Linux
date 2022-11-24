# Shell脚本：判断文件、文件夹是否存在

一、语法说明

```
-e filename #如果 filename为目录，则为真
-f filename #如果 filename为常规文件，则为真
-L filename #如果 filename为符号链接，则为真
-r filename #如果 filename可读，则为真
-w filename #如果 filename可写，则为真
-x filename #如果 filename可执行，则为真
-s filename #如果文件长度不为0，则为真
-h filename #如果文件是软链接，则为真
```

二、实例
1、判断文件夹是否存在

```
#如果文件夹不存在，则创建文件夹
tempPath="/home/parasaga/blank"
if [ ! -d "$tempPath" ]; then
mkdir $blankPath
fi`
```

2、判断文件是否存在

```
#如果文件不存在，则创建文件
tempFile="/home/parasaga/blank/error.log"
if [ ! -f "$tempFile" ]; then
touch $tempFile
fi
```

————————————————

版权声明：本文为CSDN博主「willingtolove」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。

原文链接：https://blog.csdn.net/willingtolove/article/details/119932856