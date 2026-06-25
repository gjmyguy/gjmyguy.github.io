---
title: 'Python常用API'
description: '在LeetCode刷题时必会API，掌握可以大大提高刷题效率，包括字符串，列表，元组，字典等常用高频API'
pubDate: 'Jun 25 2026'
tags: ['python','基础语法']
category: 'Python'
---



### 一. 字符串（String)
##### join
签名：`<font style="color:rgb(29, 29, 31);">'sep'.join(iterable)</font>`

介绍：<font style="color:rgb(29, 29, 31);">列表转字符串。</font>

输入：字符串可迭代对象

输出：拼接后的str

示例：`'-'.join(['a','b'])` → `'a-b'`

##### <font style="color:rgb(29, 29, 31);">split</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">s.split(sep=None, maxsplit=-1)</font>`

<font style="color:rgb(29, 29, 31);">介绍：</font><font style="color:rgb(29, 29, 31);">将字符串分割成列表。</font>

<font style="color:rgb(29, 29, 31);">输入：分隔符（默认任意空白）</font>

输出：list[str]

<font style="color:rgb(29, 29, 31);">示例：</font>`'a b c'.split()` → `['a','b','c']`

##### count
签名：`<font style="color:rgb(29, 29, 31);">s.count(sub, start, end)</font>`

介绍：<font style="color:rgb(29, 29, 31);">仅统计不重叠出现次数。可以指定起止范围，左闭右开。</font>

<font style="color:rgb(29, 29, 31);">输入：子串</font>

输出：<font style="color:rgb(29, 29, 31);">出现次数 </font>`int`

<font style="color:rgb(29, 29, 31);">示例：</font>`'ababa'.count('ab')` → `2`

##### <font style="color:rgb(29, 29, 31);">find/index:</font>
签名：`<font style="color:rgb(29, 29, 31);">s.find(sub)</font>`<font style="color:rgb(29, 29, 31);"> / </font>`<font style="color:rgb(29, 29, 31);">s.index(sub)</font>`

介绍：<font style="color:rgb(29, 29, 31);">找首次出现的索引，</font>`find` 找不到返 `-1`（安全）；`index` 找不到抛 `ValueError`。

输入：子串

输出：首次出现索引 `int`

示例：`<font style="color:rgb(29, 29, 31);">'abc'.find('b')</font>`<font style="color:rgb(29, 29, 31);"> → </font>`<font style="color:rgb(29, 29, 31);">1</font>`<font style="color:rgb(29, 29, 31);">,</font>`<font style="color:rgb(29, 29, 31);">'abc'.find('d')</font>`<font style="color:rgb(29, 29, 31);"> → </font>`<font style="color:rgb(29, 29, 31);">-1</font>`

##### <font style="color:rgb(29, 29, 31);">切片</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">s[start:end:step]</font>`

<font style="color:rgb(29, 29, 31);">介绍：字符串是不可变的，切片会返回一个新的对象，越界自动截断不报错。左闭右开，步长决定方向：</font>`step > 0` 向右取，`step < 0` 向左取。

<font style="color:rgb(29, 29, 31);">输入：索引/步长</font>

<font style="color:rgb(29, 29, 31);">输出：新 </font>`<font style="color:rgb(29, 29, 31);">str</font>`

<font style="color:rgb(29, 29, 31);">示例：反转字符串</font>`<font style="color:rgb(29, 29, 31);">s[::-1]</font>`

### 二.列表/数组（List）
##### sort
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">lst.sort(key=None, reverse=False)</font>`

介绍：**<font style="color:rgb(29, 29, 31);">原地排序</font>**<font style="color:rgb(29, 29, 31);">，无返回值。稳定排序。</font>`key` 可传函数或 `lambda`

输入：<font style="color:rgb(29, 29, 31);">原地修改</font>

输出：`None`

示例：`list.sort(key=len)`

