# **faops使用说明**
## **前言**
随着分子生物学技术的发展，人类基因组计划的进行，测序成本从之前的高不可攀，到现在高校实验室都可以支付得起测序费用，测序技术逐渐走入大众的视野。

人们可以利用基因组测序技术，改良果蔬品种，提高作物品质；基因组测序技术应用于医学领域，从基因层面上检测罕见疾病发生的起因，预测癌症的发生，进行基因治疗等；基因组测序技术有利于进化生物学的发展，从基因序列信息来分析物种进化的保守型和变异性；另外，将会有更多转基因生物问世，人类可以根据需求，改造生物的形状，使其向着有利于人类的方向发展。依靠测序技术，我们能在短时间内得到海量的序列信息，所以快速处理这些序列，得到有价值的信息是非常关键的。 

faops 就是其中一款简便集成的基因组统计分析软件, 基于C语言编写, 运行速度极快, 能极大的提高工作效率. faops 日常序列的处理包括: 格式化序列, 截取序列, 随机抽取序列, 序列统计值计算等. faops 将帮助科研工作者更好地解密DNA序列.

## **软件简介**
### 2.1 软件用途
得到一个基因序列后，我们需要对其进行一些基本的处理分析，再进一步深入研究。本软件实现的主要功能正是对一个给定的基因序列进行一些基础的分析。

Faops可以实现多项功能：

a.	统计碱基组成和序列总长；

b.	提取指定的基因序列片段；

c.	获得基因序列的反向或互补序列；

d.	替换及标准化序列名；

e.	筛选符合指定条件的基因序列；

f.	按序列名或大小分割序列；

g.	计算N50及一些其他统计数据；

h.	交错合并双端测序数据；

### 2.2 编写语言
本软件的编写语言为C语言，可在Windows环境和Linux环境运行。其中用到了C语言的一些标准库函数。
### 2.3 软件安装与编译
本软件可以使用Homebrew 或 Linuxbrew直接安装，命令如下：
```
brew install wang-q/tap/faops
```
也可下载软件包后，在Linux，macOS（gcc or clang）和Windows（MinGW）系统环境自行编译：
```
git clone https://github.com/wang-q/faops
cd faops
make
```
注: 本教程里的faops 命令例子都是在ubuntu 20.04.4 LTS 中演示的.
## 软件使用方法
在使用软件前，请务必保证输入文件为FASTA或FASTQ格式。否则数据可能无法被正确识别并处理.
### 3.1 获取使用帮助：faops help
在Terminal或者ubuntu中输入faops help，即可出现faops的各种使用说明.

用法：help

输入：
```
faops help
```
输出：
```
Usage:     faops <command> [options] <arguments>
Version:   0.8.20

Commands:
    help           print this message
    count          count base statistics in FA file(s)
    size           count total bases in FA file(s)
    masked         masked (or gaps) regions in FA file(s)
    frag           extract sub-sequences from a FA file
    rc             reverse complement a FA file
    one            extract one fa record
    some           extract some fa records
    order          extract some fa records by the given order
    replace        replace headers from a FA file
    filter         filter fa records
    split-name     splitting by sequence names
    split-about    splitting to chunks about specified size
    n50            compute N50 and other statistics
    dazz           rename records for dazz_db
    interleave     interleave two PE files
    region         extract regions from a FA file

Options:
    There're no global options.
    Type "faops command-name" for detailed options of each command.
    Options *MUST* be placed just after command.
```
### 3.2 统计碱基组成：count
命令faops count 用于统计一个或多个序列文件中各序列的总长和各序列的碱基数量分情况.(只用于统计碱基数量，不包括序列中的空格数.) faops count 可以用于同时统计多个文件中的序列，输出结果的排列顺序与输入文件名的排列顺序一致.

用法：
```
faops count <in.fa> [more_files.fa]
```

输入：
```
faops count in1.fa
```
其中in1.fa文件内容为：

