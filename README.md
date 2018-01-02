# Json-Parse
记录一下抓取网页用python解析json格式的问题

今天解析json文本报以下错误：

ValueError: Expecting property name: line 1 column 2 (char 1)


解析内容如下：

{'item_price': '{a=100;b=100}', 'ip': '39.182.65.246', 'class_id': '588111', 'class_urs': 'm15000000@163.com', 'class_name': '\xe6\x9c\x88\xe5\xba\xa6\xe7\xa5\xa8'}

解析代码：

import json

path = u'F:/project/python/test.txt'
records = [json.loads(line) for line in open(path)]

----------------------------------------------------------------------------------

经查询得知，由于JSON中，标准语法中，不支持单引号，属性或者属性值，都必须是双引号括起来

修改后如下：

records = [json.loads(line.replace("'", '"')) for line in open(path)]



再次运行后，还是有报错：

Invalid \escape: line 1 column 5863 (char 5863)

主要因unicode编码所致，用正则修改后就可以了：

regex = re.compile(r'\\(?![/u"])')
records = [json.loads(regex.sub(r"\\\\", line).replace("'", '"')) for line in open(path)]
