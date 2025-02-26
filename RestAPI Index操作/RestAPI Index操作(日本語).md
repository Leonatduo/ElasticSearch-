# 4.1 RestClientの初期化

Elasticsearchが提供するAPIでは、すべての操作がRestHighLevelClientクラスにカプセル化されており、このオブジェクトを初期化してElasticsearchとの接続を確立する必要があります。

## 3つのステップに分かれます：

### 1）item-serviceモジュールにESのRestHighLevelClient依存関係を追加：
```xml
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
</dependency>
```
### 2）SpringBootのデフォルトESバージョンは7.17.10ですが、これを上書きする必要があります：
```xml
<properties>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
    <elasticsearch.version>7.12.1</elasticsearch.version>
</properties>
```

### 3）RestHighLevelClientを初期化：
```xml
RestHighLevelClient client = new RestHighLevelClient(RestClient.builder(
        HttpHost.create("http://192.168.150.101:9200")
));
```

# 4.1 インデックスの作成

## 4.1.1.Mappingの設定
```json
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

## 4.1.2 インデックスの作成
代码分为三步：
コードは3つのステップに分かれます：

- 1）Requestオブジェクトを作成：
- インデックス作成操作のため、RequestはCreateIndexRequestです。
- 2）リクエストパラメータを追加：
- これはMapping設定をJSON形式で追加するものです。JSON文字列が長いため、コードをより優雅にするためにMAPPING_TEMPLATEという静的文字列定数を定義します。
- 3）リクエストを送信：
- client.indices()メソッドの返り値はIndicesClient型で、インデックスに関するすべての操作（インデックス作成、削除、存在確認など）を行うメソッドが含まれています。
 
## 4.1.3 インデックスの削除
```json
DELETE /hotel
```
インデックス作成と比べて：

- リクエスト方法がPUTからDELETEに変わります。
- リクエストパスはそのままです。
- リクエストパラメータはありません。

## 4.1.4 インデックスが存在するか確認
```json
GET /hotel
```
