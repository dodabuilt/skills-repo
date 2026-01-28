# Lightweight-Charts - Other

**Pages:** 13

---

## Getting started

**URL:** https://tradingview.github.io/lightweight-charts/docs/4.0

**Contents:**
- Getting started
- Requirements​
- Installation​
  - Build variants​
- License and attribution​
- Creating a chart​
- Creating a series​
- Setting and updating a data​
  - Setting the data to a series​
  - Updating the data in a series​

First of all, Lightweight Charts™ is a client-side library. This means that it does not and cannot work on the server-side (i.e. NodeJS), at least out of the box.

The code of lightweight-charts package is targeted to es2016 language specification. Thus, all the browsers you will have to work with should support this language revision (see this compatibility table). If you need to support the previous revisions, you could try to setup a transpilation of the package to the target you need to support in your build system (e.g. by using Babel). If you'll have any issues with that, please raise an issue on github with the details and we'll investigate possible ways to solve it.

The first thing you need to do to use lightweight-charts is to install it from npm:

Note that the package is shipped with TypeScript declarations, so you can easily use it within TypeScript code.

The library ships with the following build variants:

⚠️ Deprecation note: CommonJS support will be removed from the library at the start of 2024.

The Lightweight Charts™ license requires specifying TradingView as the product creator.

You shall add the "attribution notice" from the NOTICE file and a link to https://www.tradingview.com to the page of your website or mobile application that is available to your users.

As thanks for creating Lightweight Charts™, we'd be grateful if you add the attribution notice in a prominent place.

Once the library has been installed in your repo you're ready to create your first chart.

First of all, in a file where you would like to create a chart you need to import the library:

createChart is the entry-point for creating charts. You can use it to create as many charts as you need:

The result of this function is a IChartApi object, which you need to use to work with a chart instance.

Once your chart is created it is ready to display data.

The basic primitive to display a data is a series. There are different types of series:

To create a series with desired type you need to use appropriate method from IChartApi. All of them have the same naming add<type>Series, where <type> is a type of a series you'd like to create:

Please look at this page for more information about different series types.

Note that a series cannot be transferred from one type to another one since different series types have different data and options types.

Once your chart and series are created it's time to set data to the series.

Note that regardless of the series type, the API calls are the same (the type of the data might be different though).

To set the data (or to replace all data items) to a series you need to use ISeriesApi.setData method:

In a case when your data is updated (e.g. real-time updates) you might want to update the chart as well.

But using ISeriesApi.setData very often might affect the performance and we do not recommend to do this. Also it replaces all series data with the new one, and probably this is not what you're looking for.

Thus, to update the data you can use a method ISeriesApi.update. It allows you to update the last data item or add a new one much faster without affecting the performance:

**Examples:**

Example 1 (unknown):
```unknown
npm install --save lightweight-charts
```

Example 2 (sql):
```sql
import { createChart } from 'lightweight-charts';
```

Example 3 (sql):
```sql
import { createChart } from 'lightweight-charts';// ...// somewhere in your codeconst firstChart = createChart(document.getElementById('firstContainer'));const secondChart = createChart(document.getElementById('secondContainer'));
```

Example 4 (sql):
```sql
import { createChart } from 'lightweight-charts';const chart = createChart(container);const areaSeries = chart.addAreaSeries();const barSeries = chart.addBarSeries();const baselineSeries = chart.addBaselineSeries();// ... and so on
```

---

## Getting started

**URL:** https://tradingview.github.io/lightweight-charts/docs/5.0

**Contents:**
- Getting started
- Requirements​
- Installation​
  - Build variants​
- License and attribution​
- Creating a chart​
- Creating a series​
- Setting and updating a data​
  - Setting the data to a series​
  - Updating the data in a series​

Lightweight Charts™ is a client-side library that is not designed to work on the server side, for example, with Node.js.

The library code targets the ES2020 language specification. Therefore, the browsers you work with should support this language revision. Consider the following table to ensure the browser compatibility.

To support previous revisions, you can set up a transpilation process for the lightweight-charts package in your build system using tools such as Babel. If you encounter any issues, open a GitHub issue with detailed information, and we will investigate potential solutions.

To set up the library, install the lightweight-charts npm package:

The package includes TypeScript declarations, enabling seamless integration within TypeScript projects.

The library ships with the following build variants:

The Lightweight Charts™ license requires specifying TradingView as the product creator. You should add the following attributes to a public page of your website or mobile application:

As a first step, import the library to your file:

To create a chart, use the createChart function. You can call the function multiple times to create as many charts as needed:

As a result, createChart returns an IChartApi object that allows you to interact with the created chart.

When the chart is created, you can display data on it.

The basic primitive to display data is a series. The library supports the following series types:

To create a series, use the addSeries method from IChartApi. As a parameter, specify a series type you would like to create:

Note that a series cannot be transferred from one type to another one, since different series types require different data and options types.

When the series is created, you can populate it with data. Note that the API calls remain the same regardless of the series type, although the data format may vary.

To set the data to a series, you should call the ISeriesApi.setData method:

You can also use setData to replace all data items.

