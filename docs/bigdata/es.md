# es

## 类型text与keyword
text类型的数据被用来索引长文本，例如电子邮件主体部分或者一款产品的介绍，这些文本会被分析，在建立索引文档之前会被分词器进行分词，转化为词组。经过分词机制之后es允许检索到该文本切分而成的词语，但是text类型的数据不能用来过滤、排序和聚合等操作。
keyword类型的数据可以满足电子邮箱地址、主机名、状态码、邮政编码和标签等数据的要求，不进行分词，常常被用来过滤、排序和聚合。
总结：keyword：存储数据时候，不会分词建立索引，
    text：存储数据时候，会自动分词，并生成索引。

## match查询
用于在文本字段中查找匹配的词语。match查询会对text类型字段进行分词
```
GET /xxx/_search
{
  "query": {
    "match": {
      "address": "北京"
    }
  }
}
```
## term查询
用于精确匹配字段中的值。一般用于keyword
```
GET /xxx/_search
{
  "query": {
    "term": {
      "category1_new": "士多"
    }
  }
}
```

## bool查询
用于组合多个查询条件，支持must、should、must_not等关键词。
must: 文档必须完全匹配条件
should: should下面会带一个以上的条件，至少满足一个条件，这个文档就符合should
must_not: 文档必须不匹配条件
```
GET /xxx/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "state": "北京" } },
        { "term": {  "category1_new": "士多"} }
      ]
    }
  }
}
```

## range查询
用于查找某个字段在指定范围内的文档。
```
GET /xxx/_search
{
  "query": {
    "range": {
      "price": {
        "gte": 100,
        "lte": 500
      }
    }
  }
}

```
## Fuzzy查询
"Fuzzy"查询是Elasticsearch中的一种模糊匹配查询，它允许你查找与指定词语相似的文档。模糊匹配非常有用，特别是在处理拼写错误、文本数据包含打字错误或近义词时。它基于编辑距离来匹配，可以处理拼写错误或近义词。查询的主要目标是通过容忍一定数量的字符编辑操作来查找相似的词语，而不仅仅是精确匹配。
```
GET /xxx/_search
{
  "query": {
    "fuzzy": {
      "channeltype_new": "传统渠"
    }
  }
}
```
### fuzzy 查询的常见参数
1. value（必填）

- 用于指定要进行模糊匹配的值，通常是你要查找的单词或短语。  
- 例如，"value": "searh" 进行对 search 的模糊查询。
2. fuzziness（可选）  
- 控制编辑距离，即允许多少个字符插入、删除或替换。可以是：  
数字（例如：fuzziness: 2）：指定最大允许的编辑距离。  
AUTO：基于搜索词的长度自动计算允许的编辑距离。通常短词（如2个字母）使用较小的距离，而较长的词则允许更大的编辑距离。
- 例如，"fuzziness": 2 表示允许两个字符的差异。
3. prefix_length（可选）  
- 用于定义在开始时不允许有任何变化的字符数。通过这个参数，可以在实际计算模糊查询前忽略文本的前缀部分，优化查询性能。
- 例如，"prefix_length": 2 表示查询词的前两个字符不会受到模糊化的影响。
4. max_expansions（可选）  
- 限制可以生成的模糊变体的最大数量。如果设置了较大的 fuzziness 值，可能会导致查询生成大量变体，max_expansions 可用于控制这一点，避免性能下降。
- 例如，"max_expansions": 50 表示最多生成50个变体。
5. transpositions（可选）
- 是否允许字符位置的交换。默认情况下，字符交换不被认为是有效的变体（即 "ab" 不等于 "ba"）。如果设置为 true，则允许字符位置交换。
- 例如，"transpositions": true 允许字符交换。
```json
POST /your_index/_search
{
  "query": {
    "fuzzy": {
      "your_field": {
        "value": "searh",        // 要匹配的词
        "fuzziness": "AUTO",    // 自动设置模糊度
        "prefix_length": 2,     // 前2个字符不变
        "max_expansions": 50   // 允许的最大变体数
      }
    }
  }
}
```