##### sorted
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">sorted(iterable, key=None, reverse=False)</font>`

介绍：<font style="color:rgb(29, 29, 31);">支持字符串/元组/字典键。</font><font style="color:rgb(29, 29, 31);">⚠️</font><font style="color:rgb(29, 29, 31);"> 返回的是列表，非原类型</font>

输入：<font style="color:rgb(29, 29, 31);">任意可迭代</font>

输出：新 `list`

示例：`sorted('bAc')` → `['A','b','c']`

##### append/extend
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">lst.append(x)</font>`<font style="color:rgb(29, 29, 31);"> / </font>`<font style="color:rgb(29, 29, 31);">lst.extend(iter)</font>`

介绍：`<font style="color:rgb(29, 29, 31);">extend</font>`<font style="color:rgb(29, 29, 31);"> 比循环 </font>`<font style="color:rgb(29, 29, 31);">append</font>`<font style="color:rgb(29, 29, 31);"> 快,</font>`<font style="color:rgb(29, 29, 31);">append</font>`<font style="color:rgb(29, 29, 31);"> 是把元素作为整体添加到末尾；</font>`<font style="color:rgb(29, 29, 31);">extend</font>`<font style="color:rgb(29, 29, 31);"> 是把可迭代对象拆开后逐个添加到末尾。  </font>

输入：<font style="color:rgb(29, 29, 31);">单元素 / 可迭代</font>

输出：`None`

示例：`list= [1, 2]`

`list.append([3, 4]) # [1, 2, [3, 4]]`

`list.extend([3, 4]) # [1, 2, 3, 4]`

##### pop
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">lst.pop([index])</font>`

介绍： 删除并返回列表中指定位置的元素；不写下标时，默认删除最后一个元素。  

输入：<font style="color:rgb(29, 29, 31);">索引（默认末尾）</font>

输出：<font style="color:rgb(29, 29, 31);">被移除元素</font>

示例：`x = lst.pop()`

##### <font style="color:rgb(29, 29, 31);">推导式</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">[expr for x in iter if cond]</font>`

介绍：即 [表达式 for 变量 in 可迭代对象 if 满足的条件]  ，可以用一行代码快速生成列表。   

输入：<font style="color:rgb(29, 29, 31);">可迭代+条件</font>

输出：新 `list`

示例：`[x for x in nums if x%2 == 0]`

### 三.字典（Dict）
##### <font style="color:rgb(29, 29, 31);">get</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">d.get(key, default=None)</font>`

介绍： 根据键获取值；如果键不存在，不会报错，返回默认值，默认是 `None`。  

输入：<font style="color:rgb(29, 29, 31);"> 键 </font>`<font style="color:rgb(29, 29, 31);">key</font>`<font style="color:rgb(29, 29, 31);">，默认值 </font>`<font style="color:rgb(29, 29, 31);">default</font>`<font style="color:rgb(29, 29, 31);">（可选）  </font>

输出： 对应的值 / 默认值  

示例：`d= {"name": "Tom", "age": 18}``d.get("name")`→`'Tom'`

##### <font style="color:rgb(29, 29, 31);">setdefault</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">d.setdefault(key, default)</font>`

介绍： 如果键存在，返回对应的值；如果键不存在，就插入这个键和默认值，并返回默认值。  

输入： 键 `key`，默认值 `default`（可选，默认是 `None`）  

输出：  键对应的值  

示例：`d= {"name": "Tom", "age": 18}`

`d.setdefault("gender", "男")`→`{'name': 'Tom', 'age': 18, 'gender': '男'}`

##### <font style="color:rgb(29, 29, 31);">keys/values/items</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">d.keys()/d.values()/d.items()</font>`

介绍： 用于获取字典中的键、值、键值对。    

输入：<font style="color:rgb(29, 29, 31);"> 无</font>

输出：<font style="color:rgb(29, 29, 31);">视图对象（列表）</font>