```
>read0  
tCGTTTAACCCAAatcAAGGCaatACAggtGggCCGccCatgTcAcAAActcgatGAGtgGgaAaTGgAgTgaAGcaGCAtCtGctgaGCCCCATTctctAgCggaaaATGgtatCGaACcGagataAGtTAAacCgcaaCgGAtaagGgGcgGGctTCAaGtGAaGGaAGaGgGgTTcAaaAgGccCgtcGtCaaTcAaCtAAggcGgaTGtGACactCCCCtAtTtcaaGTCTTctaCccTtGaTaCGaTtcgCGTtcGaGGaGcTACaTTAaccaaGtTaatgCGAGCGcCtgCGaAcTTGccAAgTCaGCtgctCTgttCtcAggTaCAcAaGTcagccAtTGTGTCGaCGCTCT  
>read1  
taGGCGcGGgCggtgTgGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatcggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatc  
GCaTaTC  
>read2  
AtagcAagCtcAgttcaACttCAcCGGTAAaTtcttgTAGtgTcTCgacCgcCcCctTGTACtgtaGG  
>read3  
```
```
#seq    len     A       C       G       T       N
read0   359     99      89      92      79      0
read1   106     24      25      32      25      0
read2   68      15      19      14      20      0
read3   0       0       0       0       0       0
total   533     138     133     138     124     0
```

输入：
```
faops count in1.fa in2.fa
```
其中in2.fa内容为
```
>read4  
ttGGGTTggGcgCccGtgGgTtAGCaAacTTgaATGgtcGtgatgAGCGAaCTGGGtCgGCCCGaCCTtCCcTtAAtTtTACAtCTcAcGttAtgGaGCatTatcggGccggtaTC  
>read5  

>read6  
aActgggCctTtcGgaAtAAAtGATTctActtTGTcTaatCatTcAgggAGagccGCCTTcaATTACGGcGgaaCAtGGgtctGTtCAGaggTgAATTCAAATGCtGagGcaAGatcccCgATTCAcCgGgAaGcCTGtTcgTtCCgtCAtGTGgCtcaatAgcACgCCAaCaGTactggCctcgTCcCatTTGCGC  ATtCTtatAcGtGCatCagTttTATaacAtTA  
TcCgCGaCcCaGcaAaAAaGcGTgaAgtTgCTaATttaaTcatcAcaGaCCAaGCaCttcTTAAcTtttGAagGAGGattaaaGGGTcacAggAcCTtCgtCtccaccGaCaGATCgAcgCGTGgcCCG 
```
输出：
```
#seq    len     A       C       G       T       N
read0   359     99      89      92      79      0
read1   106     24      25      32      25      0
read2   68      15      19      14      20      0
read3   0       0       0       0       0       0
read4   116     21      27      35      33      0
read5   0       0       0       0       0       0
read6   358     94      91      81      92      0
total   1007    253     251     254     249     0
```

### 3.3 统计序列长度：size

命令faops size 可以统计一个或多个序列文件中序列长度信息。size和count统计的区别是: size统计包括序列中的空格数，count只统计序列中的碱基数量而不包括空格数.

用法：
```
faops size <in.fa> [more_files.fa]
```
in1.fa 和 in2.fa 文件内容同3.2

输入：
```
faops size in1.fa
```
输出：
```
read0   361
read1   110
read2   70
read3   0
```
输入：
```
faops size in2.fa
```

输出：
```
read4   122
read5   0
read6   369
```



输入：

```
faops size in1.fa in2.fa
```

输出：
```
read0   361
read1   110
read2   70
read3   0
read4   122
read5   0
read6   369
```
### 3.4 提取指定序列片段：frag
命令faops frag 可以从序列文件中准确提取指定位置的片段. 值得注意的是: 如果一个序列文件中包含多个序列, 则默认对其中第一个序列进行提取操作. 此外, 可以通过手动添加参数-l 来指定提出片段的分行宽度, 序列分行的宽度默认为80.

用法：

```
faops frag [options] <in.fa> start end <out.fa>
```
选项：	
```
-l	INT		输出时序列分行的宽度，默认为80
```
输入:
```
faops frag -l 20 in1.fa 11 110 out.fa
```
ubuntu界面提示:
```
More than one sequence in in1.fa, just using first
```
输出：out.fa文件内容：
```
>read0:11-110
CAAatcAAGGCaatACAggt
GggCCGccCatgTcAcAAAc
tcgatGAGtgGgaAaTGgAg
TgaAGcaGCAtCtGctgaGC
CCCATTctctAgCggaaaAT
```

out.fa输出文件与输入文件在同一个文件夹里面.
### 3.5 获得反向或互补序列：rc
命令faops rc 可以获得一个或多个序列的反向或互补序列. 本功能可以根据使用者实际需求, 使用不同参数生成反向、 互补、 反向且互补这三种形式的序列. 其中, 参数-f 列表里的序列名称以回车换行进行分隔.

