# 5.1 新增文档

我们需要将数据库中的商品信息导入Elasticsearch中，而不再使用假数据。

## 5.1.1 实体类

由于索引库结构与数据库结构存在一些差异，我们需要定义一个与索引库结构对应的实体类。

在`hm-service`模块的`com.hmall.item.domain.dto`包中，定义一个新的DTO：

```java
package com.hmall.item.domain.po;

import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import lombok.Data;

import java.time.LocalDateTime;

@Data
@ApiModel(description = "索引库实体")
public class ItemDoc {

    @ApiModelProperty("商品id")
    private String id;

    @ApiModelProperty("商品名称")
    private String name;

    @ApiModelProperty("价格（分）")
    private Integer price;

    @ApiModelProperty("商品图片")
    private String image;

    @ApiModelProperty("类目名称")
    private String category;

    @ApiModelProperty("品牌名称")
    private String brand;

    @ApiModelProperty("销量")
    private Integer sold;

    @ApiModelProperty("评论数")
    private Integer commentCount;

    @ApiModelProperty("是否是推广广告，true/false")
    private Boolean isAD;

    @ApiModelProperty("更新时间")
    private LocalDateTime updateTime;
}
```
## 5.1.2 API语法
``` json
POST /{索引库名}/_doc/1
{
    "name": "Jack",
    "age": 21
}
```

1. **创建Request对象**，这里是`IndexRequest`，因为添加文档就是创建倒排索引的过程。
2. **准备请求参数**，本例中就是Json文档。
3. **发送请求**。

# 5.2 查询文档

我们以根据id查询文档为例。

## 5.2.1 语法说明

查询的请求语句如下：

```json
GET /{索引库名}/_doc/{id}
```
与之前的流程类似，代码大概分为2步：

- 创建Request对象
- 准备请求参数，这里是无参，直接省略。
  发送请求

# 5.3 删除文档

删除的请求语句如下：

```json
DELETE /hotel/_doc/{id}
```
与查询相比，仅仅是请求方式从DELETE变成GET。可以想象Java代码应该依然是2步走：

- 准备Request对象，因为是删除，这次是DeleteRequest对象。要指定索引库名和id。
- 准备参数，无参，直接省略。
- 发送请求。因为是删除，所以是client.delete()方法。

# 5.4 修改文档

修改文档我们讲过两种方式：
- **全量修改**：本质是先根据id删除，再新增。
- **局部修改**：修改文档中的指定字段值。

在RestClient的API中，全量修改与新增的API完全一致，判断依据是ID：
- 如果新增时，ID已经存在，则修改。
- 如果新增时，ID不存在，则新增。

与之前类似，也是三步走：
- 1）准备Request对象。这次是修改，所以是UpdateRequest
- 2）准备参数。也就是JSON文档，里面包含要修改的字段
- 3）更新文档。这里调用client.update()方法 
