---
http://192.168.56.10:9200typora-root-url: image
typora-copy-images-to: image
typora-root-url: image
---

# 谷粒商城高级篇

## 1、Elasticsearch - 全文检索

### 简介

https://www.elastic.co/cn/what-is/elasticsearch/

全文搜索属于最常见的需求，开源的 Elasticsearch 是目前全文搜索引擎的首选。

他可以快速地存储、搜索和分析海量数据。维基百科、Stack Overflow、Github 都采用他

![image-20201026084916698](/image-20201026084916698.png)

Elatic 的底层是开源库吧Lucene。但是，你没法直接用，必须自己写代码调用它的接口，Elastic 是 Lunce 的封装，提供了 REST API 的操作接口，开箱即用

REST API：天然的跨平台

官网文档：https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html

官网中文：https://www.elastic.co/guide/cn/elasticsearch/guide/current/index.html

社区中文：

http://doc.codingdict.com/elasticsearch/

### 1.1、基本概念

#### 1.1.1 index(索引)

动词，相当于 MySQL 中的 insert；

名词，相当于MySQL 中的 DataBase

#### 1.1.2 Type(类型)

在 Index（索引）中，可以定义一个或多个类型

类似于 MySQL 中的 Table，每一种类型的数据放在一起

#### 1.1.3 Document(文档)

保存在某个索引（index）下，某种类型（Type）的一个数据（Document）,文档是 JSON 格式的，Document 就像是 MySQL 中某个 Table 里面的内容

#### 1.1.4 倒排索引机制

![image-20201026092738311](/image-20201026092738311.png)

### 1.2 Docker 安装 ES

#### 1.2.1 下载镜像

docker pull elasticsearch:7.4.2  存储和检索数据

docker pull kibana:7.4.2 可视化检索数据	

#### 1.2.2 创建实例

##### 1、ElasticSearch

配置

```bash
mkdir -p /mydata/elasticsearch/config # 用来存放配置文件
mkdir -p /mydata/elasticsearch/data  # 数据
echo "http.host: 0.0.0.0" >/mydata/elasticsearch/config/elasticsearch.yml # 允许任何机器访问
chmod -R 777 /mydata/elasticsearch/ ## 设置elasticsearch文件可读写权限
```

启动

```bash
docker run --name elasticsearch -p 9200:9200 -p 9300:9300 \
-e  "discovery.type=single-node" \
-e ES_JAVA_OPTS="-Xms64m -Xmx512m" \
-v /mydata/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
-v /mydata/elasticsearch/data:/usr/share/elasticsearch/data \
-v  /mydata/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
-d elasticsearch:7.4.2 
```

开机启动 elasticsearch

```bash
docker update elasticsearch --restart=always
```

以后再外面装好插件重启就可

特别注意：

-e ES_JAVA_OPTS="-Xms64m -Xmx128m" \ 测试环境下，设置 ES 的初始内存和最大内存，否则导致过大启动不了ES

##### 2、Kibana

```bash
docker run --name kibana -e ELASTICSEARCH_HOSTS=http://192.168.56.10:9200 -p 5601:5601 -d kibana:7.4.2

http://192.168.56.10:9200 改成自己Elasticsearch上的地址
```

##### 3、安装nginx

随便启动一个 nginx 实例，只是为了复制出配置

```
docker run -p80:80 --name nginx -d nginx:1.10   
```

将容器内的配置文件拷贝到当前目录 （注意后面有个小点

）

```bash
docker container cp nginx:/etc/nginx .  
```

创建nginx文件夹

```shell
mkdir -p /mydata/nginx/html
mkdir -p /mydata/nginx/logs
 #由于拷贝完成后会在config中存在一个nginx文件夹，所以需要将它的内容移动到conf中
 #conf 文件夹下就是原先nginx的配置
mv /mydata/nginx/conf/nginx/* /mydata/nginx/conf/
rm -rf /mydata/nginx/conf/nginx
```

别忘了后面的点

修改文件名称：mv nginx.conf 把这个conf 移动到 /mydata/nginx 下

