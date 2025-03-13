+++
date = 2025-03-06T18:00:00+08:00
title = "CTF Misc"
image = "Mc.png"
+++

# Misc

## Kali（Linux系统）

### extundelete

```bash
# 在linux上挂载光盘的命令
mkdir /mnt/disk
mount attachment.img /mnt/disk/
cd /mnt/disk 
#可以使用 eog 图片名 命令来查看图片
#使用结束后用 
umount: /mnt/disk 
#命令取消挂载
extundelete --restore-all attachment.img
# 数据恢复成功后会生成一个 RECOVERED_FILES 文件
```

### 压缩包套娃

```bash
while [ "find . -type f -name '*.tar.xz' | wc -l" -gt 0 ]; do find -type f -name "*.tar.xz" -exec tar xf '{}' \; -exec rm -- '{}' \;; done;
strings flag 查找flag字符
```

### LSB（老色比）

```bash
zsteg -e "b8,rgb,lsb,xy" 1.png > diskimage.dat

testdisk : testdisk diskimage.dat

 C to copy the selected files, c to copy the current file  
```

## Traffic（流量）

### SMTP

### TLS

```RSA
-----BEGIN RSA PRIVATE KEY-----

XXXXXXX

-----END RSA PRIVATE KEY-----
```

- wireshark 编辑即可

### **SMB**+hascat

- 省赛的题目复现，讲一下自己的理解

## Code（编码）

#### 猪圈密码：

![](2.png)

- **多类变种**

![](1.png)

![](3.png)

- #### 圣堂武士密码（猪圈密码的变种）：

![](5.png)

#### 小猫密码：

![](1.jpg)

#### 五笔密码：

![](4.png)

#### 提瓦特大陆：

![](6.png)

#### 古埃及象形文字：

![](7.png)

#### 外星人密码：

![](8.png)

#### 克林贡语密码《星际迷航》：

![](9.png)

#### 元素周期表：

![](10.png)

#### 狄德拉字符(暗精灵活神)：

![](11.png)

#### 银河字母(游戏《指挥官基恩》)：

![](12.png)

#### 跳舞的小人：

![](13.png)

#### 旗语密码：

![](14.png)

#### 国际船用信号旗：

![](16.png)

#### 夏多密码（又称曲折密码）:

![](17.png)

在以上所示的字母表密钥的底部，列有四个附加符号1，2，3，4.他们可以放在密文中的任何地方。每个附加符号指示，如何转动写有密文的纸张，再进行后续的加密或解密操作，直到出现另一个附加符号。
可以把每个附加符号中的那根线看作是指示针，它指示了纸张的上端朝上，朝下，朝左，朝右。比如说：
如果出现符号3，那么纸张就应该转动180度，使其上端朝下；
符号2表示纸张上端朝右；
符号4表示纸张上端朝左；
符号1表示纸张上端朝上

#### 多斯拉克语字母表：

![](18.png)

#### 海利亚文字：

![](19.png)

#### Covenant字体：

![](20.png)

#### top cipher:

![](15.png)

exp:

```python
cipherList = {'M':'ACEG','R':'ADEG','K':'BCEG','S':'BDEG','A':'ACEH','B':'ADEH','L':'BCEH','U':'BDEH','D':'ACEI','C':'ADEI','N':'BCEI','V':'BDEI','H':'ACFG','F':'ADFG','O':'BCFG','W':'BDFG','T':'ACFH','G':'ADFH','P':'BCFH','X':'BDFH','E':'ACFI','I':'ADFI','Q':'BCFI','Y':'BDFI'}
flag1 = ""
# s = 'BCEHACEIBDEIBDEHBDEHADEIACEGACFIBDFHACEGBCEHBCFIBDEGBDEGADFGBDEHBDEGBDFHBCEGACFIBCFGADEIADEIADFH'
# for i in range(0,len(s),4):
#     block = s[i:i+4]
#     for j in cipherList:
#         if block==cipherList[j]:
#             flag1 += j
# print(flag1)

s2 = "LDVUUCMEXMLQSSFUSXKEOCCG"
cipherList2 = ['M', 'R', 'K', 'S', 'A', 'B', 'L', 'U', 'D', 'C', 'N', 'V', 'H', 'F', 'O', 'W', 'T', 'G', 'P', 'X', 'E', 'I', 'Q', 'Y']
# cipherList2.reverse()
cipherList3 = ['Y', 'Q', 'I', 'E', 'X', 'P', 'G', 'T', 'W', 'O', 'F', 'H', 'V', 'N', 'C', 'D', 'U', 'L', 'B', 'A', 'S', 'K', 'R', 'M']
flag2 = ''
for i in s2:
    for j in cipherList2:
        if i==j:
            rank = cipherList2.index(j)
            flag2 += cipherList3[rank]

print(flag2)

```

## Matlab -> Python

```Matlab
fid=fopen('33.wav','rb');
a=fread(fid,inf,'uchar');	
n=length(a)-44;
fclose(fid);

io=imread('kkk.bmp');	
[row col]=size(io);	# 返回图像尺寸
wi=io(:);	# 二维转一维
if row*col>n
 error('文件太小');	# 要隐写的目的文件要够大
end
watermarkedaudio=a;
watermarklength=row*col;
for k=1:row*col	# 从1到row*col
 watermarkedaudio(44+k)=bitset(watermarkedaudio(44+k),1,wi(k));
end
figure;
subplot(2,1,1);plot(a);
subplot(2,1,2);plot(watermarkedaudio);
fid = fopen('2.wav', 'wb');
fwrite(fid,watermarkedaudio,'uchar');
fclose(fid);

```

- fread : 从二进制文件读取数据
- inf : 读出fid指向的打开的文件的全部数据
- imread : 读取图像
- bitset(A,pos,V) ：将A以二进制来表示，并将第pos个位置（从右数）， 设置为 V 的值，在将所得到的值转换成10进制数并返回。
- figure ：使用默认属性值创建一个新的图窗窗口
- subplot ：将当前图窗划分为 m×n 网格，并在 p 指定的位置创建坐标区
- plot ( Y ) 绘制 Y 对一组隐式 x 坐标的图。
  如果 Y 是向量，则 x 坐标范围从 1 到 length( Y )。
  如果 Y 是矩阵，则对于 Y 中的每个列，图中包含一个对应的行。x 坐标的范围是从 1 到 Y 的行数。

```Python
import numpy as np
from PIL import Image

wav = open('aaa.wav','rb')
content = wav.read()
wav.close()

bins = []
for i in range(45,45+388*100):
    bins.append(255 if int(bin(content[i])[-1:]) else 0)
flag = np.array(bins,np.uint8).reshape(388,100)

imgg = Image.fromarray(flag).save('res.bmp')
```

## Excel

- 将表格全选，选择菜单栏中的， 表单格式 => 突出显示单元格规则 => 文本包含

##pwntools

```python
send(data):                        发送数据
sendline(data) :                   发送一行数据，相当于在末尾加'\n'      
recv(numb=4096, timeout=default) : 给出接收字节数,timeout指定超时
recvuntil(delims, drop=False) :    接收到delims的pattern
（以下可以看作until的特例）
recvline(keepends=True) :          接收到"\n，keepends指定保留\n"
recvall() : 接收到EOF
recvrepeat(timeout=default) :      接收到EOF或timeout
interactive() :                    与shell交互
```

# 相关工具使用

## basecrack4.0v

```python
basecrack.py -m 自动化处理
```

## Mimikatz --dmp文件

```bash
sekurlsa::minidump lsass.dmp --加载
sekurlsa::logonpasswords full --导出密码散列值
```

