# 4.1 初始化RestClient

在 Elasticsearch 提供的 API 中，与 Elasticsearch 进行一切交互的操作都封装在一个名为 `RestHighLevelClient` 的类中，必须先完成这个对象的初始化，建立与 Elasticsearch 的连接。

## 分为三步：

### 1）在 `item-service` 模块中引入 ES 的 `RestHighLevelClient` 依赖：
```xml
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
</dependency>
```
### 2）因为 SpringBoot 默认的 ES 版本是 7.17.10，所以我们需要覆盖默认的 ES 版本：
```xml
<properties>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
    <elasticsearch.version>7.12.1</elasticsearch.version>
</properties>
```

### 3）初始化 RestHighLevelClient：
```xml
RestHighLevelClient client = new RestHighLevelClient(RestClient.builder(
        HttpHost.create("http://192.168.150.101:9200")
));

```

# 4.1 创建索引库

## 4.1.1.Mapping映射
``` json
PUT /items
{
  "mappings": {
    "properties": {
      "id": {
        "type": "keyword"
      },
      "name":{
        "type": "text",
        "analyzer": "ik_max_word"
      },
      "price":{
        "type": "integer"
      },
      "stock":{
        "type": "integer"
      },
      "image":{
        "type": "keyword",
        "index": false
      },
      "category":{
        "type": "keyword"
      },
      "brand":{
        "type": "keyword"
      },
      "sold":{
        "type": "integer"
      },
      "commentCount":{
        "type": "integer",
        "index": false
      },
      "isAD":{
        "type": "boolean"
      },
      "updateTime":{
        "type": "date"
      }
    }
  }
}
```

## 4.1.2 创建索引
代码分为三步：
- 1）创建Request对象。
  - 因为是创建索引库的操作，因此Request是CreateIndexRequest。
- 2）添加请求参数
  - 其实就是Json格式的Mapping映射参数。因为json字符串很长，这里是定义了静态字符串常量MAPPING_TEMPLATE，让代码看起来更加优雅。
- 3）发送请求
  - client.indices()方法的返回值是IndicesClient类型，封装了所有与索引库操作有关的方法。例如创建索引、删除索引、判断索引是否存在等
 
## 4.1.3 删除索引
``` json
DELETE /hotel
```
与创建索引库相比：
- 请求方式从PUT变为DELTE
- 请求路径不变
- 无请求参数

## 4.1.4 判断索引库是否存在
``` json
GET /hotel
```