示例：`for k,v in d.items():`

##### <font style="color:rgb(29, 29, 31);">del/pop</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">del d[k]</font>`<font style="color:rgb(29, 29, 31);"> / </font>`<font style="color:rgb(29, 29, 31);">d.pop(k, default)</font>`

介绍：  都可以删除元素。但`del`：直接删除，不返回值  ,`pop()`：删除并返回被删的元素  

输入：<font style="color:rgb(29, 29, 31);">键</font>

输出：`<font style="color:rgb(29, 29, 31);">None</font>`<font style="color:rgb(29, 29, 31);">/被删值</font>

示例：`d.pop('x', None)`

### 四.集合（Set）
##### <font style="color:rgb(29, 29, 31);">构造</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">set(iterable)</font>`

介绍：创建一个集合。集合中的元素**无序、唯一**，重复元素会自动去重  。

输入：<font style="color:rgb(29, 29, 31);"> 可迭代对象（如列表、元组、字符串等）  </font>

输出： 集合 `set`

示例：`set('aab')` → `{'a','b'}`

##### <font style="color:rgb(29, 29, 31);">add/discard/remove</font><font style="color:rgb(29, 29, 31);"></font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">s.add(x)</font>`<font style="color:rgb(29, 29, 31);">/</font>`<font style="color:rgb(29, 29, 31);">s.discard(x)</font>`<font style="color:rgb(29, 29, 31);">/</font>`<font style="color:rgb(29, 29, 31);">s.remove(x)</font>`

介绍： 都是集合 `set` 的常用方法，用来添加或删除元素。`add(x)`：向集合中添加元素 ,`discard(x)`：删除指定元素，元素不存在也不报错 ,`remove(x)`：删除指定元素，元素不存在会报错 。

输入：<font style="color:rgb(29, 29, 31);">元素</font>

输出：`None`

示例：`s= {1, 2, 3}` `s.add(4)`→ `{1,2,3,4}`,`s.discard(2)`→ `{1,3}`

`s.remove(5)`→ `KeyError`

##### <font style="color:rgb(29, 29, 31);">集合运算</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">s1 | s2</font>`<font style="color:rgb(29, 29, 31);">/</font>`<font style="color:rgb(29, 29, 31);">s1 & s2</font>`<font style="color:rgb(29, 29, 31);">/</font>`<font style="color:rgb(29, 29, 31);">s1 - s2</font>`<font style="color:rgb(29, 29, 31);">/</font>`<font style="color:rgb(29, 29, 31);">s1 ^ s2</font>`

介绍： 包含s1,s2的元素 / 只包含 s1 和 s2 中共有的元素  / 只包含 s1 中有而 s2 中没有的元素  / 包含 s1 和 s2 中不重复的元素（不在交集中）  

输入：<font style="color:rgb(29, 29, 31);">两个集合</font>

输出：新集合

示例： `{1,2} | {2,3}` →`{1,2,3}` ，`{1,2} & {2,3}` → `{2}`，

 `{1,2} - {2,3}`→ `{1}`，`{1,2} ^ {2,3}`→ `{1,3}`

##### <font style="color:rgb(29, 29, 31);">pop</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">s.pop()</font>`

介绍： 从集合中**随机删除并返回一个元素**（因为集合无序，无法指定下标）   

输入：<font style="color:rgb(29, 29, 31);">无</font>

输出：<font style="color:rgb(29, 29, 31);">被删除的元素</font>

示例：`<font style="color:rgb(29, 29, 31);">s.pop()</font>`

### <font style="color:rgb(29, 29, 31);">五.高频内置函数</font>
##### <font style="color:rgb(29, 29, 31);">enumerate</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">enumerate(iter, start=0)</font>`

介绍：对可迭代对象进行遍历时，同时获取**索引**和**元素**。  

