---
title: "Power BI Desktop でのデータの整形と結合"
description: "Power BI Desktop でのデータの整形と結合"
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
ms.date: 01/30/2018
ms.author: davidi
LocalizationGroup: Transform and shape data
ms.openlocfilehash: c8f2419ae2898a59907763392eb86b4877b4fd75
ms.sourcegitcommit: 88c8ba8dee4384ea7bff5cedcad67fce784d92b0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/24/2018
---
# <a name="shape-and-combine-data-in-power-bi-desktop"></a>Power BI Desktop でのデータの整形と結合
**Power BI Desktop** を使用すると、さまざまな種類のデータ ソースに接続してから、ニーズに合わせてデータを整形できます。 データの *整形* とは、データを変換することです。たとえば、列やテーブルの名前を変更したり、テキストを数値に変更したり、行を削除したり、最初の行をヘッダーとして設定したりします。 データの *結合* とは、複数のデータ ソースに接続して、必要に応じてそれらを整形してから、1 つの便利なクエリに統合することを意味します。

このドキュメントでは、いくつかの最も一般的なタスクに注目しながら、Power BI Desktop を使用したクエリの整形方法を示します。 ここで使用するクエリは、クエリを最初から作成する方法も含めて、「[Power BI Desktop の概要](desktop-getting-started.md)」でより詳しく説明しています。

Power BI Desktop の **クエリ エディター** では、リボンだけでなく右クリック メニューも広く使用することを知っておくと便利です。 **[変換]** リボンで選択できるほとんどの項目は、(列などの) 項目を右クリックし、表示されるメニューからクリックして使用することもできます。

## <a name="shape-data"></a>データの整形
クエリ エディターでデータを整形する際は、(クエリ エディターが行う) 手順ごとの指示を与えて、クエリ エディターが読み込んで表示するデータを調整します。 元のデータ ソースに影響は及びません。この特定のデータ表示のみが調整または *整形* されます。

指定する手順 (テーブル名の変更、データ型の変換、または列の削除など) はクエリ エディターによって記録され、このクエリ エディターがデータ ソースに接続するたびにこれらの手順が実行されます。そうすることで、データは常に指定されたとおりに整形されます。 自分が Power BI Desktop でクエリ エディター機能を使用するたびに、あるいは別のユーザーが **Power BI** サービスなどで共有クエリを使用する場合に、このプロセスが実行されます。 これらの手順は、**[クエリの設定]** ウィンドウの **[適用される手順]** で順番にキャプチャされます。

次の図は、整形されたクエリを表示した **[クエリの設定]** ウィンドウを示しています。これらの各手順については、以降のいくつかの段落で説明します。

![](media/desktop-shape-and-combine-data/shapecombine_querysettingsfinished.png)