终止原容器， docker stop nginx

执行命令删除容器：docker rm $Containerid

创建新的 nginx 执行以下命令

```bash
 docker run -p 80:80 --name nginx \
 -v /mydata/nginx/html:/usr/share/nginx/html \
 -v /mydata/nginx/logs:/var/log/nginx \
 -v /mydata/nginx/conf/:/etc/nginx \
 -d nginx:1.10
```



### 1.3 初步检索

#### 1.3.1、_cat

GET /_cat/nodes：查看所有节点

GET /_cat/health：查看 es 健康状况

GET /_cat/master：查看主节点

GET /_cat/incices：查看所有索引 show databases;

#### 1.3.2 索引一个文档（保存）

保存一个数据，保存在哪个索引的哪个类型下，指定用哪个唯一标识

PUT customer/external/1; 在 customer 索引下的 external 类型下保存 1号数据为

```http
PUT customer/external/1
```

#### 1.3.3、查询文档

```http
GET custome/external/1
```

结果：

```json
{
    "_index": "customer", // 在那个索引
    "_type": "external", // 在那个类型
    "_id": "1", // 记录id
    "_version": 1, 。// 版本号
    "_seq_no": 0, // 并发控制字段，每次更新就会+1，用来做乐观锁
    "_primary_term": 1, //同上，主分片重新分配，如重启，就会变化
    "found": true, 
    "_source": {
        "name": "John Doe" // 真正的内容
    }
}
```

```
更新携带：?if_seq_no=4&if_primary_term=1
```



#### 1.3.4 更新文档

```http
POST customer/external/1/_update
{
	"doc":{
		"name":"John Doew"
	}
}
或者
POST customer/external/1
{
	"name":"John Doe2"
}
或者
PUT customer/external/1
{
	"name":"jack"
}
不同：Post操作会对比源文档数据，如果相同不会有什么操作，文档 version 不增加 PUT操作总会将数据重新保存并增加 version 版本
带 _update 对比元数据如果一样就不进行任何操作
看场景
对于大并发更新，不带update
对于大并发查询并偶尔更新，带update 对比更新，重新计算分配规则
更新同时增加属性
POS customer/external/1/_update
{
	"doc":{"name":"Jane Doe","age":20}
}
PUT 和 POST 不带_update也可以
```

#### 1.3.5 删除文档&索引

```http
DELETE customer/external/1
DELETE customer
```

#### 1.3.6 bulk 批量 API

```http
POST customer/external/_bulk
{"index":{"_id":"1"}}
{"name":"John Doe"}
{"index":{"_id":"2"}}
{"name":"John Doe"}
```

语法格式

```json
{action:{metadata}}\n
{requeestBody}\n
{action:{metadata}}\n
{requesetbod }\n
```

复杂实例：

```http
POST /_bulk
{"delete":{"_index":"website","_type":"blog","_id":"123"}}
{"create":{"_index":"website","_type":"blog","_id":"123"}}
{"title":"my first blog post"}
{"index":{"_index":"website","_type":"blog"}}
{"title":"my second blog post"}
{"update":{"_index":"website","_type":"blog","_id":"123"}}
{"doc":{"title":"my updated blog post"}}
```

bulk API以此按顺序执行所有的action (动作)。如果一一个单个的动作因任何原因而失败，它将继续处理它后面剩余的动作。当bulkAPI 返回时，它将提供每个动作的状态(与发送的顺序相同)，所以您可以检查是否一个指定的动作是不是失败了。

#### 1.3.7 样本测试数据

我准备了一份顾客银行账户信息虚构的 JSON 文档样本，每个用户都有下列的 schema （模式）：

```json
{
	"account_number": 1,
	"balance": 39225,
	"firstname": "Amber",
	"lastname": "Duke",
	"age": 32,
	"gender": "M",
	"address": "880 Holmes Lane",
	"employer": "Pyrami",
	"email": "amberduke@pyrami.com",
	"city": "Brogan",
	"state": "IL"
}
```