输入：<font style="color:rgb(29, 29, 31);">可迭代对象（如列表、字符串等），可选起始索引 </font>`<font style="color:rgb(29, 29, 31);">start</font>`<font style="color:rgb(29, 29, 31);">（默认 0）  </font>

输出：`<font style="color:rgb(29, 29, 31);">(index, value)</font>`<font style="color:rgb(29, 29, 31);"> 迭代器</font>

示例：`<font style="color:rgb(29, 29, 31);">for i,v in enumerate(lst):</font>`

##### <font style="color:rgb(29, 29, 31);">zip</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">zip(*iterables)</font>`

<font style="color:rgb(29, 29, 31);">介绍： 将多个可迭代对象</font>**按位置一一配对**<font style="color:rgb(29, 29, 31);">，生成一个元组的迭代器。</font><font style="color:rgb(29, 29, 31);">并行遍历，长度不一时</font>**<font style="color:rgb(29, 29, 31);">以最短为准</font>**<font style="color:rgb(29, 29, 31);">。</font>

<font style="color:rgb(29, 29, 31);">输入：</font><font style="color:rgb(29, 29, 31);"> 多个可迭代对象</font>

<font style="color:rgb(29, 29, 31);">输出： 迭代器 ,通过</font>`<font style="color:rgb(29, 29, 31);">list()</font>`<font style="color:rgb(29, 29, 31);">查看</font>

<font style="color:rgb(29, 29, 31);">示例：</font>`<font style="color:rgb(29, 29, 31);">list(zip([1,2],['a','b']))</font>`<font style="color:rgb(29, 29, 31);"> → </font>`<font style="color:rgb(29, 29, 31);">[(1,'a'),(2,'b')]</font>`

##### <font style="color:rgb(29, 29, 31);">map/filter</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">map(func, iter)</font>`<font style="color:rgb(29, 29, 31);"> / </font>`<font style="color:rgb(29, 29, 31);">filter(func, iter)</font>`

<font style="color:rgb(29, 29, 31);">介绍： </font>`<font style="color:rgb(29, 29, 31);">map()</font>`<font style="color:rgb(29, 29, 31);">即转换，对可迭代对象的每个元素应用一个函数，返回结果的迭代器。</font>`<font style="color:rgb(29, 29, 31);">filter()</font>`<font style="color:rgb(29, 29, 31);">即筛选，对可迭代对象的每个元素应用一个</font>**返回 True/False 的函数**<font style="color:rgb(29, 29, 31);">，保留返回 True 的元素，返回迭代器。  </font>

<font style="color:rgb(29, 29, 31);">输入：</font><font style="color:rgb(29, 29, 31);"> 函数和可迭代对象</font>

<font style="color:rgb(29, 29, 31);">输出： 迭代器，通过</font>`<font style="color:rgb(29, 29, 31);">list()</font>`<font style="color:rgb(29, 29, 31);">查看</font>

<font style="color:rgb(29, 29, 31);">示例：</font>`<font style="color:rgb(29, 29, 31);">list(map(lambda x: x*2, [1,2,3])) </font>`<font style="color:rgb(29, 29, 31);">→</font>`<font style="color:rgb(29, 29, 31);"> [2, 4, 6]</font>`<font style="color:rgb(29, 29, 31);">，</font>

`<font style="color:rgb(29, 29, 31);">list(filter(lambda x: x%2==0, [1,2,3])) </font>`<font style="color:rgb(29, 29, 31);">→ </font>`<font style="color:rgb(29, 29, 31);">[2]</font>`

##### <font style="color:rgb(29, 29, 31);">sum/min/max</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">sum(iter)</font>`<font style="color:rgb(29, 29, 31);"> /</font><font style="color:rgb(29, 29, 31);"> </font>`<font style="color:rgb(29, 29, 31);">min(iter, key=func)</font>`<font style="color:rgb(29, 29, 31);">/</font>`<font style="color:rgb(29, 29, 31);">max(iter, key=func)</font>`