### 关于fuzzy查询的疑问
背景：使用fuzzy查询查询某个店铺时，发现使用**百兴隆**能够查询出**百兴隆超市(天雅居教师小区西)**的数据，但是使用**百兴隆超市**查询不出数据，当前使用的分词器为ik_smart分词器。  
分析：  
百兴隆超市(天雅居教师小区西)分词后的结果为：
```json
{
  "tokens": [
    {
      "token": "百",
      "start_offset": 0,
      "end_offset": 1,
      "type": "TYPE_CNUM",
      "position": 0
    },
    {
      "token": "兴隆",
      "start_offset": 1,
      "end_offset": 3,
      "type": "CN_WORD",
      "position": 1
    },
    {
      "token": "超市",
      "start_offset": 3,
      "end_offset": 5,
      "type": "CN_WORD",
      "position": 2
    },
    {
      "token": "天",
      "start_offset": 6,
      "end_offset": 7,
      "type": "CN_CHAR",
      "position": 3
    },
    {
      "token": "雅",
      "start_offset": 7,
      "end_offset": 8,
      "type": "CN_CHAR",
      "position": 4
    },
    {
      "token": "居",
      "start_offset": 8,
      "end_offset": 9,
      "type": "CN_CHAR",
      "position": 5
    },
    {
      "token": "教师",
      "start_offset": 9,
      "end_offset": 11,
      "type": "CN_WORD",
      "position": 6
    },
    {
      "token": "小区",
      "start_offset": 11,
      "end_offset": 13,
      "type": "CN_WORD",
      "position": 7
    },
    {
      "token": "西",
      "start_offset": 13,
      "end_offset": 14,
      "type": "CN_CHAR",
      "position": 8
    }
  ]
}
```
使用百兴隆查询时，是使用**百兴隆(不分词)**的结果和**百兴隆超市(天雅居教师小区西)**分词后的token进行编辑距离计算的。  
所以查询**百兴隆**和**兴隆**的编辑距离为1，能够查询出结果，而使用**百兴隆超市**查询时，无论是**兴隆**或者是**超市**的编辑距离都大于2(可设置的最大距离)，所以查询不出结果。
## wildcard查询
支持通配符匹配。
```
GET /xxx/_search
{
  "query": {
    "wildcard": {
      "title": "elasticsearch*"
    }
  }
}
```

## regexp查询
```
GET /xxx/_search
{
  "query": {
    "regexp": {
      "title": "elast[a-z]*ch"
    }
  }
}
```

## match_phrase查询
要求查询的词语在文档中以相同的顺序相邻出现。
```
GET /xxx/_search
{
  "query": {
    "match_phrase": {
      "description": "distributed search"
    }
  }
}
```

## 一些es的疑问

### term和filter
最近在用es做项目，其中使用到了term和filter，使用的语句为：
```
{
    "query": {
        "term": {
            "tenant_code": XXX
        }
    }
}

{
    "query": {
        "bool": {
            "filter": {
                "term": {
                    "tenant_code": XXX
                }
            }
        }
    }
}
```
这两个语句出来的数据是一样的，但是max_score在上面的为1，在下面的为0。查询资料后发现：
term查询是一种精确匹配查询，它要求文档中的字段值与指定的值完全相等。当使用term查询时，Elasticsearch会计算文档与查询条件的匹配度，如果字段值与查询条件完全匹配，它会返回1，否则返回0。因为term查询是精确匹配，所以当有匹配时，max_score为1。

bool查询是一个更通用的查询类型，允许您组合多个条件，包括过滤条件。在您的示例中，bool查询的filter子句仅包含一个term查询条件，用于过滤文档。虽然这个条件可能与term查询一样精确，但是bool查询不涉及评分，因为它只是用于过滤文档的。因此，bool查询的max_score通常是0，因为它不计算文档与查询条件的匹配度。

总结：term查询返回1或0，表示完全匹配或不匹配，而bool查询的filter子句通常返回0，因为它只是用于过滤文档的，不涉及评分。如果您只关心过滤结果而不关心评分，那么使用bool查询的filter子句通常更合适。