https://github.com/elastic/elasticsearch/edit/master/docs/src/test/resources/accounts.json

导入测试数据

POST bank/account/_bank

测试数据

![image-20201026114903942](/image-20201026114903942.png)



### 1.4 进阶检索

#### 1.4.1 SearchAPI

ES 支持两种基本方式检索:

- 一个是通过使用 REST request URL,发送搜索参数，(uri + 检索参数)
- 另一个是通过使用 REST request bod 来发送他们，(uri + 请求体)

1、检索信息

一切检索从_search开始

GET /bank/_search 检索 bank 下的所有信息，包括 type 和 docs

GET /bank/_search?q=*&sort=account_number:asc 请求参数方式检索

响应结果解释
took - Elasticearch执行搜索的时间(毫秒)

time_ out - 告诉我们搜索是否超时

_shards - 告诉我们多少个分片被搜索了，以及统计了成功/失败的搜索分片
hit - 搜索结果
hits.total - 搜索结果
hits.hits - 实际的搜索结果数组(默认为前10的文档)
sort - 结果的排序key (键) (没有则按 score 排序)
score 和 max score - 相关性得分和最高得分(全文检索用)

uri + 请求体进行检查

```java
GET /bank/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ]
}
```

HTTP 客户端工具（POSTMAN）,get请求不能携带请求体，我们变为 post也是一样的 我们 POST 一个 JSON风格的查询请求体到 _search API

需要了解，一旦搜索结果被返回，ES 就完成了这次请求的搜索，并且不会维护任何服务端的资源或者结果的 cursor（游标）

#### 1.4.2、QueryDSL

##### 1、基本语法格式

ES 提供了一个可以执行查询的 Json 风格的 DSL （domain-specifig langurage 领域特定语言），这个被成为 Query DSL ，该查询语言非常全面，并且刚开始的时候优点复杂，真正学好对他的方法是从一些基础的示例开始的

一个查询语句 的典型结构

```json
{
    QUERY_NAME:{
        ARGUMENT:VALUE,
        ARGUMENT:VALUE.....
    }
}
```

如果针对某个字段，那么它的结构如下

```json
{
    QUERY_NAME:{
        FIELD_NAME:{
            ARGUMENT:VALUE,
            ARGUMENT:VALUE.....
        }
    }
}
```

```http
GET /bank/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ],
  "from": 10,
  "size": 10
}
```

query 定义如何查询

match_all 查询类型【代表查询所有的所有】，es中可以在 query中 组合非常多的查询类型完成复杂查询

除了 query 参数之外，我们也可以传递其他的参数改变查询结构，如 sort，size

from + size 限定，完成分页功能

sort排序，多字段排序，会在前序字段相等时后续字段内部排序，否则以前序为准

##### 2、返回部分字段

```http
 GET bank/_search
 {
   "query":{
     "match_all": {}
   },
   "sort": [
     {
       "balance": {
         "order": "desc"
       }
     }
   ],
   "from": 5,
   "size": 5,
   "_source": ["firstname","lastname"]
 }
```

##### 3、match【匹配查询】

基本类型(非字符串)，精准匹配

```http
GET bank/_search
{
  "query":{
   "match": {
     "address": "mill lane"
   } 
  }
}
```

全文检索按照评分进行排序，会对检索条件进行分词匹配

##### 4、match_phrase【短语匹配】

将需要匹配的值当成一个整体单词（不分词）进行检索

```http
GET /bank/_search
{
  "query": { "match_phrase": { "address": "mill lane" } }
}
```

##### 5、multi_match【多字段匹配】

```http
GET bank/_search
{
  "query":{
    "multi_match": {
      "query": "mill",
      "fields": ["address","city"]
    }
  }
}
```

##### 6、bool 【复合查询】

bool 用来做复合查询

复合语句可以合并 任何 其他嵌套语句，包括复合语句，了解这一点是很重要的，这就意味着，复合语句之间可以互相嵌套，可以表达式非常复杂的逻辑

- must：必须达到 must 列举的所有条件

