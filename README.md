# Problems-with-use-bindings-generator
使用 bindings-generator 遇到的一些问题，以及解决方法。

windows使用的时候大多会出现路径错误的问题，用bat给genbindings.py固定参数就能避免出现问题
genbindings.bat
===
@echo off

set DIR=%~dp0

cd %DIR%

python %DIR%genbindings.py

echo end.

pause
