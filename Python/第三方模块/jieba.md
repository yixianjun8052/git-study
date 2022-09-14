jieba，中文分词库。

cmd:pip install jieba
半自动安装：下载 jieba-0.42.1.tar.gz，解压，cmd 定位到解压文件中 setup.py 目录，执行 python setup.py install。

精确模式：jieba.lcut(s)
全模式：jieba.lcut(s, cut_all=True)，
搜索引擎模式：jieba.lcut_for_search(s)
jieba.add_word(w)