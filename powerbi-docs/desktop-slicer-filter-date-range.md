---
title: "Power BI Desktop で相対日付のスライサーまたはフィルターを使用する"
description: "Power BI Desktop で相対的な日付範囲を制限するスライサーまたはフィルターを使う方法について説明します"
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
ms.date: 12/05/2017
ms.author: davidi
LocalizationGroup: Create reports
ms.openlocfilehash: 0432998e44cdb1bf95a41225b73d805ec2a2379f
ms.sourcegitcommit: 88c8ba8dee4384ea7bff5cedcad67fce784d92b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/24/2018
---
# <a name="use-a-relative-date-slicer-and-filter-in-power-bi-desktop"></a>Power BI Desktop で相対日付のスライサーおよびフィルターを使用する
**相対日付スライサー**または**相対日付フィルター**では、データ モデルの任意の日付列に時間ベースのフィルターを適用することができます。 たとえば、**相対日付スライサー**を使って、過去 30 日間 (または、1 か月、カレンダー月など) に発生した販売データのみを表示できます。 データを更新すると、相対期間が適切な相対日付制約を自動的に適用します。

![](media/desktop-slicer-filter-date-range/relative-date-range-slicer-filter_01.png)

## <a name="using-the-relative-date-range-slicer"></a>相対日付範囲スライサーの使用
他のスライサーと同じように相対日付スライサーを使用できます。 レポートの**スライサー** ビジュアルを作成し、**[フィールド]** 値の日付値を選択するだけです。 次の図では、*OrderDate* フィールドが選択されています。

![](media/desktop-slicer-filter-date-range/relative-date-range-slicer-filter_02.png)

**相対日付スライサー**の右上隅にあるカラットを選択すると、メニューが表示されます。

![](media/desktop-slicer-filter-date-range/relative-date-range-slicer-filter_03.png)

相対日付スライサーの場合は、"*相対*" を選びます。

その後、設定を選択できます。 "*相対日付のスライサー*" の最初のドロップダウンで、次のいずれかを選択できます。

* 最後
* 次へ
* この

これらの選択を次の図に示します。

![](media/desktop-slicer-filter-date-range/relative-date-range-slicer-filter_04.png)

"*相対日付スライサー*" の次の (中央) 設定では、数字を入力して、相対的な日付範囲を定義できます。

3 番目の設定では日付の測定を選択でき、次のいずれかを選択できます。

* 日
* 週
* 週 (暦)
* 月
* 月 (暦)
* 年
* 年 (暦)

これらの選択を次の図に示します。

![](media/desktop-slicer-filter-date-range/relative-date-range-slicer-filter_05.png)

一覧から "*月*" を選択して中間の設定に 2 を入力した場合、 次のようになります。今日が 7 月 20 日の場合、スライサーによって制約されるビジュアルのデータは、5 月 20 日から年 7 月 20 日 (今日の日付) までの過去 2 か月間が表示されます。

一方、"*間 (暦)*" を選択すると、5 月 1 日から 6 月 30 日 (カレンダーで完全な過去 2 か月) のデータが表示されます。

## <a name="using-the-relative-date-range-filter"></a>相対日付範囲フィルターの使用
レポート ページまたはレポート全体の相対日付範囲フィルターを作成することもできます。 そのためには、次の図に示すように、**[フィールド]** ウィンドウの **[ページ レベル フィルター]** 領域または **[レポート レベル フィルター]** 領域に、日付フィールドをドラッグします。

![](media/desktop-slicer-filter-date-range/relative-date-range-slicer-filter_06.png)

その後は、**相対日付スライサー**のカスタマイズと同様の方法で、相対的な日付範囲を変更できます。 **[フィルターの種類]** ドロップダウンから **[相対日付フィルター]** を選びます。

![](media/desktop-slicer-filter-date-range/relative-date-range-slicer-filter_07.png)

**[相対日付フィルター]** を選ぶと、スライサーと同じように中間の数値ボックスなど 3 つのセクションを変更できます。

![](media/desktop-slicer-filter-date-range/relative-date-range-slicer-filter_08.png)

レポートで相対日付制約を使うのに必要なものはこれだけです。

## <a name="limitations-and-considerations"></a>制限事項と考慮事項
現在、**相対日付範囲スライサー**とフィルターには次の制限事項と考慮事項が適用されています。

* **Power BI** のデータ モデルには、タイム ゾーン情報は含まれません。 モデルは時間を保存できますが、タイム ゾーンの指定はありません。
* スライサーとフィルターは常に UTC の時刻に基づくので、レポートでフィルターを構成して別のタイム ゾーンの同僚に送ると、どちらも同じデータが表示されます。 ただし、UTC タイム ゾーンではない場合は、予想とは異なるタイム オフセットのデータが表示される可能性があります。
* ローカル タイム ゾーンでキャプチャされたデータは、**クエリ エディター**で UTC に変換できます。

