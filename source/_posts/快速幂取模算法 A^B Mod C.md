---
title: 快速幂取模算法 A^B Mod C
id: 2444
categories:
  - Algorithm
date: 2015-03-08 15:27:28
tags:
---

先说题目：
给出3个正整数A B C，求A^B Mod C。(1 <= A,B,C <= 10^9)。例如，3 5 8，3^5 Mod 8 = 3。

立马想到的解决方法是
```php
int ans=1;
for(int i=1;i<b;i++){
    ans=ans*a;
}
ans = ans % c;
```
但是这里有个很明显的问题，如果a和b过大，就很容易溢出。

这里我们可以对mod进行优化，因子取余相乘后取余最终余数不变，即公式：
a^b mod c = (a mod c)^b mod c

那么上面算法可优化为
```php
int ans=1;
a = a % c;
for(int i=1;i<b;i++){
    ans=ans*a%c; //这里再取了一次余
}
ans = ans % c;
```

mod优化完后再看一下幂的优化：
> A^8 = A * A * A * A * A * A * A * A
>
> 总共需要7次乘法运算；
>
> 将其平均分解：
>
> A^8 = (A * A * A * A) * (A * A * A * A) = (A * A * A * A) ^ 2
>
> 这样我们就只需要4次乘法运算了；
>
> 再分解：
>
> A^8 = [(A * A) * (A * A)] ^ 2 = [(A * A) ^ 2] ^ 2
>
> 那么现在计算次数为 (A * A) 一次，两个平方两次共3次。

以上过程可以得出以下公式：
> a^b=(a^2)^(b/2); b为偶
>
> a^b=(a^2)^(b/2)*a; b为奇

最终公式为：
> a^b mod c=(a^2)^(b/2) mod c; b为偶
>
> a^b mod c=(a^2)^(b/2)*a mod c; b为奇

快速幂算法
```php
int quickPowerMod(int a, int b, int c){
    int ans = 1;
    a = a % c;
    while(b>0) {
        if(b % 2 == 1){  //可优化为 if((b&1)>0) 若幂为奇数
            ans = (ans * a) % c;
        }        
        b = b/2;  //可优化为 b>>=1 b除于2
        a = (a * a) % c;
    }
    return ans;
}
```

最后，题中讲1 <= A,B,C <= 10^9，所以用int没问题，如果数再大，就以使用java 里的 
```java
BigInteger static BigInteger bigint(BigInteger a,BigInteger b,BigInteger m){
        BigInteger ans = new BigInteger("1");
        while (b.compareTo(BigInteger.valueOf(0)) > 0) {
            if (b.mod(BigInteger.valueOf(2)).compareTo(BigInteger.valueOf(1)) == 0){ // if(n%2==1),b是奇数
                ans = ans.multiply(a).mod(m); // sq=(sq*p)%m;
            }
            a = a.multiply(a).mod(m); // p=(p*p)%m;
            b = b.divide(BigInteger.valueOf(2));
        }
        return ans;
    }
```