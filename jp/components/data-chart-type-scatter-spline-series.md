---
title: Angular データ チャート | データ可視化ツール | 散布スプラインライン チャート | データ バインディング | インフラジスティックス
_description: インフラジスティックスの Angular チャート コントロールを使用して散布スプライン チャートを作成します。Ignite UI for Angular グラフ タイプについて説明します。
_keywords: Angular charts, data chart, scatter spline charts, Ignite UI for Angular, Infragistics, Angular チャート, データ チャート, 散布スプライン チャート, インフラジスティックス
mentionedTypes: ['XamDataChart', 'ScatterSplineSeries']
_language: ja
---

# Angular 散布スプライン チャート

Ignite UI for Angular 散布ポイント チャートは、デカルト (x, y) 座標系を使用してデータをプロットする散布図のグループに属します。このシリーズは、スプラインで接続されたマーカーのコレクションとしてレンダリングされ、各マーカーにはデカルト座標系での位置を決定する X/Y の数値のペアがあります。

[`IgxScatterSplineSeriesComponent`]({environment:dvApiBaseUrl}/products/ignite-ui-angular/api/docs/typescript/latest/classes/igxscattersplineseriescomponent.html) は、不均等な間隔またはデータのクラスターに注意を促します。予測結果の収集データの標準偏差を強調表示し、科学データや統計データをプロットするためによく使用されます。[`IgxScatterSplineSeriesComponent`]({environment:dvApiBaseUrl}/products/ignite-ui-angular/api/docs/typescript/latest/classes/igxscattersplineseriescomponent.html) はデータを X 軸および Y 軸で時系列に管理およびプロットします (バインドする前はデータが時系列でない場合も)。

## Angular 散布スプライン チャートの例

<div class="sample-container loading" style="height: 500px">
    <iframe id="data-chart-type-scatter-spline-series-iframe" src='{environment:dvDemosBaseUrl}/charts/data-chart-type-scatter-spline-series' width="100%" height="100%" seamless frameBorder="0" onload="onXPlatSampleIframeContentLoaded(this);" alt="Angular 散布スプライン チャートの例"></iframe>
</div>
<div>
    <button data-localize="stackblitz" disabled class="stackblitz-btn" data-iframe-id="data-chart-type-scatter-spline-series-iframe" data-demos-base-url="{environment:dvDemosBaseUrl}">StackBlitz で表示
    </button>


</div>

<div class="divider--half"></div>

## 軸の要件

Angular データ チャート コンポーネントはさまざまなタイプの軸を提供しますが、[`IgxScatterSplineSeriesComponent`]({environment:dvApiBaseUrl}/products/ignite-ui-angular/api/docs/typescript/latest/classes/igxscattersplineseriescomponent.html) で使用できるのは以下のタイプの軸のみです。

-   [`IgxNumericXAxisComponent`]({environment:dvApiBaseUrl}/products/ignite-ui-angular/api/docs/typescript/latest/classes/igxnumericxaxiscomponent.html)
-   [`IgxNumericYAxisComponent`]({environment:dvApiBaseUrl}/products/ignite-ui-angular/api/docs/typescript/latest/classes/igxnumericyaxiscomponent.html)

## データの要件

[`IgxScatterSplineSeriesComponent`]({environment:dvApiBaseUrl}/products/ignite-ui-angular/api/docs/typescript/latest/classes/igxscattersplineseriescomponent.html) には以下のデータ要件があります。