If your data is updated, for example in real-time, you may also need to refresh the chart accordingly. To do this, call the ISeriesApi.update method that allows you to update the last data item or add a new one.

We do not recommend calling ISeriesApi.setData to update the chart, as this method replaces all series data and can significantly affect the performance.

**Examples:**

Example 1 (unknown):
```unknown
npm install --save lightweight-charts
```

Example 2 (sql):
```sql
import { createChart } from 'lightweight-charts';
```

Example 3 (sql):
```sql
import { createChart } from 'lightweight-charts';// ...const firstChart = createChart(document.getElementById('firstContainer'));const secondChart = createChart(document.getElementById('secondContainer'));
```

Example 4 (sql):
```sql
import { AreaSeries, BarSeries, BaselineSeries, createChart } from 'lightweight-charts';const chart = createChart(container);const areaSeries = chart.addSeries(AreaSeries);const barSeries = chart.addSeries(BarSeries);const baselineSeries = chart.addSeries(BaselineSeries);// ...
```

---

## From v3 to v4

**URL:** https://tradingview.github.io/lightweight-charts/docs/migrations/from-v3-to-v4

**Contents:**
- From v3 to v4
- Exported enum LasPriceAnimationMode has been removed​
- scaleMargins option has been removed from series options​
- backgroundColor from layout options has been removed​
- overlay property of series options has been removed​
- priceScale option has been removed​
- priceScale() method of chart API now requires to provide price scale id​
- drawTicks from leftPriceScale and rightPriceScale options has been renamed to ticksVisible​
- The type of outbound time values has been changed​
- seriesPrices property from MouseEventParams has been removed​

In this document you can find the migration guide from the previous version v3 to v4.

Please use LastPriceAnimationMode instead.

Previously, you could do something like the following:

And scaleMargins option was applied to series' price scale as scaleMargins option.

Since v4 this option won't be applied to the price scale and will be just ignored (if you're using TypeScript you will get a compilation error).

To fix this, you need to apply these options to series' price scale:

If you want to have solid background color you need to use background property instead, e.g. instead of:

Please follow the guide for migrating from v2 to v3 where this option was deprecated.

Please follow the guide for migrating from v2 to v3.

Before v4 you could write the following code:

And in priceScale you had a right price scale if it is visible and a left price scale otherwise.

Since v4 you have to provide an ID of price scale explicitly, e.g. if you want to get a right price scale you need to provide 'right':

Since v4 you have to use ticksVisible instead of drawTicks.

Also this option is off by default.

Previously the type of an inbound time (a values you provide to the library, e.g. in ISeriesApi.setData) was different from an outbound one (a values the library provides to your code, e.g. an argument of LocalizationOptions.timeFormatter). So the difference between types was that outbound time couldn't be a business day string.

Since v4 we improved our API in this matter and now the library will return exactly the same values back for all time-related properties.

Thus, if you provide a string to your series in ISeriesApi.setData, you'll receive exactly the same value back:

Handling this breaking change depends on your needs and your handlers, but generally speaking you need to convert provided time to a desired format manually if it is required. For example, you could use provided helpers to check the type of a time:

The property seriesPrices of MouseEventParams has been removed.

Instead, you can use MouseEventParams.seriesData - it is pretty similar to the old seriesPrices, but it contains series' data items instead of just prices:

Since v4 you have to use hoveredObjectId instead of hoveredMarkerId.

**Examples:**

Example 1 (css):
```css
const series = chart.addLineSeries({    scaleMargins: { /* options here */},});
```

Example 2 (css):
```css
const series = chart.addLineSeries();series.priceScale().applyOptions({    scaleMargins: { /* options here */},});
```

Example 3 (css):
```css
const chart = createChart({    layout: {        backgroundColor: 'red',    },});
```

Example 4 (css):
```css
const chart = createChart({    layout: {        background: {            type: ColorType.Solid,            color: 'red',        },    },});
```

---

## iOS wrapper

**URL:** https://tradingview.github.io/lightweight-charts/docs/ios

**Contents:**
- iOS wrapper
- Installation​
  - CocoaPods​
  - Swift Package Manager​
- Usage​
- How to run the provided example​

You can find the source code of the Lightweight Charts™ iOS wrapper in this repository.

This wrapper is currently still using v3.8.0. This will be updated to v4.0.0 in the near future.

You can use Lightweight Charts™ inside an iOS application. To use Lightweight Charts™ in that context, you can use our iOS wrapper, which will allow you to interact with Lightweight Charts™ library, which will be rendered in a web view.

CocoaPods is a dependency manager for Cocoa projects. For usage and installation instructions, visit their website. To integrate LightweightCharts into your Xcode project using CocoaPods, specify it in your Podfile:

The Swift Package Manager is a tool for automating the distribution of Swift code and is integrated into the swift compiler.

Once you have your Swift package set up, adding LightweightCharts as a dependency is as easy as adding it to the dependencies value of your Package.swift.

Once the library has been installed in your repo, you're ready to create your first chart.

First of all, in a file where you would like to create a chart, you need to import the library:

Create instance of LightweightCharts, which is a subclass of UIView, and add it to your view.

