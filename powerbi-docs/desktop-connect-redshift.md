---
title: "Power BI Desktop で Amazon Redshift データベースに接続する"
description: "Power BI Desktop で Amazon Redshift データベースに簡単に接続して使用する"
services: powerbi
documentationcenter: 
author: davidiseminger
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
ms.date: 12/06/2017
ms.author: davidi
LocalizationGroup: Connect to data
ms.openlocfilehash: de0e6b61197bfe25048a2722d5aab42f1c15e999
ms.sourcegitcommit: 88c8ba8dee4384ea7bff5cedcad67fce784d92b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/24/2018
---
# <a name="connect-to-amazon-redshift-in-power-bi-desktop"></a>Power BI Desktop で Amazon Redshift に接続する
**Power BI Desktop** では、**Amazon Redshift** データベースに接続し、Power BI Desktop の他のデータ ソースの場合と同様に基になっているデータを使用できます。

## <a name="connect-to-an-amazon-redshift-database"></a>Amazon Redshift データベースに接続する
**Amazon Redshift** データベースに接続するには、Power BI Desktop の **[ホーム]** リボンで **[データの取得]** を選択します。 左側のカテゴリから **[データベース]** を選択します。**[Amazon Redshift]** が表示されます。

![](media/desktop-connect-redshift/connect_redshift_3.png)

表示された **[Amazon Redshift]** ウィンドウ内のボックスに、**Amazon Redshift** サーバーとデータベースの名前を入力するか、貼り付けます。 *[サーバー]* フィールドの一部として、*ServerURL:Port* の形式でポートを指定できます。

![](media/desktop-connect-redshift/connect_redshift_4.png)

プロンプトが表示されたら、ユーザー名とパスワードを入力します。

![](media/desktop-connect-redshift/connect_redshift_5.png)

接続が正常に行われたら、**[ナビゲーター]** ウィンドウが開き、サーバー上で使用可能なデータが表示されます。その中から 1 つまたは複数の要素を選択し、**Power BI Desktop** にインポートして使用することができます。

![](media/desktop-connect-redshift/connect_redshift_6.png)

**[ナビゲーター]** ウィンドウで目的の選択を行うと、データの**読み込み**またはデータの**編集**を行うことができます。

* データの**読み込み**を選択した場合は、"*インポート*" モードまたは *DirectQuery* モードを使用してデータを読み込むように促されます。 詳細については、DirectQuery を説明している[こちらの記事](desktop-use-directquery.md)を参照してください。
* データの**編集**を選択した場合は、**クエリ エディター**が表示され、あらゆる種類の変換およびフィルターをデータに適用することができます。その多くは、基となる **Amazon Redshift** データベース自体 (サポートされている場合) に適用されます。

## <a name="next-steps"></a>次の手順
Power BI Desktop を使用して接続できるデータの種類は他にもあります。 データ ソースの詳細については、次のリソースを参照してください。

* [Power BI Desktop の概要](desktop-getting-started.md)
* [Power BI Desktop のデータ ソース](desktop-data-sources.md)
* [Power BI Desktop でのデータの整形と結合](desktop-shape-and-combine-data.md)
* [Power BI Desktop で Excel ブックに接続する](desktop-connect-excel.md)   
* [Power BI Desktop にデータを直接入力する](desktop-enter-data-directly-into-desktop.md)   

