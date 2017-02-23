### Elasticsearch 简介

Elasticsearch是一个基于[Apache Lucene(TM)](https://lucene.apache.org/core/)的开源搜索引擎。无论在开源还是专有领域，Lucene可以被认为是迄今为止最先进、性能最好的、功能最全的搜索引擎库。

但是，Lucene只是一个库。想要使用它，你必须使用Java来作为开发语言并将其直接集成到你的应用中，更糟糕的是，Lucene非常复杂，你需要深入了解检索的相关知识来理解它是如何工作的。

Elasticsearch也使用Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的RESTful API来隐藏Lucene的复杂性，从而让全文搜索变得简单。

不过，Elasticsearch不仅仅是Lucene和全文搜索，我们还能这样去描述它：
* 分布式的实时文件存储，每个字段都被索引并可被搜索
* 分布式的实时分析搜索引擎
* 可以扩展到上百台服务器，处理PB级结构化或非结构化数据

而且，所有的这些功能被集成到一个服务里面，你的应用可以通过简单的RESTful API、各种语言的客户端甚至命令行与之交互。

### 安装

ES 运行需要 Java 环境，单机安装时只需要下载对应的安装包，如：
* elasticsearch-5.0.1.rpm
* jdk-8u101-linux-x64.rpm

使用命令 `rpm -ivh Package-Name` 安装即可

### 集群配置

安装完成后，生成的配置文件位于路径 **/etc/elasticsearch/elasticsearch.yml**，调整参数：

```
cluster.name: cngb-biomigo    #属于同一集群的节点使用相同的集群名称
```
> 注意：
> ES 使用端口 **9200** 与各 client 进行交互（RESTful API），**9300** 端口与集群各节点交互

### 测试环境
Cpu: 16 cores / 32 threads

Memory: 64 GB

OS: Centos6.8

Disk: about 120GB free space per node

Cluster: 4 nodes total, 3 datanodes, 1 master node

Elasticsearch version: 5.0.1


### 测试数据
* cancerdb gene 信息 35,439 条
* cancerdb tumor_type 信息 22 条
* gemap ensembl_variation 信息 4,000,000 条

数据分别存放于 /mytest/ 下 gene，tumor_type，variation 3 个 type 里面
使用 Python 脚本加原生 sql 进行数据导入

### 索引策略
不同类型数据存放时，需要制定字段索引的分析策略（指定分析器），常用分析器有：
1. stardard 分析器。标准分析器是Elasticsearch默认使用的分析器。它根据[Unicode Consortium](http://www.unicode.org/reports/tr29/)的定义的单词边界(word boundaries)来切分文本，然后去掉大部分标点符号。最后，把所有词转为小写。
2. english 分析器。语言分析器考虑语言的特性，去掉英文中的停用词，并提取词中的词干。
3. 自定义分析器。目前只用到了字符分析器，以字符为单位对文本进行分词和索引。

测试文本：“Set the shape to semi-transparent by calling set_trans(5)”
* 标准分析器
`set, the, shape, to, semi, transparent, by, calling, set_trans, 5`
* 英文分析器
`set, shape, semi, transpar, call, set_tran, 5`
* 自定义的字符分析器
`s, se, set, e, et, t, th, the, ...`

在索引长段文本如描述信息、文献信息时，会使用英文分析器
在索引字符串和短语，如 基因 symbol 时，使用字符分析器

测试index中3个type 的索引信息如下：
```
{
  "_all": {
    "enabled": false
  },
  "settings": {
    "analysis": {
      "tokenizer": {
        "my_ngram": {
          "type": "nGram",
          "min_gram": "2",
          "max_gram": "20",
          "token_chars": [
            "letter",
            "digit"
          ]
        }
      },
      "analyzer": {
        "separate_characters": {
          "tokenizer": "my_ngram",
          "filter": [
            "lowercase"
          ]
        }
      }
    }
  },
  "mappings": {
    "gene": {
      "dynamic": false,
      "properties": {
        "uri_id": {
          "type": "text",
          "analyzer": "separate_characters"
        },
        "gene_title":{
          "type":"text",
          "analyzer": "separate_characters"
        },
        "synonym":{
          "type":"text",
          "analyzer": "separate_characters"
        },
        "name":{
          "type":"keyword",
          "index":false
        },
        "locus_group":{
          "type":"keyword",
          "index":false
        },
        "locus_type":{
          "type":"keyword",
          "index":false
        },
        "status":{
          "type":"keyword",
          "index":false
        },
        "location":{
          "type":"keyword",
          "index":false
        },
        "region":{
          "type":"keyword",
          "index":false
        },
        "dornor":{
          "type":"text"
        },
        "tumour_type":{
          "type":"text",
          "analyzer": "english"
        }
      }
    },
    "tumourType":{
      "dynamic":false,
      "properties": {
        "tumour_title":{
          "type":"text",
          "analyzer": "separate_characters"
        },
        "uri_id":{
          "type":"text",
          "analyzer": "separate_characters"
        },
        "short_name":{
          "type":"text"
        },
        "description":{
          "type":"keyword",
          "index":false
        },
        "primary_site":{
          "type":"text",
          "analyzer": "english"
        },
        "donor":{
          "type":"text"
        },
        "gene":{
          "type":"text",
          "analyzer": "separate_characters"
        }
      }
    },
    "variation":{
      "dynamic":false,
      "properties": {
        "variation_name":{
          "type":"text",
          "analyzer": "separate_characters"
        },
        "minor_allele":{
          "type":"keyword",
          "index":false
        },
        "minor_allele_freq":{
          "type":"keyword",
          "index":false
        },
        "clinical_significance":{
          "type":"keyword",
          "index":false
        },
        "consequence_types":{
          "type":"keyword",
          "index":false
        },
        "allele_string":{
          "type":"keyword",
          "index":false
        }, 
        "seq_region_start":{
          "type":"keyword",
          "index":false
        }, 
        "seq_region_end":{
          "type":"keyword",
          "index":false
        }, 
        "gene":{
          "type":"text",
          "analyzer":"separate_characters"
        },
        "synonym":{
          "type":"text",
          "analyzer":"separate_characters"
        },
        "hgvs":{
          "type":"keyword",
          "index":false
        },
        "publication_titles":{
          "type":"text",
          "analyzer":"english"
        }     
      }
    }
  }
}
```

### 测试结果
导入效率：
> 【gemap】约 2.98 h

> Total upload to ES: 4,000,000 Failed:         0 

> 10747.444(s) elapsed

>【cancer-gene】

> Total upload to ES:    35,439 Failed:         0

> 59.865(s) elapsed

>【cancer-tumour_type】

> Total upload to ES:        22 Failed:         0

> 0.627(s) elapsed


### 测试 demo
[DEMO](http://172.17.10.21/es/)

### 检索效率
* 在多 type 混合查询时，根据检索的复杂度不同，响应时间从十几毫秒到几十毫秒不等。以测试 demo 为例，检索关键词 'tp53' 可以在十几毫秒内响应（<20ms），检索关键词 'rs231' 在几十毫秒内响应（>20ms）
* 在多 type 混合查询时，可以对不同类型的字段进行分别加权。例如检索关键词 'lung'，响应的第一条信息为 tumour_type。检索关键词 'rs23'，响应的第一条信息为 variation
* 各 Type 中，不同字段的检索权重需要根据实际需求进行设置和调整
