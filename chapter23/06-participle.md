## 第六节 分词


### 6.1 分词

一个 tokenizer（分词器）接收一个字符流，将之分割为独立的 tokens（词元，通常是独立的单词），然后输出 tokens 流。

例如：whitespace tokenizer 遇到空白字符时分割文本。它会将文本 “Quick brown fox!” 分割为 [Quick,brown,fox!]。该 tokenizer（分词器）还负责记录各个 terms(词条)的顺序或 position 位置（用于 phrase 短语和 word proximity 词近邻查询），以及 term（词条）所代表的原始 word（单词）的 start（起始）和 end（结束）的 character offsets（字符串偏移量）（用于高亮显示搜索的内容）。

elasticsearch 提供了很多内置的分词器，可以用来构建 custom analyzers（自定义分词器）。[analysis](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/analysis.html)


### 6.2 安装 ik 分词