```http
GET /bank/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "gender": "M"
          }
        },
        {
          "match": {
            "address": "mill"
          }
        }
      ],
      "must_not": [
        {"match":{
          "age":"18"
        }}
      ],
      "should": [
        {"match": {
          "lastname": "Wallace"
        }}
      ]
    }
  }
}
```

- should:应该达到 should 列举的条件，如果达到会增加相关文档的评分，并不会改变查询的结果，如果 query 中只有 should 且只有一种匹配规则，那么 should的条件就会被作为默认匹配条件而区改变查询结果

```json
 "should": [
        {"match": {
          "lastname": "Wallace"
        }}
      ]
```

- must_not 必须不是指定的情况

```json
"must_not": [
        {"match":{
          "age":"18"
        }}
      ],
```

![image-20201026052225903](/image-20201026052225903.png)

##### 7、filter【结果过滤】

并不是所有的查询都需要产生分数，特别是那些仅仅用于 filtering（过滤）的文档，为了不计算分数 ES 会自动检查场景并且优化查询的执行

```http
GET /bank/_search
{
  "query": {
    "bool": {
      "must": { "match_all": {} },
      "filter": {
        "range": {
          "balance": {
            "gte": 20000,
            "lte": 30000
          }
        }
      }
    }
  }
}
```

##### 8、term

和 match 一样，匹配某个属性的值，全文检索字段用 match，其他非text字段匹配用 term

```http
GET bank/_search
{
  "query":{
    "match_phrase": {
      "address": "789 Madison Street"
    }
  }
}
```

##### 9、aggregations（执行聚合）

聚合提供了从数据分组和提取数据的能力，最简单的聚合方法大致等于 SQL GROUP BY 和 SQL 聚合函数，在 ES 中，你有执行搜索返回 hits （命中结果） 并且同时返回聚合结果，把一个响应中的所有 hits（命中结果）分隔开的能力，这是非常强大有效的，你可以执行查询和多个聚合，并且在一个使用中得到各自的（任何一个的）返回结果，使用一次简洁简化的 API 来避免网络往返

**搜索address中包含mill的所有人的年龄分布以及平均年龄**

```http
GET bank/_search
{
  "query": {
    "match": {
      "address": "mill"
    }
  },
  "aggs": {
    "ageAgg": {
      "terms": {
        "field": "age",
        "size": 10
      }
    },
    "ageAvg":{
      "avg": {
        "field": "age"
      }
    },
    "balanceAvg":{
      "avg": {
        "field": "balance"
      }
    }
    
  },
  "size":0
}
```

**按照年龄聚合，并且请求这些年龄段的这些人的平均薪资**

```java
GET bank/_search
{
  "query":{
    "match_all": {}
  },
  "aggs": {
    "ageAgg": {
      "terms": {
        "field": "age",
        "size": 10
      },
      "aggs": {
        "ageAvg": {
          "avg": {
            "field": "balance"
          }
        }
      }
    }
  }
}
```

**查出所有年龄分布，并且这些年龄段中M的平均薪资和 F 的平均薪资以及这个年龄段的总体平均薪资**

```java
GET /bank/_search
{
  "query": {
    "match_all": {}
  },
  "aggs": {
    "ageAgg": {
      "terms": {
        "field": "age",
        "size": 100
      },
      "aggs": {
        "genderAgg": {
          "terms": {
            "field": "gender.keyword",
            "size": 10
          },
          "aggs": {
            "balanceAvg": {
              "avg": {
                "field": "balance"
                }
            }
          }
        }
      }
    }
  }
}
```

#### 1.4.3 Mapping

##### 1、字段类型

![image-20201026074813810](/image-20201026074813810.png)

![image-20201026074841875](/image-20201026074841875.png)

##### 2、映射

Mapping（映射）

Mapping 是用来定义一个文档（document）,以及他所包含的属性（field）是如何存储索引的，比如使用 mapping来定义的：

- 哪些字符串属性应该被看做全文本属性（full text fields）
- 那些属性包含数字，日期或者地理位置
- 文档中的所有属性是能被索引（_all 配置）
- 日期的格式
- 自定义映射规则来执行动态添加属性

