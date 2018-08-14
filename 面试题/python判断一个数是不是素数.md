# python判断一个数是不是素数
@toc

* 素数的定义：
  * 质数（prime number）又称素数。
  * 一个大于1的自然数，除了1和它本身外，不能被其他自然数整除，换句话说就是该数除了1和它本身以外不再有其他的因数；否则称为合数。
  * 最小的素数是2，最小的合数是4
  * 1曾经被当成过素数，后来人们发觉1做为素数的话，素因数分解就不唯一（因为1做为因子可以无限乘1）。因此之后1就不被当做素数了。


## 方法一：普通青年的平方根范围筛选法

* 根据素数的定义，判断数n是不是素数，只需要从i=2开始，判断n能不能被n整除，一直到n-1，如果可以则说明不是素数。

* 另一方面，一个数若是合数，则一定能写成两个因数相乘的形式，并且两个因数中较小的那个一定小于等于n的平方根`sqrt(n)`，否则两个因数的乘积大于n。
  * 注，python中导入math模块来计算平方根
*  因此i的终止条件可以设为sqrt(n)，这种方法的时间复杂度为O（n的1.5次方）。空间复杂度为O(1)，实现如下：

```python
def prime(n):
    """
    将输入的正整数过滤出素数
    :param num: 正整数
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
```

## 方法二：2B青年的埃拉托斯特尼筛法

*  埃拉托斯特尼筛法（Sieve of Eratosthenes），详细介绍看维基百科[Sieve of Eratosthenes](http://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)
  * 埃拉托斯特尼筛法:   给出要筛数值的范围n，找出![\sqrt{n}](http://upload.wikimedia.org/wikipedia/zh/math/5/0/a/50a52d4f0509159a4fcb286c896d8b64.png)以内的素数![p_{1},p_{2},\dots,p_{k}](http://upload.wikimedia.org/wikipedia/zh/math/3/b/2/3b2bf95de725fc71fb65b4c8d3446dad.png)。先用2去筛，即把2留下，把2的倍数剔除掉；再用下一个质数，也就是3筛，把3留下，把3的倍数剔除掉；接下去用下一个质数5筛，把5留下，把5的倍数剔除掉；不断重复下去.....
 
* 这种方法的思想是设置一个标志数组`isPrimes[n]`，标志数组标示相应的每一位数是不是素数
  * 标志数组初始化全为true，之后再将不是素数的标记为false来筛选。

* 算法从i=2开始，依次将质数的倍数标记为非素数，即将标记数组的相应位改为false。
* 标记质数的倍数的时候从`i*i`也就是i的平方开始倒过来标记。
  * 不需要从`i*j`（且j<i）开始，因为`i*j`，在遇到j时已经被标记了，因为j比i小，所以遇到j比遇到i要早。
* 建议从`i*i`开始标记，这样i终止条件也为`sqrt(n)`，否则`i*i`将大于n。
* 下面一个图形象说明了这个过程，此方法时间复制度为O(n*lglgn)，空间复杂度为O(n)，见维基百科。

![PrimeNumbers]($resource/PrimeNumbers.gif)

### 优化
* 使用python的生成器方法生成一个超大的字典，这样做的好处减少了内存的消耗。
* 循环只需到n的平方根square_root就行了，然后测试一下，最大的8位数基本40s能求出所有质因子。
用到一些小优化，在python 2 中`while 1：`要比`while True：`要快，`if value：`要比`if value == True：`，不相信的话可以测试一下，然后看一下它们生成的操作码，另外大数计算中最好不用`for`循环，而用`while`循环。
代码实现如下：

```python

```