<font style="color:rgb(29, 29, 31);">介绍：求和，求最小值，求最大值</font>

<font style="color:rgb(29, 29, 31);">输入：</font><font style="color:rgb(29, 29, 31);"> 可迭代对象和可选的key（函数）</font>

<font style="color:rgb(29, 29, 31);">输出：</font><font style="color:rgb(29, 29, 31);">数值/元素</font>

<font style="color:rgb(29, 29, 31);">示例：</font>`<font style="color:rgb(29, 29, 31);">lst= [("a", 3), ("b", 5), ("c", 2)]</font>`<font style="color:rgb(29, 29, 31);"></font>

`<font style="color:rgb(29, 29, 31);">max(lst, key=lambda x: x[1]) #按每个元组的第二个元素比较</font>`<font style="color:rgb(29, 29, 31);">→ </font>`<font style="color:rgb(29, 29, 31);">('b', 5)</font>`

##### <font style="color:rgb(29, 29, 31);">ord/chr</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">ord(char)</font>`<font style="color:rgb(29, 29, 31);"> / </font>`<font style="color:rgb(29, 29, 31);">chr(int)</font>`

<font style="color:rgb(29, 29, 31);">介绍：</font>`<font style="color:rgb(29, 29, 31);">ord()</font>`<font style="color:rgb(29, 29, 31);">返回单个字符对应的 Unicode / ASCII 编码。</font>`<font style="color:rgb(29, 29, 31);">chr(int)</font>`<font style="color:rgb(29, 29, 31);"> 返回整数对应的 Unicode / ASCII 字符。  </font>

<font style="color:rgb(29, 29, 31);">输入：</font><font style="color:rgb(29, 29, 31);">单个字符 /整数</font>

<font style="color:rgb(29, 29, 31);">输出：</font><font style="color:rgb(29, 29, 31);">数值/元素</font>

<font style="color:rgb(29, 29, 31);">示例：</font>`<font style="color:rgb(29, 29, 31);">ord('A') </font>`<font style="color:rgb(29, 29, 31);">→</font>`<font style="color:rgb(29, 29, 31);">65</font>`<font style="color:rgb(29, 29, 31);">，</font>`<font style="color:rgb(29, 29, 31);">chr(97) </font>`<font style="color:rgb(29, 29, 31);">→</font>`<font style="color:rgb(29, 29, 31);">'a'</font>`

### <font style="color:rgb(29, 29, 31);">六.collections模块</font>
##### <font style="color:rgb(29, 29, 31);">Counter</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">Counter(iterable)</font>`

<font style="color:rgb(29, 29, 31);">介绍： 用于统计可迭代对象中每个元素出现的次数，返回一个字典子类，键是元素，值是次数。</font>

<font style="color:rgb(29, 29, 31);">输入：</font><font style="color:rgb(29, 29, 31);"> 可迭代对象（列表、字符串、元组等）  </font>

<font style="color:rgb(29, 29, 31);">输出：</font>`<font style="color:rgb(29, 29, 31);">Counter</font>`<font style="color:rgb(29, 29, 31);"> 对象（类似字典）  </font>

<font style="color:rgb(29, 29, 31);">示例：</font>`<font style="color:rgb(29, 29, 31);">Counter('aab')</font>`<font style="color:rgb(29, 29, 31);"> → </font>`<font style="color:rgb(29, 29, 31);">{'a':2,'b':1}</font>`

##### <font style="color:rgb(29, 29, 31);">most_common</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">c.most_common(n=None)</font>`

<font style="color:rgb(29, 29, 31);">介绍：返回出现次数前 n 多的元素及次数 , 按次数从高到低排序。如果 </font>`<font style="color:rgb(29, 29, 31);">n</font>`<font style="color:rgb(29, 29, 31);"> 不填，返回所有元素。  </font>