用法：
```
faops rc [options] <in.fa> <out.fa> 
```
选项：
```
-n			保持序列名相同（不添加RC_）
-r			仅反向，添加R_
-c			仅互补，添加C_
-f 	STR	    仅对给定列表中的序列进行反向或互补操作
-l	INT		输出时序列分行的宽度，默认为80
```
输入:
```
faops rc -l 60 -n -f list.file in2.fa out.fa
```
in2.fa文件内容:
```
>read4  
ttGGGTTggGcgCccGtgGgTtAGCaAacTTgaATGgtcGtgatgAGCGAaCTGGGtCgGCCCGaCCTtCCcTtAAtTtTACAtCTcAcGttAtgGaGCatTatcgg
>read5 
TTTAAAGGGC
>read6  
aActgggCctTtcGgaAtAAAtGATTctActtTGTcTaatCatTcAgggAGagccGCCTTcaATTACGGcGgaaCAtGGgtctGTtCAGaggTgAATTCAAATGCtGagGcaAGatcccCgATTCAcCgGgAaGcCTGtTcgTtCCgtCAtGTGgCtcaatAgcACgCCAaCaGTactggCctcgTCcCatTTGCGC  
ATtCTtatAcGtGCatCagTttTATaacAtTATcCgCGaCcCaGcaAaAAaGcGTgaAgtTgCTaATttaaTcatcAcaGaCCAaGCaCttcTTAAcTtttGAagGAGGattaaaGGGTcacAggAcCTtCgtCtccaccGaCaGA

```

list.file文件内容:
```
read5
read6
```
输出：out.fa文件内容：
```
>read4
ttGGGTTggGcgCccGtgGgTtAGCaAacTTgaATGgtcGtgatgAGCGAaCTGGGtCgG
CCCGaCCTtCCcTtAAtTtTACAtCTcAcGttAtgGaGCatTatcgg
>read5
GCCCTTTAAA
>read6
TCtGtCggtggaGacGaAGgTccTgtgACCCtttaatCCTCctTCaaaAgTTAAgaaGtG
CtTGGtCtgTgatgAttaaATtAGcAacTtcACgCtTTtTtgCtGgGtCGcGgATAaTgt
tATAaaActGatGCaCgTataAGaAT  GCGCAAatGgGAcgagGccagtACtGtTGGcG
TgcTattgaGcCACaTGacGGaAcgAaCAGgCtTcCcGgTGAATcGgggatCTtgCctCa
GCATTTGAATTcAcctCTGaACagacCCaTGttcCgCCGTAATtgAAGGCggctCTcccT
gAatGattAgACAaagTagAATCaTTTaTtcCgaAagGcccagTt
```
### 3.6 根据列表提取特定序列：some
命令some按照提供的 list.file 列表文件里的序列名称提取指定的序列。本功能根据所需序列的名称列表进行操作。列表文件list.file 中序列名称以回车进行分隔。

用法：	
```
faops some [options] <in.fa> <list.file> <out.fa>
```
选项：
```
-i			反向提取名称不在列表中的序列
-l	INT		输出时序列分行的宽度，默认为80
```
输入：
```
$ faops some -i -l 60 in1.fa list.file out.fa 
```
in1.fa文件内容:
```
>read0  
tCGTTTAACCCAAatcAAGGCaatACAggtGggCCGccCatgTcAcAAActcgatGAGtgGgaAaTGgAgTgaAGcaGCAtCtGctgaGCCCCATTctctAgCggaaaATGgtatCGaACcGagataAGtTAAacCgcaaCgGAtaagGgGcgGGctTCAaGtGAaGGaAGaGgGgTTcAaaAgGccCgtcGtCaaTcAaCtAAggcGgaTGtGACactCCCCtAtTtcaaGTCTTctaCccTtGaTaCGaTtcgCGTtcGaGGaGcTACaTTAaccaaGtTaatgCGAGCGcCtgCGaAcTTGccAAgTCaGCtgctCTgttCtcAggTaCAcAaGTcagccAtTGTGTCGaCGCTCT  
>read1  
taGGCGcGGgCggtgTgGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatcggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatcGCaTaTC  
>read2  
AtagcAagCtcAgttcaACttCAcCGGTAAaTtcttgTAGtgTcTCgacCgcCcCctTGTACtgtaGG  
>read3  

```

list.file文件内容:
```
read0  
read3 
```

