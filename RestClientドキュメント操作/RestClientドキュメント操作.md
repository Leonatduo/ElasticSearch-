# 5.1 新規文書の追加

データベースから商品情報をElasticsearchにインポートする必要があり、ダミーデータは使用しません。

## 5.1.1 エンティティクラス

インデックス構造とデータベース構造にはいくつかの違いがあるため、インデックス構造に対応するエンティティクラスを定義する必要があります。

`hm-service`モジュールの`com.hmall.item.domain.dto`パッケージ内で新しいDTOを定義します：

```java
package com.hmall.item.domain.po;

import io.swagger.annotations.ApiModel;
import io.swagger.annotations.ApiModelProperty;
import lombok.Data;

import java.time.LocalDateTime;

@Data
@ApiModel(description = "インデックスライブラリエンティティ")
public class ItemDoc {

    @ApiModelProperty("商品ID")
    private String id;

    @ApiModelProperty("商品名")
    private String name;

    @ApiModelProperty("価格（分）")
    private Integer price;

    @ApiModelProperty("商品画像")
    private String image;

    @ApiModelProperty("カテゴリ名")
    private String category;

    @ApiModelProperty("ブランド名")
    private String brand;

    @ApiModelProperty("売上")
    private Integer sold;

    @ApiModelProperty("コメント数")
    private Integer commentCount;

    @ApiModelProperty("広告かどうか、true/false")
    private Boolean isAD;

    @ApiModelProperty("更新日時")
    private LocalDateTime updateTime;
}
```
## 5.1.2 API構文
```java
POST /{インデックス名}/_doc/1
{
    "name": "Jack",
    "age": 21
}
```

# 5.2 文書の検索
ここでは、IDに基づいて文書を検索する例を見ていきます。

## 5.2.1 構文説明
```java
GET /{インデックス名}/_doc/{id}
```
以前と同じように、コードは大きく2つのステップに分かれます：

- Requestオブジェクトを作成
- リクエストパラメータを準備（今回は引数なしで、省略します）
- リクエストを送信
  
# 5.3 文書の削除
削除リクエストの構文は以下の通りです：
```java
DELETE /hotel/_doc/{id}
```
検索と比べて、リクエスト方法がDELETEからGETに変わっただけです。Javaコードも2ステップで行うことができます：

- Requestオブジェクトを準備します。削除のため、今回はDeleteRequestオブジェクトを使います。インデックス名とIDを指定します。
- パラメータを準備します。引数なしで、省略します。
- リクエストを送信します。削除なので、client.delete()メソッドを使用します。
# 5.4 文書の更新
文書の更新には2つの方法があります：

- 全量更新：実際には、IDに基づいて削除してから再度追加します。
- 部分的更新：文書内の特定のフィールド値を変更します。
RestClientのAPIでは、全量更新と追加のAPIはまったく同じで、判断基準はIDです：

- 追加時にIDが既に存在する場合、更新します。
- 追加時にIDが存在しない場合、追加します。

以前と同じように、これも3つのステップで行います：

- Requestオブジェクトを準備します。今回は更新なので、UpdateRequestを使用します。
- パラメータを準備します。ここではJSON文書で、変更したいフィールドを含みます。
- 文書を更新します。ここではclient.update()メソッドを使用します。
