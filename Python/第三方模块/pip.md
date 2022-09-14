最新的 Python 安装程序已经集成了 pip，pip 可以帮助我们方便地管理 Python 第三方包（库）。我们可以在...\Python37\Scripts\目录下查看是否存在 pip.exe 文件，并确保该目录已添加到“环境变量”的“PATH”下面。打开 Windows 命令提示符，输入“pip”命令，确保该命令可以执行。

pip不能用时：
python -m ensurepip
python -m pip install --upgrade pip

pip -h
pip help
pip show

pip 命令 | 命令解释
---- | ----
pip download 软件包名[\=\=版本号] | 下载扩展库的指定版本，如果未指定，则下载库中的最新版本
pip list | 列出当前环境下所有已经安装的模块
pip install 软件包名[\=\=版本号] | 在线安装指定版本号的软件包，如果未指定版本号，则下载最新版
pip install 软件包名.whl | 通过 whl 离线安装文件进行安装
pip install 包 1 包 2 包 3 … | 支持在线依次安装包 1、包 2 等，包名之间用空格隔开
pip install -r list.txt | 依次安装在 list.txt 中指定的扩展包
pip install --upgrade 软件包 | 升级软件包
pip uninstall 软件包名[\=\=版本号] | 卸载指定版本的软件包