<font style="color:rgb(29, 29, 31);">输入：</font><font style="color:rgb(29, 29, 31);">整数 </font>`<font style="color:rgb(29, 29, 31);">n</font>`<font style="color:rgb(29, 29, 31);">（可选，表示前 n 个元素）  </font>

<font style="color:rgb(29, 29, 31);">输出： 列表，元素是 </font>`<font style="color:rgb(29, 29, 31);">(元素, 次数)</font>`<font style="color:rgb(29, 29, 31);"> 的元组  </font>

<font style="color:rgb(29, 29, 31);">示例：</font>`<font style="color:rgb(29, 29, 31);">Counter([1,2,2,3,3,3]).most_common(2) </font>`<font style="color:rgb(29, 29, 31);">→ </font>`<font style="color:rgb(29, 29, 31);">[(3,3), (2,2)]</font>`

<font style="color:rgb(29, 29, 31);">示例：</font>`<font style="color:rgb(29, 29, 31);">Counter('aab')</font>`<font style="color:rgb(29, 29, 31);"> → </font>`<font style="color:rgb(29, 29, 31);">{'a':2,'b':1}</font>`

##### <font style="color:rgb(29, 29, 31);">defaultdict</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">defaultdict(factory)</font>`

<font style="color:rgb(29, 29, 31);">介绍：字典的子类，访问不存在的键时，会自动生成默认值，而不是报 </font>`<font style="color:rgb(29, 29, 31);">KeyError</font>`<font style="color:rgb(29, 29, 31);"> </font>

<font style="color:rgb(29, 29, 31);">输入：</font>`<font style="color:rgb(29, 29, 31);">default_factory</font>`<font style="color:rgb(29, 29, 31);">：一个函数，用来生成默认值（可选，默认 </font>`<font style="color:rgb(29, 29, 31);">None</font>`<font style="color:rgb(29, 29, 31);">）  </font>

<font style="color:rgb(29, 29, 31);">输出： </font>`<font style="color:rgb(29, 29, 31);">defaultdict</font>`<font style="color:rgb(29, 29, 31);"> 对象（类似字典）</font>

<font style="color:rgb(29, 29, 31);">示例：</font>`<font style="color:rgb(29, 29, 31);">d=defaultdict(int)</font>`→`不存在键默认0`,

`d=defaultdict(list) `→`不存在键默认 []`

##### <font style="color:rgb(29, 29, 31);">deque</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">deque(iterable, maxlen=None)</font>`

<font style="color:rgb(29, 29, 31);">介绍： 双端队列（double-ended queue），支持</font>**两端高效添加或删除**<font style="color:rgb(29, 29, 31);">元素。比列表在头部操作更高效。  </font>

<font style="color:rgb(29, 29, 31);">输入： 可迭代对象（可选，用于初始化队列）  </font>

<font style="color:rgb(29, 29, 31);">输出：</font>`<font style="color:rgb(29, 29, 31);">deque</font>`<font style="color:rgb(29, 29, 31);"> 对象  </font>

<font style="color:rgb(29, 29, 31);">示例：</font>`<font style="color:rgb(29, 29, 31);">d = deque([1,2,3])</font>`

`<font style="color:rgb(29, 29, 31);">d.append(4) # 右侧添加</font>`<font style="color:rgb(29, 29, 31);"> →</font>`<font style="color:rgb(29, 29, 31);">deque([1,2,3,4])</font>`

`<font style="color:rgb(29, 29, 31);">d.appendleft(0) # 左侧添加</font>`<font style="color:rgb(29, 29, 31);">→</font>`<font style="color:rgb(29, 29, 31);">deque([0,1,2,3,4])</font>`

`<font style="color:rgb(29, 29, 31);">d.pop() # 右侧删除</font>`<font style="color:rgb(29, 29, 31);">→</font>`<font style="color:rgb(29, 29, 31);">4</font>`

