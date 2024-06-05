---
title: OGB
katex: true
date: 2024-04-28 09:41:06
excerpt: 介绍OGB的使用情况和各个部分的代码解析
tag: GNN
---

# 基本使用
[Open Graph Benchmark](https://ogb.stanford.edu/)是图机器学习的评价平台，从[代码](https://github.com/snap-stanford/ogb/tree/master/ogb)上看，第一层分为图，节点，连接三大部分，之后每个部分都各有数据库和evaluate。每个数据库都包含[PyG](https://pytorch-geometric.readthedocs.io/en/latest/)和[DGL](https://www.dgl.ai/)，和与第三方包无关的数据库三个版本，master是数据库的元数据信息，通过运行make_master_file写入

- 安装
```bash
conda info -e
conda activate xx
pip install ogb
conda list #查看ogb版本
```
- 基本用法
```python
#载入数据
from ogb.(graph/link/node)propred import (Pyg/Dgl)(Graph/Node/Link)PropPredDataset
from (torch_geometric.data/torch.utils.data) import DataLoader


dataset=(...)PropPredDataset(name="...",root="dataset/")
split_idx=GraphDataset.get_idx_split()
split_edge=LinkDataset.get_edge_split()
split_index=NodeDataset.get_idx_split()


train_loader=DataLoader(dataset[split_idx['train']],...)
valid_loader=DataLoader(dataset[split_idx['valid']],...)
test_loader=DataLoader(dataset[split_idx['test']],...)


train_triples=split_edge['train']
valid_triples=split_edge['valid']
test_triples=split_edge['test']


#评估方法
from ogb.(graph/link/node)propred import Evaluator

evaluator=Evaluator(name="...")

input_dict=...
print(evaluator.expected_input_format)
print(evaluator.expected_output_format)


result_dict=evaluator.eval(input_dict)




```

初始化dataset对象之后就会下载对应数据,在当前文件夹产生name命名的文件夹，mapping下是id到实体的映射，各种数据库的介绍可以看[这里](https://ogb.stanford.edu/docs/dataset_overview/)


# Link


## PairRE
这里分析一下[ogbl-biokg](https://ogb.stanford.edu/docs/linkprop/#ogbl-biokg)中的[PairRE](https://github.com/ant-research/KnowledgeGraphEmbeddingsViaPairedRelationVectors_PairRE)和[ogbl-wikikg2](https://ogb.stanford.edu/docs/linkprop/#ogbl-wikikg2)中的[tripleRE](https://github.com/yulong-CSAI/TripleRE)

### run

```bash
#examples.sh 

python run.py --do_train --cuda --do_valid --do_test --evaluate_train \
  --model PairRE -n 128 -b 256 -d 2000 -g 12 -a 1.0 -adv -dr \
    -lr 0.001 --max_steps 300000 --cpu_num 2 --test_batch_size 32

```

下面是对所用数据库情况的展示

```python

from ogb.linkproppred import LinkPropPredDataset


dataset: LinkPropPredDataset=LinkPropPredDataset(name='ogbl-biokg')
split_edge: Any | dict[str, Any]=dataset.get_edge_split()

train_triples=split_edge['train']

print(*values: list(train_triples.keys()))

print(*values: train_triples[*values: 'head_type'][*values…:5])
print(*values: train_triples[*values: 'head'][*values: :5])
print(*values: train_triples[*values: 'relation'][*values: :5])

"""

['head_type', 'head', 'relation', 'tail_type', 'tail']
['disease', 'disease', 'disease', 'disease', 'disease']
[ 1718  4903  5480  3148 10300 ]
[0 0 0 0 0] #边的id从0开始,因此max(relation)+1 就是边的数量


"""


print(*values: list(dataset[0].keys()))

for i,k in enumerate(iterable: dataset[iterable: 0][iterab…'num_nodes_dict']):
    print(*values: k+":"+str(dataset[0]['num_nodes_dict'][k]))


"""


[edge_index_dict', 'edge_feat_dict', 'node_feat_dict', 'num_nodes_dict', 'edge_reltype']
disease:10687
drug:10533
function:45085
protein:17499
sideeffect:9969

```

```python


#run.py main()
train_count, train_true_head, train_true_tail = defaultdict(lambda: 4), defaultdict(list), defaultdict(list)
 
for i in tqdm(iterable: range(len(train_triples['head']))):
    head, relation, tail = train_triples['head'][i], train_triples['relation'][i], train_triples['tail'][i]
    head_type, tail_type = train_triples['head_type'][i], train_triples['tail_type'][i]
    train_count[(head, relation, head_type)] += 1
    train_count[(tail, -relation-1, tail_type)] += 1
    train_true_head[(relation, tail)].append(head)
    train_true_tail[(head, relation)].append(tail)

```
train_count初始值是4,为什么?,这里不清楚为什么用`train_count[(tail, -relation-1, tail_type)] += 1`来计数，


### DataSet

```python

entity_dict: dict[Unknown, Unknown] = dict()
cur_idx = 0
for key in dataset[0]['num_nodes_dict']:
    entity_dict[key] = (cur_idx, cur_idx + dataset[0]['num_nodes_dict'][key])
    cur_idx += dataset[0]['num_nodes_dict'][key]
                             

positive_sample: list[Unknown] = [head + self.entity_dict[head_type][0], relation, tail + self.entity_dict[tail_type][0]]

if self.mode == 'head-batch':
    negative_sample: Unknown = torch.randint(self.entity_dict[head_type][0], self.entity_dict[head_type][1], (self.negative_sample_size,))
elif self.mode == 'tail-batch':
    negative_sample: Unknown = torch.randint(self.entity_dict[tail_type][0], self.entity_dict[tail_type][1], (self.negative_sample_size,))
                                         


```
每个实体的id都根据类型相加，可能是因为每个类型的id都是从0开始，为了统一，因此做`head + self.entity_dict[head_type][0]`的操作


[torch.randint](https://pytorch.org/docs/stable/generated/torch.randint.html)产生negative_sample_size个随机数



BidrectionalOneShotIterator的作用是交替产生头尾训练数据
```python
def __next__(self) -> Unknown:
    self.step += 1
    if self.step % 2 == 0:
        data: Unknown = next(self.iterator_head)
    else:
        data: Unknown = next(self.iterator_tail)
    return data


```

### train
```python
kge_model: KGEModel = KGEModel(
    model_name=args.model,
    nentity=nentity,
    nrelation=nrelation,
    hidden_dim=args.hidden_dim,
    gamma=args.gamma,
    double_entity_embedding=args.double_entity_embedding,
    double_relation_embedding=args.double_relation_embedding,
    evaluator=evaluator                                                                
)



log: dict[str, Unknown] | Unknown = kge_model.train_step(model: kge_model, optimizer: optimizer, train_iterator: train_iterator, args: args)


positive_sample, negative_sample, subsampling_weight, mode = next(train_iterator)

negative_score: Unknown = model((positive_sample, negative_sample), mode=mode)

positive_score: Unknown = model(positive_sample) #mode=single_batch


#head-batch
tail_part, head_part = sample

```
head-batch和tail-batch的差别就只是negative_sample不一样，head-batch是从head_type中采样负样本，head_part.shape=(batch_size,negative_sample_size),tail_part.shape=(batch_size,3)



forward中得到embedding没有用nn.Embedding而是直接用torch.index_select


真正的模型实现是
```python
relation: Unknown = torch.index_select(
    self.relation_embedding,
    dim=0,
    index=tail_part[:, 1]        #relation.shape=(batch_size,dim*2),因为指定了double_relation_embedding
).unsqueeze(1) #relation.shape=(batch_size,1,dim*2)








def PairRE(self, head, relation, tail, mode) -> Unknown:
    re_head, re_tail = torch.chunk(relation, 2, dim=2) #(batch_size,1,dim),纵向切分
    head = F.normalize(head, 2, -1)
    tail = F.normalize(tail, 2, -1)
    score: Unknown = head * re_head - tail * re_tail
    score: Unknown = self.gamma.item() - torch.norm(score, p=1, dim=2)
    return score


```

## TripleRE

### run

```bash
python run.py --do_train --do_valid --cuda --do_test --evaluate_train \
          --model TripleRE -n 128 -b 512 -d 200 -g 6 -a 1.0 -adv -tr \
          -lr 0.0005 --max_steps 700000 --cpu_num 2 --test_batch_size 32


```
和PairRE的参数和代码基本一致,下面直接看model.py


### TransE
```python

def TransE(self, head, relation, tail, mode) -> Unknown:
	if mode == 'head-batch':
		score: Unknown = head + (relation - tail)
	else:
		score: Unknown = (head + relation) - tail

	score: Unknown = self.gamma.item() - torch.norm(score, p=1, dim=2)
	return score

```
> 这里的embedding.shape=[batch_size,1,dim]

实际上两种batch的计算结果是一样的，满足结合律，这里的方法与[原方法](https://yangoosen.github.io/2024/04/12/KGEmbedding/#TransH)的不同是这里norm用的是绝对值，而不是p=2，另外score和loss的计算方法也有不同，这里的score直接算了$\gamma$,后面的loss直接取平均

### DistMult
```python
def DistMult(self, head, relation, tail, mode) -> Unknown:
    if mode == 'head-batch':
        score: Unknown = head * (relation * tail)
    else:
        score: Unknown = (head * relation) * tail

    score: Unknown = score.sum(dim = 2)
    return score


```
基本实现了[原方法](https://yangoosen.github.io/2024/04/12/KGEmbedding/#DistMult)


### ComplEx
```python

def ComplEx(self, head, relation, tail, mode) -> Tensor:
    re_head, im_head = torch.chunk(input: head, chunks: 2, dim=2)
    re_relation, im_relation = torch.chunk(input: relation, chunks: 2, dim=2)
    re_tail, im_tail = torch.chunk(input: tail, chunks: 2, dim=2)

    if mode == 'head-batch':
        re_score: Tensor = re_relation * re_tail + im_relation * im_tail
        im_score: Tensor = re_relation * im_tail - im_relation * re_tail
        score: Tensor = re_head * re_score + im_head * im_score
    else:
        re_score: Tensor = re_head * re_relation - im_head * im_relation
        im_score: Tensor = re_head * im_relation + im_head * re_relation
        score: Tensor = re_score * re_tail + im_score * im_tail

    score: Tensor = score.sum(dim = 2)
    return score


```

head-batch是head是负样本，这里实现的情况是
$$
score=Re(head) * Re(score) + Im(head) * Im(score)\\
=Re(head)*(Re(relation) * Re(tail) + Im(relation) * Im(tail))+Im(head)*(Re(relation) * Im(tail) - Im(relation) * Re(tail))\\
=Re(rel)*Re(head)*Re(tail)\\+Im(Re)*Re(head)*Im(tail)\\+Re(rel)*Im(head)*Im(tail)\\-Im(rel)*Im(head)*Re(tail)

$$
基本实现了[原方法](https://yangoosen.github.io/2024/04/12/KGEmbedding/#ComplEx)


### RotatE
```python

def RotatE(self, head, relation, tail, mode) -> Unknown:
    pi: float = 3.14159265358979323846

    re_head, im_head = torch.chunk(head, 2, dim=2)
    re_tail, im_tail = torch.chunk(tail, 2, dim=2)

    #Make phases of relations uniformly distributed in [-pi, pi]

    phase_relation: Unknown = relation/(self.embedding_range.item()/pi)

    re_relation: Unknown = torch.cos(phase_relation)
    im_relation: Unknown = torch.sin(phase_relation)

    if mode == 'head-batch':
        re_score: Unknown = re_relation * re_tail + im_relation * im_tail
        im_score: Unknown = re_relation * im_tail - im_relation * re_tail
        re_score: Unknown = re_score - re_head
        im_score: Unknown = im_score - im_head
    else:
        re_score: Unknown = re_head * re_relation - im_head * im_relation
        im_score: Unknown = re_head * im_relation + im_head * re_relation
        re_score: Unknown = re_score - re_tail
        im_score: Unknown = im_score - im_tail

    score: Unknown = torch.stack([re_score, im_score], dim = 0)
    score: Unknown = score.norm(dim = 0)

    score: Unknown = self.gamma.item() - score.sum(dim = 2)
    return score

```

$\frac{embedding}{range}\pi$使得relation可以用角度表示，基本实现了[原方法](https://yangoosen.github.io/2024/04/12/KGEmbedding/#RotatE)

### RotateEv2
```python
def RotatEv2(self, head, relation, tail, mode, r_norm=None) -> Unknown:
    pi: float = 3.14159265358979323846

    re_head, im_head = torch.chunk(head, 2, dim=2)
    re_tail, im_tail = torch.chunk(tail, 2, dim=2)

    #Make phases of relations uniformly distributed in [-pi, pi]
    phase_relation: Unknown = relation/(self.embedding_range.item()/pi)

    re_relation: Unknown = torch.cos(phase_relation)
    im_relation: Unknown = torch.sin(phase_relation)

    re_relation_head, re_relation_tail = torch.chunk(re_relation, 2, dim=2)
    im_relation_head, im_relation_tail = torch.chunk(im_relation, 2, dim=2)

    re_score_head: Unknown = re_head * re_relation_head - im_head * im_relation_head
    im_score_head: Unknown = re_head * im_relation_head + im_head * re_relation_head

    re_score_tail: Unknown = re_tail * re_relation_tail - im_tail * im_relation_tail
    im_score_tail: Unknown = re_tail * im_relation_tail + im_tail * re_relation_tail

    re_score: Unknown = re_score_head - re_score_tail
    im_score: Unknown = im_score_head - im_score_tail

    score: Unknown = torch.stack([re_score, im_score], dim = 0)
    score: Unknown = score.norm(dim = 0)

    score: Unknown = self.gamma.item() - score.sum(dim = 2)
    return score

```

TripleRE只是在PairRE的基础上多了+re_mid

### TransH
```python


if model_name in ['TransH']:
    self.norm_vector: Unknown = nn.Parameter(torch.zeros(nrelation, self.relation_dim))
    nn.init.uniform_(
        tensor=self.norm_vector,
        a=-self.embedding_range.item(),
        b=self.embedding_range.item()
    )


def TransH(self, head, relation, tail, mode) -> Unknown:
    def _transfer(e, norm) -> Unknown:
        norm = F.normalize(norm, p = 2, dim = -1)
        if e.shape[0] != norm.shape[0]:
            e = e.view(-1, norm.shape[0], e.shape[-1])
            norm = norm.view(-1, norm.shape[0], norm.shape[-1])
            e = e - torch.sum(e * norm, -1, True) * norm
            return e.view(-1, e.shape[-1])
        else:
            return e - torch.sum(e * norm, -1, True) * norm

    r_norm: Unknown = self.norm_vector(relation)
    h: Unknown = _transfer(e: h, norm: r_norm)
    t: Unknown = _transfer(e: t, norm: r_norm)

    """
    if self.norm_flag:
        h = F.normalize(h, 2, -1)
        r = F.normalize(r, 2, -1)
        t = F.normalize(t, 2, -1)
    """

    if mode != 'normal':
        h: Unknown = h.view(-1, r.shape[0], h.shape[-1])
        t: Unknown = t.view(-1, r.shape[0], t.shape[-1])
        r: Unknown = r.view(-1, r.shape[0], r.shape[-1])
    if mode == 'head_batch':
        score: Unknown = h + (r - t)
    else:
        score: Unknown = (h + r) - t
    score: Unknown = self.gamma.item() - torch.norm(score, p=1, dim=2)
    return score

```
这里TripleRE的model.py中实现的TransH似乎有误，norm_vector是Parameter，怎么能调用norm_vector(embedding)??，可以看[OpenKE](https://github.com/thunlp/OpenKE/blob/OpenKE-PyTorch/openke/module/model/TransH.py)的实现,r_norm是r对应超平面的法向量，r是关系的嵌入，基本实现了[原方法](https://yangoosen.github.io/2024/04/12/KGEmbedding/#TransH)


