「[Power BI Desktop の概要](https://powerbi.uservoice.com/knowledgebase/articles/471664)」の退職後の生活のデータ (Web データ ソースに接続すると入手できます) を使って、このデータをニーズに合わせて整形しましょう。

クエリ エディターがテーブルを読み込んだとき、テキストから数値に自動的に変換されなかった列が 1 つあったので、まずはこれを数値に変換する必要があります。 列ヘッダーを右クリックして **[型の変更] \> [整数]** の順に選ぶだけで変更できるので、心配は不要です。 複数の列を選ぶには、まず 1 つの列を選んでから、**Shift** キーを押したまま追加の隣接する列を選びます。その後、列ヘッダーを右クリックして選んだ列をすべて変更します。 また、**Ctrl** キーを使用すると、隣接していない列を選ぶこともできます。

![](media/desktop-shape-and-combine-data/shapecombine_changetype.png)

さらに、 **[変換]** リボンで、列をテキストからヘッダーに *変換* することもできます。 **[変換]** リボンを次に示します。矢印は、現在のデータ型を別のデータ型に変換する **[データ型]** ボタンを指しています。

![](media/desktop-shape-and-combine-data/queryoverview_transformribbonarrow.png)

データに適用されたすべての整形手順が **[クエリの設定]** の **[適用される手順]** に反映されていることにご注意ください。 整形プロセスからいずれかの手順を削除する場合は、手順の左側にある "**X**” を選びます。 次の図の **[適用される手順]** には、これまでの手順が反映されています。すなわち、Web サイトへの接続 ( **ソース** )、テーブルの選択 ( **ナビゲーション** )、およびテーブルの読み込み中にテキスト ベースの数値型の列がクエリ エディターによって *テキスト型* から *整数型* に自動的に変更されたこと ( **型の変更** ) です。 順位付けの 1 つの列が自動的に数値ベースの型に変更されなかったので、その理由を次のいくつかの段落で確認します。

![](media/desktop-shape-and-combine-data/shapecombine_appliedstepsearly.png)

このクエリを処理する前に、データにいくつかの変更を加えて、処理可能な状態にする必要があります。

* *最初の列を削除する* – この列は不要です。[お住まいの州の退職後の生活ランキングをチェック] という冗長な行が含まれているに過ぎません。これは、このデータ ソースが Web ベースのテーブルであることに伴う結果です。
* *いくつかのエラーを修正する* – **[医療品質]** という列では、州のランクにいくつかの同順位が含まれています。これは、Web サイトでは番号の後に *(同順位)* というテキストが付いて記載されていました。 Web サイトでは適切に機能していますが、この列をテキスト型からデータ型に手動で変換する必要があります。 Power BI Desktop を使えばこれを簡単に修正できます。そうすることで、クエリの **[適用される手順]** の優れた機能が実証されます。
* *テーブル名を変更する* – **Table 0** は役立つ記述子ではありませんが、それを変更することは簡単です。

最初の列を削除するには、列を選んでからリボンの **[ホーム]** タブを選びます。その後、次の図に示すように、**[列の削除]** を選びます。

![](media/desktop-shape-and-combine-data/shapecombine_removecolumnsretirement.png)

次に、テキスト列を数値に変換する必要があります。 **[医療品質]** 列の型をテキスト型から数値型 ( *整数型* または *10 進数型* ) に変更するだけなので、最初は単純に見えます。 しかし、**テキスト型**から**整数型**に変更するとき、その列の値をよく見てください。クエリ エディターがいくつかのエラーを報告していることが分かります。

![](media/desktop-shape-and-combine-data/shapecombine_error.png)

各エラーに関する詳細を取得する方法はいくつかあります。 (" **エラー**” という語をクリックせずに) セルを選択するか、" **エラー** ” という語を直接クリックできます。 " *エラー* ” という語を直接クリック **せずに**セルを選択すると、クエリ エディターによってエラー情報がウィンドウの下部に表示されます。

![](media/desktop-shape-and-combine-data/shapecombine_errorinfo.png)

" *エラー* ” という語を直接クリックすると、クエリによって **[クエリの設定]** ウィンドウに **[適用される手順]** が作成され、エラーに関する情報が表示されます。

![](media/desktop-shape-and-combine-data/shapecombine_errorselect.png)

クエリ エディターに戻るには、手順の隣にある " **X** ” をクリックしてその手順を削除する必要があります。

最新の **[適用される手順]**を選択すると、次の図のように、上記で説明したエラーが表示されます。

![](media/desktop-shape-and-combine-data/shapecombine_querystep1.png)

クエリ エディターは手順を順番に記録するため、 **[適用される手順]**で型を変更する前の手順を選択すると、次の図のように、変換前のそのセルの値を確認できます。

![](media/desktop-shape-and-combine-data/shapecombine_querystep2.png)

では、これらの値を修正して *から* 、型を変更します。 クエリ エディターは手順を順番に記録しますが、互いに独立しているため、順番に並んだそれぞれの**適用される手順**を上下に移動することができます。 いずれかの手順を右クリックすると、適用可能な操作のメニューが表示されます: **名前の変更**、**削除**、**最後まで****削除** (現在の手順とそれ以降の手順もすべて削除する)、**上へ移動**、または**下へ移動**。

![](media/desktop-shape-and-combine-data/shapecombine_querystepreorder.png)

さらに、 **[適用される手順]** 一覧のどの位置にある手順でも選択でき、順番のその地点からデータの整形を続行できます。 新しい手順は、現在選択されている **適用される手順**の直後に自動的に挿入されます。 試しに実行してみましょう。

最初に、 **[適用される手順]** で、 **[医療品質]** 列の型を変更する手順の直前にある手順をクリックします。 次に、セルに "(同順位)" というテキストがある値を、数値のみが残るように置き換えます。 "35 (同順位)" を含むセルを右クリックしてから、表示されるメニューで *[値の置換...]* をクリックします。 **[適用される手順]** でどの手順が現在選択されているか (型の変更の前の手順) に注意してください。

![](media/desktop-shape-and-combine-data/shapecombine_replacevalues.png)

手順を挿入しているので、これ以降の手順によってクエリ エディターが中断する危険があることを示す警告が表示されます。 注意して慎重に操作する必要があります。 これは、手順の作成、削除、挿入、および順序の変更の方法を示してクエリ エディターの非常に優れた機能を強調するためのチュートリアルなので、このまま先に進んで **[挿入]**を選択します。

![](media/desktop-shape-and-combine-data/shapecombine_insertstep.png)

同順位が 3 つあるため、それぞれの値を置き換えます。 適用される手順を新規作成すると、クエリ エディターは操作に基づいて手順に名前を付けます。このケースでは "**置換された値**” です。 クエリに同じ名前の手順が複数存在する場合、クエリ エディターは、それらを区別するために、後続の**適用される手順**のそれぞれに対して (順番に) 番号を追加します。

次の図は、 **[クエリの設定]** における 3 つの " **置換された値** " 手順を示していますが、より興味深いことも示しています。 **[医療品質]** 列からテキスト "(同順位)" の各インスタンスを削除したので、 **型の変更** 手順が *エラーなし* で完了しているということです。

![](media/desktop-shape-and-combine-data/shapecombine_replacedvaluesok.png)

> [!NOTE]
> **エラーの削除**も行えます (リボンまたは右クリック メニューを使用します)。これにより、エラーを含む行がすべて削除されます。 このケースでは、データから "*(同順位)*" を含む州がすべて削除されることになりますが、それは意図する処理ではありません。希望する処理は、すべての州をテーブルに保持することです。

さて、少し深入りしましたが、これはクエリ エディターがいかに強力で多目的に使えるかを示す良い一例でした。

最後に、このテーブルの名前を分かりやすい名前に変更したいと思います。 レポートを作成することになった場合は、分かりやすいテーブル名を付けると特に役立ちます。とりわけ、複数のデータ ソースに接続し、**レポート** ビューの **[フィールド]** ウィンドウにそれらがすべて一覧表示される場合に役立ちます。

テーブル名の変更は簡単です。次の図のように、**[クエリの設定]** ウィンドウの **[プロパティ]** で、テーブルの新しい名前を入力し、**Enter** キーを押します。 このテーブルの名前を *RetirementStats* としましょう。

![](media/desktop-shape-and-combine-data/shapecombine_renametable.png)

これで、データを必要な範囲まで整形しました。 次に、別のデータ ソースに接続し、データを結合しましょう。

## <a name="combine-data"></a>データの結合
さまざまな州に関するこのデータは興味深く、追加の分析作業とクエリの構築に役立ちます。 ただし、1 つ問題があります。ここにあるほとんどのデータでは、州コードの 2 文字の省略形を使用し、州の完全名を使用していません。 何らかの方法により、州名をその省略形に関連付ける必要があります。

幸運にも、ぴったりな別の公共データ ソースがあります。しかし、この退職者テーブルに接続する前に、いくらかの整形を行う必要があります。 州の省略形の Web リソースを次に示します。

<http://en.wikipedia.org/wiki/List_of_U.S._state_abbreviations>

クエリ エディターの **[ホーム]** リボンで、**[新しいソース] \> [Web]** の順に選び、アドレスを入力して [OK] を選びます。その Web ページの内容がナビゲーターに表示されます。

 ![](media/desktop-shape-and-combine-data/designer_gsg_usstateabbreviationsnavigator.png)

ここでは "**Table[edit]**” を選びました。このテーブルには必要なデータが含まれているためです。しかし、テーブルのデータから余分なものを省いて、必要なものにするにはかなりの作業が必要です。

> [!TIP]
> 次の手順を、より迅速にまたはより簡単に実行する方法はありますか? はい。2 つのテーブル間に *リレーションシップ* を作成し、そのリレーションシップに基づいてデータを整形するという方法があります。 次の手順は、テーブルの操作について学習するのに適しています。リレーションシップにより、複数のテーブルからのデータをすばやく使用できるようになることを理解してください。
> 
> 

このデータを整形するには、次の手順を実行します。

* 先頭の 2 行を削除する – これらは Web ページのテーブルを作成した方法のために生じたものであり、不要です。 **[ホーム]** リボンで、**[行の削減] \> [行の削除] \> [上位行の削除]** を選びます。

![](media/desktop-shape-and-combine-data/shapecombine_removetoprows.png)

**[上位行の削除]** ウィンドウが表示され、削除する行数を指定できます。

* 末尾の 26 行を削除する – これらはすべて準州であり、含める必要はありません。 **[ホーム]** リボンで、**[行の削減] \> [行の削除] \> [下位行の削除]** を選びます。

![](media/desktop-shape-and-combine-data/shapecombine_removebottomrows.png)

* RetirementStats テーブルには Washington DC の情報がないため、一覧からフィルター処理する必要があります。 [地域ステータス] 列の隣にあるドロップダウン矢印を選択し、 **[連邦地区]**の隣にあるチェックボックスの選択を解除します。

![](media/desktop-shape-and-combine-data/shapecombine_filterdc.png)

* いくつかの不要な列を削除する – 州と公式の 2 文字の省略形のマッピングのみが必要であるため、 **[列 2]**、 **[列 3]**、および **[列 5]** ～ **[列 10]**は削除できます。 まず [列 2] を選択してから、 **Ctrl キー** を押したまま削除する他の列をクリックします (これにより、複数の連続していない列を選択できます)。 リボンの [ホーム] タブで、**[列の削除] \> [列の削除]** を選びます。

![](media/desktop-shape-and-combine-data/shapecombine_removecolumns.png)

* 最初の行をヘッダーとして使用する – 先頭の 3 行を削除したため、現在の先頭の行が必要なヘッダーになります。 リボンの **[ホーム]** タブまたは **[変換]** タブで、 **[最初の行をヘッダーとして使用する]** を選択します。

![](media/desktop-shape-and-combine-data/shapecombine_usefirstrowasheaders.png)

>[!NOTE]
>ここで、クエリ エディターにおいて適用される手順の*順番*は重要であり、データの整形方法に影響を与えることを指摘しておきます。 また、1 つの手順がこれ以降の別の手順にどのような影響を与える可能性があるかを検討することも重要です。適用される手順から手順を削除すると、クエリの手順の順番による影響のため、これ以降のステップは最初に意図したとおりに動作しない可能性があります。

>[!NOTE]
>クエリ エディター ウィンドウのサイズを変更して幅を狭くすると、表示スペースを最大限利用するようにリボン項目の一部が小さくなります。 クエリ エディター ウィンドウの幅を広げると、広くなったリボン領域を最大限活用するようにリボン項目が拡大されます。

* 列およびテーブル自体の名前を変更する – 通常どおり、列の名前を変更するには 2 つの方法があります。まず列を選んでからリボンの **[変換]** タブで **[名前の変更]** を選ぶか、右クリックしてから表示されるメニューで **[名前の変更...]** を選びます。 次の図では両方のオプションに矢印が付いていますが、選択するのは一方だけです。

![](media/desktop-shape-and-combine-data/shapecombine_rename.png)

名前を " *州名* ” と " *州コード* ” に変更します。 テーブルの名前を変更するには、 **[クエリの設定]** ウィンドウの **[名前]** ボックスに名前を入力します。 このテーブルの名前を " *StateCodes* ” としましょう。

StateCodes テーブルを希望どおりに整形したので、これらの 2 つのテーブルまたはクエリを 1 つに結合します。現在あるテーブルはデータに適用したクエリの結果であるため、"*クエリ*" と呼ばれることがあります。

クエリの結合には、 *マージ* と *追加* という主な 2 つの方法があります。

別のクエリに追加する 1 つ以上の列がある場合は、クエリを **マージ** します。 既存のクエリに追加するデータの追加の行がある場合は、クエリを **追加** します。

ここではクエリをマージします。 最初に、クエリ エディターの左ウィンドウから、 *他のクエリとマージする* クエリを選びます。この場合、「 *RetirementStats* 」です。 次に、リボンの **[ホーム]** タブから **[結合] \> [クエリの結合]** を選びます。

![](media/desktop-shape-and-combine-data/shapecombine_mergequeries.png)

転送対象外のデータを含めたり転送したりせずにデータを結合するため、プライバシー レベルを設定するように求められます。

![](media/desktop-shape-and-combine-data/shapecombine_mergequeriesb.png)

次に、**[マージ]** ウィンドウが表示されて、選んだテーブルにマージするテーブルを選んでから、マージに使用する一致する列を選ぶよう求めるメッセージが表示されます。 *RetirementStats* テーブル (クエリ) から州を選びます。次に、*StateCodes* クエリを選びます (この場合は、他のクエリが 1 つのみであるため簡単です。多数のデータ ソースに接続する場合、選択対象のクエリが多数になります)。 一致する列を正しく選ぶと (*RetirementStats* の **[州]** と *StateCodes* の **[州名]**)、**[マージ]** ウィンドウは次のようになり、**[OK]** ボタンが有効になります。

![](media/desktop-shape-and-combine-data/shapecombine_merge.png)

クエリの末尾に **NewColumn** が作成されます。これは、既存のクエリとマージされたテーブル (クエリ) のコンテンツです。 マージされたクエリのすべての列が **NewColumn** に凝縮されますが、テーブルの **[展開]** を選ぶと、必要な列をどれでも含めることができます。

![](media/desktop-shape-and-combine-data/shapecombine_mergenewcolumn.png)

マージされたテーブルを展開して含める列を選ぶには、展開アイコンを選びます (![展開](media/desktop-shape-and-combine-data/icon.png))。 **[展開]** ウィンドウが表示されます。

![](media/desktop-shape-and-combine-data/shapecombine_mergeexpand.png)

このケースでは、**[州コード]** 列のみが必要であるため、この列のみを選んで **[OK]** を選びます。 [元の列名をプレフィックスとして使用する] のチェックボックスは、不要なためチェックを外します。選んだままにすると、マージされた列の名前は **NewColumn.州コード** (元の列名、または **NewColumn** とドットの後にクエリに含める列の名前を続けたもの) になります。

>[!NOTE]
>**NewColumn** テーブルの導入操作を試してみることができます。 少し実験してみてください。結果に満足できない場合は、**[クエリの設定]** ウィンドウの **[適用される手順]** の一覧からその手順を削除してください。クエリは、**[展開]** の手順を適用する前の状態に戻ります。 これは、展開プロセスが希望どおりになるまで何回でも好きなだけ実行できる無料のやり直しのようなものです。

2 つのデータ ソースを結合した 1 つのクエリ (テーブル) ができました。それぞれがニーズに合わせて整形されています。 このクエリは、いずれかの州の住宅費の統計、人口統計、または求人情報など、その他の多くの興味深いデータ接続の基礎となっています。

変更を適用し、クエリ エディターを閉じるには、**[ホーム]** リボン タブから [閉じて適用する] を選びます。変換されたデータセットが Power BI Desktop に表示されます。レポートの作成に利用できます。

![](media/desktop-shape-and-combine-data/shapecombine_closeandapply.png)

## <a name="next-steps"></a>次の手順
Power BI Desktop を使用すると、さまざまなことを行えます。 そのような機能について詳しくは、次のリソースをご覧ください。

* [Power BI Desktop の概要](desktop-getting-started.md)
* [Power BI Desktop でのクエリの概要](desktop-query-overview.md)
* [Power BI Desktop のデータ ソース](desktop-data-sources.md)
* [Power BI Desktop におけるデータへの接続](desktop-connect-to-data.md)
* [Power BI Desktop での一般的なクエリ タスク](desktop-common-query-tasks.md)   