`<font style="color:rgb(29, 29, 31);">d.popleft() # 左侧删除</font>`<font style="color:rgb(29, 29, 31);">→</font>`<font style="color:rgb(29, 29, 31);">0</font>`

### <font style="color:rgb(29, 29, 31);">七.bisect模块</font>
##### <font style="color:rgb(29, 29, 31);">bisect_left</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">bisect.bisect_left(a, x, lo=0, hi=len(a))</font>`

<font style="color:rgb(29, 29, 31);">介绍：</font><font style="color:rgb(29, 29, 31);">在有序列表</font><font style="color:rgb(29, 29, 31);"> </font>`<font style="color:rgb(29, 29, 31);">a</font>`<font style="color:rgb(29, 29, 31);"> 中查找 </font>`<font style="color:rgb(29, 29, 31);">x</font>`<font style="color:rgb(29, 29, 31);"> 的插入位置。如果 </font>`<font style="color:rgb(29, 29, 31);">x</font>`<font style="color:rgb(29, 29, 31);"> 已经存在于列表中，返回</font>**<font style="color:rgb(29, 29, 31);">最左侧（即第一个）</font>**<font style="color:rgb(29, 29, 31);">出现的位置索引。</font>

<font style="color:rgb(29, 29, 31);">输入：</font>`<font style="color:rgb(29, 29, 31);">a</font>`<font style="color:rgb(29, 29, 31);">：有序列表（升序）,</font>`<font style="color:rgb(29, 29, 31);">x</font>`<font style="color:rgb(29, 29, 31);">：要查找或插入的目标值,</font>`<font style="color:rgb(29, 29, 31);">lo</font>`<font style="color:rgb(29, 29, 31);">：搜索起始索引（可选，默认 0）,</font>`<font style="color:rgb(29, 29, 31);">hi</font>`<font style="color:rgb(29, 29, 31);">：搜索结束索引（可选，默认列表长度）</font>

<font style="color:rgb(29, 29, 31);">输出： </font>`int`（插入位置的索引）

<font style="color:rgb(29, 29, 31);">示例：</font>

```python
bisect.bisect_left([1, 2, 2, 3], 2)  # → 1 (第一个2的位置)
bisect.bisect_left([1, 3, 5], 4)     # → 2 (4应该插在3和5之间，索引为2)
```

##### <font style="color:rgb(29, 29, 31);">bisect_right</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">bisect.bisect_right(a, x, lo=0, hi=len(a))</font>`

<font style="color:rgb(29, 29, 31);">介绍：</font><font style="color:rgb(29, 29, 31);">在有序列表</font><font style="color:rgb(29, 29, 31);"> </font>`<font style="color:rgb(29, 29, 31);">a</font>`<font style="color:rgb(29, 29, 31);"> 中查找 </font>`<font style="color:rgb(29, 29, 31);">x</font>`<font style="color:rgb(29, 29, 31);"> 的插入位置。如果 </font>`<font style="color:rgb(29, 29, 31);">x</font>`<font style="color:rgb(29, 29, 31);"> 已经存在于列表中，返回</font>**<font style="color:rgb(29, 29, 31);">最右侧（即最后一个）</font>**<font style="color:rgb(29, 29, 31);">出现的位置索引。</font>

<font style="color:rgb(29, 29, 31);">输入：</font>`<font style="color:rgb(29, 29, 31);">a</font>`<font style="color:rgb(29, 29, 31);">：有序列表（升序）,</font>`<font style="color:rgb(29, 29, 31);">x</font>`<font style="color:rgb(29, 29, 31);">：要查找或插入的目标值,</font>`<font style="color:rgb(29, 29, 31);">lo</font>`<font style="color:rgb(29, 29, 31);">：搜索起始索引（可选，默认 0）,</font>`<font style="color:rgb(29, 29, 31);">hi</font>`<font style="color:rgb(29, 29, 31);">：搜索结束索引（可选，默认列表长度）</font>

<font style="color:rgb(29, 29, 31);">输出： </font>`int`（插入位置的索引）

