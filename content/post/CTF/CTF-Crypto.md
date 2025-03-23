+++
date = 2025-03-13T18:00:00+08:00
title = "CTF Crypto"
image = "Zs.png"
categories = ["CTF"]
+++

zyt大佬捏：[horostal.github.io](horostal.github.io)
## Pisano 周期

2<sub>n+2</sub> = b * u<sub>n+1</sub> + c * u<sub>n</sub>

```python
#sage
g = 17665922529512695488143524113273224470194093921285273353477875204196603230641896039854934719468650093602325707751568
mod = 100000007
R = BinaryRecurrenceSequence(6,1)
#计算序列的周期
cycle = R.period(mod)
print(R(g%cycle)%mod)

#output :41322239
```
### Virgenene(维吉尼亚)

取一个长度为$x$的$key$，则在每一个长度为$x$的明文区间内，每一个字母本身都在进行凯撒式移位加密。例如选取$key$为`vector`，则对应数字为`(21, 4, 2, 19, 14, 17)`，之后对明文进行加密：

```python
(明文) h   e   r   e   i   s   h   o   w   i   t   w   o   r   k   s
(密钥) 21  4   2   19  14  17  21  4   2   19  14  17  21  4   2   19
(密文) c   i   t   x   w   j   c   s   y   b   h   n   j   v   m   l
```

那如果有密文，可以通过猜测明文前几位算出$key$，找到循环点并截取出正确的$key$，在进行解密即可。推荐一个爆破维吉尼亚的网站：https://www.guballa.de/vigenere-solver，下面是自己手撸的加解密和猜测密钥。

```python
def encode(plaintext, key):
    li = []
    j = 0
    for i in range(len(plaintext)):
        if 'a' <= plaintext[i] <= 'z':
            li.append(chr((ord(plaintext[i]) - ord('a') + ord(key[j].lower()) - ord('a')) % 26 + ord('a')))
            j = (j + 1) % len(key)
        elif 'A' <= plaintext[i] <= 'Z':
            li.append(chr((ord(plaintext[i]) - ord('A') + ord(key[j].upper()) - ord('A')) % 26 + ord('A')))
            j = (j + 1) % len(key)
        else:
            li.append(plaintext[i])
    print(''.join(li))
    # print(''.join([chr((ord(st[i]) - ord('a') + ord(s[i % len(s)]) - ord('a')) % 26 + ord('a')) for i in range(len(st))]))

def decode(ciphertext, key):
    li = []
    j = 0
    for i in range(len(ciphertext)):
        if 'a' <= ciphertext[i] <= 'z':
            li.append(chr((ord(ciphertext[i]) - ord(key[j].lower())) % 26 + ord('a')))
            j = (j + 1) % len(key)
        elif 'A' <= ciphertext[i] <= 'Z':
            li.append(chr((ord(ciphertext[i]) - ord(key[j].upper())) % 26 + ord('A')))
            j = (j + 1) % len(key)
        else:
            li.append(ciphertext[i])
    print(''.join(li))
    # print(''.join([chr((ord(t[i]) - ord('a') - ord(s[i % len(s)]) + ord('a')) % 26 + ord('a')) for i in range(len(t))]))

def check(plaintext, ciphertext):
    t = min(len(plaintext), len(ciphertext))
    li = []
    for i in range(t):
        if 'a' <= ciphertext[i] <= 'z':
            li.append(chr((ord(ciphertext[i]) - ord(plaintext[i].lower())) % 26 + ord('a')))
        elif 'A' <= ciphertext[i] <= 'Z':
            li.append(chr((ord(ciphertext[i]) - ord(plaintext[i].upper())) % 26 + ord('A')))
        else:
            continue
    return ''.join(li)
    # ''.join([chr((ord(ciphertext[i]) - ord(plaintext[i])) % 26 + ord('a')) for i in range(t)])
```
