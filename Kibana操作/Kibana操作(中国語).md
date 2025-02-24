# 2. 创建索引库

## 2.1 Mapping 映射属性

Mapping 是对索引库中文档的约束，常见的 mapping 属性包括：

- **type**：字段数据类型，常见的简单类型有：
  - 字符串：`text`（可分词的文本）、`keyword`（精确值，例如：品牌、国家、ip地址）
  - 数值：`long`、`integer`、`short`、`byte`、`double`、`float`
  - 布尔：`boolean`
  - 日期：`date`
  - 对象：`object`
  
- **index**：是否创建索引，默认为 `true`。
- **analyzer**：使用哪种分词器。
- **properties**：该字段的子字段。

## 2.2 索引库的 CRUD

这里使用 Kibana 编写 DSL 的方式来演示。

### 2.2.1 创建索引库和映射

基本语法：

- 请求方式：`PUT`
- 请求路径：`/索引库名`，可以自定义
- 请求参数：mapping 映射

```json
PUT /索引库名称
{
  "mappings": {
    "properties": {
      "字段名": {
        "type": "text",
        "analyzer": "ik_smart"
      },
      "字段名2": {
        "type": "keyword",
        "index": "false"
      },
      "字段名3": {
        "properties": {
          "子字段": {
            "type": "keyword"
          }
        }
      }
    }
  }
}
```

### 2.2.1 查询数据库

基本语法：

请求方式：GET
请求路径：/索引库名
请求参数：无

```json
GET /heima
```
### 2.2.2 删除数据库

语法：**

请求方式：DELETE

请求路径：/索引库名

请求参数：无

**格式：
```json
DELETE /索引库
```

# 3.文档操作
## 3.1.新增文档
语法：
```json
POST /索引库名/_doc/文档id
{
    "字段1": "值1",
    "字段2": "值2",
    "字段3": {
        "子属性1": "值3",
        "子属性2": "值4"
    },
    // ...
}
```
## 3.2修改文档
语法：
```json
PUT /heima/_doc/1
{
    "info": "黑马程序员高级Java讲师",
    "email": "zy@itcast.cn",
    "name": {
        "firstName": "云",
        "lastName": "赵"
    }
}

```