<font style="color:rgb(29, 29, 31);">示例：</font>

```python
bisect.bisect_right([1, 2, 2, 3], 2) # → 3 (最后一个2的后面，索引为3)
bisect.bisect([1, 2, 2, 3], 2)       # → 3 (bisect 是 bisect_right 的别名)
```

##### <font style="color:rgb(29, 29, 31);">insort_left</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">bisect.insort_left(a, x, lo=0, hi=len(a))</font>`

<font style="color:rgb(29, 29, 31);">介绍：</font><font style="color:rgb(29, 29, 31);">在保持列表有序的前提下插入元</font><font style="color:rgb(29, 29, 31);">素 </font>`<font style="color:rgb(29, 29, 31);">x</font>`<font style="color:rgb(29, 29, 31);">，</font><font style="color:rgb(29, 29, 31);">如果有重复元素，插在</font>**<font style="color:rgb(29, 29, 31);">左边</font>**<font style="color:rgb(29, 29, 31);">。</font>

<font style="color:rgb(29, 29, 31);">输入：</font>`<font style="color:rgb(29, 29, 31);">a</font>`<font style="color:rgb(29, 29, 31);">：有序列表（升序）,</font>`<font style="color:rgb(29, 29, 31);">x</font>`<font style="color:rgb(29, 29, 31);">：要插入的目标值,</font>`<font style="color:rgb(29, 29, 31);">lo</font>`<font style="color:rgb(29, 29, 31);">：搜索起始索引（可选，默认 0）,</font>`<font style="color:rgb(29, 29, 31);">hi</font>`<font style="color:rgb(29, 29, 31);">：搜索结束索引（可选，默认列表长度）</font>

<font style="color:rgb(29, 29, 31);">输出： None</font>

<font style="color:rgb(29, 29, 31);">示例：</font>

```python
a = [1, 3, 5]
bisect.insort_left(a, 3)
# a 变为 [1, 3, 3, 5] (新的3插在旧的3前面)
```

##### <font style="color:rgb(29, 29, 31);">insort_right</font>
<font style="color:rgb(29, 29, 31);">签名：</font>`<font style="color:rgb(29, 29, 31);">bisect.insort_right(a, x, lo=0, hi=len(a))</font>`

<font style="color:rgb(29, 29, 31);">介绍：</font><font style="color:rgb(29, 29, 31);">在保持列表有序的前提下插入元</font><font style="color:rgb(29, 29, 31);">素 </font>`<font style="color:rgb(29, 29, 31);">x</font>`<font style="color:rgb(29, 29, 31);">，</font><font style="color:rgb(29, 29, 31);">如果有重复元素，插在</font>**<font style="color:rgb(29, 29, 31);">右边</font>**<font style="color:rgb(29, 29, 31);">。</font>

<font style="color:rgb(29, 29, 31);">输入：</font>`<font style="color:rgb(29, 29, 31);">a</font>`<font style="color:rgb(29, 29, 31);">：有序列表（升序）,</font>`<font style="color:rgb(29, 29, 31);">x</font>`<font style="color:rgb(29, 29, 31);">：要插入的目标值,</font>`<font style="color:rgb(29, 29, 31);">lo</font>`<font style="color:rgb(29, 29, 31);">：搜索起始索引（可选，默认 0）,</font>`<font style="color:rgb(29, 29, 31);">hi</font>`<font style="color:rgb(29, 29, 31);">：搜索结束索引（可选，默认列表长度）</font>

<font style="color:rgb(29, 29, 31);">输出： None</font>

<font style="color:rgb(29, 29, 31);">示例：</font>

```python
a = [1, 3, 5]
bisect.insort_right(a, 3)
# a 变为 [1, 3, 3, 5] (新的3插在旧的3后面)
# 注意：虽然列表看起来一样，但插入的索引不同（一个是1，一个是2）
```
