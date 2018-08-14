##　python基础部分

* 1.	写代码实现：val = “i am a string”,实现一个方法，将字符串逆序输出。
```python
val = "I am hero"

# 第一种方法，利用列表的反向排序
def reversestr(str1):
    """
    对传进来的字符串做一个逆序输出处理
    :param str1:
    :return:
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
    :param str1:
    :return:
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
