---
title: Bitmap 算法解释与应用
tags:
  - 算法
id: 2514
categories:
  - Algorithm
date: 2016-04-04 17:38:23
---

有这样一个问题：给40亿个不重复的unsigned int的整数，没排序。再给一个数，快速判断这个数是否存在40亿个整数中？内存限制2G。

40亿个数，我们先看内存占用：（40亿*4）/1024/1024/1024=14.9G （int 占4字节32bit）,所以用传统的内存排序肯定不可以。

> 这里要用到Bitmap算法，Bitmap就是用一个bit位来映射某个元素的value值，key则是该元素值

就是用一个bit位来标识一个int整数，这样需要的内存空间为：40亿/8/1024/1024=476M。

假设我们要对5个不重复的数（4，7，2，5，3）排序。确定最大值是7，数值范围是0~7，共8个数，需要8bit，即1byte。

从0计数，从右往左，即 
00000000  -->  76543210

给这5个数赋值：
00010000 (第一个数4)
10010000 (第二个数7)
10010100 (第三个数2)
10110100 (第四个数5)
10111100 (第五个数3)

最终结果为10111100，遍历这8个bit位，将值为1输出，排序结果为 （7，5，4，3，2）

Java中对应的实现是BitSet类，非线程安全

```java
public static void main(String[] args) { 
    int[] intArray = { 4, 7, 2, 5, 3 };
    BitSet bitSet = new BitSet(8);
    
    for (int i = 0; i < intArray.length; i++) {
        bitSet.set(intArray[i], true);
    }
    
    System.out.println(bitSet);
}
```

其它说明：
Bitmap排序需要的时间复杂度和空间复杂度依赖于数据中最大的数字比如7，因此空间上需要1个字节大小的内存，时间上需要遍历完整个数组。当数据类似（1，1000，10万）只有3个数据的时候，用bitmap时间复杂度和空间复杂度相当大，只有当数据比较密集时才有优势。

同类问题：
【1】已知某个文件内包含一些电话号码，每个号码为8个数字，统计不同号码的个数。
解决方法： 8位最多99999999,占用100,000,000个bit,大小为12MB，这样用12M内存就表示来所有8位数的电话。

【2】2.5亿个整数中找出不重复的整数的个数，内存空间不足以同时容纳这2.5亿个整数。
解决方法： 将bitmap扩展一下，用2bit表示一个数即可，0表示未出现，1表示出现一次，2表示出现2次及以上，在遍历这些数的时候，如果对应位置的值是0，则将其置为1；如果是1，将其置为2；如果是2，则保持不变。或者我们不用2bit来进行表示，我们用两个bit-map即可模拟实现这个2bit-map，都是一样的道理。

【3】对10亿个不重复的整数进行排序。
【4】找出10亿个数字中重复的数字。
解决方法：bitmap初始化数据，当发现数组中的值是1，则判断出该数字是重复出现的。

【5】一个序列里除了一个元素，其他元素都会重复出现3次，设计一个时间复杂度与空间复杂度最低的算法，找出这个不重复的元素。

```java
    public static void main(String[] args) {  
        int[] list = {2, 3, 6, 3, 2, 5, 3, 2, 6, 6, 9, 9, 7, 7}; 
        int len = list.length * 2;  
        BitSet bs = new BitSet(len);  
    
        for (int i = 0; i < list.length; i++) {  
            int n = 2 * list[i];  
            boolean b1 = bs.get(n);  
            boolean b2 = bs.get(n + 1);  
            if (!b1 && !b2) { // 00  
                bs.set(n, true);  
                bs.set(n + 1, false);  
            }  
            else if (b1 && !b2) { // 01  
                bs.set(n + 1, true);  
                bs.set(n, false);  
            }  
            else if (!b1 && b2) { // 10  
                bs.set(n, n + 1, true);  
            }  
        }  
        for (int i = 0; i < bs.length(); i += 2) {  
            boolean b1 = bs.get(i);  
            boolean b2 = bs.get(i + 1);  
            if (!b1 && !b2) { // 00  
                // 0次  
            }  
            else if (b1 && !b2) { // 01  
                System.out.println(i / 2 + "一次");  
            }  
            else if (!b1 && b2) { // 10  
                System.out.println(i / 2 + "两次");  
            }  
            else if (b1 && b2) { // 10  
                System.out.println(i / 2 + "三次");  
            }  
        }  
    }
```