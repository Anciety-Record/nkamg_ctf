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
Example 2(大雾

MISC Traffic Light
我们拿到的是一个gif图，将其放入Photoshop提取出图层（即每一帧）
 
啊嘞？这个图有一千多帧啊，不过亮灯似乎是有规律的，我们先从图层1开始数，绿灯亮，计0，红灯亮，计1，黄灯亮，计2，数出前几十帧就可以得到这些内容：
 
我们看到2每隔8个0或1出现一次，而中间的0、1可以组成二进制ascii编码，这样可对应出flag文本。

你们搞的这个gif啊，Excited！
最后总共就有三件大事：
1.利用过人的毅力统计出全部内容：
2.根据英语常识判断有没有数错编码
3.去医院治色盲
 

MISC Quotes
纯脑洞题
文本中切断单词的空格和加号是可疑的，因此我们计数了单词的字母数量
 
例如第一个红框有23个字母，我们对应到字母表第23位的w，以此类推得到wordgames。然后我们只需在提交页面来回尝试各种变体，发现
flag{Word Games}
是正确答案。
