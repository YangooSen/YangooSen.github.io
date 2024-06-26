---
title: biopythonSeq
katex: true
date: 2024-06-25 09:42:09
excerpt: biopython中的序列处理
tag: bioinfo
---

# Biopython 基本使用

Biopython 是 Python 在 生信方面应用的第三方包，开源在 [github](https://github.com/biopython/biopython)，使用的是 `Biopython License Agreement`，非常开放而基本没有任何使用限制

![logo](head.png)

## 安装
```bash
# 创建 conda 环境
conda create --name biopy python==3.8

#激活环境
conda activate biopy

#安装
conda install biopython==1.77
```

下面测试安装是否成功

```python
import Bio

print(Bio.__version__)

# 这里输出 1.77 版本
```
> 这里指定安装 1.77 版本，其他版本涉及到模块的增删，不方便统一讲解

## 基本组成

Biopython 有多个模块构成，下面简单介绍常用的几个子包，这里的"常用"是主观而言，不可能覆盖到所有读者的使用情况，如果有其他这里没有涉及到的模块可以参见[官方文档](https://biopython.org/wiki/Documentation)

### Alphabet

生信中有很多字母表示，比如 DNA 中的 ACTG 核苷酸，20种氨基酸的缩写表示，每种都有一个字母代替。然而有些字母存在歧义，例如字母 S 可能表示 C 或者 G，而 H 可能表示 A 或者 C 或者 T

> 在生物信息学中，字母 S 既可以表示核苷酸 C，也可以表示核苷酸 G，这是由于国际核酸序列符号（IUPAC）为核酸序列设定的简化表示法。
>
> IUPAC代码用于表示核酸序列中的模糊性，即某个位置上可能有多种核苷酸。具体来说：
> - A：腺嘌呤（Adenine）
> - C：胞嘧啶（Cytosine）
> - G：鸟嘌呤（Guanine）
> - T：胸腺嘧啶（Thymine）
> - U：尿嘧啶（Uracil）
> - R：嘌呤（A 或 G）
> - Y：嘧啶（C 或 T）
> - S：强配对（C 或 G），指 Strong，它们之间形成的氢键比 A 和 T 或 A 和 U 之间的氢键多
> - W：弱配对（A 或 T），指 weak
> - K：氢键较弱的配对（G 或 T）
> - M：氨基（A 或 C）
> - B：不是腺嘌呤（C 或 G 或 T）
> - D：不是胞嘧啶（A 或 G 或 T）
> - H：不是鸟嘌呤（A 或 C 或 T）
> - V：不是胸腺嘧啶（A 或 C 或 G）
> - N：任何核苷酸（A 或 C 或 G 或 T）

在 Biopython 中这些歧义词表被称为 `ambiguous_dna`
```python
import Bio.Alphabet
from Bio.Alphabet import IUPAC

print(IUPAC.ambiguous_dna.letters)
print(IUPAC.unambiguous_dna.letters)

#GATCRYWSMKHBVDN
#GATC

```

另外实际情况中可能会出现不常见到的氨基酸和核苷酸，为此有 `ExtendedIUPACProteain`和`ExtendIUPACDNA`。针对二级结构还有`SecondaryStructure`词表，表示 **H**elix, **T**urn, **S**trand, **C**oil

```python
import Bio.Alphabet
from Bio.Alphabet import IUPAC

print(Bio.Alphabet.ThreeLetterProtein.letters)
print(IUPAC.IUPACProtein.letters)
print(IUPAC.ExtendedIUPACDNA.letters)
print(IUPAC.ExtendedIUPACProtein.letters)

#['Ala', 'Asx', 'Cys', 'Asp', 'Glu', 'Phe', 'Gly', 'His', 'Ile', 'Lys', 'Leu', 'Met', 'Asn', 'Pro', 'Gln', 'Arg', 'Ser', 'Thr', 'Sec', 'Val', 'Trp', 'Xaa', 'Tyr', 'Glx']
#ACDEFGHIKLMNPQRSTVWY
#GATCBDSW
#ACDEFGHIKLMNPQRSTVWYBXZJUO

```
Alphabets 被用于定义序列组成，需要配合 Seq 一起使用



### Seq

利用 Seq 可以解析字符串为任何序列，例如
```python
import Bio.Alphabet                                                                                            
from Bio.Alphabet import IUPAC
from Bio.Seq import Seq                                                                                                               
seq=Seq("CCGGGTT",IUPAC.unambiguous_dna) #识别为DNA序列                                                        
print(seq.transcribe()) #转录结果  CCGGGUU
print(seq.translate()) #翻译结果   PG

```
将字符串识别为DNA序列，即可使用作为DNA的一系列操作，例如转录和翻译等方法，一个合理的序列既可以被识别为DNA，也可以被识别为RNA。然而一旦识别为RNA后就只能使用作为RNA的方法，例如只能翻译而不能转录了，但是可以使用逆转录方法

```python
import Bio.Alphabet
from Bio.Alphabet import IUPAC
from Bio.Seq import Seq


seq=Seq("CCGGGUU",IUPAC.unambiguous_rna) #识别为RNA序列
#print(seq.transcribe()) #error
print(seq.translate()) #翻译结果 PG
print(seq.back_transcribe()) #逆转录结果 CCGGGT

```
通过对象调用对应的方法，也可以直接利用类中提供的函数

```python
import Bio.Alphabet
from Bio.Alphabet import IUPAC
from Bio.Seq import translate,transcribe,back_transcribe

dnaseq="ATGGTATAA"
print(translate(dnaseq)) #翻译结果 MV*
print(transcribe(dnaseq)) #转录结果 AUGGUAUAA

rnaseq=transcribe(dnaseq)
print(translate(rnaseq)) #翻译结果 MV*
print(back_transcribe(rnaseq)) #逆转录结果 ATGGTATAA

```

> 要注意Biopython中的transcribe函数只是简单地将核苷酸字母替换为它的互补核苷酸字母，函数已经假定输入的链一定是非模板链

函数和对象调用方法不同，函数输出的仅仅是字符串，而不是序列对象

Seq对象也有类似string一样的方法，比如切片等

```python
from Bio.Seq import Seq
import Bio.Alphabet
from Bio.Alphabet import IUPAC

seq=Seq("CCGGGTTAACGTA",IUPAC.unambiguous_dna)

print(seq[:5])
print(len(seq))
print(seq)

"""
CCGGG
13
CCGGGTTAACGTA
"""

```


### MutableSeq

Seq对象设定为不可修改的，例如

```python
seq[0]="T"
```

将会报错，而通过定义为MutableSeq则可转变为可修改对象

```python
from Bio.Seq import Seq
import Bio.Alphabet
from Bio.Alphabet import IUPAC

seq=Seq("CCGGGTTAACGTA",IUPAC.unambiguous_dna)

mut=seq.tomutable()
print(mut)

#转变为Muteable对象后即可修改内容
mut[0]='T'

print(mut)

"""
CCGGGTTAACGTA
TCGGGTTAACGTA
"""

```

可以简单将Muteable的序列对象理解为一个列表，它可以使用append，insert，pop，remove方法等，也有一些 DNA 序列特定的方法

```python
from Bio.Seq import Seq
import Bio.Alphabet
from Bio.Alphabet import IUPAC

seq=Seq("CCGGGTTAACGTA",IUPAC.unambiguous_dna)

mut=seq.tomutable()

mut.reverse()  #翻转
print(mut)

mut.complement() #互补链
print(mut)

mut.reverse_complement() #翻转互补链
print(mut)

"""
ATGCAATTGGGCC
TACGTTAACCCGG
CCGGGTTAACGTA

"""

```

### SeqRecord

Seq是Biopython中的重要的一个类，但是有时生信处理中需要的不仅是一个序列，还需要它的名字，id，描述等信息，有时也需要和外部数据库做链接。Biopython中提供了SeqRecord类来做这件事，SeqRecord即可以理解为带有元数据的Seq对象

```python
from Bio.Seq import Seq
import Bio.Alphabet
from Bio.Alphabet import IUPAC
from Bio.SeqRecord import SeqRecord


seq=Seq("CCGGGTTAACGTA",IUPAC.unambiguous_dna)
record=SeqRecord(seq,id="0001",name="MHC gene")

print(record)

"""
ID: 0001
Name: MHC gene
Description: <unknown description>
Number of features: 0
Seq('CCGGGTTAACGTA', IUPACUnambiguousDNA())
"""

```
SeqRecord 有两个主要的属性

- id，序列的唯一标识符
- seq，真正的序列对象

另外还有一些附属属性

- name，序列的名字
- description，针对序列的详细表述
- dbxrefs，字符串的列表，表示的是序列在数据库中的id
- features，SeqFeature对象的列表，表示了在`Genebank`记录中找到的相关特征，特别是用SeqIO来读取时十分常用
- annotations，针对序列的后续更多信息，不能在此对象初始化的时候指定这个属性

```python
from Bio.Seq import Seq
import Bio.Alphabet
from Bio.Alphabet import generic_protein
from Bio.SeqRecord import SeqRecord



record=SeqRecord(
        Seq("mdstnvrsgmksrkkkpkttvidddddcmtcsacqsklvkisditkvsldyintmrgntlacaacgsslkll",generic_protein),
        id="P20994.1",
        name="p20994",
        description="Protein A19",
        dbxrefs=["Pfam:PF05077","DIP:2186N"])

record.annotations["note"]="a simple note"

print(record)

"""
ID: P20994.1
Name: p20994
Description: Protein A19
Database cross-references: Pfam:PF05077, DIP:2186N
Number of features: 0
/note=a simple note
Seq('mdstnvrsgmksrkkkpkttvidddddcmtcsacqsklvkisditkvsldyint...kll', ProteinAlphabet())

"""

```