输出：out.fa文件内容：
```
>read1
taGGCGcGGgCggtgTgGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatc
ggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatcGCaTaTC  
>read2
AtagcAagCtcAgttcaACttCAcCGGTAAaTtcttgTAGtgTcTCgacCgcCcCctTGT
ACtgtaGG  
```
### 3.7 给定顺序重排序列：order
命令order可以将序列文件数据全部读入内存，然后按照提供的顺序输出到新文件中。本功能类似some，但内存调度策略存在差别：order会一次性占用更高的内存。

用法：	
```
faops order [options] <in.fa> <list.file> <out.fa>
```
输入：
```
$ faops order -l 60 in1.fa \
 	<(faops size in1.fa | sort -n -r -k2,2 | cut -f 1) \
out.fa  
```
in1.fa文件内容:
```
>read0  
tCGTTTAACCCAAatcAAGGCaatACAggtGggCCGccCatgTcAcAAActcgatGAGtgGgaAaTGgAgTgaAGcaGCAtCtGctgaGCCCCATTctc  
tAgCggaaaATGgtatCGaACcGagataAGtTAAacCgcaaCgGAtaagGgGcgGGctTCAaGtGAaGGaAGaGgGgTTcAaaAgGccCgtcGt  
CaaTcAaCtAAggcGgaTGtGACactCCCCtAtTtcaaGTCTTctaCccTtGaTaCGaTtcgCGTtcGaGGaGcT  
ACaTTAaccaaGtTaatgCGAGCGcCtgCGaAcTTGccAAgTCaGCtgctCTgttCtcAggTaCAcAaGTcagccAtTGTGTCGaCGCTCT  
>read1  
taGGCGcGGgCggtgTgGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatcggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatc  
GCaTaTC  
>read2  
AtagcAagCtcAgttcaACttCAcCGGTAAaTtcttgTAGtgTcTCgacCgcCcCctTGTACtgtaGG  
caAtaGTaaTgAcTagGaCGTaagagAcCaccaCagGAAgAGAATccgCGaAtcCcTcacCCttGGTC  
ctGgAttttgcgcgTggtatgagGgAGtctcaATTGTCaccTaC  
GTatcccAGCgCtAcAcaAGAcTaCAtCTggCatTAG  
>read3  

```


此处列表为根据序列长度由大到小的排列。从而实现了根据长短顺序重排序列。

输出：out.fa文件内容：
```
>read0
tCGTTTAACCCAAatcAAGGCaatACAggtGggCCGccCatgTcAcAAActcgatGAGtg
GgaAaTGgAgTgaAGcaGCAtCtGctgaGCCCCATTctc  tAgCggaaaATGgtatCGa
ACcGagataAGtTAAacCgcaaCgGAtaagGgGcgGGctTCAaGtGAaGGaAGaGgGgTT
cAaaAgGccCgtcGt  CaaTcAaCtAAggcGgaTGtGACactCCCCtAtTtcaaGTCTT
ctaCccTtGaTaCGaTtcgCGTtcGaGGaGcT  ACaTTAaccaaGtTaatgCGAGCGcC
tgCGaAcTTGccAAgTCaGCtgctCTgttCtcAggTaCAcAaGTcagccAtTGTGTCGaC
GCTCT  
>read2
AtagcAagCtcAgttcaACttCAcCGGTAAaTtcttgTAGtgTcTCgacCgcCcCctTGT
ACtgtaGG  caAtaGTaaTgAcTagGaCGTaagagAcCaccaCagGAAgAGAATccgCG
aAtcCcTcacCCttGGTC  ctGgAttttgcgcgTggtatgagGgAGtctcaATTGTCac
cTaC  GTatcccAGCgCtAcAcaAGAcTaCAtCTggCatTAG  
>read1
taGGCGcGGgCggtgTgGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatc
ggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatc  GCaTaTC  
>read3

```
### 3.8 替换序列名：replace
命令replace能够实现对特定序列名的替换。也可以仅对指定序列的提取、改名并输出。

用法：	
```
faops replace [options] <in.fa> <replace.tsv> <out.fa>
```

