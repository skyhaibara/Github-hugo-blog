+++
date = 2025-03-06T18:00:00+08:00
title = "CTF Misc"
image = "Cover/Mc.webp"
categories = ["CTF"]
weight = 1
+++

# Misc

## Kali(Linux 系统)

### extundelete

从 ext 文件系统的磁盘/分区镜像中**恢复被删除的文件**。CTF 里经常给一个 `.img` 镜像,要求挂载后用 extundelete 把删除项恢复出来再看图。

```bash
# 在 linux 上挂载光盘的命令
mkdir /mnt/disk
mount attachment.img /mnt/disk/
cd /mnt/disk 
# 可以使用 eog 图片名 命令来查看图片
# 使用结束后用 
umount /mnt/disk 
# 命令取消挂载
extundelete --restore-all attachment.img
# 数据恢复成功后会生成一个 RECOVERED_FILES 文件
```

### 压缩包套娃

题目把 flag 包在 N 层压缩包里(`.tar.xz` 套 `.tar.xz` 套 ...),手动解到吐血。这个一行命令循环解压,解完即删,直到没有 `.tar.xz` 为止:

```bash
while [ "find . -type f -name '*.tar.xz' | wc -l" -gt 0 ]; do find -type f -name "*.tar.xz" -exec tar xf '{}' \; -exec rm -- '{}' \;; done;
strings flag        # 查找 flag 字符
```

### LSB(老色比)

LSB Stego 的谐音梗 —— **L**east **S**ignificant **B**it,最低有效位隐写。用 `zsteg` 工具按指定通道顺序提取 PNG 中藏的数据:

```bash
zsteg -e "b8,rgb,lsb,xy" 1.png > diskimage.dat
```

## Traffic(流量)

### SMTP

邮件传输协议,流量分析里**能从 PCAP 还原邮件正文、附件、登录凭据**。Wireshark 跟踪 SMTP 流可以直接看到 `MAIL FROM`、`RCPT TO`、`DATA` 阶段的明文,base64 附件也能提取出来。

### TLS

加密流量,**需要私钥或 (Pre)-Master-Secret 才能解密**。题目通常把 RSA 私钥放在附件里,Wireshark → Edit → Preferences → Protocols → TLS → RSA keys list 配置后,加密包就会自动还原成明文。

```RSA
-----BEGIN RSA PRIVATE KEY-----

XXXXXXX 标志类型

-----END RSA PRIVATE KEY-----
```

- wireshark 编辑即可

### SMB + hashcat

SMB 文件共享协议,流量里**可能包含 NTLMv2 哈希形式的登录凭据**,提取后用 hashcat 暴破即可拿到明文密码。省赛常考。

- 省赛的题目复现,讲一下自己的理解

## olevba

