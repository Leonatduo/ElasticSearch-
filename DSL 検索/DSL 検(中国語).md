# 1. DSL查询文档

Elasticsearch的查询依然是基于JSON风格的DSL来实现的。

## 1.1. DSL查询分类

Elasticsearch提供了基于JSON的DSL（Domain Specific Language）来定义查询。常见的查询类型包括：

- **查询所有**：查询出所有数据，一般测试用。例如：`match_all`
- **全文检索（full text）查询**：利用分词器对用户输入内容分词，然后去倒排索引库中匹配。例如：
    - `match_query`
    - `multi_match_query`
- **精确查询**：根据精确词条值查找数据，一般是查找`keyword`、数值、日期、`boolean`等类型字段。例如：
    - `ids`
    - `range`
    - `term`
- **地理（geo）查询**：根据经纬度查询。例如：
    - `geo_distance`
    - `geo_bounding_box`
- **复合（compound）查询**：复合查询可以将上述各种查询条件组合起来，合并查询条件。例如：
    - `bool`
    - `function_score`

查询的语法基本一致：

```json
GET /indexName/_search
{
  "query": {
    "查询类型": {
      "查询条件": "条件值"
    }
  }
}
```
我们以查询所有为例，其中：

查询类型为match_all
没有查询条件
json
复制
编辑
// 查询所有
```json
GET /indexName/_search
{
  "query": {
    "match_all": {}
  }
}
```
## 1.2. 全文检索查询

### 1.2.1. 使用场景

全文检索查询的基本流程如下：

1. 对用户搜索的内容做分词，得到词条
2. 根据词条去倒排索引库中匹配，得到文档id
3. 根据文档id找到文档，返回给用户

比较常用的场景包括：

- 商城的输入框搜索
- 百度输入框搜索

### 1.2.2. 基本语法

常见的全文检索查询包括：

- **match查询**：单字段查询
- **multi_match查询**：多字段查询，任意一个字段符合条件就算符合查询条件

**match查询语法**如下：

```json
GET /indexName/_search
{
  "query": {
    "match": {
      "FIELD": "TEXT"
    }
  }
}
```
multi_match查询语法如下：
```json
GET /indexName/_search
{
  "query": {
    "multi_match": {
      "query": "TEXT",
      "fields": ["FIELD1", "FIELD2"]
    }
  }
}
```
## 1.3. 精确查询

精确查询一般是查找`keyword`、数值、日期、`boolean`等类型字段。所以不会对搜索条件分词。常见的有：

- **term**：根据词条精确值查询
- **range**：根据值的范围查询

### 1.3.1. term查询

因为精确查询的字段是**不分词**的字段，因此查询的条件也必须是不分词的词条。查询时，用户输入的内容跟自动值完全匹配时才认为符合条件。如果用户输入的内容过多，反而搜索不到数据

**语法说明**：

```json
// term查询
GET /indexName/_search
{
  "query": {
    "term": {
      "FIELD": {
        "value": "VALUE"
      }
    }
  }
}
```
match查询语法（精确匹配）：
```json
GET /indexName/_search
{
  "query": {
    "match": {
      "FIELD": "TEXT"
    }
  }
}
```

### 1.3.2. range查询

范围查询，一般应用在对数值类型做范围过滤的时候。比如做价格范围过滤

**语法说明**：

```json
// term查询
GET /indexName/_search
{
  "query": {
    "term": {
      "FIELD": {
        "value": "VALUE"
      }
    }
  }
}
```
### 1.3.3. 总结
精确查询常见的有哪些？

- term查询：根据词条精确匹配，一般搜索keyword类型、数值类型、布尔类型、日期类型字段
- range查询：根据数值范围查询，可以是数值、日期的范围
