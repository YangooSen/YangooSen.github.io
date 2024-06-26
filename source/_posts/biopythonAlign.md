---
title: biopythonAlign
katex: true
date: 2024-06-26 11:10:24
excerpt: biopython中的序列比对
tag: bioinfo
---

### Align

Align 模块是 biopython 中处理序列比对的重要模块，其中关键的类是 MultipleSeqAlignment，这个类的对象存储了**已经比对好**的序列

```python
from Bio.Alphabet import generic_protein
from Bio.Align import MultipleSeqAlignment
from Bio.Seq import Seq
from Bio.SeqRecord import SeqRecord

seq1='MHQAIFYDNW'
seq2='MH--IFY-NW'

seqrec1=SeqRecord(Seq(seq1,generic_protein),id='asp')                                                seqrec2=SeqRecord(Seq(seq2,generic_protein),id='unk')

align=MultipleSeqAlignment([seqrec1,seqrec2])

print(align)

"""
ProteinAlphabet() alignment with 2 rows and 10 columns
MHQAIFYDNW asp
MH--IFY-NW unk
"""

```

MultipleSeqAlignment 可以以Seq或SeqRecord的列表初始化，也支持通过 append 增加新序列

```python
seq3='-HQAIFYDN-'
seqrec3=SeqRecord(Seq(seq3,generic_protein),id='cas')

align.append(seqrec3)

print(align)

"""
ProteinAlphabet() alignment with 3 rows and 10 columns
MHQAIFYDNW asp
MH--IFY-NW unk
-HQAIFYDN- cas
"""
```

可以将 MultipleSeqAlignment 看做一个 DataFrame，支持索引和切片，还有计数等操作

```python

print(align[0])

print(align[:2,5:])

print(len(align))

for seq in align:
    print(seq)

"""
ID: asp
Name: <unknown name>
Description: <unknown description>
Number of features: 0
Seq('MHQAIFYDNW', ProteinAlphabet())
ProteinAlphabet() alignment with 2 rows and 5 columns
FYDNW asp
FY-NW unk
3
ID: asp
Name: <unknown name>
Description: <unknown description>
Number of features: 0
Seq('MHQAIFYDNW', ProteinAlphabet())
ID: unk
Name: <unknown name>
Description: <unknown description>
Number of features: 0
Seq('MH--IFY-NW', ProteinAlphabet())
ID: cas
Name: <unknown name>
Description: <unknown description>
Number of features: 0
Seq('-HQAIFYDN-', ProteinAlphabet())
"""

```

### AlignIO

读取序列比对文件的时候可用此类的方法，需要输入文件和比对格式，支持的比对格式有

- clustal
- emboss
- fasta
- fasta-m10
- ig
- maf
- nexus
- phylip
- phylip-sequential
- phylip-relaxed
- stockholm

只有一个比对时可用 read 函数，返回 MultipleSeqAlignment 对象；多于一个比对时可用 parse 函数，返回一个迭代器


```python
from Bio import AlignIO
align=AlignIO.read('foo.fasta','fasta')

for alignment in AlignIO.parse('foo.aln','clustal'):
    print(len(alignment))

```

写入磁盘可用 write 函数，比上面的两个函数多需要一个存储的内容，返回成功写入的个数

```python

AlignIO.write(align,'foo.phy','phylip')
```

转换比对格式可用 convert 函数，返回成功写入的个数

```python
AlignIO.convert('start.fasta','fasta','end.aln','clustal')
```

### AlignInfo

AlignInfo 用于提取序列比对的相关信息，它提供了 print_info_content 函数和 SummaryInfo，PSSM 两个类


```python
from Bio import AlignIO
from Bio.Align import AlignInfo

# 读取序列比对
alignment = AlignIO.read("example.aln", "clustal")

# 创建 SummaryInfo 对象
summary_align = AlignInfo.SummaryInfo(alignment)

# 生成一致性序列
consensus = summary_align.dumb_consensus()
print(f"Consensus sequence: {consensus}")

# 计算信息内容
info_content = summary_align.information_content()
print(f"Information content: {info_content}")

# 生成位置特定评分矩阵
pssm = summary_align.pos_specific_score_matrix()
print(f"PSSM: {pssm}")

```

### ClustalW

与 ClustalW 交互的程序接口，需要下载比对程序


```python
from Bio.Align.Applications import ClustalwCommandline
from Bio import AlignIO

# 定义 ClustalW 可执行文件的路径
clustalw_exe = "/path/to/clustalw"

# 定义输入和输出文件
in_file = "example.fasta"
out_file = "example.aln"

# 设置 ClustalW 命令行参数
clustalw_cline = ClustalwCommandline(clustalw_exe, infile=in_file, outfile=out_file)

# 运行 ClustalW
stdout, stderr = clustalw_cline()

# 读取比对结果
alignment = AlignIO.read(out_file, "clustal")
print(alignment)


```
