---
title: biopythonIO
katex: true
date: 2024-06-27 08:58:58
excerpt: biopython中的io处理
tag: bioinfo
---

### SeqIO

SeqIO 是 Biopython 中的统一IO处理模块，既可以读取序列文件，也可以读取序列比对文件（识别为多个 SeqRecord 对象）

读取序列支持的格式包括

- ace
- clustal
- embl
- emboss
- fasta
- fsta-m10
- genbank
- ig
- nexus
- phd
- phylip
- stockholm
- swiss
- tab

读取多个序列对象用 parse，单个对象用 read

```python
from Bio import SeqIO

# 读取 FASTA 文件
for record in SeqIO.parse("example.fasta", "fasta"):
    print(record.id)
        print(record.seq)


```

写入文件用 write，第一个参数是一个 iterable

```python
from Bio import SeqIO
from Bio.Seq import Seq
from Bio.SeqRecord import SeqRecord

# 创建序列记录
record = SeqRecord(Seq("AGTACACTGGT"), id="example", description="An example sequence")
records = [record]

# 写入 FASTA 文件
SeqIO.write(records, "output.fasta", "fasta")


```

### BLAST


BLAST（Basic Local Alignment Search Tool）是用于比较生物序列的强大工具，用于在数据库中寻找与查询序列相似的序列。BLAST 有多个版本和变体，每个版本和变体都有特定的用途和优化目标。这是最常用和最广泛使用的 BLAST 版本，由美国国家生物技术信息中心（NCBI）开发和维护。变体包括：

- blastn：用于核酸序列（DNA/RNA）的比对。
- blastp：用于蛋白质序列的比对。
- blastx：将核酸序列翻译成蛋白质，然后与蛋白质数据库进行比对。
- tblastn：将蛋白质序列与核酸序列数据库进行比对，核酸序列在比对过程中被翻译成蛋白质。
- tblastx：将核酸序列翻译成蛋白质，然后与另一组翻译后的核酸序列进行比对。

> 这里的后缀指的应该是
> - n nucleotide 核苷酸
> - p protein 蛋白质
> - x 混合
> - t translate 表示先翻译再比对

在 NCBI 的网络服务器上能够运行小型 BLAST 任务，但比较大的任务就必须在本地机器上跑，这样灵活性更好，比如能输入更大的数据，并且在自己的数据库上进行搜索

Bio.Blast.Applications 模块主要用于构建和执行各种 BLAST 程序的命令行调用。这些程序包括 

- blastn
- blastp
- blastx
- tblastn
- tblastx

分别用于不同类型的序列比对，对应的程序分别是 Ncbi{tblastn}Commandline，只需要替换中间的部分

```python
from Bio.Blast.Applications import NcbiblastnCommandline

# 创建 blastn 命令行对象
blastn_cline = NcbiblastnCommandline(cmd=BLAST_EXE, query="query.fasta", db="nt", evalue=0.001, outfmt=5, out="output.xml")

# 执行 blastn
stdout, stderr = blastn_cline()

# 检查标准输出和错误输出
print(stdout)
print(stderr)


```
输出保存在 stdout 和 output.xml 中

> 以下是 NcbiblastnCommandline 中常用参数的意义：
> - query: 输入的查询序列文件（FASTA 格式）。
> - db: 要比对的数据库。
> - evalue: 期望值阈值，用于过滤结果。通常设置为一个较小的值（如 0.001）。
> - outfmt: 输出格式。常见的格式有：
> - 0 - Pairwise
> - 5 - XML（适用于 NCBIXML 解析）
> - 6 - 表格（TAB）
> - out: 输出文件名。
> - task: 指定 blastn 的任务类型，如：
> - blastn - 标准 blastn
> - blastn-short - 用于比对短序列
> - dc-megablast - 用于比较远缘的核酸序列
> - word_size: 词长（默认为 11）。较大的词长会加快速度但可能降低敏感性。
> - gapopen: 开启一个 gap（缺口）的罚分。
> - gapextend: 延伸一个 gap 的罚分。
> - penalty: 错误匹配的罚分。
> - reward: 正确匹配的奖励分。
> - num_threads: 使用的线程数，默认为 1。增加线程数可以加快比对速度。


输出文件是 xml 格式，Biopython 内置了 NCBIXML 类来针对读取 BLAST 输出文件

```python
from Bio.Blast import NCBIXML

# 打开 BLAST 输出文件
with open("output.xml") as result_handle:
    blast_records = NCBIXML.parse(result_handle)

    # 迭代解析 BLAST 记录
    for blast_record in blast_records:
        print(f"Query: {blast_record.query}")

        for alignment in blast_record.alignments:
            for hsp in alignment.hsps:
                if hsp.expect < 0.001:  # 根据期望值筛选
                    print(f"****Alignment****")
                    print(f"sequence: {alignment.title}")
                    print(f"length: {alignment.length}")
                    print(f"E-value: {hsp.expect}")
                    print(f"Score: {hsp.score}")
                    print(f"Identities: {hsp.identities}")
                    print(f"Gaps: {hsp.gaps}")
                    print(f"Query: {hsp.query[0:75]}...")
                    print(f"Match: {hsp.match[0:75]}...")
                    print(f"Subject: {hsp.sbjct[0:75]}...")


```
输出信息中主要包括两部分，第一部分是此次任务的元数据，比如所用数据库，程序版本等，第二部分则是比对结果

### 生物学数据

Biopython 中还包括了很多生物信息学中常用的数据和工具，下面介绍常用的模块

#### CodonTable

Bio.Data.CodonTable 模块包含遗传密码表（codon tables），这些表将 DNA/RNA 三联体编码子（codons）映射到相应的氨基酸。Biopython 提供了多种遗传密码表，以适应不同的生物系统和研究需求。

```python
from Bio.Data import CodonTable

standard_table = CodonTable.unambiguous_dna_by_id[1]  # 标准遗传密码表
print(standard_table.forward_table)  # 查看 DNA 编码子到氨基酸的映射

```

#### IUPAC Data

Bio.Data.IUPACData 提供了 IUPAC 编码的核酸和氨基酸字母表。这些编码表有助于标准化和统一表示生物分子的序列。

```python
from Bio.Data import IUPACData

print(IUPACData.ambiguous_dna_values)  # 查看模糊 DNA 字母和它们可能代表的碱基

```

#### SCOP Data

Bio.Data.SCOPData 包含与 SCOP（Structural Classification of Proteins）数据库相关的信息。SCOP 是一个对已知蛋白质结构进行分级和分类的数据库。

#### Dictionaries

Bio.Data 模块还包括其他常量和字典，用于生物信息学的各种计算和分析。例如，氨基酸的三字母和单字母代码，蛋白质和 DNA 的物理化学性质等。

```python
from Bio.Data import CodonTable, IUPACData

print(CodonTable.unambiguous_dna_by_id[1].forward_table)  # 标准遗传密码表的前向表
print(IUPACData.protein_letters)  # 氨基酸的单字母代码


```




