Add any series to the chart and store a reference to it.

Add data to the series.

The GitHub repository for LightweightChartsIOS contains an example of the library in action. To run the example, start by cloning the repository, go to the Example directory, and then run

**Examples:**

Example 1 (ruby):
```ruby
pod 'LightweightCharts', '~> 3.8.0'
```

Example 2 (swift):
```swift
dependencies: [    .package(url: "https://github.com/tradingview/LightweightChartsIOS", .upToNextMajor(from: "4.0.0"))]
```

Example 3 (swift):
```swift
import LightweightCharts
```

Example 4 (swift):
```swift
var chart: LightweightCharts!// ...chart = LightweightCharts()view.addSubview(chart)// ... setup layout
```

---

## Getting started

**URL:** https://tradingview.github.io/lightweight-charts/docs/4.1

**Contents:**
- Getting started
- Requirements​
- Installation​
  - Build variants​
- License and attribution​
- Creating a chart​
- Creating a series​
- Setting and updating a data​
  - Setting the data to a series​
  - Updating the data in a series​

First of all, Lightweight Charts™ is a client-side library. This means that it does not and cannot work on the server-side (i.e. NodeJS), at least out of the box.

The code of lightweight-charts package targets the es2016 language specification. Thus, all the browsers you will have to work with should support this language revision (see this compatibility table). If you need to support the previous revisions, you could try to setup a transpilation of the package to the target you need to support in your build system (e.g. by using Babel). If you'll have any issues with that, please raise an issue on github with the details and we'll investigate possible ways to solve it.

The first thing you need to do to use lightweight-charts is to install it from npm:

Note that the package is shipped with TypeScript declarations, so you can easily use it within TypeScript code.

The library ships with the following build variants:

⚠️ Deprecation note: CommonJS support will be removed from the library at the start of 2024.

The Lightweight Charts™ license requires specifying TradingView as the product creator.

You shall add the "attribution notice" from the NOTICE file and a link to https://www.tradingview.com to the page of your website or mobile application that is available to your users.

As thanks for creating Lightweight Charts™, we'd be grateful if you add the attribution notice in a prominent place.

Once the library has been installed in your repo you're ready to create your first chart.

First of all, in a file where you would like to create a chart you need to import the library:

createChart is the entry-point for creating charts. You can use it to create as many charts as you need:

The result of this function is a IChartApi object, which you need to use to work with a chart instance.

Once your chart is created it is ready to display data.

The basic primitive to display a data is a series. There are different types of series:

To create a series with desired type you need to use appropriate method from IChartApi. All of them have the same naming add<type>Series, where <type> is a type of a series you'd like to create:

Please look at this page for more information about different series types.

Note that a series cannot be transferred from one type to another one since different series types have different data and options types.

Once your chart and series are created it's time to set data to the series.

Note that regardless of the series type, the API calls are the same (the type of the data might be different though).

To set the data (or to replace all data items) to a series you need to use ISeriesApi.setData method:

In a case when your data is updated (e.g. real-time updates) you might want to update the chart as well.

But using ISeriesApi.setData very often might affect the performance and we do not recommend to do this. Also it replaces all series data with the new one, and probably this is not what you're looking for.

Thus, to update the data you can use a method ISeriesApi.update. It allows you to update the last data item or add a new one much faster without affecting the performance:

**Examples:**

Example 1 (unknown):
```unknown
npm install --save lightweight-charts
```

Example 2 (sql):
```sql
import { createChart } from 'lightweight-charts';
```

Example 3 (sql):
```sql
import { createChart } from 'lightweight-charts';// ...// somewhere in your codeconst firstChart = createChart(document.getElementById('firstContainer'));const secondChart = createChart(document.getElementById('secondContainer'));
```

Example 4 (sql):
```sql
import { createChart } from 'lightweight-charts';const chart = createChart(container);const areaSeries = chart.addAreaSeries();const barSeries = chart.addBarSeries();const baselineSeries = chart.addBaselineSeries();// ... and so on
```

---

## From v2 to v3

**URL:** https://tradingview.github.io/lightweight-charts/docs/migrations/from-v2-to-v3

**Contents:**
- From v2 to v3
- Time Scale API​
- Two price scales​
  - Default behavior​
  - Left price scale​
  - No price scale​
  - Creating overlay​
  - Move price scale from right to left or vice versa​

Lightweight Charts™ library 3.0 announces the major improvements: supporting two price scales and improving the time scale API. In order of keep the API clear and consistent, we decided to allow breaking change of the API.

In this document you can find the migration guide from the previous version to 3.0.

Previously, to handle changing visible time range you needed to use subscribeVisibleTimeRangeChange and unsubscribeVisibleTimeRangeChange to subscribe and unsubscribe from visible range events. These methods were available in the chart object (e.g. you call it like chart.subscribeVisibleTimeRangeChange(func)).

In 3.0 in order to make API more consistent with the new API we decided to move these methods to ITimeScaleApi (along with the new subscription methods ITimeScaleApi.subscribeVisibleLogicalRangeChange and ITimeScaleApi.unsubscribeVisibleLogicalRangeChange).

