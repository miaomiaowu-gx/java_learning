## 第六节 分词


### 6.1 分词

一个 tokenizer（分词器）接收一个字符流，将之分割为独立的 tokens（词元，通常是独立的单词），然后输出 tokens 流。

例如：whitespace tokenizer 遇到空白字符时分割文本。它会将文本 “Quick brown fox!” 分割为 [Quick,brown,fox!]。该 tokenizer（分词器）还负责记录各个 terms(词条)的顺序或 position 位置（用于 phrase 短语和 word proximity 词近邻查询），以及 term（词条）所代表的原始 word（单词）的 start（起始）和 end（结束）的 character offsets（字符串偏移量）（用于高亮显示搜索的内容）。

elasticsearch 提供了很多内置的分词器，可以用来构建 custom analyzers（自定义分词器）。[analysis](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/analysis.html)

#### 6.1.1 Standard tokenizer

按照空格进行分词。

```json
POST _analyze
{
  "analyzer": "standard",
  "text": "The 2 QUICK Brown-Foxes jumped over the lazy dog's bone."
}
```

结果：

```json
{
  "tokens" : [
    {
      "token" : "the",
      "start_offset" : 0,
      "end_offset" : 3,
      "type" : "<ALPHANUM>",
      "position" : 0
    },
    {
      "token" : "2",
      "start_offset" : 4,
      "end_offset" : 5,
      "type" : "<NUM>",
      "position" : 1
    },
    {
      "token" : "quick",
      "start_offset" : 6,
      "end_offset" : 11,
      "type" : "<ALPHANUM>",
      "position" : 2
    },
    {
      "token" : "brown",
      "start_offset" : 12,
      "end_offset" : 17,
      "type" : "<ALPHANUM>",
      "position" : 3
    },
    {
      "token" : "foxes",
      "start_offset" : 18,
      "end_offset" : 23,
      "type" : "<ALPHANUM>",
      "position" : 4
    },
    {
      "token" : "jumped",
      "start_offset" : 24,
      "end_offset" : 30,
      "type" : "<ALPHANUM>",
      "position" : 5
    },
    {
      "token" : "over",
      "start_offset" : 31,
      "end_offset" : 35,
      "type" : "<ALPHANUM>",
      "position" : 6
    },
    {
      "token" : "the",
      "start_offset" : 36,
      "end_offset" : 39,
      "type" : "<ALPHANUM>",
      "position" : 7
    },
    {
      "token" : "lazy",
      "start_offset" : 40,
      "end_offset" : 44,
      "type" : "<ALPHANUM>",
      "position" : 8
    },
    {
      "token" : "dog's",
      "start_offset" : 45,
      "end_offset" : 50,
      "type" : "<ALPHANUM>",
      "position" : 9
    },
    {
      "token" : "bone",
      "start_offset" : 51,
      "end_offset" : 55,
      "type" : "<ALPHANUM>",
      "position" : 10
    }
  ]
}
```

但中文支持不好，如当 `"text"` 内容为 `全文检索学习` 时，会每个字都拆分，并不是按词语拆分。

### 6.2 安装 ik 分词

所有的语言分词，默认使用的都是 `Standard Analyzer`，但是这些分词器针对于中文的分词，并不友好。为此需要安装中文的分词器。