查看 mapping 信息

GET bank/_mapping

修改 mapping 信息

https://www.elastic.co/guide/en/elasticsearch/reference/7.10/mapping-types.html

自动猜测的映射类型

![image-20201026075424198](/image-20201026075424198.png)

##### 3、新版本改变

- 关系型数据库中两个数据表示是独立的，即使他们里面有相同名称的列也不影响使用，但ES中不是这样的。elasticsearch 是基于Lucene开发的搜索引擎，而ES中不同type下名称相同的filed 最终在Lucene,中的处理方式是一样的。

- 两个不同 type下的两个user_ name, 在ES同-个索引下其实被认为是同一一个filed,你必须在两个不同的type中定义相同的filed映射。否则，不同typpe中的相同字段称就会在处理中出现神突的情况，导致Lucene处理效率下降。
- 去掉type就是为了提高ES处理数据的效率。

ES 7.x 

URL 中的 type 参数 可选，比如索引一个文档不再要求提供文档类型

ES 8.X

不在支持 URL 中的 type 参数

解决：

1、将索引从多类型迁移到单类型，每种类型文档一个独立的索引

2、将已存在的索引下的类型数据，全部迁移到指定位置即可，详见数据迁移

**1、创建映射**

```http
PUT /my_index
{
  "mappings":{
    "properties": {
      "age":{"type":"integer"},
      "email":{"type":"keyword"}
    }
  }
}
```

**2、添加新的字段映射**

```http
PUT /my_index/_mapping
{
  "properties":{
    "employeeid":{
      "type":"keyword",
      "index":false
    }
  }
}
```

##### 3、更新映射

对于已经存在的映射字段，我们不能更新，更新必须创建新的索引进行数据迁移

**4、数据迁移**

先创 new_twitter 的正确映射，然乎使用如下方式进行数据迁移

```http
POST _reindex [固定写法]
{
  "source":{
    "index":"twitter"
  },
  "dest":{
    "index":"new_twitter"
  }
}
## 将旧索引的 type 下的数据进行迁移
POST _reindex
{
  "source": {
    "index":"twitter",
    "type":"tweet"
  },
  "dest":{
    "index":"twweets"
  }
}
```

参考官网：https://www.elastic.co/guide/en/elasticsearch/reference/7.10/mapping-types.html

参数映射规则：https://www.elastic.co/guide/en/elasticsearch/reference/7.10/mapping-params.html#mapping-params

#### 1.4.4 分词

一个 **tokenizer**（分词器）接收一个字符流，将之分割为独立的 **tokens**（词元，通常是独立的单词），然后输出 **token** 流

列如，witespace tokenizer 遇到的空白字符时分割文本，它会将文本 “Quick brown fox” 分割为 【Quick brown fox】

该 **tokenizer** (分词器)还负责记录各个**term** (词条)的顺序或 **position** 位置(用于**phrase**短语和**word** proximity词近邻查询)，以及

**term** (词条)所代表的原始 **word** (单词)的start(起始)和end (结束)的 **character** **offsets** (字符偏移量) (用于 高亮显示搜索的内容)。

**Elasticsearch** 提供了很多内置的分词器，可以用来构建custom analyzers(自定义分词器)

##### 1、安装 ik 分词器

注意:不能用默认的 elasticsearch-plugin.install xxx.zip 进行自动安装

https://github.com/medcl/elasticsearch-analysis-ik/releases 下载与 es对应的版本

安装后拷贝到 plugins 目录下

##### 2、测试

分词器

![image-20201026092255250](/image-20201026092255250.png)

**3、自定义词库**

修改 /usr/share/elasticsearch/plugins/ik/config/中的 IKAnalyzer.cfg.xml

/usr/share/elasticsearch/plugins/ik/config

