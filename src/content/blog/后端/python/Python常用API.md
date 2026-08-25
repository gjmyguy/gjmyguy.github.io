---
title: 'Python常用API'
description: '在LeetCode刷题时必会API，掌握可以大大提高刷题效率，包括字符串，列表，元组，字典等常用高频API'
pubDate: 'Jun 25 2026'
tags: ['python','基础语法']
---



### 一. 字符串（String)
##### join
签名：`'sep'.join(iterable)`

介绍：列表转字符串。

输入：字符串可迭代对象

输出：拼接后的str

示例：`'-'.join(['a','b'])` → `'a-b'`

##### split
签名：`s.split(sep=None, maxsplit=-1)`

介绍：将字符串分割成列表。

输入：分隔符（默认任意空白）

输出：list[str]

示例：`'a b c'.split()` → `['a','b','c']`

##### count
签名：`s.count(sub, start, end)`

介绍：仅统计不重叠出现次数。可以指定起止范围，左闭右开。

输入：子串

输出：出现次数 `int`

示例：`'ababa'.count('ab')` → `2`

##### find/index:
签名：`s.find(sub)` / `s.index(sub)`

介绍：找首次出现的索引，`find` 找不到返 `-1`（安全）；`index` 找不到抛 `ValueError`。

输入：子串

输出：首次出现索引 `int`

示例：`'abc'.find('b')` → `1`,`'abc'.find('d')` → `-1`

##### 切片
签名：`s[start:end:step]`

介绍：字符串是不可变的，切片会返回一个新的对象，越界自动截断不报错。左闭右开，步长决定方向：`step > 0` 向右取，`step < 0` 向左取。

输入：索引/步长

输出：新 `str`

示例：反转字符串`s[::-1]`

### 二.列表/数组（List）
##### sort
签名：`lst.sort(key=None, reverse=False)`

介绍：**原地排序**，无返回值。稳定排序。`key` 可传函数或 `lambda`

输入：原地修改

输出：`None`

示例：`list.sort(key=len)`

##### sorted
签名：`sorted(iterable, key=None, reverse=False)`

介绍：支持字符串/元组/字典键。⚠️ 返回的是列表，非原类型

输入：任意可迭代

输出：新 `list`

示例：`sorted('bAc')` → `['A','b','c']`

##### append/extend
签名：`lst.append(x)` / `lst.extend(iter)`

介绍：`extend` 比循环 `append` 快,`append` 是把元素作为整体添加到末尾；`extend` 是把可迭代对象拆开后逐个添加到末尾。

输入：单元素 / 可迭代

输出：`None`

示例：`list= [1, 2]`

`list.append([3, 4]) # [1, 2, [3, 4]]`

`list.extend([3, 4]) # [1, 2, 3, 4]`

##### pop
签名：`lst.pop([index])`

介绍： 删除并返回列表中指定位置的元素；不写下标时，默认删除最后一个元素。

输入：索引（默认末尾）

输出：被移除元素

示例：`x = lst.pop()`

##### 推导式
签名：`[expr for x in iter if cond]`

介绍：即 [表达式 for 变量 in 可迭代对象 if 满足的条件]  ，可以用一行代码快速生成列表。

输入：可迭代+条件

输出：新 `list`

示例：`[x for x in nums if x%2 == 0]`

### 三.字典（Dict）
##### get
签名：`d.get(key, default=None)`

介绍： 根据键获取值；如果键不存在，不会报错，返回默认值，默认是 `None`。

输入： 键 `key`，默认值 `default`（可选）

输出： 对应的值 / 默认值

示例：`d= {"name": "Tom", "age": 18}``d.get("name")`→`'Tom'`

##### setdefault
签名：`d.setdefault(key, default)`

介绍： 如果键存在，返回对应的值；如果键不存在，就插入这个键和默认值，并返回默认值。

输入： 键 `key`，默认值 `default`（可选，默认是 `None`）

输出：  键对应的值

示例：`d= {"name": "Tom", "age": 18}`

`d.setdefault("gender", "男")`→`{'name': 'Tom', 'age': 18, 'gender': '男'}`

##### keys/values/items
签名：`d.keys()/d.values()/d.items()`

介绍： 用于获取字典中的键、值、键值对。

输入： 无

输出：视图对象（列表）

示例：`for k,v in d.items():`