选项：	
```
-s			仅将存在于列表里面的序列改名后输出到指定位置
-l	INT		输出时序列分行的宽度，默认为80
```
输入：
```
faops replace -l 60 -s in1.fa replace.tsv out.fa 
```
in1.fa文件内容：
```
>read0  
tCGTTTAACCCAAatcAAGGCaatACAggtGggCCGccCatgTcAcAAActcgatGAGtgGgaAaTGgAgTgaAGcaGCAtCtGctgaGCCCCATTctctAgCggaaaATGgtatCGaACcGagataAGtTAAacCgcaaCgGAtaagGgGcgGGctTCAaGtGAaGGaAGaGgGgTTcAaaAgGccCgtcGtCaaTcAaCtAAggcGgaTGtGACactCCCCtAtTtcaaGTCTTctaCccTtGaTaCGaTtcgCGTtcGaGGaGcTACaTTAaccaaGtTaatgCGAGCGcCtgCGaAcTTGccAAgTCaGCtgctCTgttCtcAggTaCAcAaGTcagccAtTGTGTCGaCGCTCT
>read1  
taGGCGcGGgCggtgTgGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatcggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatcGCaTaTC  
>read2  
AtagcAagCtcAgttcaACttCAcCGGTAAaTtcttgTAGtgTcTCgacCgcCcCctTGTACtgtaGG  
>read3
```
replace.tsv文件内容：
```
read1	read2
read2	read1
```
输出：out.fa文件内容：
```
>read2
taGGCGcGGgCggtgTgGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatc
ggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatcGCaTaTC  
>read1
AtagcAagCtcAgttcaACttCAcCGGTAAaTtcttgTAGtgTcTCgacCgcCcCctTGT
ACtgtaGG  
```
### 3.9 过滤特定长度区间的序列：filter
命令filter可以进行长度条件筛选。设置指定的最长和最短通过量，即可完成指定长度区间序列的筛选，并还可以使用参数实现其他一些功能：比如去除序列名重复的序列，简化序列名，过滤掉N含量较高的序列，将IUPAC模糊碱基替换为N，统一转大写，同一序列置于一行等等。
用法：	
```
faops filter [options] <in.fa> <out.fa>	
```
选项：	
```
-a	INT		设置序列大小的下限
-z	INT		设置序列大小的上限
-n	INT		设置N出现次数的上限
-u			去除名称重复的序列，保留第一个
-U			将序列统一转为大写
-b			将同一序列取消换行且合并为一行
-N			将IUPAC模糊碱基用N替换
-s 			将序列名简化
-l	INT     输出时序列分行的宽度，默认为80
```
输入：
```
faops filter -l 60 -a 20 -z 200 -U in2.fa out.fa  
```
in2.fa文件内容:
```
>read4  
ttGGGTTggGcgCccGtgGgTtAGCaAacTTgaATGgtcGtgatgAGCGAaCTGGGtCgGCCCGaCCTtCCcTtAAtTtTACAtCTcAcGttAtgGaGCatTatcggGccggtaTC  
>read5 

>read6  
aActgggCctTtcGgaAtAAAtGATTctActtTGTcTaatCatTcAgggAGagccGCCTTcaATTACGGcGgaaCAtGGgtctGTtCAGaggTgAATTCAAATGCtGagGcaAGatcccCgATTCAcCgGgAaGcCTGtTcgTtCCgtCAtGTGgCtcaatAgcACgCCAaCaGTactggCctcgTCcCatTTGCGC  
ATtCTtatAcGtGCatCagTttTATaacAtTATcCgCGaCcCaGcaAaAAaGcGTgaAgtTgCTaATttaaTcatcAcaGaCCAaGCaCttcTTAAcTtttGAagGAGGattaaaGGGTcacAggAcCTtCgtCtccaccGaCaGATCgAcgCGTGgcCCG 
```
输出：out.fa文件内容：
```
>read4
TTGGGTTGGGCGCCCGTGGGTTAGCAAACTTGAATGGTCGTGATGAGCGAACTGGGTCGG
CCCGACCTTCCCTTAATTTTACATCTCACGTTATGGAGCATTATCGGGCCGGTATC  
```
### 3.10 按序列名称分割序列文件：split-name
命令split-name可以按序列名称对序列数据文件进行切割。

用法：
```
faops split-name [options] <in.fa> <out.dir> 
```
out.dir表示输出文件夹

