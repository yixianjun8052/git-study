wordcloud，词云库（重点：空格分词的长文本）。

依赖库：
numpy、wrapt、pillow、matplotlib（numpy、kiwisolver、 python-dateutil、pyparsing、cycle）

库干了四步：以空格分隔单词，统计单词出现的次数并过滤，根据统计配置字体字号，布局颜色环境尺寸。
步骤一：配置对象参数（font_path：字体。中文可用 msyh,ttc 微软雅黑。stop_words：排除的单词，可以用集合等）
w = wordcloud.WordCloud(width=400, height=200, min_font_size=4, max_font_size=20,font_step=1, font_path="msyh.ttc", max_words=200, stopwords={"Python"},background_color="white")
步骤二：加载词云文本，w.generate(text)
步骤三：输出词云文件，w.to_file(fpath)
？mask 修改词云的形状