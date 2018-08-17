# python基础

1. 写代码实现：val = “i am a string”,实现一个方法，将字符串逆序输出。
```python
val = "I am hero"

# 第一种方法，利用列表的反向排序
def reversestr(str1):
    """
    对传进来的字符串做一个逆序输出处理
    """
    list1 = str1.split()
    # 列表reverse之后就己经是逆序了
    list1.reverse()
    # 再用空格做join转成字符串
    val2 = " ".join(list1)
    return val2

# 第二种方法，利用反向切片
def reversestr2(str1):
    """
    对传进来的字符串做一个逆序输出处理
    """
    list1 = str1.split()
    # 使用反向切片进行倒序
    list1 = list1[::-1]
    # 再用空格做join转成字符串
    val2 = " ".join(list1)
    return val2

print(reversestr(val))
print(reversestr2(val))
```


2. 判断101-200之间有多少个质数？
提示：一个大于1的自然数，除了1和它自身外，不能被其他自然数整除的书叫质数。

* 第一种方法，用平方根的范围去做普通的筛选
```python
import math

# 第一种方法，用平方根的范围去做筛选 
def prime(n):
    """
    将输入的正整数过滤出素数
    :param n: 正整数
    :return: true为素数，None不为素数  
    """  
    # 求出平方根并取整数部分
    sqrtn = int(math.sqrt(n))
    # 1不属于素数，先把它直接排除
    if n <= 1:return None
    # 从2到平方根的范围去被整除
    for i in range(2, sqrtn + 1):
        if n % i == 0:
        # 如果能被范围内的数整除，则不为素数，返回None
            return None
    # 反之则为素数
    return True   

# 求出101到200之间有多少个质数 
list1 = [i for i in range(101, 201)]
list2 = [ ]
for i in list1:
    if prime(i):
        list2.append(i)
num = str(len(list2))
print("101到200之间有%s个质数" % num)
print(list2)
```


* 第二种方法，使用`埃拉托斯特尼筛法`

```python
# 埃拉托斯特尼筛法

def gen_super_dict(n):
    """
    使用python生成器方法生成一个超大字典，用于减少内存消耗
    :param n:接收一个参数n，只要不小于i则返回i
    :return:i
    """
    # 因为0和1都不是素数，所以从2取起
    i = 2
    while 1:
        if i > n:break
        yield i
        i += 1

def gen_prime_list(n):
    """
    传入一个整数，实际上取的是从2开始到此整数范围的素数列表出来
    n为素数的取值范围
    :param n: 传入一个整数
    :return:返回取值范围内的所有素数
    """
    super_dict = {}
    primes_list = []

    # 将值传入生成大字典的函数中
    for x in gen_super_dict(n):
        super_dict[x] = True

    i = 2
    while 1:
        if i > n:
            break
        j = i * i
        while 1:
            if j > n:
                break
            if super_dict[i]:
                super_dict[j] = False
            j += i
        i += 1

    for key,value in super_dict.items():
        if value:
            primes_list.append(key)
    return primes_list

# 求出101到200之间有多少个质数
# 直接先用gen_prime_list方法取出100以内的质数
set1 = set(gen_prime_list(100))
# 接着取201以内的质数
set2 = set(gen_prime_list(201))
# 取两者之间的差集，就得得到100到201之间的质数了
set3 = set2 - set1
num2 = str(len(set3))
print("101到200之间有%s个质数" % num2)
```

3. 简述正则表达式中的贪婪匹配并举例说明。
贪婪模式:尽可能多的匹配所搜索的字符串。
例如，对于字符串 "wwz"，'w+?' 将匹配单个 "w"，而 'w+' 将匹配所有 'w'。

4. 请编写一个函数实现将IP地址转换成一个整数。
         如 10.3.9.12 转换规则为：
                  10            00001010
                    3            00000011
                  9            00001001
                  12            00001100
       再将以上二进制拼接起来计算十进制结果，即：
              00001010 00000011 00001001 00001100 = ？
              
