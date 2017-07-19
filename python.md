python 全栈面试
===============

1. 装饰器
不带参数
```
import time
import random
import functools

def runtime(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        rest = func(*args, **kwargs)
        end_time = time.time()
        print(func.__name__, end_time-start_time)
        return rest

    return wrapper

@runtime
def test():
    time.sleep(random.random())


test()
```

带参数
```
import time
import random
import functools

def runtime(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            start_time = time.time()
            rest = func(*args, **kwargs)
            end_time = time.time()
            print(text, end_time-start_time)
            return rest

        return wrapper

    return decorator

@runtime('test run time: ')
def test():
    time.sleep(random.random())


test()
print(test.__name__)
```

类装饰器
```
import time
import random
import functools


class runtime(object):
    def __init__(self, text):
        self.text = text

    def __call__(self, func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            start_time = time.time()
            func(*args, **kwargs)
            end_time = time.time()
            print(self.text, end_time-start_time)

        return wrapper

@runtime('test run time: ')
def test():
    time.sleep(random.random())

test()
print(test.__name__)
```

为什么用functools.wraps
看一下test.__name__, 为保证test和原来一样
functools.partial 减少函数参数
```
def add(a, b):
    return a+b
plus3 = functools.partial(add, 3)
plus3(5)
```

2. __init__, __new__, __call__
class A(object):
    def __new__(cls, *args, **kwargs):
        print('__new__', args, kwargs)
        return object.__new__(cls, *args, **kwargs)
    def __init__(self, *args, **kwargs):
        print('__init__', args, kwargs)

A(haha=1)

3. super
在Python3.0里语法有所改变:你可以用super().__init__()替换super(ChildB, self).__init__()
2.3 之前 深度优先搜索算法
形式类C3

4. 元类
type, metaclass
type(类名, 父类的元组（针对继承的情况，可以为空），包含属性的字典（名称和值）)
元类就是用来创建类的“东西”
函数type实际上是一个元类。type就是Python在背后用来创建所有类的元类

```
# metaclass是创建类，所以必须从`type`类型派生：
class ListMetaclass(type):
    def __new__(cls, name, bases, attrs):
        attrs['add'] = lambda self, value: self.append(value)
        return type.__new__(cls, name, bases, attrs)

class MyList(list):
    __metaclass__ = ListMetaclass # 指示使用ListMetaclass来定制类

a = MyList()
a.add(1)
print(a)
```


5. 怎么引发python内存泄露
被退出才回收的对象引用
交叉引用， “引用循环”

```
class ConnectionHandler(object):
    def __init__(self, connection):
        self._conn = connection


class Connection(object):
    def __init__(self, sock, address)
        self._conn_handler = ConnectionHandler(self) # XXX
```

6. gil, jit, python并发
8. 一个简单的flask程序
7. sqlalchemy 强大的地方
automap, table inheritance, hybrid_property, 灵活
8. gunicorn, uwsgi, nginx 关系, 并发

9. select, poll, epoll
https://segmentfault.com/a/1190000003063859

10. mysql和pg不同，有点
11. 索引失效

12. 为什么选用react，react, vue, angular 比较
13. 一个简单的react, router, redux 模型
14. 一个简单的vue, vuex 模型
15. [{key: 3}, {key: 2}, {key: 1}] 排序
```
_.sortBy(arr, function(item) {
  return item.key;
});
```

16. soa，微服务，rpc
http://dockone.io/article/394

17. OAuth
http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html

18. 排序
https://zhuanlan.zhihu.com/p/25074334
http://python.jobbole.com/82270/
冒泡 O(n2) O(n2)
快排 O(nlogn) O(n2)

插入排序, 复杂度O(n2)
```
def insert_sort(lists):
    count = len(lists)
    for i in range(1, count):
        temp = lists[i]
        j = i - 1

        while j >= 0:
            if lists[j] > temp:
                lists[j+1] = lists[j]
                lists[j] = temp

            j -= 1

    return lists
```

冒泡排序，复杂度O(n2)
```
def bubble_sort(lists):
    count = len(lists)
    
    for i in range(0, count):
        for j in range(i + 1, count):
            if lists[i] > lists[j]:
                lists[i], lists[j] = lists[j], lists[i]

    return lists
```
