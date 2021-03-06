---
title: "Power BI Desktop での条件付きテーブルの書式設定"
description: "テーブルへのカスタマイズされた書式の適用"
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
LocalizationGroup: Create reports
ms.openlocfilehash: 30be54ccbc6f0ccdc322070d59ccb44de6460b78
ms.sourcegitcommit: 88c8ba8dee4384ea7bff5cedcad67fce784d92b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/24/2018
---
# <a name="conditional-formatting-in-tables"></a>テーブルでの条件付き書式設定
テーブルで条件付き書式を使用すると、グラデーション カラーの使用など、セルの値に基づいてカスタマイズされたセルの背景色を指定できます。 条件付き書式にアクセスするには、Power BI Desktop の **[視覚化]** ウィンドウの **[フィールド]** ウェルで、書式設定する **[値]** ウェルの値の横にある下向きの矢印を選択します (または、フィールドを右クリックします)。 条件付き書式を管理できるのは、**[フィールド]** ウェルの **[値]** 領域のフィールドだけです。

![](media/desktop-conditional-table-formatting/table-formatting_1.png)

表示されるダイアログ ボックスでは、色や、*最小*と*最大*の値を構成できます。 **[左右逆方向]** ボックスをオンにした場合は、オプションの*中央*値も構成できます。

![](media/desktop-conditional-table-formatting/table-formatting_2.png)

テーブルに適用すると、条件付きで書式設定されたセルに適用されているすべてのカスタム テーブル スタイルが、上記の手順で適用したカスタマイズされた書式で上書きされます。

![](media/desktop-conditional-table-formatting/table-formatting_3.png)

視覚化から条件付き書式を削除するには、再度フィールドを右クリックし、**[条件付き書式の削除]** を選択します。

![](media/desktop-conditional-table-formatting/table-formatting_4.png)