[`olevba`](https://github.com/decalage2/oletools) 是 oletools 套件里的核心工具,用于**从 Office 文档(.doc / .xls / .docm 等)中提取并分析 VBA 宏代码**,扫描自动执行、字符串解码、Shell 调用等可疑行为。CTF 钓鱼附件题里几乎必备。

```bash
olevba <file>            # 提取并分析 VBA
olevba <file> --decode   # 自动解码混淆字符串
```

![olevba 运行示例](/CTF/Misc/22.webp)

## Code(编码)

CTF Misc 里常见的"图像替代"密码 —— 把字母映射为图形 / 符号 / 物体。识别这类密码的关键是**先认出风格**(中世纪、奇幻、游戏 IP、外星等),再查对应字母表还原。

<div class="cipher-grid">

<figure class="cipher-card">
<img src="/CTF/Misc/2.webp" alt="猪圈密码">
<figcaption><strong>猪圈密码 (Pigpen)</strong><br>中世纪共济会图形替代,九宫格 + X 形组合,经典款,变种极多。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/5.webp" alt="圣堂武士密码">
<figcaption><strong>圣堂武士密码</strong><br>猪圈变种,源自圣殿骑士团十字会符号体系。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/1.webp" alt="小猫密码">
<figcaption><strong>小猫密码</strong><br>萌系替换密码,每个字母对应一个猫脸符号,趣味题常用。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/4.webp" alt="五笔密码">
<figcaption><strong>五笔密码</strong><br>把五笔输入法的字根按键映射为字母,考输入法熟练度。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/6.webp" alt="提瓦特大陆">
<figcaption><strong>提瓦特大陆</strong><br>出自《原神》游戏内的七国通用语字母表,二次元 CTF 包装。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/7.webp" alt="古埃及象形文字">
<figcaption><strong>古埃及象形文字</strong><br>真实历史文字,部分 CTF 借用《罗塞塔石碑》的字母对照表。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/8.webp" alt="外星人密码">
<figcaption><strong>外星人密码</strong><br>几何符号字母表,无统一标准,常见于科幻包装题。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/9.webp" alt="克林贡语">
<figcaption><strong>克林贡语 (Klingon)</strong><br>出自《星际迷航》,虚构语言但有完整官方字母表,可直接查表。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/10.webp" alt="元素周期表">
<figcaption><strong>元素周期表</strong><br>用化学元素符号(Au、Fe...)拼出字母串,按周期表原子序索引。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/11.webp" alt="狄德拉字符">
<figcaption><strong>狄德拉字符</strong><br>出自《暗精灵》(Dark Elves) 设定的虚构精灵语字母表。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/12.webp" alt="银河字母">
<figcaption><strong>银河字母</strong><br>游戏《指挥官基恩》(Commander Keen) 设计的 8-bit 风格字母表。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/13.webp" alt="跳舞的小人">
<figcaption><strong>跳舞的小人</strong><br>出自福尔摩斯短篇《跳舞的小人》(柯南·道尔),不同舞姿代表字母。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/14.webp" alt="旗语密码">
<figcaption><strong>旗语密码</strong><br>国际海事旗语,两只手臂角度组合表示字母。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/16.webp" alt="国际船用信号旗">
<figcaption><strong>国际船用信号旗</strong><br>ICS 标准旗帜,每面对应一个字母或数字。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/18.webp" alt="多斯拉克语">
<figcaption><strong>多斯拉克语</strong><br>《权力的游戏》中游牧民族多斯拉克人的虚构文字。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/19.webp" alt="海利亚文字">
<figcaption><strong>海利亚文字</strong><br>《塞尔达传说》中海拉鲁王国通用语,任天堂粉丝梗。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/20.webp" alt="Covenant字体">
<figcaption><strong>Covenant 字体</strong><br>《光环》(Halo) 中星盟使用的外星文字。</figcaption>
</figure>

<figure class="cipher-card">
<img src="/CTF/Misc/21.webp" alt="DTMF 键盘">
<figcaption><strong>DTMF 键盘</strong><br>电话双音多频,每个数字对应一对频率,可通过音频频谱反推数字。</figcaption>
</figure>

</div>

### 夏多密码(又称曲折密码)

![](/CTF/Misc/17.webp)

在以上字母表密钥的底部,列有四个附加符号 `1` `2` `3` `4`,它们可以放在密文中的任何地方。每个附加符号指示**如何转动写有密文的纸张**,再进行后续的加密或解密操作,直到出现另一个附加符号。

每个附加符号中的那根线可以看作是指示针,它指示纸张上端朝向:

| 符号 | 朝向 |
|---|---|
| `1` | 纸张上端朝上 |
| `2` | 纸张上端朝右 |
| `3` | 纸张上端朝下(转动 180°) |
| `4` | 纸张上端朝左 |

### top cipher

![](/CTF/Misc/15.webp)

4 字母组(`ACEG` / `ADEG` ...)替换密码,有 24 字母全表对照。常见解密思路:把每 4 个密文字符切片,在字母表里查对应明文字母。

**exp:**

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

## pwntools

`pwntools` 是 Pwn 题里最常用的 Python 库,封装了进程交互、汇编、ELF 解析、ROP 等功能。下面是 IO 部分最常用的 API 速查:

```python
send(data):                        发送数据
sendline(data) :                   发送一行数据,相当于在末尾加 '\n'
recv(numb=4096, timeout=default) : 给出接收字节数,timeout 指定超时
recvuntil(delims, drop=False) :    接收到 delims 的 pattern
# 以下可以看作 until 的特例
recvline(keepends=True) :          接收到 '\n',keepends 指定是否保留 \n
recvall() :                        接收到 EOF
recvrepeat(timeout=default) :      接收到 EOF 或 timeout
interactive() :                    与 shell 交互
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
