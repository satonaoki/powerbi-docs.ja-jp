---
title: "Azure SQL Database のスケジュール設定された更新に関するトラブルシューティング"
description: "Power BI における Azure SQL Database のスケジュール設定された更新に関するトラブルシューティング"
services: powerbi
documentationcenter: 
author: markingmyname
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
LocalizationGroup: Troubleshooting
ms.openlocfilehash: 5f699ed013f35edd4f958b3c958e56ebeb05029c
ms.sourcegitcommit: 88c8ba8dee4384ea7bff5cedcad67fce784d92b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/24/2018
---
# <a name="troubleshooting-scheduled-refresh-for-azure-sql-databases-in-power-bi"></a>Power BI における Azure SQL Database のスケジュール設定された更新に関するトラブルシューティング
スケジュール設定された更新を設定するための手順の詳細については、「[Power BI でのデータの更新](refresh-data.md)」をご確認ください。

Azure SQL Database のスケジュール設定された更新を設定していて、資格情報の編集中にエラー (エラー コード 400) が発生した場合は、次の操作を実行して適切なファイアウォール規則を設定します。

1. Azure の管理ポータルにログインします
2. 更新を構成している対象の Azure SQL サーバーに移動します
3. 利用できるサービス セクションで 'Windows Azure サービス' をオンにします

![](media/service-admin-troubleshooting-scheduled-refresh-azure-sql-databases/azurerefresh.png)  

他にわからないことがある場合は、 [Power BI コミュニティを利用してください](http://community.powerbi.com/)。