##### del/pop
签名：`del d[k]` / `d.pop(k, default)`

介绍：  都可以删除元素。但`del`：直接删除，不返回值  ,`pop()`：删除并返回被删的元素

输入：键

输出：`None`/被删值

示例：`d.pop('x', None)`

### 四.集合（Set）
##### 构造
签名：`set(iterable)`

介绍：创建一个集合。集合中的元素**无序、唯一**，重复元素会自动去重  。

输入： 可迭代对象（如列表、元组、字符串等）

输出： 集合 `set`

示例：`set('aab')` → `{'a','b'}`

##### add/discard/remove
签名：`s.add(x)`/`s.discard(x)`/`s.remove(x)`

介绍： 都是集合 `set` 的常用方法，用来添加或删除元素。`add(x)`：向集合中添加元素 ,`discard(x)`：删除指定元素，元素不存在也不报错 ,`remove(x)`：删除指定元素，元素不存在会报错 。

输入：元素

输出：`None`

示例：`s= {1, 2, 3}` `s.add(4)`→ `{1,2,3,4}`,`s.discard(2)`→ `{1,3}`

`s.remove(5)`→ `KeyError`

##### 集合运算
签名：`s1 | s2`/`s1 & s2`/`s1 - s2`/`s1 ^ s2`

介绍： 包含s1,s2的元素 / 只包含 s1 和 s2 中共有的元素  / 只包含 s1 中有而 s2 中没有的元素  / 包含 s1 和 s2 中不重复的元素（不在交集中）

输入：两个集合

输出：新集合

示例： `{1,2} | {2,3}` →`{1,2,3}` ，`{1,2} & {2,3}` → `{2}`，

 `{1,2} - {2,3}`→ `{1}`，`{1,2} ^ {2,3}`→ `{1,3}`

##### pop
签名：`s.pop()`

介绍： 从集合中**随机删除并返回一个元素**（因为集合无序，无法指定下标）

输入：无

输出：被删除的元素

示例：`s.pop()`

### 五.高频内置函数
##### enumerate
签名：`enumerate(iter, start=0)`

介绍：对可迭代对象进行遍历时，同时获取**索引**和**元素**。

输入：可迭代对象（如列表、字符串等），可选起始索引 `start`（默认 0）

输出：`(index, value)` 迭代器

示例：`for i,v in enumerate(lst):`

##### zip
签名：`zip(*iterables)`

介绍： 将多个可迭代对象**按位置一一配对**，生成一个元组的迭代器。并行遍历，长度不一时**以最短为准**。

输入： 多个可迭代对象

输出： 迭代器 ,通过`list()`查看

示例：`list(zip([1,2],['a','b']))` → `[(1,'a'),(2,'b')]`

##### map/filter
签名：`map(func, iter)` / `filter(func, iter)`

介绍： `map()`即转换，对可迭代对象的每个元素应用一个函数，返回结果的迭代器。`filter()`即筛选，对可迭代对象的每个元素应用一个**返回 True/False 的函数**，保留返回 True 的元素，返回迭代器。

输入： 函数和可迭代对象

输出： 迭代器，通过`list()`查看

示例：`list(map(lambda x: x*2, [1,2,3])) `→` [2, 4, 6]`，

`list(filter(lambda x: x%2==0, [1,2,3])) `→ `[2]`

##### sum/min/max
签名：`sum(iter)` / `min(iter, key=func)`/`max(iter, key=func)`

介绍：求和，求最小值，求最大值

输入： 可迭代对象和可选的key（函数）

输出：数值/元素

示例：`lst= [("a", 3), ("b", 5), ("c", 2)]`

`max(lst, key=lambda x: x[1]) #按每个元组的第二个元素比较`→ `('b', 5)`

##### ord/chr
签名：`ord(char)` / `chr(int)`

介绍：`ord()`返回单个字符对应的 Unicode / ASCII 编码。`chr(int)` 返回整数对应的 Unicode / ASCII 字符。

输入：单个字符 /整数

输出：数值/元素

示例：`ord('A') `→`65`，`chr(97) `→`'a'`

### 六.collections模块
##### Counter
签名：`Counter(iterable)`

介绍： 用于统计可迭代对象中每个元素出现的次数，返回一个字典子类，键是元素，值是次数。

输入： 可迭代对象（列表、字符串、元组等）

