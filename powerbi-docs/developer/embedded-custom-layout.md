---
title: "Power BI の埋め込みコンテンツによるカスタム レイアウト"
description: "アプリケーションに Power BI コンテンツを埋め込むときのカスタム レイアウトについて説明します。"
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
ms.date: 12/19/2017
ms.author: maghan
ms.openlocfilehash: 351cfa27cc092af4bc5650a09730bc8a2661abbc
ms.sourcegitcommit: 6e693f9caf98385a2c45890cd0fbf2403f0dbb8a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/30/2018
---
# <a name="custom-layouts"></a>カスタム レイアウト


カスタム レイアウトを使って、元のレポートとは異なるレイアウトでレポートを埋め込みます。 新しいレイアウトの定義は、ページ サイズのみを定義する場合と、ビジュアルのサイズ、位置、可視性を制御する場合では異なります。

カスタム レイアウトを定義するには、カスタム レイアウト オブジェクトを定義して、埋め込み構成の設定オブジェクトに渡します。 さらに、LayoutType を Custom に設定します。 詳しくは、「[Embed Configuration Details](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embed-Configuration-Details)」(埋め込み構成の詳細) をご覧ください。

```javascript
var embedConfig = {
    ...
    settings: {
            layoutType: models.LayoutType.Custom
    customLayout: {...}
    }
};
```

## <a name="object-definition"></a>オブジェクトの定義

```javascript
interface ICustomLayout {
  pageSize?: IPageSize;
  displayOption?: DisplayOption;
  pagesLayout?: PagesLayout;
}

enum PageSizeType {
  Widescreen,
  Standard,
  Cortana,
  Letter,
  Custom
}
interface IPageSize {
  type: PageSizeType;
}
interface ICustomPageSize extends IPageSize {
  width?: number;
  height?: number;
}

enum DisplayOption {
  FitToPage,
  FitToWidth,
  ActualSize
}
```

- `pageSize`: キャンバス領域のサイズ (つまり、レポートの空白領域) を制御するには、ページ サイズを使います。
- `displayOptions`: 使用できる値は、FitToWidth、FitToPage、ActualSize です。 iframe に収まるようにキャンバスを拡大縮小する方法を制御します。
- `pagesLayout`: 各ビジュアルのレイアウトを制御します。 詳しくは、「ページ レイアウト」をご覧ください。

## <a name="pages-layout"></a>ページ レイアウト

ページ レイアウト オブジェクトの定義は基本的に各ページのレイアウトを定義することであり、ページごとに各ビジュアルのレイアウトを定義します。
PageLayout は省略できます。 ページのレイアウトを定義しないと、既定のレイアウト (レポートに保存されます) が適用されます。

pagesLayout は、ページ名から PageLayout オブジェクトへのマップです。 定義は次のとおりです。

```javascript
type PagesLayout = { [key: string]: IPageLayout; };
```

PageLayout に含まれるビジュアル レイアウト マップでは、各ビジュアル名をビジュアル レイアウト オブジェクトにマップします。

```javascript
interface IPageLayout {
  visualsLayout: { [key: string]: IVisualLayout; };
}
```

## <a name="visual-layout"></a>ビジュアル レイアウト

ビジュアルのレイアウトを定義するには、新しい位置とサイズおよび新しい可視性の状態を渡します。

```javascript
interface IVisualLayout {
  x?: number;
  y?: number;
  z?: number;
  width?: number;
  height?: number;
  displayState?: IVisualContainerDisplayState;
}

interface IVisualContainerDisplayState {
  mode: VisualContainerDisplayMode;
}

enum VisualContainerDisplayMode {
  Visible,
  Hidden
}
```

- `x,y,z`: ビジュアルの新しい位置を定義します。
- `width`、height: ビジュアルの新しいサイズを定義します。
- `displayState`: ビジュアルを表示するかどうかを定義します。


## <a name="update-layout"></a>レイアウトの更新

レポートが読み込まれている間であればいつでも、updateSettings メソッドを使って、レポートのレイアウトを更新できます。 「[設定の更新](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Update-Settings)」をご覧ください。

## <a name="code-example"></a>コードの例

```javascript
// Get models. models contains enums that can be used.
var models = window['powerbi-client'].models;
    
var embedConfiguration = {
    type: 'report',
    id: '5dac7a4a-4452-46b3-99f6-a25915e0fe55',
    embedUrl: 'https://app.powerbi.com/reportEmbed',
    tokenType: models.TokenType.Embed,
    accessToken: 'H4...rf',
    settings: {
            layoutType: models.LayoutType.Custom
        customLayout: {
            pageSize: {
                type: models.PageSizeType.Custom,
                width: 1600,
                height: 1200
            },
            displayOption: models.DisplayOption.ActualSize,
            pagesLayout: {
                "ReportSection1" : {
                    visualsLayout: {
                        "VisualContainer1": {
                            x: 1,
                            y: 1,
                            z: 1,
                            width: 400,
                            height: 300,
                            displayState: {
                                mode: models.VisualContainerDisplayMode.Visible
                            }
                        },
                        "VisualContainer2": {
                            displayState: {
                                mode: models.VisualContainerDisplayMode.Hidden
                            }
                        },
                    }
                }
            }
        }
    }
};
     
// Get a reference to the embedded report HTML element
var embedContainer = document.getElementById('embedContainer');
 
// Embed the report and display it within the div container.
var report = powerbi.embed(embedContainer, embedConfiguration);

```


## <a name="see-also"></a>参照

[Power BI ダッシュボード、レポート、およびタイルを埋め込む](embedding-content.md)   
[Power BI コミュニティに質問する](https://community.powerbi.com/)

