---
title: "Power BI でテンプレート コンテンツ パックを作成する"
description: "テンプレート コンテンツ パックのオーサリング"
services: powerbi
documentationcenter: 
author: guyinacube
manager: kfile
backup: 
editor: 
tags: 
qualityfocus: no
qualitydate: 
ms.service: powerbi
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 10/09/2017
ms.author: asaxton
ms.openlocfilehash: 332b8eb7087bbf70559a1e63b0736c9cb9289349
ms.sourcegitcommit: 284b09d579d601e754a05fba2a4025723724f8eb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/15/2017
---
# <a name="author-template-content-packs-in-power-bi"></a>Power BI でテンプレート コンテンツ パックを作成する
Power BI Desktop と PowerBI.com を使用するテンプレート コンテンツ パックをオーサリングします。コンテンツ パックには 4 つのコンポーネントがあります。

* クエリによって、データへの[接続](../desktop-connect-to-data.md)と[変換](../desktop-query-overview.md)ができるだけでなく、[パラメーター](https://powerbi.microsoft.com/blog/deep-dive-into-query-parameters-and-power-bi-templates/)も定義することができます  
* [リレーションシップ](../desktop-create-and-manage-relationships.md)、[メジャー](../desktop-measures.md)、および Q&A の機能強化を作成するためのデータ モデル  
* レポート [ページ](../desktop-report-view.md)には、データに対する洞察を提供するためのビジュアルとフィルターが含まれます  
* [ダッシュボード](../service-dashboards.md)と[タイル](../service-dashboard-create.md)は、含まれている洞察の概要を提供します  

既存の Power BI 機能など、各部分について精通しているかもしれません。 コンテンツ パックを構築する場合に、各側面について考慮すべき追加事項があるなら、詳細については以下の各セクションを参照してください。

<a name="queries"></a>

## <a name="queries"></a>クエリ
テンプレート コンテンツ パックの場合、Power BI Desktop で開発されたクエリは、データ ソースへの接続とデータのインポートのために使用されます。 これらのクエリは一貫したスキーマを返すために必要とされ、スケジュールされたデータ更新のためにサポートされています (直接クエリはサポートされていません)。

テンプレート コンテンツ パックは、コンテンツ パックごとに 1 つのデータ ソースをサポートするだけなので、クエリを慎重に定義します。 1 つのデータ ソースは、同じ認証を必要とするソースとして定義されます。 すべて同じ API エンドポイントを呼び出し、同じ認証を使用する場合、複数の API を異なるクエリで呼び出すことができます。 Power BI コンテンツ パックは、異なる認証を必要とする複数のソースをサポートしません。

### <a name="connect-to-your-api"></a>API に接続する
始めに、Power BI Desktop から API に接続し、クエリの構築を開始できます。

Power BI Desktop の難しい設定のいらないデータ コネクタを利用し、API に接続できます。 Web データ コネクタ ([データの取得]、[Web]) を利用して Rest API に接続するか、OData コネクタ ([データの取得]、[OData]) を利用して OData フィードに接続できます。 API が基本認証に対応している場合にのみ、これらのコネクタは難しい設定なしで使用できます。

> [!NOTE]
> API で OAuth 2.0 や Web API キーなど、その他の種類の認証が利用される場合、Power BI Desktop が API に接続され、認証されるように、独自のデータ コネクタを作成する必要があります。 コンテンツ パックに独自のデータ コネクタを作成する方法については、[ここ](https://aka.ms/DataConnectors)にあるデータ コネクタ ドキュメントを参照してください。 
> 
> 

### <a name="consider-the-source"></a>ソースを検討する
クエリは、データ モデルに含まれるデータを定義します。 システムのサイズに応じて、顧客がビジネス シナリオに合った管理可能なサイズを扱っていることを確認するために、これらのクエリにフィルターを含める必要があります。

Power BI コンテンツ パックは、同時に複数のユーザーに対して複数のクエリを実行できます。  先にスロットリングと同時実行ストラテジーを計画し、コンテンツ パック フォールト トレランスを行う方法を尋ねます。

### <a name="schema-enforcement"></a>スキーマの施行
クエリがシステム内の変更や、モデルを分割できる更新時のスキーマでの変更に対して弾力があることを確認します。 ソースがいくつかのクエリに対して Null または欠落したスキーマの結果を返すことができる場合、空の表を返すことを考慮するか、ユーザーにとって意味のあるカスタム エラー メッセージをスローします。

### <a name="parameters"></a>パラメーター
Power BI Desktop の[パラメーター](https://powerbi.microsoft.com/blog/deep-dive-into-query-parameters-and-power-bi-templates/)により、ユーザーは入力値の提供が可能になります。その入力値は、ユーザーによって取得されるデータをカスタマイズするものです。 事前のパラメーターにより、詳細なクエリやレポートを構築する時間を費やした後での修正作業を回避できると考えられます。

> [!NOTE]
> テンプレート コンテンツ パックは現在、テキスト パラメーターのみをサポートします。 開発中に他のパラメーター型を使用できますが、[テスト](template-content-pack-testing.md#templates)部分の間にユーザーによって提供されるすべての値はリテラルとなります。
> 
> 

### <a name="additional-query-tips"></a>追加のクエリ ヒント
* すべての列が適切に入力されたことを確認する  
* 列にわかりやすい名前をつける (Q&A を参照してください)  
* 共有ロジックについては、関数またはクエリの使用を検討する  
* プライバシーのレベルは現在サービスでサポートされていない - プライバシーのレベルについてプロンプトが表示される場合、相対パスを使用するようクエリを再作成する必要が生じることがあります  

## <a name="data-model"></a>データ モデル
適切に定義されたデータ モデルにより、顧客は簡単かつ直感的にコンテンツ パックと対話できるようになります。 Power BI Desktop で、データ モデルを作成します。

> [!NOTE]
> 基本的なモデリング (型指定と列名) の多くは、[クエリ](#queries)で作業する必要があります。
> 
> 

### <a name="qa"></a>Q&A
モデリングはまた、Q&A が顧客に対してどれほど結果を提供できるかに影響します。 一般的に使用される列にシノニムを追加し、列が[クエリ](#queries)で適切に命名されていることを確認します。

### <a name="additional-data-model-tips"></a>追加のデータ モデルのヒント
* すべての値の列に適用される書式設定がある
    >[!NOTE]
    >種類はクエリで適用する必要があります。  
* すべてのメジャーに適用される書式設定がある  
* 既定の概要作成が設定されています。 特に該当する場合には、「概要作成しない」 (例えば一意の値の場合など)  
* 該当する場合、データのカテゴリが設定されている  
* 必要に応じて、リレーションシップが設定される  

## <a name="reports"></a>レポート
レポートのページでは、コンテンツ パックに含まれるデータへの追加の洞察を提供します。 レポートのページを使用して、コンテンツ パックが処理しようとしている主要なビジネス上の質問に回答します。 Power BI Desktop を使用して、レポートを作成します。

> [!NOTE]
> コンテンツ パックにただ 1 つのレポートしか含まれていない場合、シナリオの特定のセクションを呼び出すためにさまざまなページを利用することができます。
> 
> 

### <a name="additional-report-tips"></a>追加のレポートに関するヒント
* クロス フィルター処理のために、1 ページあたり 1 つ以上のビジュアルを使用する  
* ビジュアルを慎重に揃える (重複しない)  
* ページは「4:3」または「16:9」モードのレイアウトに設定する  
* 表示されるすべての集計は合理的な数値 (平均値、一意の値)  
* スライシングは合理的な結果を生成する  
* ロゴは、少なくとも上位のレポートに存在する  
* 要素は可能な限り、クライアントのカラー スキームにある  

<a name="dashboard"></a>

## <a name="dashboard"></a>ダッシュボード
ダッシュボードは、顧客にとってコンテンツ パックとの対話の主なポイントです。 これにはコンテンツの概要が含まれるはずですが、特にビジネス シナリオにとって重要なメトリックが含まれます。

テンプレート コンテンツ パックのためのダッシュボードを作成するには、[データを取得] から [ファイル] を介して PBIX をアップロードするだけ、または Power BI Desktop から直接発行するだけです。

> [!NOTE]
> テンプレート コンテンツ パックには現在、コンテンツ パックごとに 1 つのレポートとデータセットが必要です。 コンテンツ パックで使用されるダッシュボードに複数のレポートまたはデータセットからのコンテンツをピン留めしないでください。
> 
> 

### <a name="additional-dashboard-tips"></a>追加のダッシュボードのヒント
* ダッシュボード上のタイルが一貫性のあるようにピン留めされた場合に、同じテーマを保持する  
* テーマにロゴをピン留めすることで、顧客はどこからのパックかがわかる  
* ほとんどの画面解像度で動作する推奨のレイアウトは、5 ~ 6 の小タイル幅である  
* すべてのダッシュボード タイルには適切なタイトルおよびサブタイトルが必要  
* 垂直または水平方向など、さまざまなシナリオに対応するダッシュボードのグループ化を検討する  

<a name="restrictions"></a>

## <a name="summary-of-restrictions"></a>制限事項のまとめ
上記のセクションに記載されているように、現在テンプレート コンテンツ パックには、一連の制限があります。  

| サポートされている | "*サポートされていない*" |
| --- | --- |
| PBI のデスクトップに組み込まれているデータセット |"*他のコンテンツ パックまたは Excel ファイルなどの入力からのデータセット*" |
| クラウドの [スケジュールされたデータ更新] がサポートされているデータ ソース |"*直接クエリまたはオンプレム接続はサポートされていない*" |
| 適切な場所に一貫したスキーマまたはエラーを返すクエリ |"*動的スキーマまたはカスタム スキーマ*" |
| データセットごとに 1 つのデータ ソース |"*マッシュアップなどの複数のデータ ソースや、複数のデータ ソースとして検出された URL*" |
| テキスト タイプのパラメーター |"*その他のパラメーターの型 (date など) または "値の許可リスト"*" |
| 1 つのダッシュボード、レポートおよびデータセット |"*複数のダッシュボード、レポート、またはデータセット*" |

## <a name="next-step"></a>次の手順
[コンテンツ パックのテストと提出](template-content-pack-testing.md)
