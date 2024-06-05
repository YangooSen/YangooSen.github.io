---
title: py2neo
katex: true
date: 2024-04-08 15:32:42
excerpt: py2neo的api使用和注意点
tag: GNN
---

> 操作neo4j的数据有两种形式：可以传入cypher语句，然后用graph.run执行，返回游标；另一种方式是用py2neo内置的类和方法来操作，下面主要介绍第二种方法
> 另外需要注意的是，图数据库是能够帮我们尽量快速地找出不同节点的关系，因此向一个节点或者关系中插入很多无关的数据是完全没有必要的，会浪费很多硬盘资源，检索也会消耗更多时间。合理的做法是只把与图有关的数据存入图数据库，其他无关的属性信息可以存在其他地方，减少检索和存储开支

```bash
neo4j start
connect:
bolt://localhost:7687
user:neo4j
pwd:Wwxx!

ip:wlp5s0
```


# Graph

[Graph](https://neo4j-contrib.github.io/py2neo/workflow.html#graph-objects)是具体的某个数据库，是本地类与服务器数据库交互的接口
```python
graph = Graph('http://localhost:7474',auth=('neo4j',password), name = 'neo4j')
```
如果name指定的数据库不存在会报错，初始只有neo4j和system，可以在`neo4j start`之后在[本地](http://localhost:7474/browser/)查看

## schema
[schema](https://neo4j-contrib.github.io/py2neo/workflow.html#py2neo.Graph.schema)返回[数据库模式对象](https://neo4j-contrib.github.io/py2neo/workflow.html#py2neo.Graph.schema)，具体作用看下面介绍

## create
[create](https://neo4j-contrib.github.io/py2neo/workflow.html#py2neo.Graph.create)用于添加节点和关系，创建Node类和Relationship类之后可以传入
```python
graph.create(subgraph)
```
> 添加关系的时候先查找一下是否已经存在这个节点，如果已经存在的话，建立关系的时候需要用已经存在于数据库中的节点，而不要新建，否则会出现新节点
## push
[push](https://neo4j-contrib.github.io/py2neo/workflow.html#py2neo.Graph.push)用于修改节点和边的标签和属性，修改Node或Relationship的实例之后可以push来修改已有的节点和边
```python
graph.push(subgraph)
```

## delete
[delete](https://neo4j-contrib.github.io/py2neo/workflow.html#py2neo.Transaction.delete)用于删除节点和关系，**删除节点时会把与节点有关的边也删除，删除关系会把边和头尾节点都删除**
```python
graph.delete(subgraph)
```
> 这里要注意删除边的时候会把端节点也删除，如果需要只删除边可以用[separate](https://neo4j-contrib.github.io/py2neo/workflow.html#py2neo.Transaction.separate)，会留下孤立节点


# Node
[Node](https://neo4j-contrib.github.io/py2neo/data/index.html#node-objects)是本地的节点类，只有被`graph.create`或`graph.merge`之后才会存入数据库
```python
node = Node(label,k1=v1,k2=v2,k3=v3)
```
> 注意label的字符串最好不要带空格，只用一个完整的单词，不然有些地方可能会出问题，比如修改展示的style.grass可能会识别不了
> 另外修改节点和边在网站展示的style，可以看[这里](https://zhuanlan.zhihu.com/p/439199149)
## identity
[identity](https://neo4j-contrib.github.io/py2neo/data/index.html#py2neo.data.Node.identity)返回数据库中已有Node的id，如果不在数据库中会返回None
> 重载的==只会根据id来判断相等，如果还没有存入数据库的节点用==，它只会与自己相等，其他逗号返回false

## labels
[labels](https://neo4j-contrib.github.io/py2neo/data/index.html#py2neo.data.Node.labels)返回节点的所有label，可以用`add_label`和`remove_label`操作label

## dict
[dict(node)](https://neo4j-contrib.github.io/py2neo/data/index.html#py2neo.data.Node.items)返回节点的属性字典，节点属性的相关方法和dict的方法基本一致，删除节点的属性用`del node[key]`

# Relationship

[Relationship](https://neo4j-contrib.github.io/py2neo/data/index.html#py2neo.data.Node.items) 创建边，如果没有指定边的类型会默认为`T0`，可以继承类修改此默认设置，详见官方文档
```python
rel=Relationship(head,r_type,tail,k1=v1,k2=v2,k3=v3)
```
> 如果建立关系的时候，起始节点或者结束节点不存在，则在建立关系的同时建立这个节点
## identity
[identity](https://neo4j-contrib.github.io/py2neo/data/index.html#py2neo.data.Relationship.identity)和节点的`identity`类似，返回已存在数据库中边的id，边相等判断是头尾节点id相等和边类型r_type相等

## dict
[dict(rel)](https://neo4j-contrib.github.io/py2neo/data/index.html#py2neo.data.Relationship.items)和节点的方法类似

# NodeMatcher
[NodeMatcher](https://neo4j-contrib.github.io/py2neo/matching.html)在指定数据库中匹配符合条件的节点
```python
matcher=NodeMatcher(graph)
node_match=matcher.match(label,k1=v1,k2=v2)
```
返回[NodeMatch](https://neo4j-contrib.github.io/py2neo/matching.html#nodematch-objects)对象，可以用循环迭代，也可以用`len(NodeMatch)`计数，用`NodeMatch.all()`获得Node的列表
> 如果匹配规则较复杂，可以先拿到所有节点，再用for循环筛选
> ```python
> nodes=NodeMatcher(graph).match()
> for node in nodes:
>     #filter
>  ```
# RelationshipMatcher
[RelationshipMatcher](https://neo4j-contrib.github.io/py2neo/matching.html#relationshipmatcher-objects)和NodeMatcher类似，可以用边类型和属性匹配，也可以指定待查找边的端节点,[传入node的set表示node可以指代头节点或尾节点](https://neo4j-contrib.github.io/py2neo/matching.html#relationshipmatcher-objects)
```python
matcher=RelationshipMatcher(graph)
rel_match=relation_matcher.match((node,),r_type,k1=v1,k2=v2)
```
> 匹配规则较复杂也可以先拿到所有边再用for循环筛选

# Schema
[Schema](https://neo4j-contrib.github.io/py2neo/workflow.html#schema-objects)是图数据库的模式，可以得到数据库中已有的节点标签种类和边种类
```python
schema=graph.schema
node_labels=schema.node_labels
rel_types=schema.relation_types
```
## create_index
[create_index](https://neo4j-contrib.github.io/py2neo/workflow.html#py2neo.Schema.create_index)为标签是label的节点在属性k1和k2上创建索引，`drop_index`删除索引
```python
schema.create_index(label,k1,k2)
```
> 最好在插入数据之前就建立好索引，否则索引的建立会很耗时间。索引可以大幅度降低大规模数据的查询速度
> 用sechma的相关方法创建或查看index的时候总会报错，最终选择直接运行cypher语句来创建节点索引，关于图数据的索引看[这里](https://neo4j.com/docs/cypher-manual/current/indexes/)
> ```python
> graph.run(CREATE INDEX index_name FOR (n:label) ON (n.k1,n.k2))
> ```

# example
下面的代码是将本地的[DRKG](https://github.com/gnn4dr/DRKG?spm=wolai.workspace.0.0.6880767brB3zhW)三元组数据集插入到neo4j图数据库中
```python
from ctypes.wintypes import PRECT
import pandas as pd
from py2neo import Graph,Node,Relationship,NodeMatcher
import numpy as np



def printCols(df):
    data=dict(df.iloc[:5,:])
    for k,v in data.items():
        print(k)
        print(v)
        print("="*10)


# 20:48 -> 15:03 插入97000+个节点，920000条边
class DRKGData:
    def __init__(self) -> None:
        path='./drkg.tsv'
        columns=['head','relation','tail']
        self.df=self.getData(path,columns)
        #self.df=self.df.iloc[:10000,:]

        self.Graph=Graph('http://localhost:7474',auth=(account,password), name = 'neo4j')
        #print(self.Graph.schema.node_labels)
        #self.Graph.run("create index for (n:Gene) on (n.id)")
        #print(self.Graph.schema.get_indexes("Gene"))
        #self.Graph.schema.create_index("Gene","id")


        self.delete_old()
        print("delete old data done")
        self.name2id=self.getName2id()
        print("insert node done")
        #print(self.name2entity)
        self.insert()
        print("done")



    def getName2id(self):

        nodeid=0
        name2id=dict()

        heads=set(self.df.iloc[:,0])
        tails=set(self.df.iloc[:,2])
        nodes=heads|tails

        for node in nodes:
            #注意name中最好不要有空格，不然结果中颜色会出问题
            entity=Node(node.split("::")[0],id=nodeid,name=node)
            #这里已经插入了节点
            self.Graph.create(entity)
            name2id[node]=nodeid
            nodeid+=1


        return name2id
    def delete_old(self):
        self.Graph.delete_all()

    def insert(self):
        #插入边
        matcher=NodeMatcher(self.Graph)

        for index,row in self.df.iterrows():
            headid=self.name2id[row['head']]
            tailid=self.name2id[row['tail']]

            head=matcher.match(id=headid).first()
            tail=matcher.match(id=tailid).first()
            rel=Relationship(head,row["relation"],tail)

            self.Graph.create(rel)


    def getData(self,path,columns):
        df=pd.read_csv(path,sep='\t')
        df.columns=columns
        return df



data=DRKGData()

```


