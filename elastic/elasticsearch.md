# elasticsearch 可伸缩的搜索数据库
## 什么是elasticsearch
    - 分布式的基于json的nosql数据库
    - 基于restfulAPI
    - 可以从logs metrics application中追踪数据
## 和关系型数据库的对比
    DB => indexes  
    tables => types
    rows => documents
    columns => fields

## 倒排索引
  elasticsearch的原理是将keyword存为查询条件，id为查询内容，再从查出的id找出对应的内容

> | id | name | 
> | :----:| :----: | 
> | 1001 | My name is Jack. | 
> | 1002 | My name is Sam. | 
---
> | keyword | id | 
> | :-----| ----: | 
> | Jack | 1001 | 
> | name | 1001,1002 |
## 启动elasticsearch
    windows
    在bin中点击elasticsearch.bat
    访问localhost:9200 
    若无法访问：在config/elasticsearch.yml 93行中找到
    ---
        xpack.security.http.ssl:
        enabled: false
        keystore.path: certs/http.p12

        # Enable encryption and mutual authentication between cluster nodes
        xpack.security.transport.ssl:
        enabled: false
    将两个enabled改为false
## 修改密码
    在cmd中：elasticsearch-reset-password -u elastic -i
    输入 y
    重复两次密码
## 索引操作
    创建索引相当于创建数据库
    PUT： http://localhost:9200/shopping
    创建了一个index，名为shopping
    ---
    获取名为shopping的索引
    GET： http://localhost:9200/shopping
    ---
    获取全部索引
    GET：http://localhost:9200/_cat/indices?v
    ---
    删除索引
    DELEET: http://localhost:9200/shopping
 ### 添加
    POST: http://localhost:9200/shopping/_doc
    将JSON数据发送到ES中(不是幂等的操作)
    此操作不能选择PUT，因为创建的index,ES会随机生成一个_id，每次添加生成的_id不一样
    ---
    自定义_id
    POST/PUT: http://localhost:9200/shopping/_doc/1001
    ---
    按照_id查询
    GET: http://localhost:9200/shopping/_doc/1001
    ---
    查询所有
    GET: http://localhost:9200/shopping/_search
### 修改
    两种方式：PUT，POST
    第一种：
    PUT: http://localhost:9200/shopping/_doc/1001 
    通过JSON传递数据，将所有字段更改
    ---
    第二种
    POST: http://localhost:9200/shopping/_update/1001
    可以只写要更改字段的JSON数据
### 删除
    DELETE：http://localhost:9200/shopping/_doc/1001
### 查询
    1.请求路径　　
    GET: http://localhost:9200/shopping/_search?q=category:param
    ---
    2.请求体查询
    GET: http://localhost:9200/shopping/_search
    
    "query":{
        "match":{
            "category": param
        }
    }
    --- 
    查询所有
    "query":{
        "match_all":{
           
        }
    }
    ---
    分页查询
    "query":{
        "match_all":{
           
        }
    }，
    "from": 0,//第1页（页码-1）*size
    "size": 2,//两条数据
    "_source": ["title"],//条件查询
    "sort": { //排序
        "price": {
            order:"desc"
        }
    }
### 条件查询，同时满足条件
    {
        "query":{
            "bool":{ //查询条件
                "must":[ //must:同时条件 
                    {
                        "match":{
                            "category":"小米"
                        }
                    },
                    {
                        "match":{
                            "price": 1999.0
                        }
                    }
                ]
            }
        }
    }
### 条件查询，多种匹配条件
    {
        "query":{
            "bool":{ //查询条件
                "should":[ //should:多个满足条件 
                    {
                        "match":{
                            "category":"小米"
                        }
                    },
                    {
                        "match":{
                            "category": "华为"
                        }
                    }
                ]
            }
        }
    }
### 范围查询
    {
        "query":{
            "bool":{ //查询条件
                "should":[ //should:多个满足条件 
                    {
                        "match":{
                            "category":"小米"
                        }
                    },
                    {
                        "match":{
                            "category": "华为"
                        }
                    }
                ],
                {
                    "range":{
                        "price":{
                            "gt": 5000
                        }
                    }
                }
            }
        }
    }
### 全文检索查询
    ES会将文字内容进行拆解，查询时会进行倒排索引的查询，使用两个不同的字组成的词也会把带有每个字的数据查询出来 如：“效公” 也会查询带有效和带有公的数据，防止这种现象要使用match_phrase替换match使用完全匹配
### 完全匹配查询
    {
        "query": {
            "match_phrase":{
                "category": //"小米"
            }
        },
        "highlight": {
            "category": {} //对查询内容进行高亮
        }
    }
### 聚合操作
    {
        "aggs": { //聚合操作
            "price_group":{ //名称，随便起
               "terms": { //分组
                    "fields": "price" //分组字段
               }
            }
        },
        "size": 0 //查出0条数据，不要原始数据
    }
    --- 
    平均值
    {
        "aggs": { //聚合操作
            "price_group":{ //名称，随便起
               "avg": { //分组
                    "fields": "price" //分组字段
               }
            }
        },
        "size": 0 //查出0条数据,只要平均值
    }
## 映射
    创建index
    PUT: http://localhost:9200/user
    PUT: http://localhost:9200/user/mapping
    {
        "properties": {
            "name" : {
                "type": "text",
                "index": true
            },
            "sex" : {
                "type": "keyword",
                "index": true
            },
            "tel" : {
                "type": "keyword",
                "index": false
            }
        }
    }
    ---
    增加数据
    PUT: http://localhost:9200/user_create/1001
    {
        "name": "Jack",
        "sex": "male",
        "tel": "123456"
    }
    ---
    查询
    GET: http://localhost:9200/user/_search
    {
        "query": {
            "match": {
                "name": "Ja" //可以被拆分
            }
        }
    }
    ---
    {
        "query": {
            "match": {
                "sex": "ma" //keyword只能完全匹配
            }
        }
    }
    ---
    {
        "query": {
            "match": {
                "tel": "123456" //没有使用index，所以不能被查询
            }
        }
    }
## 集成JAVA
    创建项目　elasticsearch-lesson
    添加依赖
    ---
    <dependencies>
        <dependency>
            <groupId>org.elasticsearch</groupId>
            <artifactId>elasticsearch</artifactId>
            <version>8.7.1</version>
        </dependency>
        <dependency>
            <groupId>org.elasticsearch.client</groupId>
            <artifactId>elasticsearch-rest-client</artifactId>
            <version>8.7.1</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.12.7.1</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.1</version>
        </dependency>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.20.0</version>
        </dependency>
    </dependencies>
    ---
### 创建ES客户端
    RestHighLevelClient esClient  = new RestHighLevelClient(RestClient.builder(new HttpHost("127.0.0.1",9200,"http")));
    可以使用try-with-resource或者手动关闭资源(esClient.close())