So, to migrate your code to 3.0 you just need to replace:

We understand disadvantages of breaking changes in the API, so we have not removed support of the current API at all, but have deprecated it, so the most common cases will continue to work.

You can refer to the new API here.

Following are migration rules.

Default behavior is not changed. If you do not specify price scale options, the chart will have the right price scale visible and all the series will assign to it.

If you need the price scale to be drawn on the left side, you should make the following changes. instead of

then specify target price scale while creating a series:

New version fully supports this case via the old API, however this support will be removed in the future releases.

To create chart without any visible price scale, instead of

New version fully supports this case via the old API, however this support will be removed in the future releases.

To create an overlay series, instead of

New version fully supports this case via the old API, however this support will be removed in the future releases.

To do this, instead of

New version does not support this case via the old API, so, if you use it, you should migrate your code in order of keeping it working.

**Examples:**

Example 1 (css):
```css
const chart = LightweightCharts.createChart(container, {    priceScale: {        position: 'left',    },});
```

Example 2 (css):
```css
const chart = LightweightCharts.createChart(container, {    rightPriceScale: {        visible: false,    },    leftPriceScale: {        visible: true,    },});
```

Example 3 (css):
```css
const histSeries = chart.addHistogramSeries({    priceScaleId: 'left',});
```

Example 4 (css):
```css
const chart = LightweightCharts.createChart(container, {    priceScale: {        position: 'none',    },});
```

---

## Getting started

**URL:** https://tradingview.github.io/lightweight-charts/docs/3.8

**Contents:**
- Getting started
- Installation​
- License and attribution​
- Creating a chart​
- Creating a series​
- Setting and updating a data​
  - Setting the data to a series​
  - Updating the data in a series​

The first thing you need to do to use lightweight-charts is to install it from npm:

Note that the package is shipped with TypeScript declarations, so you can easily use it within TypeScript code.

The Lightweight Charts™ license requires specifying TradingView as the product creator.

You shall add the "attribution notice" from the NOTICE file and a link to https://www.tradingview.com to the page of your website or mobile application that is available to your users.

As thanks for creating Lightweight Charts™, we'd be grateful if you add the attribution notice in a prominent place.

Once the library has been installed in your repo you're ready to create your first chart.

First of all, in a file where you would like to create a chart you need to import the library:

createChart is the entry-point for creating charts. You can use it to create as many charts as you need:

The result of this function is a IChartApi object, which you need to use to work with a chart instance.

Once your chart is created it is ready to display data.

The basic primitive to display a data is a series. There are different types of series:

To create a series with desired type you need to use appropriate method from IChartApi. All of them have the same naming add<type>Series, where <type> is a type of a series you'd like to create:

Please look at this page for more information about different series types.

Note that a series cannot be transferred from one type to another one since different series types have different data and options types.

Once your chart and series are created it's time to set data to the series.

Note that regardless of the series type, the API calls are the same (the type of the data might be different though).

To set the data (or to replace all data items) to a series you need to use ISeriesApi.setData method:

In a case when your data is updated (e.g. real-time updates) you might want to update the chart as well.

But using ISeriesApi.setData very often might affect the performance and we do not recommend to do this. Also it replaces all series data with the new one, and probably this is not what you're looking for.

Thus, to update the data you can use a method ISeriesApi.update. It allows you to update the last data item or add a new one much faster without affecting the performance:

**Examples:**

Example 1 (unknown):
```unknown
npm install --save lightweight-charts
```

Example 2 (sql):
```sql
import { createChart } from 'lightweight-charts';
```

Example 3 (sql):
```sql
import { createChart } from 'lightweight-charts';// ...// somewhere in your codeconst firstChart = createChart(firstContainer);const secondChart = createChart(secondContainer);
```

Example 4 (sql):
```sql
import { createChart } from 'lightweight-charts';const chart = createChart(container);const areaSeries = chart.addAreaSeries();const barSeries = chart.addBarSeries();const baselineSeries = chart.addBaselineSeries();// ... and so on
```

---

## Release Notes

**URL:** https://tradingview.github.io/lightweight-charts/docs/release-notes

**Contents:**
- Release Notes
- 5.1.0​
  - Major Updates in 5.1​
    - Data Conflation​
- 5.0.9​
- 5.0.8​
- 5.0.7​
- 5.0.6​
- 5.0.5​
- 5.0.4​

Version 5.1.0 introduces data conflation, a powerful performance optimization feature designed for charts with very large datasets. For most use cases with typical dataset sizes, this feature will operate transparently in the background. However, if you're working with datasets containing tens of thousands of data points or more, conflation can dramatically improve rendering performance when users zoom out.

Data conflation is an automatic performance optimization that merges data points when zoomed out, significantly improving rendering performance for large datasets. When bar spacing falls below a threshold where multiple data points would be rendered in less than 0.5 pixels of screen space, the library intelligently combines them into single points.

This feature is particularly beneficial for applications displaying historical data spanning years or real-time feeds that accumulate large amounts of data over time. For typical use cases with moderate dataset sizes, conflation can remain disabled without any impact.

