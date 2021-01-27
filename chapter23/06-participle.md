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


1）查看 elasticsearch 版本号

```
{
  "name" : "d248687cf2f9",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "XXM1pkC1QVKuBDdtRcj7pQ",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

2）安装 ik 分词


```shell
cd /mydata/elasticsearch/plugins
yum install wget
wget https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.6.2/elasticsearch-analysis-ik-7.6.2.zip
yum install unzip
unzip elasticsearch-analysis-ik-7.6.2.zip -d ik
rm -rf elasticsearch-analysis-ik-7.6.2.zip 
```

3）重启容器或虚拟机。

```shell
docker restart elasticsearch
```

#### 6.2.1 测试分词器

##### 1) 默认分词器

```json
GET my_index/_analyze
{
   "text":"我是中国人"
}
```

输出结果：

```json
{
  "tokens" : [
    {
      "token" : "我",
      "start_offset" : 0,
      "end_offset" : 1,
      "type" : "<IDEOGRAPHIC>",
      "position" : 0
    },
    {
      "token" : "是",
      "start_offset" : 1,
      "end_offset" : 2,
      "type" : "<IDEOGRAPHIC>",
      "position" : 1
    },
    {
      "token" : "中",
      "start_offset" : 2,
      "end_offset" : 3,
      "type" : "<IDEOGRAPHIC>",
      "position" : 2
    },
    {
      "token" : "国",
      "start_offset" : 3,
      "end_offset" : 4,
      "type" : "<IDEOGRAPHIC>",
      "position" : 3
    },
    {
      "token" : "人",
      "start_offset" : 4,
      "end_offset" : 5,
      "type" : "<IDEOGRAPHIC>",
      "position" : 4
    }
  ]
}
```

##### 2) `ik_smart` 分词器

```json
GET my_index/_analyze
{
   "analyzer": "ik_smart", 
   "text":"我是中国人"
}
```

结果：

```json
{
  "tokens" : [
    {
      "token" : "我",
      "start_offset" : 0,
      "end_offset" : 1,
      "type" : "CN_CHAR",
      "position" : 0
    },
    {
      "token" : "是",
      "start_offset" : 1,
      "end_offset" : 2,
      "type" : "CN_CHAR",
      "position" : 1
    },
    {
      "token" : "中国人",
      "start_offset" : 2,
      "end_offset" : 5,
      "type" : "CN_WORD",
      "position" : 2
    }
  ]
}
```

##### 3) `ik_max_word` 分词器


```json
GET my_index/_analyze
{
   "analyzer": "ik_max_word", 
   "text":"我是中国人"
}
```

结果：

```json
{
  "tokens" : [
    {
      "token" : "我",
      "start_offset" : 0,
      "end_offset" : 1,
      "type" : "CN_CHAR",
      "position" : 0
    },
    {
      "token" : "是",
      "start_offset" : 1,
      "end_offset" : 2,
      "type" : "CN_CHAR",
      "position" : 1
    },
    {
      "token" : "中国人",
      "start_offset" : 2,
      "end_offset" : 5,
      "type" : "CN_WORD",
      "position" : 2
    },
    {
      "token" : "中国",
      "start_offset" : 2,
      "end_offset" : 4,
      "type" : "CN_WORD",
      "position" : 3
    },
    {
      "token" : "国人",
      "start_offset" : 3,
      "end_offset" : 5,
      "type" : "CN_WORD",
      "position" : 4
    }
  ]
}
```


### 6.3 自定义扩展词库

1）给虚拟机扩大内存

2）安装并启动 nginx

```
# 启动一个 nginx 实例(为了复制其配置)
docker run -p 80:80 --name nginx -d nginx:1.10

# 将容器内的配置文件拷贝到当前目录
cd /mydata
mkdir nginx
# 别忘了 .
docker container cp nginx:/etc/nginx .
cd nginx/
# 配置文件已拷贝
[root@localhost nginx]# ls
conf.d  fastcgi_params  koi-utf  koi-win  mime.types  modules  nginx.conf  scgi_params  uwsgi_params  win-utf
# 暂停容器
docker stop nginx
# 移除容器
docker rm nginx
# 将文件夹 nginx 重新命名 conf
cd ..
mv nginx conf
# 新建 nginx 文件夹
mkdir nginx
# 将 conf 文件夹移动到 nginx 中
mv conf nginx/
# 
cd nginx/
mkdir html
mkdir logs

# 创建新的 nginx
docker run -p 80:80 --name nginx \
-v /mydata/nginx/html:/usr/share/nginx/html \
-v /mydata/nginx/logs:/var/log/nginx \
-v /mydata/nginx/conf:/etc/nginx \
-d nginx:1.10
```

3）配置转发

上述命令返回时带有 `WARNING: IPv4 forwarding is disabled. Networking will not work.`。

```
vi /etc/sysctl.conf

#配置转发
net.ipv4.ip_forward=1

#重启服务，让配置生效
systemctl restart network

#查看是否成功,如果返回为“net.ipv4.ip_forward = 1”则表示成功
sysctl net.ipv4.ip_forward
```

4）重新启动 nginx

```
docker start nginx
# 开机启动
docker update nginx --restart=always
```

#### 6.3.1 测试是否安装成功

```
[root@localhost html]# pwd
/mydata/nginx/html
[root@localhost html]# vi index.html
# 添加 <h1>GuliMall</h1>
```

浏览器访问 `http://192.168.56.10/` 页面上会显示：

<img src="./img23/14-nginx-test.png" width=300>







