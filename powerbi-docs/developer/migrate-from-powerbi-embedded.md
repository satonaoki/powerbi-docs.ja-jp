---
title: "Power BI に Power BI Embedded ワークスペース コレクション コンテンツを移行する方法"
description: "Power BI Embedded から Power BI サービスに移行し、アプリでの埋め込みで先進機能を利用する方法について説明します。"
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
ms.date: 07/21/2017
ms.author: asaxton
ms.openlocfilehash: 430f1d1a49e510bac66c448b2dceaad1f2537073
ms.sourcegitcommit: 99cc3b9cb615c2957dde6ca908a51238f129cebb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/13/2017
---
# <a name="how-to-migrate-power-bi-embedded-workspace-collection-content-to-power-bi"></a>Power BI に Power BI Embedded ワークスペース コレクション コンテンツを移行する方法
Power BI Embedded から Power BI サービスに移行し、アプリでの埋め込みで先進機能を利用する方法について説明します。

最近、Microsoft は [Power BI Premium を発表](https://powerbi.microsoft.com/blog/microsoft-accelerates-modern-bi-adoption-with-power-bi-premium/)しました。この新しい容量ベースのライセンス モデルは、ユーザーによるコンテンツのアクセス、共有および配布方法の柔軟性を高めます。 また、Power BI サービスのスケーラビリティとパフォーマンスが向上します。

Power BI Premium の導入に伴い、Power BI Embedded および Power BI サービスが統合され、Power BI コンテンツをアプリにより迅速に埋め込むことができるようになります。 これは、1 つの API サーフェスで一貫性のある一連の機能を利用でき、コンテンツを埋め込む際にダッシュボード、ゲートウェイ、アプリ ワークスペースなどの最新の Power BI 機能にアクセスできることを意味します。 今後は、Power BI Desktop で作業を開始し、Power BI Premium でデプロイできるようになります。Power BI Premium は 2017 年の第 2 四半期の終わりに一般提供される予定です。

現在の Power BI Embedded サービスは、統合版が一般提供された後、期間限定で引き続き利用可能です。エンタープライズ契約されたお客様は現在の契約が満了するまでご利用いただけます。ダイレクト チャネルまたは CSP チャネル経由で Power BI Embedded を入手されたお客様は、Power BI Premium が一般提供されてから 1 年間はご利用いただけます。  この記事では、Azure サービスから Power BI サービスに移行するためのいくつかのガイダンスと、アプリケーションの変更について予想されることを示します。

> [!IMPORTANT]
> 移行操作と Power BI サービスに依存関係がある場合でも、**埋め込みトークン**を使用すれば、アプリケーションのユーザーは Power BI に依存することはありません。 ユーザーは、アプリケーションに埋め込まれたコンテンツを表示するために Power BI にサインアップする必要はありません。 この埋め込み方法を使用して、Power BI 以外のユーザーにサービスを提供することができます。
> 
> 

![](media/migrate-from-powerbi-embedded/powerbi-embed-flow.png)

## <a name="prepare-for-the-migration"></a>移行の準備をする
Power BI Embedded Azure サービスから Power BI サービスへの移行の準備のために行う必要がある作業がいくつかあります。 使用可能なテナントと、Power BI Pro ライセンスを持つユーザーが必要になります。

1. Azure Active Directory (Azure AD) テナントにアクセスできることを確認します。
   
    使用するテナントのセットアップを判別する必要があります。
   
   * 既存の企業の Power BI テナントを使用しますか。
   * アプリケーションで個別のテナントを使用しますか。
   * 顧客ごとに個別のテナントを使用しますか。
     
     アプリケーション、または顧客ごとに新しいテナントを作成することにした場合は、「[Azure Active Directory テナントを作成する](create-an-azure-active-directory-tenant.md)」または「[Azure Active Directory テナントを取得する方法](https://docs.microsoft.com/azure/active-directory/develop/active-directory-howto-tenant)」を参照してください。
2. アプリケーションの "マスター" アカウントとして機能する、この新しいテナントのユーザーを作成します。 そのアカウントで Power BI にサインアップする必要があります。また、このアカウントに Power BI Pro のライセンスが割り当てられている必要があります。

## <a name="accounts-within-azure-ad"></a>Azure AD 内のアカウント
次のアカウントがテナント内に存在する必要があります。

> [!NOTE]
> これらのアカウントでは、アプリ ワークスペースを使用するために、Power BI Pro ライセンスが必要になります。
> 
> 

1. テナント管理者ユーザー。
   
    このユーザーは、埋め込むために作成されたすべてのアプリ ワークスペースのメンバーにすることをお勧めします。
2. コンテンツを作成するアナリストのアカウント。
   
    これらのユーザーは、必要に応じて、アプリ ワークスペースに割り当てる必要があります。
3. アプリケーション *マスター* ユーザー アカウント、またはサービス アカウント。
   
    アプリケーション バックエンドにはこのアカウントの資格情報が格納され、Power BI REST API で使用する Azure AD トークンを取得するために使用されます。 このアカウントは、アプリケーションの埋め込みトークンを生成するために使用されます。 また、このアカウントは、埋め込むために作成されたアプリ ワークスペースの管理者にする必要があります。
   
   > [!NOTE]
   > これは、埋め込みの目的で使用される組織内の通常のユーザー アカウントです。
   > 
   > 

## <a name="app-registration-and-permissions"></a>アプリの登録とアクセス許可
Azure AD 内でアプリケーションを登録し、特定のアクセス許可を付与する必要があります。

### <a name="register-an-application"></a>アプリケーションを登録する
REST API の呼び出しを行うには、Azure AD にアプリケーションを登録する必要があります。 そのためには、Power BI アプリ登録ページだけでなく、Azure Portal に移動して追加構成を適用します。 詳しくは、「[Azure AD アプリを登録して Power BI コンテンツを埋め込む](register-app.md)」をご覧ください。

アプリケーションの**マスター** アカウントを使用してアプリケーションを登録する必要があります。

## <a name="create-app-workspaces-required"></a>アプリ ワークスペースを作成する (必須)
アプリケーションが複数の顧客にサービスを提供している場合、アプリ ワークスペースを利用することで、より適切に分離することができます。 ダッシュボードとレポートは顧客間で分離されます。 その後、アプリ ワークスペースごとに Power BI アカウントを使用して、顧客間でさらにアプリケーション エクスペリエンスを分離できます。

> [!IMPORTANT]
> Power BI 以外のユーザー向けに埋め込みを利用するために、個人用ワークスペースを使用することはできません。
> 
> 

Power BI 内でアプリ ワークスペースを作成するには、Pro ライセンスを持つユーザーが必要です。 アプリ ワークスペースを作成する Power BI ユーザーは、既定ではそのワークスペースの管理者になります。

> [!NOTE]
> アプリケーションの*マスター* アカウントは、ワークスペースの管理者である必要があります。
> 
> 

## <a name="content-migration"></a>コンテンツの移行
ワークスペース コレクションから Power BI サービスへのコンテンツの移行は、現在のソリューションと並行して行うことができ、ダウンタイムを必要としません。

Power BI Embedded から Power BI サービスにコンテンツをコピーする際に役立つ**移行ツール**を使用することができます。 これは特に、多くのコンテンツがある場合に役立ちます。 詳細については、「[Power BI Embedded 移行ツール](migrate-tool.md)」を参照してください。

コンテンツの移行は主に 2 つの API に依存します。

1. .pbix のダウンロード - この API では、2016 年 10 月以降に Power BI にアップロードされた PBIX ファイルをダウンロードできます。
2. .pbix のインポート - この API では Power BI に PBIX をアップロードします。

いくつかの関連するコード スニペットについては、「[Code snippets for migrating content from Power BI Embedded](migrate-code-snippets.md)」 (Power BI Embedded からコンテンツを移行するためのコード スニペット) を参照してください。

### <a name="report-types"></a>レポートの種類
レポートにはいくつかの種類があり、それぞれ若干異なる移行フローが必要になります。

#### <a name="cached-dataset--report"></a>キャッシュ データセットとレポート
キャッシュ データセットは、ライブ接続や DirectQuery 接続とは異なり、データをインポートした PBIX ファイルを指します。

**フロー**

1. PaaS ワークスペースからの .pbix のダウンロード API の呼び出しを行います。
2. PBIX を保存します。
3. SaaS ワークスペースへの .pbix のインポートの呼び出しを行います。

#### <a name="directquery-dataset--report"></a>DirectQuery データセットとレポート
**フロー**

1. GET https://api.powerbi.com/v1.0/collections/{collection_id}/workspaces/{wid}/datasets/{dataset_id}/Default.GetBoundGatewayDataSources を呼び出し、受信した接続文字列を保存します。
2. PaaS ワークスペースからの .pbix のダウンロード API の呼び出しを行います。
3. PBIX を保存します。
4. SaaS ワークスペースへの .pbix のインポートの呼び出しを行います。
5. POST https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/Default.SetAllConnections を呼び出して、接続文字列を更新します。
6. GET https://api.powerbi.com/v1.0/myorg/datasets/{dataset_id}/Default.GetBoundGatewayDataSources を呼び出して、GW ID とデータ ソースを取得します。
7. PATCH https://api.powerbi.com/v1.0/myorg/gateways/{gateway_id}/datasources/{datasource_id} を呼び出して、ユーザーの資格情報を更新します。

#### <a name="old-dataset--reports"></a>古いデータセットとレポート
これらは、2016 年 10 月より前に作成されたデータセット/レポートです。 .pbix のダウンロードでは、2016 年 10 月より前にアップロードされた PBIX はサポートされません。

**フロー**

1. 開発環境 (内部ソース管理) から PBIX を取得します。
2. SaaS ワークスペースへの .pbix のインポートの呼び出しを行います。

#### <a name="push-dataset--report"></a>プッシュ データセットとレポート
.pbix のダウンロードでは*プッシュ API* データセットはサポートされません。 プッシュ API データセット データを PaaS から SaaS に移植することはできません。

**フロー**

1. データセット Json で "データセットの作成" API を呼び出し、SaaS ワークスペースにデータセットを作成します。
2. 作成したデータセット用にレポートを再構築します*。

いくつかの回避策を使用して、PaaS から SaaS にプッシュ API レポートを移行することができます。その場合、以下の手順を試します。

1. ダミーの PBIX をいくつか PaaS ワークスペースにアップロードします。
2. プッシュ API レポートを複製し、それを手順 1. に示されているダミーの PBIX にバインドします。
3. ダミーの PBIX でプッシュ API レポートをダウンロードします。
4. SaaS ワークスペースにダミーの PBIX をアップロードします。
5. SaaS ワークスペースでプッシュ データセットを作成します。
6. プッシュ API データセットにレポートを再バインドします。

## <a name="create-and-upload-new-reports"></a>新しいレポートを作成してアップロードする
Power BI Embedded Azure サービスから移行したコンテンツに加え、Power BI Desktop を使用してレポートとデータセットを作成してから、アプリ ワークスペースにそれらのレポートを発行することができます。 レポートを発行するエンド ユーザーには、アプリ ワークスペースに発行するための Power BI Pro ライセンスが必要です。

## <a name="rebuild-your-application"></a>アプリケーションを再構築する
1. powerbi.com 内のレポートの場所と Power BI REST API を使用するには、アプリケーションを変更する必要があります。
2. アプリケーションの*マスター* アカウントを使用して、AuthN/AuthZ 認証を再構築します。 このユーザーが他のユーザーの代わりに動作できるようにするには、[埋め込みトークン](https://msdn.microsoft.com/library/mt784614.aspx)を利用します。
3. Powerbi.com からレポートをアプリケーションに埋め込みます。

## <a name="map-your-users-to-a-power-bi-user"></a>ユーザーを Power BI ユーザーにマップする
アプリケーション内で管理するユーザーを、アプリケーション用の*マスター* Power BI 資格情報にマップします。 この Power BI *マスター* アカウントの資格情報はアプリケーション内に格納され、埋め込みトークンの作成に使用されます。

## <a name="what-to-do-when-you-are-ready-for-production"></a>運用環境の準備ができたときに実行する作業
運用環境に移行する準備ができたら、以下の手順を実行する必要があります。

* 開発用に個別のテナントを使用する場合は、アプリ ワークスペース、ダッシュボードおよびレポートが運用環境で利用可能であることを確認する必要があります。 また、運用テナントの Azure AD でアプリケーションを作成し、手順 1. のとおり、適切なアプリにアクセス許可を割り当てたことを確認する必要があります。
* ニーズに合う容量を購入します。 その場合、「[Embedded analytics capacity planning whitepaper](https://aka.ms/pbiewhitepaper)」 (埋め込み分析の容量計画に関するホワイト ペーパー) を使用できます。これは、必要になる可能性があるものを把握するのに役立ちます。 購入する準備ができたら、[Office 365 管理センター](https://portal.office.com/adminportal/home#/catalog)内で購入できます。
  
  > [AZURE.INFORMATION] Power BI Premium の購入方法については、「[Power BI Premium の購入方法](../service-admin-premium-purchase.md)」を参照してください。
  > 
  > 
* アプリ ワークスペースを編集し、[詳細] で Premium 容量にそれを割り当てます。
  
    ![](media/migrate-from-powerbi-embedded/powerbi-embedded-premium-capacity.png)
* 更新されたアプリケーションを運用環境にデプロイし、Power BI サービスからのレポートの埋め込みを開始します。

## <a name="after-migration"></a>移行後
Azure 内でいくつかのクリーンアップを行う必要があります。

* Power BI Embedded の Azure サービス内にデプロイ済みのソリューションからすべてのワークスペースを除去します。
* Azure 内に存在するすべてのワークスペース コレクションを削除します。

## <a name="next-steps"></a>次の手順
[Power BI で埋め込み](embedding.md)  
[Power BI Embedded 移行ツール](migrate-tool.md)  
[Power BI Embedded からコンテンツを移行するためのコード スニペット](migrate-code-snippets.md)  
[Power BI ダッシュボード、レポート、およびタイルを埋め込む方法](embedding-content.md)  
[Power BI Premium とは](../service-premium.md)  
[JavaScript API Git リポジトリ](https://github.com/Microsoft/PowerBI-JavaScript)  
[Power BI C# Git リポジトリ](https://github.com/Microsoft/PowerBI-CSharp)  
[JavaScript 埋め込みサンプル](https://microsoft.github.io/PowerBI-JavaScript/demo/)  
[埋め込み分析の容量計画に関するホワイト ペーパー](https://aka.ms/pbiewhitepaper)  
[Power BI Premium ホワイト ペーパー](https://aka.ms/pbipremiumwhitepaper)  

他にわからないことがある場合は、 [Power BI コミュニティで質問してみてください](http://community.powerbi.com/)。