```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
        <comment>IK Analyzer 扩展配置</comment>
        <!--用户可以在这里配置自己的扩展字典 -->
        <entry key="ext_dict"></entry>
         <!--用户可以在这里配置自己的扩展停止词字典-->
        <entry key="ext_stopwords"></entry>
        <!--用户可以在这里配置远程扩展字典 -->
         <entry key="remote_ext_dict">http://192.168.56.10/es/fenci.txt</entry>
        <!--用户可以在这里配置远程扩展停止词字典-->
        <!-- <entry key="remote_ext_stopwords">words_location</entry> -->
</properties>

```



### 1.5 Elasticsearch - Rest - client

1、9300：TCP

Spring-data-elasticsearch:transport-api.jar

SpringBoot版本不同，`transport-api.jar` 不同，不能适配 es 版本

7.x 已经不在适合使用，8 以后就要废弃

**2、9200：HTTP**

JestClient 非官方，更新慢

RestTemplate:默认发送 HTTP 请求，ES很多操作都需要自己封装、麻烦

HttpClient：同上

Elasticsearch - Rest - Client：官方RestClient，封装了 ES 操作，API层次分明

最终选择 Elasticsearch - Rest - Client （elasticsearch - rest - high - level - client）

#### 1.5.1 SpringBoot 整合

1、Pom.xml

```xml
<!-- 导入es的 rest-high-level-client -->
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
    <version>7.4.2</version>
</dependency>
```

为什么要导入这个？这个配置那里来的？

官网：https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high-getting-started-maven.html

#### 1.5.2 Config配置

```java
/**
 * @author gcq
 * @Create 2020-10-26
 *
 * 1、导入配置
 * 2、编写配置，给容器注入一个RestHighLevelClient
 * 3、参照API 官网进行开发
 */
@Configuration
public class GulimallElasticsearchConfig {


    public static final RequestOptions COMMON_OPTIONS;
    static {
        RequestOptions.Builder builder = RequestOptions.DEFAULT.toBuilder();
//        builder.addHeader("Authorization", "Bearer " + TOKEN);
//        builder.setHttpAsyncResponseConsumerFactory(
//                new HttpAsyncResponseConsumerFactory
//                        .HeapBufferedResponseConsumerFactory(30 * 1024 * 1024 * 1024));
        COMMON_OPTIONS = builder.build();
    }



    @Bean
    public RestHighLevelClient esRestClient() {
        RestClientBuilder builder = null;
        builder = RestClient.builder(new HttpHost("192.168.56.10", 9200, "http"));

        RestHighLevelClient client = new RestHighLevelClient(builder);
//        RestHighLevelClient client = new RestHighLevelClient(
//                RestClient.builder(
//                        new HttpHost("localhost", 9200, "http"),
//                        new HttpHost("localhost", 9201, "http")));
        return client;
    }

}
```

官网：https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high-getting-started-initialization.html

#### 1.5.3 使用

> 测试是否注入成功

```java
@Autowired
private RestHighLevelClient client;

@Test
public void contextLoads() {
    System.out.println(client);
}
```

> 测试是否能 添加 或更新数据

```java
/**
 * 添加或者更新
 * @throws IOException
 */
@Test
public void indexData() throws IOException {
    IndexRequest indexRequest = new IndexRequest("users");
    User user = new User();
    user.setAge(19);
    user.setGender("男");
    user.setUserName("张三");
    String jsonString = JSON.toJSONString(user);
    indexRequest.source(jsonString,XContentType.JSON);

    // 执行操作
    IndexResponse index = client.index(indexRequest, GulimallElasticsearchConfig.COMMON_OPTIONS);

    // 提取有用的响应数据
    System.out.println(index);
}
```

测试复杂检索