Fixed price axis label positioning when using plugin views that don't implement the optional fixedCoordinate() method. Previously, labels were incorrectly treated as positioned at coordinate 0, causing false overlap detection. (PR #1993, fixes #1986, contributed by @tpunt)

Fixed time scale fitContent method to properly respect the rightOffset option when rightOffsetPixels is not set. This addresses a regression introduced in version 5.0.9. (PR #1989, fixes #1988)

We'd like to thank our external contributors for their valuable contributions to this release:

Changes since the last published version.

Plugin & Indicator Examples

We'd like to thank our external contributors for their valuable contributions to this release:

Changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Version 5.0.0 represents a significant milestone in the evolution of Lightweight Charts™, delivering on our commitment to keep the library truly "lightweight". Despite adding numerous new features, improvements, and fixes, we've managed to reduce the bundle size by up to 16%, bringing the base bundle size down to just 35kB. This remarkable reduction was achieved through enhanced tree-shaking capabilities, modernized architecture, and careful optimization of core features.

This release introduces highly requested features like multi-pane support and new chart types. It also modernizes the codebase and improves its architecture to set a foundation for future enhancements without compromising on size.

One of our most requested features, multi-pane support is now available. It allows you to create complex chart layouts with multiple independent viewing areas. This enhancement enables sophisticated technical analysis setups and better visualization of related data series. Additional key benefits include:

See Panes for more information.

See Chart types for more information about the Yield Curve Chart and Options Chart types.

We've prepared a comprehensive migration guide to help you upgrade from v4 to v5. Key areas to note:

See the full migration guide: Migrating from v4 to v5

This release uses ES2020 syntax and no longer supports CommonJS. If you need to support older environments, you'll need to set up transpilation for the lightweight-charts package in your build system using tools like Babel.

As always, we thank you for your support and help in making Lightweight Charts™ the best product on the financial web. We look forward to seeing what you build with these new capabilities!

You can always send us your feedback via GitHub. Happy trading!

See changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Changes since the last published version.

Version 4.1 of Lightweight Charts introduces exciting new features, including the introduction of Plugins, which provide developers the ability to extend the library's functionality. Additionally, this release includes enhancements to customize the horizontal scale and various minor improvements and bug fixes.

Developers can now leverage the power of Plugins in Lightweight Charts. Two types of Plugins are supported - Custom Series and Drawing Primitives, offering the ability to define new series types and create custom visualizations, drawing tools, and annotations.

With the flexibility provided by these plugins, developers can create highly customizable charting applications for their users.

To get started with plugins, please refer to our Plugins Documentation for a better understanding of what is possible and how plugins work. You can also explore our collection of plugin examples (with a preview hosted here) for inspiration and guidance on implementing specific functionality.

To help you get started quickly, we have created an NPM package called create-lwc-plugin, which sets up a plugin project for you. This way, you can hit the ground running with your plugin development.

Horizontal Scale Customization

The horizontal scale is no longer restricted to only time-based values. The API has been extended to allow customization of the horizontal scale behavior, and enable uses cases like options chart where price values are displayed in the horizontal scale. The most common use-case would be to customise the tick marks behaviour.

The createChartEx function should be used instead of the usual createChart function, and an instance of a class implementing IHorzScaleBehavior should be provided.

A simple example can be found in this test case: horizontal-price-scale.js

Thanks to our Contributors for this Release:

You can always send us your feedback via GitHub. We look forward to hearing from you! And as always, happy trading!

See issues assigned to this version's milestone or changes since the last published version.

As always, we thank you for your support and help in making Lightweight Charts™ the best product on the financial web. And a big shout out to our hero contributors @victorbrambati, and @UcheAzubuko!

You can always send us your feedback via GitHub.

We look forward to hearing from you! And as always, happy trading! Team TradingView

See issues assigned to this version's milestone or changes since the last published version.

Long overdue as it’s been nearly 1 year since our last major update, but behold before all the changes that have happened over the last 12 months.

In total, more than 20 tickets have been addressed with one of the most important ones being fancy-canvas – the library we use to configure HTML canvas in Lightweight Charts™.

Please view the migration guide here: Migrating from v3 to v4.

As always, we thank you for your support and help in making Lightweight Charts™ the best product on the financial web. And a big shout out to our hero contributors thanhlmm, CommanderRoot, samhainsamhainsamhain & colleague Nipheris! You can always send us your feedback via GitHub. We look forward to hearing from you! And as always, happy trading! Team TradingView

See issues assigned to this version's milestone or changes since the last published version.

We're happy to announce the next release of Lightweight Charts™ library. This release includes many improvements and bug fixes (as usual), but we are thrilled to say that from this version the library has its own documentation website that replaces the documentation in the repository. Check it out and share your feedback in this discussion thread.

Thanks to our contributors:

See issues assigned to this version's milestone or changes since the last published version.

Thanks to our contributors:

See issues assigned to this version's milestone or changes since the last published version.

See changes since the last published version.

On this day 10 years ago, 10th September 2011, the very first version of the TradingView website was deployed. To celebrate 10th anniversary we're happy to announce the new version of lightweight-charts library v3.6.0 🎉🎉🎉

Thanks to our contributors:

See issues assigned to this version's milestone or changes since the last published version.

A note about rendering order of series, which might be interpret as a bug or breaking change since this release

This is not really a breaking change, but might be interpret like that. In #794 we've fixed the wrong order of series, thus now all series will be displayed in opposite order (they will be displayed in order of creating now; previously they were displayed in reversed order).

To fix that, just change the order of creating the series (thus instead of create series A, then series B create series B first and then series A) - see #812.

Thanks to our contributors:

See issues assigned to this version's milestone or changes since the last published version.

See issues assigned to this version's milestone or changes since the last published version.

Thanks to our contributors:

See issues assigned to this version's milestone or changes since the last published version.

Thanks to our contributors:

See issues assigned to this version's milestone or changes since the last published version.

It's a just re-published accidentally published 3.1.4 version, which didn't actually fix the issue #536.

Version 3.1.4 has been deprecated.

See issues assigned to this version's milestone or changes since the last published version.

See issues assigned to this version's milestone or changes since the last published version.

See issues assigned to this version's milestone or changes since the last published version.

See issues assigned to this version's milestone or changes since the last published version.

Undocumented breaking changes

We know that some of users probably used some hacky-workarounds calling internal methods to achieve multi-pane support. In this release, to reduce size of the bundle we dropped out a code for pane's separator (which allows to resize panes).

As soon this workaround is undocumented and we don't support this feature yet - we don't bump a major version. But we think it's better to let you know that it has been changed.

Thanks to our contributors:

See issues assigned to this version's milestone or changes since the last published version.

See issues assigned to this version's milestone or changes since the last published version.

We have some breaking changes since the latest version due some features and API improvements:

See breaking changes doc with migration guide to migrate smoothly.

Thanks to our contributors:

See issues assigned to this version's milestone or changes since the last published version.

Thanks to our contributors:

See issues assigned to this version’s milestone or changes since the last published version.

See issues assigned to this version’s milestone or changes since the last published version.

Thanks to our contributors:

See issues assigned to this version’s milestone or changes since the last published version.

The docs for this version are available here.

---

## From v4 to v5

**URL:** https://tradingview.github.io/lightweight-charts/docs/migrations/from-v4-to-v5

**Contents:**
- From v4 to v5
- Table of Contents​
- Series changes​
  - Overview of Changes​
  - Migration Steps​
    - Before (v4)​
    - After (v5)​
    - Migration Reference​
  - Usage Examples​
- Series Markers​

In this document you can find the migration guide from the previous version v4 to v5.

Replace all series creation calls with the new addSeries syntax. Here's how the migration works for each series type:

Here's how to migrate each series type:

UMD (Universal Module Definition):

Note: Make sure to import the specific series type (e.g., LineSeries, AreaSeries) along with createChart when using ES Modules. For UMD builds, all series types are available under the LightweightCharts namespace.

If your application doesn't use markers, you can now benefit from a smaller bundle size as this functionality is no longer included in the core package.

In the new version of Lightweight Charts, the watermark feature has undergone significant changes:

The TextWatermark plugin is now available as follows:

The options structure for watermarks has been revised:

To use the plugin, you need pass a pane object to the createTextWatermark function. The pane object specifies where the watermark should be attached:

Here's a comprehensive example demonstrating how to implement a text watermark in the new version:

Some of the plugin types and interfaces have been renamed due to the additional of Pane Primitives.

**Examples:**

Example 1 (sql):
```sql
// Example with Line Series in v4import { createChart } from 'lightweight-charts';const chart = createChart(container, {});const lineSeries = chart.addLineSeries({ color: 'red' });
```

Example 2 (sql):
```sql
// Example with Line Series in v5import { createChart, LineSeries } from 'lightweight-charts';const chart = createChart(container, {});const lineSeries = chart.addSeries(LineSeries, { color: 'red' });
```

Example 3 (sql):
```sql
import { createChart, LineSeries } from 'lightweight-charts';const chart = createChart(container, {});const lineSeries = chart.addSeries(LineSeries, { color: 'red' });
```

Example 4 (css):
```css
const chart = LightweightCharts.createChart(container, {});const lineSeries = chart.addSeries(LightweightCharts.LineSeries, { color: 'red' });
```

---

## Getting started

**URL:** https://tradingview.github.io/lightweight-charts/docs/4.2

**Contents:**
- Getting started
- Requirements​
- Installation​
  - Build variants​
- License and attribution​
- Creating a chart​
- Creating a series​
- Setting and updating a data​
  - Setting the data to a series​
  - Updating the data in a series​

First of all, Lightweight Charts™ is a client-side library. This means that it does not and cannot work on the server-side (i.e. NodeJS), at least out of the box.

The code of lightweight-charts package targets the es2016 language specification. Thus, all the browsers you will have to work with should support this language revision (see this compatibility table). If you need to support the previous revisions, you could try to setup a transpilation of the package to the target you need to support in your build system (e.g. by using Babel). If you'll have any issues with that, please raise an issue on github with the details and we'll investigate possible ways to solve it.

The first thing you need to do to use lightweight-charts is to install it from npm:

Note that the package is shipped with TypeScript declarations, so you can easily use it within TypeScript code.

The library ships with the following build variants:

⚠️ Deprecation note: CommonJS support will be removed from the library at the start of 2024.

The Lightweight Charts™ license requires specifying TradingView as the product creator.

You shall add the "attribution notice" from the NOTICE file and a link to https://www.tradingview.com/ to the page of your website or mobile application that is available to your users.

As thanks for creating Lightweight Charts™, we'd be grateful if you add the attribution notice in a prominent place.

Once the library has been installed in your repo you're ready to create your first chart.

First of all, in a file where you would like to create a chart you need to import the library:

createChart is the entry-point for creating charts. You can use it to create as many charts as you need:

The result of this function is a IChartApi object, which you need to use to work with a chart instance.

Once your chart is created it is ready to display data.

The basic primitive to display a data is a series. There are different types of series:

To create a series with desired type you need to use appropriate method from IChartApi. All of them have the same naming add<type>Series, where <type> is a type of a series you'd like to create:

Please look at this page for more information about different series types.

Note that a series cannot be transferred from one type to another one since different series types have different data and options types.

Once your chart and series are created it's time to set data to the series.

Note that regardless of the series type, the API calls are the same (the type of the data might be different though).

To set the data (or to replace all data items) to a series you need to use ISeriesApi.setData method:

In a case when your data is updated (e.g. real-time updates) you might want to update the chart as well.

But using ISeriesApi.setData very often might affect the performance and we do not recommend to do this. Also it replaces all series data with the new one, and probably this is not what you're looking for.

Thus, to update the data you can use a method ISeriesApi.update. It allows you to update the last data item or add a new one much faster without affecting the performance:

**Examples:**

Example 1 (unknown):
```unknown
npm install --save lightweight-charts
```

Example 2 (sql):
```sql
import { createChart } from 'lightweight-charts';
```

Example 3 (sql):
```sql
import { createChart } from 'lightweight-charts';// ...// somewhere in your codeconst firstChart = createChart(document.getElementById('firstContainer'));const secondChart = createChart(document.getElementById('secondContainer'));
```

Example 4 (sql):
```sql
import { createChart } from 'lightweight-charts';const chart = createChart(container);const areaSeries = chart.addAreaSeries();const barSeries = chart.addBarSeries();const baselineSeries = chart.addBaselineSeries();// ... and so on
```

---

## Android wrapper

**URL:** https://tradingview.github.io/lightweight-charts/docs/android

**Contents:**
- Android wrapper
- Installation​
- Usage​
- How to run the provided example​

You can find the source code of the Lightweight Charts™ Android wrapper in this repository.

This wrapper is currently still using v3.8.0. This will be updated to v4.0.0 in the near future.

You can use Lightweight Charts™ inside an Android application. To use Lightweight Charts™ in that context, you can use our Android wrapper, which will allow you to interact with Lightweight Charts™ library, which will be rendered in a web view.

Requires minSdkVersion 21, and installed WebView with support of ES6

In /gradle_module/build.gradle

Add view to the layout.

Configure the chart layout.

Add any series to the chart and store a reference to it.

Add data to the series.

The GitHub repository for lightweight-charts-android contains an example of the library in action. You can run the example (LighweightCharts.app) by cloning the repository and opening it in Android Studio. You will need to have NodeJS/NPM installed.

**Examples:**

Example 1 (unknown):
```unknown
allprojects {    repositories {        google()        mavenCentral()    }}
```

Example 2 (json):
```json
dependencies {    //...    implementation 'com.tradingview:lightweightcharts:3.8.0'}
```

Example 3 (xml):
```xml
<androidx.constraintlayout.widget.ConstraintLayout        android:layout_width="match_parent"        android:layout_height="match_parent">        <com.tradingview.lightweightcharts.view.ChartsView            android:id="@+id/charts_view"            android:layout_width="0dp"            android:layout_height="0dp"            app:layout_constraintBottom_toBottomOf="parent"            app:layout_constraintLeft_toLeftOf="parent"            app:layout_constraintRight_toRightOf="parent"            app:layout_constraintTop_toTopOf="parent" /></androidx.constraintlayout.widget.ConstraintLayout>
```

Example 4 (kotlin):
```kotlin
charts_view.api.applyOptions {    layout = layoutOptions {        background = SolidColor(Color.LTGRAY)        textColor = Color.BLACK.toIntColor()    }    localization = localizationOptions {        locale = "ru-RU"        priceFormatter = PriceFormatter(template = "{price:#2:#3}$")        timeFormatter = TimeFormatter(            locale = "ru-RU",            dateTimeFormat = DateTimeFormat.DATE_TIME        )    }}
```

---

## Getting started

**URL:** https://tradingview.github.io/lightweight-charts/docs/next

**Contents:**
- Getting started
- Requirements​
- Installation​
  - Build variants​
- License and attribution​
- Creating a chart​
- Creating a series​
- Setting and updating a data​
  - Setting the data to a series​
  - Updating the data in a series​

Lightweight Charts™ is a client-side library that is not designed to work on the server side, for example, with Node.js.

The library code targets the ES2020 language specification. Therefore, the browsers you work with should support this language revision. Consider the following table to ensure the browser compatibility.

To support previous revisions, you can set up a transpilation process for the lightweight-charts package in your build system using tools such as Babel. If you encounter any issues, open a GitHub issue with detailed information, and we will investigate potential solutions.

To set up the library, install the lightweight-charts npm package:

The package includes TypeScript declarations, enabling seamless integration within TypeScript projects.

The library ships with the following build variants:

The Lightweight Charts™ license requires specifying TradingView as the product creator. You should add the following attributes to a public page of your website or mobile application:

As a first step, import the library to your file:

To create a chart, use the createChart function. You can call the function multiple times to create as many charts as needed:

As a result, createChart returns an IChartApi object that allows you to interact with the created chart.

When the chart is created, you can display data on it.

The basic primitive to display data is a series. The library supports the following series types:

To create a series, use the addSeries method from IChartApi. As a parameter, specify a series type you would like to create:

Note that a series cannot be transferred from one type to another one, since different series types require different data and options types.

When the series is created, you can populate it with data. Note that the API calls remain the same regardless of the series type, although the data format may vary.

To set the data to a series, you should call the ISeriesApi.setData method:

You can also use setData to replace all data items.

If your data is updated, for example in real-time, you may also need to refresh the chart accordingly. To do this, call the ISeriesApi.update method that allows you to update the last data item or add a new one.

We do not recommend calling ISeriesApi.setData to update the chart, as this method replaces all series data and can significantly affect the performance.

**Examples:**

Example 1 (unknown):
```unknown
npm install --save lightweight-charts
```

Example 2 (sql):
```sql
import { createChart } from 'lightweight-charts';
```

Example 3 (sql):
```sql
import { createChart } from 'lightweight-charts';// ...const firstChart = createChart(document.getElementById('firstContainer'));const secondChart = createChart(document.getElementById('secondContainer'));
```

Example 4 (sql):
```sql
import { AreaSeries, BarSeries, BaselineSeries, createChart } from 'lightweight-charts';const chart = createChart(container);const areaSeries = chart.addSeries(AreaSeries);const barSeries = chart.addSeries(BarSeries);const baselineSeries = chart.addSeries(BaselineSeries);// ...
```

---

## Getting started

**URL:** https://tradingview.github.io/lightweight-charts/docs

**Contents:**
- Getting started
- Requirements​
- Installation​
  - Build variants​
- License and attribution​
- Creating a chart​
- Creating a series​
- Setting and updating a data​
  - Setting the data to a series​
  - Updating the data in a series​

Lightweight Charts™ is a client-side library that is not designed to work on the server side, for example, with Node.js.

The library code targets the ES2020 language specification. Therefore, the browsers you work with should support this language revision. Consider the following table to ensure the browser compatibility.

To support previous revisions, you can set up a transpilation process for the lightweight-charts package in your build system using tools such as Babel. If you encounter any issues, open a GitHub issue with detailed information, and we will investigate potential solutions.

To set up the library, install the lightweight-charts npm package:

The package includes TypeScript declarations, enabling seamless integration within TypeScript projects.

The library ships with the following build variants:

The Lightweight Charts™ license requires specifying TradingView as the product creator. You should add the following attributes to a public page of your website or mobile application:

As a first step, import the library to your file:

To create a chart, use the createChart function. You can call the function multiple times to create as many charts as needed:

As a result, createChart returns an IChartApi object that allows you to interact with the created chart.

When the chart is created, you can display data on it.

The basic primitive to display data is a series. The library supports the following series types:

To create a series, use the addSeries method from IChartApi. As a parameter, specify a series type you would like to create:

Note that a series cannot be transferred from one type to another one, since different series types require different data and options types.

When the series is created, you can populate it with data. Note that the API calls remain the same regardless of the series type, although the data format may vary.

To set the data to a series, you should call the ISeriesApi.setData method:

You can also use setData to replace all data items.

If your data is updated, for example in real-time, you may also need to refresh the chart accordingly. To do this, call the ISeriesApi.update method that allows you to update the last data item or add a new one.

We do not recommend calling ISeriesApi.setData to update the chart, as this method replaces all series data and can significantly affect the performance.

**Examples:**

Example 1 (unknown):
```unknown
npm install --save lightweight-charts
```

Example 2 (sql):
```sql
import { createChart } from 'lightweight-charts';
```

Example 3 (sql):
```sql
import { createChart } from 'lightweight-charts';// ...const firstChart = createChart(document.getElementById('firstContainer'));const secondChart = createChart(document.getElementById('secondContainer'));
```

Example 4 (sql):
```sql
import { AreaSeries, BarSeries, BaselineSeries, createChart } from 'lightweight-charts';const chart = createChart(container);const areaSeries = chart.addSeries(AreaSeries);const barSeries = chart.addSeries(BarSeries);const baselineSeries = chart.addSeries(BaselineSeries);// ...
```

---
