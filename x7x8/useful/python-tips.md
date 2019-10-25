# Python常用小技巧汇总
```python
# 列表转字符串 list -> str
name_list = ['Zarten_1', 'Zarten_2', 'Zarten_3']
name_str = '&'.join(name_list) # &为列表元素之间分隔符
print(type(name_str), name_str)
# 字符串转列表 str -> list
name_str = ' id : Zarten'
name_list = name_str.split(' ') #在列表中每个元素以空格分开
print(type(name_list), name_list)
# 字符串转字典 str -> dict
name_str = "{'name_1':'Zarten_1', 'name_2' : 'Zarten_2'}"
name_dict = eval(name_str)
print(type(name_dict), name_dict)
# 字典转字符串 dict -> str
name_dict = {'name_1':'Zarten_1', 'name_2' : 'Zarten_2'}
name_str = str(name_dict)
print(type(name_str), name_str)
# 将键key转成元组
name_dict = {'name_1':'Zarten_1', 'name_2' : 'Zarten_2'}
name_tuple = tuple(name_dict) #列表-list即可
print(type(name_tuple), name_tuple)
# 将值value转成元组
name_dict = {'name_1':'Zarten_1', 'name_2' : 'Zarten_2'}
name_tuple = tuple(name_dict.values())
print(type(name_tuple), name_tuple)
# 本地时间格式化
import time
now = time.strftime('%Y-%m-%d %H:%M:%S', time.localtime())
print(now)
# 获取uuid
import uuid
uuid_str = str(uuid.uuid1()).replace('-', '')
print(uuid_str)
# 反向迭代
for i in reversed(range(1, 10)):
    print(i, end=',')
# 获取位置序号
l = ['zarten1', 'zarten2', 'zarten3']
for i in enumerate(l):
    print(i) 
# 将2个字典融合

a = {'name' : 'Zarten'}
b = {'age' : 18}
a.update(b)
print(a)
print(b)
# 首字母转为大写
a = 'my name is Zarten'
print(a.title())
# 项目中增加搜索路径
import os
import sys

curPath = os.path.abspath(os.path.dirname(__file__))
rootPath = os.path.split(curPath)[0]
sys.path.append(os.path.split(rootPath)[0])
# 获取对象的所占空间（内存空间）
import sys
names = ['zarten_1', 'zarten_2']
names_size = sys.getsizeof(names) #字节数
print('size:', names_size)
```