### Painless 
Painless 是 Elasticsearch 中的一种脚本语言，用于执行查询、聚合和转换数据。它是一种轻量级的脚本语言，专门设计用于 Elasticsearch 中的高性能脚本执行。Painless 语言的目标是提供一种安全且高效的方式来编写和执行 Elasticsearch 查询和聚合中的自定义脚本逻辑。
使用方式为：
```
"script": {
    "source": """
                if (ctx._source.containsKey('XXXX')) {
                    if (ctx._source.XXXX == null) {
                        ctx._source.XXXX = [params.XXXX]
                    } else {
                        boolean exists = false;
                        for (item in ctx._source.XXXX) {
                            if (item.XXXX == params.XXXX.XXXX && item.XXXX == params.XXXX.XXXX) {
                                exists = true;
                                break;
                            }
                        }
                        if (!exists) {
                            ctx._source.XXXX.add(params.XXXX)
                        }
                    }
                } else {
                    ctx._source.XXXX = [params.XXXX]
                }
            """,
    "lang": "painless",
    "params": {
        "XXXX": {
            "XXXX": doc['XXXX'],
            "XXXX": doc['XXXX']
        }
    }
}
```

### 查询时间
之前新建索引的时间字段，类型一般为date，但是某次没注意用的是keyword类型，在使用范围查询的时候发现查出来的数据不对，查了资料后发现：
如果是date类型，可正常查询，如果是keyword类型，需要在字段后面添加.keyword后才能正常查询，示例：
```
一、date
"insert_time":{
  "type": "date",
  "format" : "yyyy-MM-dd HH:mm:ss"
}

"query": {
    "bool": {
        "must": [
            {
                "range": {
                    "insert_time": {
                        "gt": last_insert_time
                    }
                }
            },
            {
                "term": {
                    "xxx": xxx
                }
            }
        ]
    }
}
}
二、keywrod

"query": {
    "bool": {
        "must": [
            {
                "range": {
                    "insert_time.keyword": {
                        "gt": last_insert_time
                    }
                }
            },
            {
                "term": {
                    "xxx": xxx
                }
            }
        ]
    }
}
}
```
### Tokenizer
es的默认Tokenizer是standard，可通过以下语句查看分词:
```
GET /_analyze
{
  "text": "A Quick Brown Fox"
}
```
Elasticsearch 提供了多种内置的 Tokenizer，以下是一些常用的 Tokenizer：

standard：基于 Unicode 文本分割算法的语法分词器，可以用于大多数语言。它会在大多数标点符号处分割文本，但不会移除标点符号。它是 Elasticsearch 的默认分词器。

letter：在遇到非字母字符时分割文本。

lowercase：和 letter 分词器类似，但会将所有词项转换为小写。

whitespace：在遇到任何空白字符时分割文本。

uax_url_email：和 standard 分词器类似，但会将 URL 和电子邮件地址识别为单个词项。

classic：一种基于英语语法的分词器。

ngram：将文本或单词分解为小片段，用于部分单词匹配。

edge_ngram：这个分词器也会将文本或单词分解为小片段，但只返回与词的开头锚定的 n-gram。

ngram配置示例：
```
PUT my-index-000001
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_analyzer": {
          "tokenizer": "my_tokenizer"
        }
      },
      "tokenizer": {
        "my_tokenizer": {
          "type": "ngram",
          "min_gram": 3,
          "max_gram": 3,
          "token_chars": [
            "letter",
            "digit"
          ]
        }
      }
    }
  }
}

POST my-index-000001/_analyze
{
  "analyzer": "my_analyzer",
  "text": "2 Quick Foxes."
}
```

### 对text类型数据进行全匹配
在建索引的时候使用"fields": {"keyword": {"type": "keyword"}}
```
GET /xxx/_search
{
  "query": {
    "term": {
      "xx.keyword": "士多"
    }
  }
}
```

### template的使用
_template 是用来定义索引模板的工具。索引模板可以帮助你在创建索引时自动应用一些配置，比如映射（mappings）、设置（settings）和别名（aliases），从而简化和规范索引的创建过程。  
```
# 创建模板
PUT _template/my_template
{
  "index_patterns": ["test-*"],
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  },
  "mappings": {
    "properties": {
      "timestamp": {
        "type": "date"
      },
      "message": {
        "type": "text"
      },
      "user_id": {
        "type": "keyword"
      }
    }
  },
  "aliases": {
    "logs_alias": {}
  }
}

# 创建索引
PUT test-2024-07-16
# 查看
GET test-2024-07-16/_mapping
```