选项：	
```
-l	INT		输出时序列分行的宽度，默认为80
```
输入： 
```
faops split-name -l 60 in1.fa out.dir
```
in1.fa文件内容如下：
```
>read0  
tCGTTTAACCCAAatcAAGGCaatACAggtGggCCGccCatgTcAcAAActcgatGAGtgGgaAaTGgAgTgaAGcaGCAtCtGctgaGCCCCATTctctAgCggaaaATGgtatCGaACcGagataAGtTAAacCgcaaCgGAtaagGgGcgGGctTCAaGtGAaGGaAGaGgGgTTcAaaAgGccCgtcGtCaaTcAaCtAAggcGgaTGtGACactCCCCtAtTtcaaGTCTTctaCccTtGaTaCGaTtcgCGTtcGaGGaGcTACaTTAaccaaGtTaatgCGAGCGcCtgCGaAcTTGccAAgTCaGCtgctCTgttCtcAggTaCAcAaGTcagccAtTGTGTCGaCGCTCT
>read1  
taGGCGcGGgCggtgTgGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatcggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatcGCaTaTC  
>read2  
AtagcAagCtcAgttcaACttCAcCGGTAAaTtcttgTAGtgTcTCgacCgcCcCctTGTACtgtaGG  
>read3
```
输出文件夹名称为out.dir，文件夹中有四个.fa 文件
```
>read0
tCGTTTAACCCAAatcAAGGCaatACAggtGggCCGccCatgTcAcAAActcgatGAGtg
GgaAaTGgAgTgaAGcaGCAtCtGctgaGCCCCATTctctAgCggaaaATGgtatCGaAC
cGagataAGtTAAacCgcaaCgGAtaagGgGcgGGctTCAaGtGAaGGaAGaGgGgTTcA
aaAgGccCgtcGtCaaTcAaCtAAggcGgaTGtGACactCCCCtAtTtcaaGTCTTctaC
ccTtGaTaCGaTtcgCGTtcGaGGaGcTACaTTAaccaaGtTaatgCGAGCGcCtgCGaA
cTTGccAAgTCaGCtgctCTgttCtcAggTaCAcAaGTcagccAtTGTGTCGaCGCTCT

>read1
taGGCGcGGgCggtgTgGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatc
ggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatcGCaTaTC  

>read2
AtagcAagCtcAgttcaACttCAcCGGTAAaTtcttgTAGtgTcTCgacCgcCcCctTGT
ACtgtaGG  

>read3

```
### 3.11 按byte 大小对序列文件进行切割：split-about
命令split-about可以按byte大小对序列数据文件进行切割。

用法：	
```
faops split-about [options] <in.fa> <approx_size> <out.dir>
```
approx_size 指的是碱基片段的长度.

