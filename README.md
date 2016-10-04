# Problems-with-use-bindings-generator
使用 bindings-generator 遇到的一些问题，以及解决方法。

问题：windows使用的时候大多会出现路径错误的问题，用bat给genbindings.py固定参数就能避免出现问题
===
genbindings.bat

@echo off

set DIR=%~dp0

cd %DIR%

python %DIR%genbindings.py

echo end.

pause

原因：1.其实是genbindings.py需要全路径（windows中相对路径写成/path这样是不能访问的。脚本中是用当前file路径加/直接拼接的，所以直接genbindings.py没加路径的话，读取的当前file路径为空）

2.需要进入genbindings.py所在路径再执行命令（读取ini文件时，只使用的文件名，未加路径）。

问题：在player上运行正常，打成apk就不行了
=== 
(当前的apk包也可以再eclipse中看log).

1.检查是否引入mk文件，引入的是LOCAL_MODULE 一般名字以 _static 结尾

2.添加注册模块，即导出的tolua。 在项目中使用的与在player中不是同一个，需要再添加一次。（一般在Classes文件夹里的某个文件，3.3是lua_module_register.h）

3.检查mk是否正确。


mk命令理解
===
LOCAL_EXPORT_C_INCLUDES 暴露本包的一些头文件地址。
=
（LOCAL_EXPORT_C_INCLUDES := $(LOCAL_PATH)/include）引用本包，可以看到include的里面的头文件。 默认是mk所在地址。

遍历文件目录及子目录下的文件
=
LIB_SOURCES := $(LOCAL_PATH)/Sources

CLASSES_FILES := $(wildcard $(LIB_SOURCES)/*.cpp)

LOCAL_SRC_FILES := $(CLASSES_FILES:$(LOCAL_PATH)/%=%)