输出：`Counter` 对象（类似字典）

示例：`Counter('aab')` → `{'a':2,'b':1}`

##### most_common
签名：`c.most_common(n=None)`

介绍：返回出现次数前 n 多的元素及次数 , 按次数从高到低排序。如果 `n` 不填，返回所有元素。

输入：整数 `n`（可选，表示前 n 个元素）

输出： 列表，元素是 `(元素, 次数)` 的元组

示例：`Counter([1,2,2,3,3,3]).most_common(2) `→ `[(3,3), (2,2)]`

示例：`Counter('aab')` → `{'a':2,'b':1}`

##### defaultdict
签名：`defaultdict(factory)`

介绍：字典的子类，访问不存在的键时，会自动生成默认值，而不是报 `KeyError`

输入：`default_factory`：一个函数，用来生成默认值（可选，默认 `None`）

输出： `defaultdict` 对象（类似字典）

示例：`d=defaultdict(int)`→`不存在键默认0`,

`d=defaultdict(list) `→`不存在键默认 []`

##### deque
签名：`deque(iterable, maxlen=None)`

介绍： 双端队列（double-ended queue），支持**两端高效添加或删除**元素。比列表在头部操作更高效。

输入： 可迭代对象（可选，用于初始化队列）

输出：`deque` 对象

示例：`d = deque([1,2,3])`

`d.append(4) # 右侧添加` →`deque([1,2,3,4])`

`d.appendleft(0) # 左侧添加`→`deque([0,1,2,3,4])`

`d.pop() # 右侧删除`→`4`

`d.popleft() # 左侧删除`→`0`

### 七.bisect模块
##### bisect_left
签名：`bisect.bisect_left(a, x, lo=0, hi=len(a))`

介绍：在有序列表 `a` 中查找 `x` 的插入位置。如果 `x` 已经存在于列表中，返回**最左侧（即第一个）**出现的位置索引。

输入：`a`：有序列表（升序）,`x`：要查找或插入的目标值,`lo`：搜索起始索引（可选，默认 0）,`hi`：搜索结束索引（可选，默认列表长度）

输出： `int`（插入位置的索引）

示例：

```python
bisect.bisect_left([1, 2, 2, 3], 2)  # → 1 (第一个2的位置)
bisect.bisect_left([1, 3, 5], 4)     # → 2 (4应该插在3和5之间，索引为2)
```

##### bisect_right
签名：`bisect.bisect_right(a, x, lo=0, hi=len(a))`

介绍：在有序列表 `a` 中查找 `x` 的插入位置。如果 `x` 已经存在于列表中，返回**最右侧（即最后一个）**出现的位置索引。

输入：`a`：有序列表（升序）,`x`：要查找或插入的目标值,`lo`：搜索起始索引（可选，默认 0）,`hi`：搜索结束索引（可选，默认列表长度）

输出： `int`（插入位置的索引）

示例：

```python
bisect.bisect_right([1, 2, 2, 3], 2) # → 3 (最后一个2的后面，索引为3)
bisect.bisect([1, 2, 2, 3], 2)       # → 3 (bisect 是 bisect_right 的别名)
```

##### insort_left
签名：`bisect.insort_left(a, x, lo=0, hi=len(a))`

介绍：在保持列表有序的前提下插入元素 `x`，如果有重复元素，插在**左边**。

输入：`a`：有序列表（升序）,`x`：要插入的目标值,`lo`：搜索起始索引（可选，默认 0）,`hi`：搜索结束索引（可选，默认列表长度）

输出： None

示例：

```python
a = [1, 3, 5]
bisect.insort_left(a, 3)
# a 变为 [1, 3, 3, 5] (新的3插在旧的3前面)
```

##### insort_right
签名：`bisect.insort_right(a, x, lo=0, hi=len(a))`

介绍：在保持列表有序的前提下插入元素 `x`，如果有重复元素，插在**右边**。

输入：`a`：有序列表（升序）,`x`：要插入的目标值,`lo`：搜索起始索引（可选，默认 0）,`hi`：搜索结束索引（可选，默认列表长度）

输出： None

示例：

```python
a = [1, 3, 5]
bisect.insort_right(a, 3)
# a 变为 [1, 3, 3, 5] (新的3插在旧的3后面)
# 注意：虽然列表看起来一样，但插入的索引不同（一个是1，一个是2）
```