-   データソースはデータ項目の配列またはリストである必要があります。
-   データソースはデータ項目を少なくとも 1 つ含む必要があります。含まない場合はチャートに散布シェイプ シリーズを描画しません。
-   すべてのデータ項目には、[`xMemberPath`]({environment:dvApiBaseUrl}/products/ignite-ui-angular/api/docs/typescript/latest/classes/igxscatterbasecomponent.html#xmemberpath) と [`yMemberPath`]({environment:dvApiBaseUrl}/products/ignite-ui-angular/api/docs/typescript/latest/classes/igxscatterbasecomponent.html#ymemberpath)  プロパティにマップされる 2 つの数値データ列を含める必要があります。

上記データ要件を満たすデータソースとして [SampleScatterStats](data-chart-data-sources-stats.md) を使用できます。

```ts
public dataSource: any[] = SampleScatterStats.getCountries();
```

## モジュールの要件

[`IgxScatterSplineSeriesComponent`]({environment:dvApiBaseUrl}/products/ignite-ui-angular/api/docs/typescript/latest/classes/igxscattersplineseriescomponent.html) を作成するには、以下のモジュールが必要です。<!-- Angular, React, WebComponents --> <!-- end: Angular, React, WebComponents --><!-- Blazor -->モジュールはアプリケーションのエントリ ポイントに登録する必要があります。

-   DataChartCoreModule        
-   DataChartScatterCoreModule
-   DataChartScatterModule   
-   DataChartInteractivityModule
-   ScatterSplineSeriesModule
    <!-- end: Blazor -->

```ts
// axis' modules:
import { IgxNumericYAxis } from 'igniteui-angular-charts';
import { IgxNumericXAxis } from 'igniteui-angular-charts';
// series' modules:
import { IgxScatterSplineSeries } from 'igniteui-angular-charts';
// data chart's modules:

import { IgxDataChartCoreModule } from 'igniteui-angular-charts';
import { IgxDataChartScatterCoreModule } from 'igniteui-angular-charts';
import { IgxDataChartScatterModule } from 'igniteui-angular-charts';

@NgModule({
    imports: [
        // ...
        IgxDataChartCoreModule,
        IgxDataChartScatterCoreModule,
        IgxDataChartScatterModule,
    ]
})
export class AppModule { /* ... */ }
```

## コード例

このコードは、[`IgxScatterSplineSeriesComponent`]({environment:dvApiBaseUrl}/products/ignite-ui-angular/api/docs/typescript/latest/classes/igxscattersplineseriescomponent.html) を使用して Ignite UI for Angular データ チャートのインスタンスを作成し、それをデータソースにバインドする方法を示します。

```html
 <igx-data-chart
    [dataSource]="dataSource"
    width="700px"
    height="500px">
    <igx-numeric-x-axis name="xAxis" isLogarithmic="true"></igx-numeric-x-axis>
    <igx-numeric-y-axis name="yAxis" isLogarithmic="true"></igx-numeric-y-axis>
    <igx-scatter-spline-series
        name="series1"
        xAxisName="xAxis"
        yAxisName="yAxis"
        xMemberPath="Population"
        yMemberPath="GdpTotal">
    </igx-scatter-spline-series>
 </igx-data-chart>
```

## シリーズの外観

[マーカー](data-chart-series-markers.md) プロパティを使用してマーカーの外観をカスタマイズできます。マーカーごとに [`brush`]({environment:dvApiBaseUrl}/products/ignite-ui-angular/api/docs/typescript/latest/classes/igxseriescomponent.html#brush) と [`thickness`]({environment:dvApiBaseUrl}/products/ignite-ui-angular/api/docs/typescript/latest/classes/igxseriescomponent.html#thickness) のビジュアルを変更することもできます。以下のこのコード スニペットは、これらのプロパティの使用方法を示しています。

```html
<igx-scatter-spline-series
 name="series1"
 brush="Purple"
 markerType="Square"
 markerBrush="White"
 markerOutline="Blue"
 thickness="2">
</igx-scatter-spline-series>
```

```ts
const series1 = new IgxScatterSplineSeries({ name: "series1" });
series1.markerType = MarkerType.Square;
series1.markerBrush = "White";
series1.markerOutline = "Blue";
series1.brush = "Purple";
series1.thickness = 2;
```

### その他のリソース

-   [軸のタイプ](data-chart-axis-types.md)
-   [軸の共有](data-chart-axis-sharing.md)
-   [チャート凡例](data-chart-legends.md)
-   [シリーズ マーカー](data-chart-series-markers.md)
-   [シリーズのツールチップ](data-chart-series-tooltips.md)
-   [シリーズ トレンドライン](data-chart-series-trendlines.md)
-   [シリーズ タイプ](data-chart-series-types.md)