选项：
```
-e			每个分割文件的分到的序列数量是平均的
-m	INT		最多分割的份数
-l	INT		输出时序列分行的宽度，默认为80
````

in1.fa文件内容为：
```
>read0  
tCGTTTAACCCAAatcAAGGCaatACAggtGggCCGccCatgTcAcAAActcgatGAGtgGgaAaTGgAgTgaAGcaGCAtCtGctgaGCCCCATTctctAgCggaaaATGgtatCGaACcGagataAGtTAAacCgcaaCgGAtaagGgGcgGGctTCAaGtGAaGGaAGaGgGgTTcAaaAgGccCgtcGtCaaTcAaCtAAggcGgaTGtGACactCCCCtAtTtcaaGTCTTctaCccTtGaTaCGaTtcgCGTtcGaGGaGcTACaTTAaccaaGtTaatgCGAGCGcCtgCGaAcTTGccAAgTCaGCtgctCTgttCtcAggTaCAcAaGTcagccAtTGTGTCGaCGCTCT
>read1  
taGGCGcGGgCggtgTgGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatcggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatcGCaTaTC  
>read2  
AtagcAagCtcAgttcaACttCAcCGGTAAaTtcttgTAGtgTcTCgacCgcCcCctTGTACtgtaGG  
>read3
```

输入： 
```
faops split-about -l 60 -e -m 3 in1.fa 25 out.dir
```

输出到out.dir文件夹，有两个.fa文件，000和001。

000文件内容：
```
>read0
tCGTTTAACCCAAatcAAGGCaatACAggtGggCCGccCatgTcAcAAActcgatGAGtg
GgaAaTGgAgTgaAGcaGCAtCtGctgaGCCCCATTctctAgCggaaaATGgtatCGaAC
cGagataAGtTAAacCgcaaCgGAtaagGgGcgGGctTCAaGtGAaGGaAGaGgGgTTcA
aaAgGccCgtcGtCaaTcAaCtAAggcGgaTGtGACactCCCCtAtTtcaaGTCTTctaC
ccTtGaTaCGaTtcgCGTtcGaGGaGcTACaTTAaccaaGtTaatgCGAGCGcCtgCGaA
cTTGccAAgTCaGCtgctCTgttCtcAggTaCAcAaGTcagccAtTGTGTCGaCGCTCT
>read1
taGGCGcGGgCggtgTgGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatc
ggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatcGCaTaTC  
```


001文件内容：
```
>read2
AtagcAagCtcAgttcaACttCAcCGGTAAaTtcttgTAGtgTcTCgacCgcCcCctTGT
ACtgtaGG  
>read3
```
### 3.11 计算N50及其他一些统计值：n50

在计算生物学中，N50是一组contig或scaffold的统计数据。N50类似于长度的平均值或中值，但对于较长的contig具有更重要的意义。命令n50可以计算N50值。并且可以通过参数设置，计算其他统计数据。

用法：	
```
faops n50 [options] <in.fa> [more_files.fa]
```
选项：
```
-H			只返回统计值，没有统计值名
-N	INT		计算Nx统计值，默认值50
-S			同时显示碱基总数
-A			同时显示平均碱基数
-E			计算E值大小
-C			同时显示总序列数
-g	INT		选取基因序列大小，而不是使用全部文件数据
```
输入：
```
faops n50 -S -A -E -C in3.fa
```
In3.fa 文件内容：
```
>read0
aGaC
>read1
ccTa
>read2
cTGGG
>read3
Ttaact
>read4
cgatctt
>read5
AGctGCGt
```
输出:
```
N50     6
S       34
A       5.67
E       6.06
C       6
```
### 3.13序列信息标准化：dazz

命令dazz可以对序列信息命名进行新的标准化，为下游组装分析做准备。重复名称的序列仅保留第一个。

用法：
```
faops dazz [options] <in.fa> <out.fa>
```
选项：	
```
-p	STR	  设置新的序列名前缀，默认为read
-s	INT	  起始序号，默认为1
-a		  不删除重复序列
-l	INT	  输出时序列分行的宽度，默认为80
```
输入：
```
faops dazz -l 60 -p test in1.fa out.fa 
```
in1.fa文件内容:
```
>read0  
tCGTTTAACCCAAatcAAGGCaatACAggtGggCCGccCatgTcAcAAActcgatGAGtgGgaAaTGgAgTgaAGcaGCAtCtGctgaGCCCCATTctctAgCggaaaATGgtatCGaACcGagataAGtTAAacCgcaaCgGAtaagGgGcgGGctTCAaGtGAaGGaAGaGgGgTTcAaaAgGccCgtcGtCaaTcAaCtAAggcGgaTGtGACactCCCCtAtTtcaaGTCTTctaCccTtGaTaCGaTtcgCGTtcGaGGaGcTACaTTAaccaaGtTaatgCGAGCGcCtgCGaAcTTGccAAgTCaGCtgctCTgttCtcAggTaCAcAaGTcagccAtTGTGTCGaCGCTCT
>read1  
taGGCGcGGgCggtgTgGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatcggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatcGCaTaTC  
>read2  
AtagcAagCtcAgttcaACttCAcCGGTAAaTtcttgTAGtgTcTCgacCgcCcCctTGTACtgtaGG  
>read3

```
输出：
```
>test/1/0_359
tCGTTTAACCCAAatcAAGGCaatACAggtGggCCGccCatgTcAcAAActcgatGAGtg
GgaAaTGgAgTgaAGcaGCAtCtGctgaGCCCCATTctctAgCggaaaATGgtatCGaAC
cGagataAGtTAAacCgcaaCgGAtaagGgGcgGGctTCAaGtGAaGGaAGaGgGgTTcA
aaAgGccCgtcGtCaaTcAaCtAAggcGgaTGtGACactCCCCtAtTtcaaGTCTTctaC
ccTtGaTaCGaTtcgCGTtcGaGGaGcTACaTTAaccaaGtTaatgCGAGCGcCtgCGaA
cTTGccAAgTCaGCtgctCTgttCtcAggTaCAcAaGTcagccAtTGTGTCGaCGCTCT
>test/2/0_108
taGGCGcGGgCggtgTgGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatc
ggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatcGCaTaTC  
>test/3/0_70
AtagcAagCtcAgttcaACttCAcCGGTAAaTtcttgTAGtgTcTCgacCgcCcCctTGT
ACtgtaGG  
>test/4/0_0
```
### 3.14 合并双端测序文件：interleave
命令interleave可以将双端测序的两个文件交错合并。可以只输入一个文件，此时另一端会以N为序列内容。

用法：	
```
faops interleave [options] <R1.fa> [R2.fa]
```

选项：	
```
-p	STR	  设置新的序列名前缀，默认为read
-s	INT   起始序号，默认为0
-l	INT	  输出时序列分行的宽度，默认为0
```
输入：
```
faops interleave -p test in2.fa in4.fa
```
In2.fa文件内容:
```
>read4  
ttGGGTTggGcgCccGtgGgTtAGCaAacTTgaATGgtcGtgatgAGCGAaCTGGGtCgGCCCGaCCTtCCcTtAAtTtTACAtCTcAcGttAtgGaGCatTatcggGccggtaTC  
>read5 