```java
 @Test
    public void searchTest() throws IOException {
        // 1、创建检索请求
        SearchRequest searchRequest = new SearchRequest();
        // 指定索引
        searchRequest.indices("bank");
        // 指定 DSL，检索条件
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();

        sourceBuilder.query(QueryBuilders.matchQuery("address", "mill"));

        //1、2 按照年龄值分布进行聚合
        TermsAggregationBuilder aggAvg = AggregationBuilders.terms("ageAgg").field("age").size(10);
        sourceBuilder.aggregation(aggAvg);

        //1、3 计算平均薪资
        AvgAggregationBuilder balanceAvg = AggregationBuilders.avg("balanceAvg").field("balance");
        sourceBuilder.aggregation(balanceAvg);


        System.out.println("检索条件" + sourceBuilder.toString());

        searchRequest.source(sourceBuilder);

        // 2、执行检索
        SearchResponse searchResponse = client.search(searchRequest, GulimallElasticsearchConfig.COMMON_OPTIONS);

        // 3、分析结果
        System.out.println(searchResponse.toString());

        // 4、拿到命中得结果
        SearchHits hits = searchResponse.getHits();
        // 5、搜索请求的匹配
        SearchHit[] searchHits = hits.getHits();
        // 6、进行遍历
        for (SearchHit hit : searchHits) {
            // 7、拿到完整结果字符串
            String sourceAsString = hit.getSourceAsString();
            // 8、转换成实体类
            Accout accout = JSON.parseObject(sourceAsString, Accout.class);
            System.out.println("account:" + accout );
        }

        // 9、拿到聚合
        Aggregations aggregations = searchResponse.getAggregations();
//        for (Aggregation aggregation : aggregations) {
//
//        }
        // 10、通过先前名字拿到对应聚合
        Terms ageAgg1 = aggregations.get("ageAgg");
        for (Terms.Bucket bucket : ageAgg1.getBuckets()) {
            // 11、拿到结果
            String keyAsString = bucket.getKeyAsString();
            System.out.println("年龄:" + keyAsString);
            long docCount = bucket.getDocCount();
            System.out.println("个数：" + docCount);
        }
        Avg balanceAvg1 = aggregations.get("balanceAvg");
        System.out.println("平均薪资：" + balanceAvg1.getValue());
        System.out.println(searchResponse.toString());
    }
```

结果：

```java
accout:GulimallSearchApplicationTests.Accout(account_number=970, balance=19648, firstname=Forbes, lastname=Wallace, age=28, gender=M, address=990 Mill Road, employer=Pheast, email=forbeswallace@pheast.com, city=Lopezo, state=AK)accout:GulimallSearchApplicationTests.Accout(account_number=136, balance=45801, firstname=Winnie, lastname=Holland, age=38, gender=M, address=198 Mill Lane, employer=Neteria, email=winnieholland@neteria.com, city=Urie, state=IL)accout:GulimallSearchApplicationTests.Accout(account_number=345, balance=9812, firstname=Parker, lastname=Hines, age=38, gender=M, address=715 Mill Avenue, employer=Baluba, email=parkerhines@baluba.com, city=Blackgum, state=KY)accout:GulimallSearchApplicationTests.Accout(account_number=472, balance=25571, firstname=Lee, lastname=Long, age=32, gender=F, address=288 Mill Street, employer=Comverges, email=leelong@comverges.com, city=Movico, state=MT)年龄:38
个数:2
年龄:28
个数:1
年龄:32
个数:1
平均薪水:25208.0
```

总结：参考官网的API 和对应在 kibana 中发送的请求，在代码中通过调用对应API实现效果

官网：https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/java-rest-high-search.html#java-rest-high-search-request-optional

ELK  

Elasticsearch 用于检索数据

logstach：存储数据

Kiban:视图化查看数据

![image-20201027120603889](/image-20201027120603889.png)







# 2、商品上架

上架的商品才可以在网站展示

上架的商品需要可以检索

### 2.1 商品Mapping

分析：商品上架在 es 中是存入 sku 还是 spu ？

1、检索的时候输入名字，是需要按照 sku 的 title进行全文检索的

2、检索使用商品规格，规格是 spu 的公共属性，每个 spu 是一样的

3、按照分类 id 进去的 都是直接列出 spu的，还可以切换

4、我们如果将 sku 的 全量信息 保存在 es 中 （包括 spu 属性），就太多量字段了

5、如果我们将 spu 以及他包含的 sku 信息保存到 es 中，也可以方



