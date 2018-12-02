## MISC Write Up

### Traffic Light

1. 使用 `ImageMagic` 或 `PhotoShop` 等工具逐帧分离 `gif` 文件形成 `png`：

   ```cmd
   $ magick convert Traffic_Light.gif -coalesce png/%05.png
   ```

   *PostScript*：安装 ImageMagic

   ```cmd
   $ choco install ImageMagic
   ```

2. 使用以下 bash 命令：

   ```bash
   $ cd png
   
   $ ll | awk '{print $5}' | tr -d '\n' | sed 's/5333/1/g; s/5042//g; s/5282/0/g; s/5347/\n/g;' > encoding.txt
   ```

3. 创建以下 `python` 脚本并运行，即得到 flag：

   ```python
   with open("encoding.txt","r") as f:
   	l = f.read().splitlines() 
   
   int_l = [int(l[i+1],2) for i in range(len(l)-1)]
   char_l = [chr(int_l[i]) for i in range(len(int_l))]
   
   print(''.join(char_l))
   ```

   ```bash
   $ python decode.py
   flag{Pl34s3_p4y_4tt3nt10n_t0_tr4ff1c_s4f3ty_wh3n_y0u_4r3_0uts1d3}
   ```

### Quotes

1. 使用以下 bash 命令：

   ```bash
   $ sed 's/+//g' Quotes.txt | awk -v RS=" " '{print length($1)}' > number.txt
   ```

2. 创建以下 `python` 脚本并运行：

   ```python
   with open("number.txt", "r") as f:
   	l = f.read().splitlines()
   
   
   l_i = [int(i,10) for i in l]
   l_c = [chr(ord('a') + i -1) if i is not 0 else ' ' for i in l_i]
   
   print(''.join(l_c))
   ```

   ```bash
   $ python decode.py
   word games
   ```

## Method 2

### MISC Traffic Light

我们拿到的是一个gif图，将其放入Photoshop提取出图层（即每一帧）
 
啊嘞？这个图有一千多帧啊，不过亮灯似乎是有规律的，我们先从图层1开始数，绿灯亮，计0，红灯亮，计1，黄灯亮，计2，数出前几十帧就可以得到这些内容：

 ```bash
0110011020110110 0201100001201100 1112011110112010
```

我们看到2每隔8个0或1出现一次，而中间的0、1可以组成二进制ascii编码，这样可对应出flag文本。

```bash
0110011020110110 0201100001201100 1112011110112010 1000020110110020
f         l         a        g         {        P         l     
0110011200110100 2011100112001100 1120101111120111 0000200110100201
3         4          s       3         _         p        4
1110012010111112 0011010020111010 0201110100200110 0112011011102011
y       _        4           t          t       3         n
1010020011000120 0110000201101110 2010111112011101 0020011000020101
t       1       0         n         _         t         0      _
1111201110100201 1100102001101002 0110011020110011 0200110001201100
      t          r        4         f       f         1
0112010111112011 1001120011010020 1100110200110011 2011101002011110
c      _         s        4      f        3          t        
0120101111120111 0111201101000200 1100112011011102 0101111120111100
y      _      w        h        3         n         _        y
1200110000201110 1012010111112001 1010020111001020 0110011201011111
   0         u         _        4         r         3         _
2001100002011101 0120111010020111 0011200110001201 1001002001100112
   0         u        t         s         1        d        3
01111101
}

flag{Pl34s3_p4y_4tt3nt10n_t0_tr4ff1c_s4f3ty_wh3n_y0u_4r3_0uts1d3} 
```

你们搞的这个gif啊，Excited！
最后总共就有三件大事：
1. 利用过人的毅力统计出全部内容：
2. 根据英语常识判断有没有数错编码
3. 去医院治色盲
 

### MISC Quotes

纯脑洞题
文本中切断单词的空格和加号是可疑的，因此我们计数了单词的字母数量
 
例如第一个红框有23个字母，我们对应到字母表第23位的w，以此类推得到wordgames。然后我们只需在提交页面来回尝试各种变体，发现
`flag{word games}`
是正确答案。