>read6  
aActgggCctTtcGgaAtAAAtGATTctActtTGTcTaatCatTcAgggAGagccGCCTTcaATTACGGcGgaaCAtGGgtctGTtCAGaggTgAATTCAAATGCtGagGcaAGatcccCgATTCAcCgGgAaGcCTGtTcgTtCCgtCAtGTGgCtcaatAgcACgCCAaCaGTactggCctcgTCcCatTTGCGC  
ATtCTtatAcGtGCatCagTttTATaacAtTATcCgCGaCcCaGcaAaAAaGcGTgaAgtTgCTaATttaaTcatcAcaGaCCAaGCaCttcTTAAcTtttGAagGAGGattaaaGGGTcacAggAcCTtCgtCtccaccGaCaGATCgAcgCGTGgcCCG 
```
In4.fa文件内容:
```
>read0
cTTGccAAgTCaGCtgctCTgttCtcAggTaCAcAaGTcagccAtTGTGTCGaCGCTCTatg
>read1
ggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatcGCaTaTCgct
>read2
GTatcccAGCgCtAcAcaAGAcTaCAtCTggCatTAGacttc
```

输出：
```
>test0/1
ttGGGTTggGcgCccGtgGgTtAGCaAacTTgaATGgtcGtgatgAGCGAaCTGGGtCgGCCCGaCCTtCCcTtAAtTtTACAtCTcAcGttAtgGaGCatTatcggGccggtaTC
>test0/2
cTTGccAAgTCaGCtgctCTgttCtcAggTaCAcAaGTcagccAtTGTGTCGaCGCTCTatg
>test1/1

>test1/2
ggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatcGCaTaTCgct
>test2/1
aActgggCctTtcGgaAtAAAtGATTctActtTGTcTaatCatTcAgggAGagccGCCTTcaATTACGGcGgaaCAtGGgtctGTtCAGaggTgAATTCAAATGCtGagGcaAGatcccCgATTCAcCgGgAaGcCTGtTcgTtCCgtCAtGTGgCtcaatAgcACgCCAaCaGTactggCctcgTCcCatTTGCGC  ATtCTtatAcGtGCatCagTttTATaacAtTATcCgCGaCcCaGcaAaAAaGcGTgaAgtTgCTaATttaaTcatcAcaGaCCAaGCaCttcTTAAcTtttGAagGAGGattaaaGGGTcacAggAcCTtCgtCtccaccGaCaGATCgAcgCGTGgcCCG
>test2/2
GTatcccAGCgCtAcAcaAGAcTaCAtCTggCatTAGacttc
```
### 3.15 摘取序列中指定位置的片段：region
命令region可以在一个FASTA文件中摘取一个或多个指定的序列片段。本功能可以使用参数标注摘取的正反方向。

用法：	
```
faops region [options] <in.fa> <region.txt> <out.fa>
```
选项：	
```
-s			在序列名中标注提取方向
-l	INT		输出时序列分行的宽度，默认为80
```
输入：
```
faops region -s -l 60 in1.fa region.txt out.fa
```
region.txt文件内容:
```
read0:35-50,25-120
read1:10-30
```
输出：out.fa文件内容：
```
>read0(+):35-50
CGccCatgTcAcAAAc
>read0(+):25-120
ACAggtGggCCGccCatgTcAcAAActcgatGAGtg
GgaAaTGgAgTgaAGcaGCAtCtGctgaGCCCCATTctctAgCggaaaATGgtatCGaAC
>read1(+):10-30
gCggtgTgGATTAaggCAGag
```
### **总结**
Faops具有功能集约化的特点，仅需一款软件即可对基因序列数据进行多方面的分析。Faops软件能帮助我们准确分析基因组序列，为我们的研究带来便捷。

本工具由南京大学生科院王强老师wang-q@outlook.com 开发，更多内容请参考: https://github.com/wang-q/faops
这是一个开源的软件,任何人可以进行版本修改或再开发.












































