# Lightweight-Charts - Guides

**Pages:** 16

---

## Series

**URL:** https://tradingview.github.io/lightweight-charts/docs/series-types

**Contents:**
- Series
- Supported types​
  - Area​
  - Bar​
  - Baseline​
  - Candlestick​
  - Histogram​
  - Line​
  - Custom series (plugins)​
- Customization​

This article describes supported series types and ways to customize them.

This series is represented with a colored area between the time scale and line connecting all data points:

This series illustrates price movements with vertical bars. The length of each bar corresponds to the range between the highest and lowest price values. Open and close values are represented with the tick marks on the left and right side of the bar, respectively:

This series is represented with two colored areas between the the base value line and line connecting all data points:

This series illustrates price movements with candlesticks. The solid body of each candlestick represents the open and close values for the time period. Vertical lines, known as wicks, above and below the candle body represent the high and low values, respectively:

This series illustrates the distribution of values with columns:

This series is represented with a set of data points connected by straight line segments:

The library enables you to create custom series types, also known as series plugins, to expand its functionality. With this feature, you can add new series types, indicators, and other visualizations.

To define a custom series type, create a class that implements the ICustomSeriesPaneView interface. This class defines the rendering code that Lightweight Charts™ uses to draw the series on the chart. Once your custom series type is defined, it can be added to any chart instance using the addCustomSeries() method. Custom series types function like any other series.

For more information, refer to the Plugins article.

Each series type offers a unique set of customization options listed on the SeriesStyleOptionsMap page.

You can adjust series options in two ways:

Specify the default options using the corresponding parameter while creating a series:

Use the ISeriesApi.applyOptions method to apply other options on the fly:

**Examples:**

Example 1 (css):
```css
const chartOptions = { layout: { textColor: 'black', background: { type: 'solid', color: 'white' } } };const chart = createChart(document.getElementById('container'), chartOptions);const areaSeries = chart.addSeries(AreaSeries, { lineColor: '#2962FF', topColor: '#2962FF', bottomColor: 'rgba(41, 98, 255, 0.28)' });const data = [{ value: 0, time: 1642425322 }, { value: 8, time: 1642511722 }, { value: 10, time: 1642598122 }, { value: 20, time: 1642684522 }, { value: 3, time: 1642770922 }, { value: 43, time: 1642857322 }, { value: 41, time: 1642943722 }, { value: 43, time: 1643030122 }, { value: 56, time: 1643116522 }, { value: 46, time: 1643202922 }];areaSeries.setData(data);chart.timeScale().fitContent();
```

Example 2 (css):
```css
const chartOptions = { layout: { textColor: 'black', background: { type: 'solid', color: 'white' } } };const chart = createChart(document.getElementById('container'), chartOptions);const barSeries = chart.addSeries(BarSeries, { upColor: '#26a69a', downColor: '#ef5350' });const data = [  { open: 10, high: 10.63, low: 9.49, close: 9.55, time: 1642427876 },  { open: 9.55, high: 10.30, low: 9.42, close: 9.94, time: 1642514276 },  { open: 9.94, high: 10.17, low: 9.92, close: 9.78, time: 1642600676 },  { open: 9.78, high: 10.59, low: 9.18, close: 9.51, time: 1642687076 },  { open: 9.51, high: 10.46, low: 9.10, close: 10.17, time: 1642773476 },  { open: 10.17, high: 10.96, low: 10.16, close: 10.47, time: 1642859876 },  { open: 10.47, high: 11.39, low: 10.40, close: 10.81, time: 1642946276 },  { open: 10.81, high: 11.60, low: 10.30, close: 10.75, time: 1643032676 },  { open: 10.75, high: 11.60, low: 10.49, close: 10.93, time: 1643119076 },  { open: 10.93, high: 11.53, low: 10.76, close: 10.96, time: 1643205476 },  { open: 10.96, high: 11.90, low: 10.80, close: 11.50, time: 1643291876 },  { open: 11.50, high: 12.00, low: 11.30, close: 11.80, time: 1643378276 },  { open: 11.80, high: 12.20, low: 11.70, close: 12.00, time: 1643464676 },  { open: 12.00, high: 12.50, low: 11.90, close: 12.30, time: 1643551076 },  { open: 12.30, high: 12.80, low: 12.10, close: 12.60, time: 1643637476 },  { open: 12.60, high: 13.00, low: 12.50, close: 12.90, time: 1643723876 },  { open: 12.90, high: 13.50, low: 12.70, close: 13.20, time: 1643810276 },  { open: 13.20, high: 13.70, low: 13.00, close: 13.50, time: 1643896676 },  { open: 13.50, high: 14.00, low: 13.30, close: 13.80, time: 1643983076 },  { open: 13.80, high: 14.20, low: 13.60, close: 14.00, time: 1644069476 },];barSeries.setData(data);chart.timeScale().fitContent();
```

Example 3 (css):
```css
const chartOptions = { layout: { textColor: 'black', background: { type: 'solid', color: 'white' } } };const chart = createChart(document.getElementById('container'), chartOptions);const baselineSeries = chart.addSeries(BaselineSeries, { baseValue: { type: 'price', price: 25 }, topLineColor: 'rgba( 38, 166, 154, 1)', topFillColor1: 'rgba( 38, 166, 154, 0.28)', topFillColor2: 'rgba( 38, 166, 154, 0.05)', bottomLineColor: 'rgba( 239, 83, 80, 1)', bottomFillColor1: 'rgba( 239, 83, 80, 0.05)', bottomFillColor2: 'rgba( 239, 83, 80, 0.28)' });const data = [{ value: 1, time: 1642425322 }, { value: 8, time: 1642511722 }, { value: 10, time: 1642598122 }, { value: 20, time: 1642684522 }, { value: 3, time: 1642770922 }, { value: 43, time: 1642857322 }, { value: 41, time: 1642943722 }, { value: 43, time: 1643030122 }, { value: 56, time: 1643116522 }, { value: 46, time: 1643202922 }];baselineSeries.setData(data);chart.timeScale().fitContent();
```

Example 4 (css):
```css
const chartOptions = { layout: { textColor: 'black', background: { type: 'solid', color: 'white' } } };const chart = createChart(document.getElementById('container'), chartOptions);const candlestickSeries = chart.addSeries(CandlestickSeries, { upColor: '#26a69a', downColor: '#ef5350', borderVisible: false, wickUpColor: '#26a69a', wickDownColor: '#ef5350' });const data = [{ open: 10, high: 10.63, low: 9.49, close: 9.55, time: 1642427876 }, { open: 9.55, high: 10.30, low: 9.42, close: 9.94, time: 1642514276 }, { open: 9.94, high: 10.17, low: 9.92, close: 9.78, time: 1642600676 }, { open: 9.78, high: 10.59, low: 9.18, close: 9.51, time: 1642687076 }, { open: 9.51, high: 10.46, low: 9.10, close: 10.17, time: 1642773476 }, { open: 10.17, high: 10.96, low: 10.16, close: 10.47, time: 1642859876 }, { open: 10.47, high: 11.39, low: 10.40, close: 10.81, time: 1642946276 }, { open: 10.81, high: 11.60, low: 10.30, close: 10.75, time: 1643032676 }, { open: 10.75, high: 11.60, low: 10.49, close: 10.93, time: 1643119076 }, { open: 10.93, high: 11.53, low: 10.76, close: 10.96, time: 1643205476 }];candlestickSeries.setData(data);chart.timeScale().fitContent();
```

---

## Price scale

**URL:** https://tradingview.github.io/lightweight-charts/docs/price-scale

**Contents:**
- Price scale
- Create price scale​
- Modify price scale​
- Remove price scale​

The price scale (or price axis) is a vertical scale that maps prices to coordinates and vice versa. The conversion rules depend on the price scale mode, the chart's height, and the visible part of the data.

By default, a chart has two visible price scales: left and right. Additionally, you can create an unlimited number of overlay price scales, which remain hidden in the UI. Overlay price scales allow series to be plotted without affecting the existing visible scales. This is particularly useful for indicators like Volume, where values can differ significantly from price data.

To create an overlay price scale, assign priceScaleId to a series. Note that the priceScaleId value should differ from price scale IDs on the left and right. The chart will create an overlay price scale with the provided ID.

If a price scale with such ID already exists, a series will be attached to the existing price scale. Further, you can use the provided price scale ID to retrieve its API object using the IChartApi.priceScale method.

See the Price and Volume article for an example of adding a Volume indicator using an overlay price scale.

To modify the left price scale, use the leftPriceScale option. For the right price scale, use rightPriceScale. To change the default settings for an overlay price scale, use the overlayPriceScales option.

You can use the IChartApi.priceScale method to retrieve the API object for any price scale. Similarly, to access the API object for the price scale that a series is attached to, use the ISeriesApi.priceScale method.

The default left and right price scales cannot be removed, you can only hide them by setting the visible option to false.

An overlay price scale exists as long as at least one series is attached to it. To remove an overlay price scale, remove all series attached to this price scale.

---

## Chart types

**URL:** https://tradingview.github.io/lightweight-charts/docs/chart-types

**Contents:**
- Chart types
- Standard Time-based Chart​
- Yield Curve Chart​
- Options Chart (Price-based)​
- Custom Horizontal Scale Chart​
- Choosing the Right Chart Type​

Lightweight Charts offers different types of charts to suit various data visualization needs. This article provides an overview of the available chart types and how to create them.

The standard time-based chart is the most common type, suitable for displaying time series data.

This chart type uses time values for the horizontal scale and is ideal for most financial and time series data visualizations.

The yield curve chart is specifically designed for displaying yield curves, common in financial analysis.

Use this chart type when you need to visualize yield curves or similar financial data where the horizontal scale represents time durations rather than specific dates.

If you want to spread out the beginning of the plot further and don't need a linear time scale, you can enforce a minimum spacing around each point by increasing the minBarSpacing option in the TimeScaleOptions. To prevent the rest of the chart from spreading too wide, adjust the baseResolution to a larger number, such as 12 (months).

The options chart is a specialized type that uses price values on the horizontal scale instead of time.

This chart type is particularly useful for financial instruments like options, where the price is a more relevant x-axis metric than time.

For advanced use cases, Lightweight Charts allows creating charts with custom horizontal scale behavior.

This method provides the flexibility to define custom horizontal scale behavior, allowing for unique and specialized chart types.

Each chart type provides specific functionality and is optimized for different use cases. Consider your data structure and visualization requirements when selecting the appropriate chart type for your application.

**Examples:**

Example 1 (sql):
```sql
import { createChart } from 'lightweight-charts';const chart = createChart(document.getElementById('container'), options);
```

Example 2 (css):
```css
const chartOptions = { layout: { textColor: 'black', background: { type: 'solid', color: 'white' } } };const chart = createChart(document.getElementById('container'), chartOptions);const areaSeries = chart.addSeries(AreaSeries, { lineColor: '#2962FF', topColor: '#2962FF', bottomColor: 'rgba(41, 98, 255, 0.28)' });const data = [{ value: 0, time: 1642425322 }, { value: 8, time: 1642511722 }, { value: 10, time: 1642598122 }, { value: 20, time: 1642684522 }, { value: 3, time: 1642770922 }, { value: 43, time: 1642857322 }, { value: 41, time: 1642943722 }, { value: 43, time: 1643030122 }, { value: 56, time: 1643116522 }, { value: 46, time: 1643202922 }];areaSeries.setData(data);chart.timeScale().fitContent();
```

Example 3 (sql):
```sql
import { createYieldCurveChart } from 'lightweight-charts';const chart = createYieldCurveChart(document.getElementById('container'), options);
```

Example 4 (css):
```css
const chartOptions = {    layout: { textColor: 'black', background: { type: 'solid', color: 'white' } },    yieldCurve: { baseResolution: 1, minimumTimeRange: 10, startTimeRange: 3 },    handleScroll: false, handleScale: false,};const chart = createYieldCurveChart(document.getElementById('container'), chartOptions);const lineSeries = chart.addSeries(LineSeries, { color: '#2962FF' });const curve = [{ time: 1, value: 5.378 }, { time: 2, value: 5.372 }, { time: 3, value: 5.271 }, { time: 6, value: 5.094 }, { time: 12, value: 4.739 }, { time: 24, value: 4.237 }, { time: 36, value: 4.036 }, { time: 60, value: 3.887 }, { time: 84, value: 3.921 }, { time: 120, value: 4.007 }, { time: 240, value: 4.366 }, { time: 360, value: 4.290 }];lineSeries.setData(curve);chart.timeScale().fitContent();
```

---

## Crosshair and Grid Line Width Calculations

**URL:** https://tradingview.github.io/lightweight-charts/docs/plugins/pixel-perfect-rendering/widths/crosshair

**Contents:**
- Crosshair and Grid Line Width Calculations

It is recommend that you first read the Pixel Perfect Rendering page.

The following functions can be used to get the calculated width that the library would use for a crosshair or grid line at a specific device pixel ratio.

**Examples:**

Example 1 (typescript):
```typescript
/** * Default grid / crosshair line width in Bitmap sizing * @param horizontalPixelRatio - horizontal pixel ratio * @returns default grid / crosshair line width in Bitmap sizing */export function gridAndCrosshairBitmapWidth(    horizontalPixelRatio: number): number {    return Math.max(1, Math.floor(horizontalPixelRatio));}/** * Default grid / crosshair line width in Media sizing * @param horizontalPixelRatio - horizontal pixel ratio * @returns default grid / crosshair line width in Media sizing */export function gridAndCrosshairMediaWidth(    horizontalPixelRatio: number): number {    return (        gridAndCrosshairBitmapWidth(horizontalPixelRatio) / horizontalPixelRatio    );}
```

---

## Panes

**URL:** https://tradingview.github.io/lightweight-charts/docs/panes

**Contents:**
- Panes
- Customization Options​
- Managing Panes​

Panes are essential elements that help segregate data visually within a single chart. Panes are useful when you have a chart that needs to show more than one kind of data. For example, you might want to see a stock's price over time in one pane and its trading volume in another. This setup helps users get a fuller picture without cluttering the chart.

By default, Lightweight Charts™ has a single pane, however, you can add more panes to the chart to display different series in separate areas. For detailed examples and code snippets on how to implement panes in your charts see tutorial.

Lightweight Charts™ offers a few customization options to tailor the appearance and behavior of panes:

Pane Separator Color: Customize the color of the pane separators to match the chart design or improve visibility.

Separator Hover Color: Enhance user interaction by changing the color of separators on mouse hover.

Resizable Panes: Opt to enable or disable the resizing of panes by the user, offering flexibility in how data is displayed.

While the specific methods to manipulate panes are covered in the detailed example, it's important to note that Lightweight Charts™ provides an API for pane management. This includes adding new panes, moving series between panes, adjusting pane height, and removing panes. The API ensures that developers have full control over the pane lifecycle and organization within their charts.

---

## Series Primitives

**URL:** https://tradingview.github.io/lightweight-charts/docs/plugins/series-primitives

**Contents:**
- Series Primitives
- Views​
  - IPrimitivePaneView​
    - Interactive Demo of zOrder layers​
  - ISeriesPrimitiveAxisView​
- Lifecycle Methods​
  - attached​
  - detached​
- Updating Views​
- Extending the Autoscale Info​

Primitives are extensions to the series which can define views and renderers to draw on the chart using CanvasRenderingContext2D.

Primitives are defined by implementing the ISeriesPrimitive interface. The interface defines the basic functionality and structure required for creating custom primitives.

The primary purpose of a series primitive is to provide one, or more, views to the library which contain the state and logic required to draw on the chart panes.

There are two types of views which are supported within ISeriesPrimitive which are:

The library will evoke the following getter functions (if defined) to get references to the primitive's defined views for the corresponding section of the chart:

The first three views allow drawing on the corresponding panes (main chart pane, price scale pane, and horizontal time scale pane) using the CanvasRenderingContext2D and should implement the ISeriesPrimitivePaneView interface.

The views returned by the priceAxisViews and timeAxisViews getter methods should implement the ISeriesPrimitiveAxisView interface and are used to define labels to be drawn on the corresponding scales.

Below is a visual example showing the various sections of the chart where a Primitive can draw.

The IPrimitivePaneView interface can be used to define a view which provides a renderer (implementing the IPrimitivePaneRenderer interface) for drawing on the corresponding area of the chart using the CanvasRenderingContext2D API. The view can define a zOrder to control where in the visual stack the drawing will occur (See PrimitivePaneViewZOrder for more information).

Renderers should provide a draw method which will be given a CanvasRenderingTarget2D target on which it can draw. Additionally, a renderer can optionally provide a drawBackground method for drawing beneath other elements on the same zOrder.

CanvasRenderingTarget2D is explained in more detail on the Canvas Rendering Target page.

Below is an interactive demo chart illustrating where each zOrder is drawn relative to the existing chart elements such as the grid, series, and crosshair.

The ISeriesPrimitiveAxisView interface can be used to define a label on the price or time axis.

This interface provides several methods to define the appearance and position of the label, such as the coordinate method, which should return the desired coordinate for the label on the axis. It also defines optional methods to set the fixed coordinate, text, text color, background color, and visibility of the label.

Please see the ISeriesPrimitiveAxisView interface for more details.

Your primitive can use the attached and detached lifecycle methods to manage the lifecycle of the primitive, such as creating or removing external objects and event handlers.

This method is called when the primitive is attached to a chart. The attached method is evoked with a single argument containing properties for the chart, series, and a callback to request an update. The chart and series properties are references to the chart API and the series API instances for convenience purposes so that they don't need to be manually provided within the primitive's constructor (if needed by the primitive).

The requestUpdate callback allows the primitive to notify the chart that it should be updated and redrawn.

This method is called when the primitive is detached from a chart. This can be used to remove any external objects or event handlers that were created during the attached lifecycle method.

Your primitive should update the views in the updateAllViews() method such that when the renderers are evoked, they can draw with the latest information. The library invokes this method when it wants to update and redraw the chart. If you would like to notify the library that it should trigger an update then you can use the requestUpdate callback provided by the attached lifecycle method.

The autoscaleInfo() method can be provided to extend the base autoScale information of the series. This can be used to ensure that the chart is automatically scaled correctly to include all the graphics drawn by the primitive.

Whenever the chart needs to calculate the vertical visible range of the series within the current time range then it will evoke this method. This method can be omitted and the library will use the normal autoscale information for the series. If the method is implemented then the returned values will be merged with the base autoscale information to define the vertical visible range.

Please note that this method will be evoked very often during scrolling and zooming of the chart, thus it is recommended that this method is either simple to execute, or makes use of optimisations such as caching to ensure that the chart remains responsive.

---

## Plugins

**URL:** https://tradingview.github.io/lightweight-charts/docs/plugins/intro

**Contents:**
- Plugins
- Custom series​
- Primitives​
  - Series primitives​
  - Pane primitives​

Plugins allow you to extend the library's functionality and render custom elements, such as new series, drawing tools, indicators, and watermarks.

You can create plugins of the following types:

Custom series allow you to define new types of series with custom data structures and rendering logic. For implementation details, refer to the Custom Series Types article.

Use the addCustomSeries method to add a custom series to the chart. Then, you can manage it through the same API available for built-in series. For example, call the setData method to populate the series with data.

Primitives allow you to define custom visualizations, drawing tools, and chart annotations. You can render them at different levels in the visual stack to create complex, layered compositions.

Series primitives are attached to a specific series and can render on the main pane, price and time scales. For implementation details, refer to the Series Primitives article.

Use the attachPrimitive method to add a primitive to the chart and attach it to the series.

Pane primitives are attached to a chart pane rather than a specific series. You can use them to create chart-wide annotations and features like watermarks. For implementation details, refer to the Pane Primitives article.

Note that pane primitives cannot render on the price or time scale.

Use the attachPrimitive method to add a primitive to the chart and attach it to the pane.

**Examples:**

Example 1 (javascript):
```javascript
class MyCustomSeries {    /* Class implementing the ICustomSeriesPaneView interface */}// Create an instantiated custom seriesconst customSeriesInstance = new MyCustomSeries();const chart = createChart(document.getElementById('container'));const myCustomSeries = chart.addCustomSeries(customSeriesInstance, {    // Options for MyCustomSeries    customOption: 10,});const data = [    { time: 1642425322, value: 123, customValue: 456 },    /* ... more data */];myCustomSeries.setData(data);
```

Example 2 (javascript):
```javascript
class MyCustomPrimitive {    /* Class implementing the ISeriesPrimitive interface */}// Create an instantiated series primitiveconst myCustomPrimitive = new MyCustomPrimitive();const chart = createChart(document.getElementById('container'));const lineSeries = chart.addSeries(LineSeries);const data = [    { time: 1642425322, value: 123 },    /* ... more data */];// Attach the primitive to the serieslineSeries.attachPrimitive(myCustomPrimitive);
```

Example 3 (javascript):
```javascript
class MyCustomPanePrimitive {    /* Class implementing the IPanePrimitive interface */}// Create an instantiated pane primitiveconst myCustomPanePrimitive = new MyCustomPanePrimitive();const chart = createChart(document.getElementById('container'));// Get the main paneconst mainPane = chart.panes()[0];// Attach the primitive to the panemainPane.attachPrimitive(myCustomPanePrimitive);
```

---

## Histogram Column Width Calculations

**URL:** https://tradingview.github.io/lightweight-charts/docs/plugins/pixel-perfect-rendering/widths/columns

**Contents:**
- Histogram Column Width Calculations

It is recommend that you first read the Pixel Perfect Rendering page.

The following functions can be used to get the calculated width that the library would use for a histogram column at a specific bar spacing and device pixel ratio.

You can use the calculateColumnPositionsInPlace function instead of the calculateColumnPositions function to perform the calculation on an existing array of items without needing to create additional arrays (which is more efficient). It is recommended that you memoize the majority of the calculations below to improve the rendering performance.

**Examples:**

Example 1 (typescript):
```typescript
const alignToMinimalWidthLimit = 4;const showSpacingMinimalBarWidth = 1;/** * Spacing gap between columns. * @param barSpacingMedia - spacing between bars (media coordinate) * @param horizontalPixelRatio - horizontal pixel ratio * @returns Spacing gap between columns (in Bitmap coordinates) */function columnSpacing(barSpacingMedia: number, horizontalPixelRatio: number) {    return Math.ceil(barSpacingMedia * horizontalPixelRatio) <=        showSpacingMinimalBarWidth        ? 0        : Math.max(1, Math.floor(horizontalPixelRatio));}/** * Desired width for columns. This may not be the final width because * it may be adjusted later to ensure all columns on screen have a * consistent width and gap. * @param barSpacingMedia - spacing between bars (media coordinate) * @param horizontalPixelRatio - horizontal pixel ratio * @param spacing - Spacing gap between columns (in Bitmap coordinates). (optional, provide if you have already calculated it) * @returns Desired width for column bars (in Bitmap coordinates) */function desiredColumnWidth(    barSpacingMedia: number,    horizontalPixelRatio: number,    spacing?: number) {    return (        Math.round(barSpacingMedia * horizontalPixelRatio) -        (spacing ?? columnSpacing(barSpacingMedia, horizontalPixelRatio))    );}interface ColumnCommon {    /** Spacing gap between columns */    spacing: number;    /** Shift columns left by one pixel */    shiftLeft: boolean;    /** Half width of a column */    columnHalfWidthBitmap: number;    /** horizontal pixel ratio */    horizontalPixelRatio: number;}/** * Calculated values which are common to all the columns on the screen, and * are required to calculate the individual positions. * @param barSpacingMedia - spacing between bars (media coordinate) * @param horizontalPixelRatio - horizontal pixel ratio * @returns calculated values for subsequent column calculations */function columnCommon(    barSpacingMedia: number,    horizontalPixelRatio: number): ColumnCommon {    const spacing = columnSpacing(barSpacingMedia, horizontalPixelRatio);    const columnWidthBitmap = desiredColumnWidth(        barSpacingMedia,        horizontalPixelRatio,        spacing    );    const shiftLeft = columnWidthBitmap % 2 === 0;    const columnHalfWidthBitmap = (columnWidthBitmap - (shiftLeft ? 0 : 1)) / 2;    return {        spacing,        shiftLeft,        columnHalfWidthBitmap,        horizontalPixelRatio,    };}interface ColumnPosition {    left: number;    right: number;    shiftLeft: boolean;}/** * Calculate the position for a column. These values can be later adjusted * by a second pass which corrects widths, and shifts columns. * @param xMedia - column x position (center) in media coordinates * @param columnData - precalculated common values (returned by `columnCommon`) * @param previousPosition - result from this function for the previous bar. * @returns initial column position */function calculateColumnPosition(    xMedia: number,    columnData: ColumnCommon,    previousPosition: ColumnPosition | undefined): ColumnPosition {    const xBitmapUnRounded = xMedia * columnData.horizontalPixelRatio;    const xBitmap = Math.round(xBitmapUnRounded);    const xPositions: ColumnPosition = {        left: xBitmap - columnData.columnHalfWidthBitmap,        right:            xBitmap +            columnData.columnHalfWidthBitmap -            (columnData.shiftLeft ? 1 : 0),        shiftLeft: xBitmap > xBitmapUnRounded,    };    const expectedAlignmentShift = columnData.spacing + 1;    if (previousPosition) {        if (xPositions.left - previousPosition.right !== expectedAlignmentShift) {            // need to adjust alignment            if (previousPosition.shiftLeft) {                previousPosition.right = xPositions.left - expectedAlignmentShift;            } else {                xPositions.left = previousPosition.right + expectedAlignmentShift;            }        }    }    return xPositions;}function fixPositionsAndReturnSmallestWidth(    positions: ColumnPosition[],    initialMinWidth: number): number {    return positions.reduce((smallest: number, position: ColumnPosition) => {        if (position.right < position.left) {            position.right = position.left;        }        const width = position.right - position.left + 1;        return Math.min(smallest, width);    }, initialMinWidth);}function fixAlignmentForNarrowColumns(    positions: ColumnPosition[],    minColumnWidth: number) {    return positions.map((position: ColumnPosition) => {        const width = position.right - position.left + 1;        if (width <= minColumnWidth) return position;        if (position.shiftLeft) {            position.right -= 1;        } else {            position.left += 1;        }        return position;    });}/** * Calculates the column positions and widths for the x positions. * This function creates a new array. You may get faster performance using the * `calculateColumnPositionsInPlace` function instead * @param xMediaPositions - x positions for the bars in media coordinates * @param barSpacingMedia - spacing between bars in media coordinates * @param horizontalPixelRatio - horizontal pixel ratio * @returns Positions for the columns */export function calculateColumnPositions(    xMediaPositions: number[],    barSpacingMedia: number,    horizontalPixelRatio: number): ColumnPosition[] {    const common = columnCommon(barSpacingMedia, horizontalPixelRatio);    const positions = new Array<ColumnPosition>(xMediaPositions.length);    let previous: ColumnPosition | undefined = undefined;    for (let i = 0; i < xMediaPositions.length; i++) {        positions[i] = calculateColumnPosition(            xMediaPositions[i],            common,            previous        );        previous = positions[i];    }    const initialMinWidth = Math.ceil(barSpacingMedia * horizontalPixelRatio);    const minColumnWidth = fixPositionsAndReturnSmallestWidth(        positions,        initialMinWidth    );    if (common.spacing > 0 && minColumnWidth < alignToMinimalWidthLimit) {        return fixAlignmentForNarrowColumns(positions, minColumnWidth);    }    return positions;}export interface ColumnPositionItem {    x: number;    column?: ColumnPosition;}/** * Calculates the column positions and widths for bars using the existing * array of items. * @param items - bar items which include an `x` property, and will be mutated to contain a column property * @param barSpacingMedia - bar spacing in media coordinates * @param horizontalPixelRatio - horizontal pixel ratio * @param startIndex - start index for visible bars within the items array * @param endIndex - end index for visible bars within the items array */export function calculateColumnPositionsInPlace(    items: ColumnPositionItem[],    barSpacingMedia: number,    horizontalPixelRatio: number,    startIndex: number,    endIndex: number): void {    const common = columnCommon(barSpacingMedia, horizontalPixelRatio);    let previous: ColumnPosition | undefined = undefined;    for (let i = startIndex; i < Math.min(endIndex, items.length); i++) {        items[i].column = calculateColumnPosition(items[i].x, common, previous);        previous = items[i].column;    }    const minColumnWidth = (items as ColumnPositionItem[]).reduce(        (smallest: number, item: ColumnPositionItem, index: number) => {            if (!item.column || index < startIndex || index > endIndex)                return smallest;            if (item.column.right < item.column.left) {                item.column.right = item.column.left;            }            const width = item.column.right - item.column.left + 1;            return Math.min(smallest, width);        },        Math.ceil(barSpacingMedia * horizontalPixelRatio)    );    if (common.spacing > 0 && minColumnWidth < alignToMinimalWidthLimit) {        (items as ColumnPositionItem[]).forEach(            (item: ColumnPositionItem, index: number) => {                if (!item.column || index < startIndex || index > endIndex) return;                const width = item.column.right - item.column.left + 1;                if (width <= minColumnWidth) return item;                if (item.column.shiftLeft) {                    item.column.right -= 1;                } else {                    item.column.left += 1;                }                return item.column;            }        );    }}
```

---

## Pane Primitives

**URL:** https://tradingview.github.io/lightweight-charts/docs/plugins/pane-primitives

**Contents:**
- Pane Primitives
- Key Differences from Series Primitives​
- Adding a Pane Primitive​
- Implementing a Pane Primitive​

In addition to Series Primitives, the library now supports Pane Primitives. These are essentially the same as Series Primitives but are designed to draw on the pane of a chart rather than being associated with a specific series. Pane Primitives can be used for features like watermarks or other chart-wide annotations.

Pane Primitives can be added to a chart using the attachPrimitive method on the IPaneApi interface. Here's an example:

To create a Pane Primitive, you should implement the IPanePrimitive interface. This interface is similar to ISeriesPrimitive, but with some key differences:

Here's a basic example of a Pane Primitive implementation:

For more details on implementing Pane Primitives, refer to the IPanePrimitive interface documentation.

**Examples:**

Example 1 (javascript):
```javascript
const chart = createChart(document.getElementById('container'));const pane = chart.panes()[0]; // Get the first (main) paneconst myPanePrimitive = new MyCustomPanePrimitive();pane.attachPrimitive(myPanePrimitive);
```

Example 2 (javascript):
```javascript
class MyCustomPanePrimitive {    paneViews() {        return [            {                renderer: {                    draw: target => {                        // Custom drawing logic here                    },                },            },        ];    }    // Other methods as needed...}
```

---

## Time scale

**URL:** https://tradingview.github.io/lightweight-charts/docs/time-scale

**Contents:**
- Time scale
- Overview​
  - Time scale appearance​
  - Time scale API​
- Visible range​
  - Data range​
  - Logical range​
- Chart margin​

Time scale (or time axis) is a horizontal scale that displays the time of data points at the bottom of the chart.

The horizontal scale can also represent price or other custom values. Refer to the Chart types article for more information.

Use TimeScaleOptions to adjust the time scale appearance. You can specify these options in two ways:

Call the IChartApi.timeScale method to get an instance of the ITimeScaleApi interface. This interface provides an extensive API for controlling the time scale. For example, you can adjust the visible range, convert a time point or index to a coordinate, and subscribe to events.

Visible range is a chart area that is currently visible on the canvas. This area can be measured with both data and logical range. Data range usually includes bar timestamps, while logical range has bar indices.

You can adjust the visible range using the following methods:

The data range includes only values from the first to the last bar visible on the chart. If the visible area has empty space, this part of the scale is not included in the data range.

Note that you cannot extrapolate time with the setVisibleRange method. For example, the chart does not have data prior 2018-01-01 date. If you set the visible range from 2016-01-01, it will be automatically adjusted to 2018-01-01.

If you want to adjust the visible range more flexible, operate with the logical range instead.

The logical range represents a continuous line of values. These values are logical indices on the scale that illustrated as red lines in the image below:

The logical range starts from the first data point across all series, with negative indices before it and positive ones after.

The indices can have fractional parts. The integer part represents the fully visible bar, while the fractional part indicates partial visibility. For example, the 5.2 index means that the fifth bar is fully visible, while the sixth bar is 20% visible. A half-index, such as 3.5, represents the middle of the bar.

In the library, the logical range is represented with the LogicalRange object. This object has the from and to properties, which are logical indices on the time scale. For example, the visible logical range on the chart above is approximately from -4.73 to 5.05.

The setVisibleLogicalRange method allows you to specify the visible range beyond the bounds of the available data. This can be useful for setting a chart margin or aligning series visually.

Margin is the space between the chart's borders and the series. It depends on the following time scale options:

You can specify these options as described above.

Note that if a series contains only a few data points, the chart may have a large margin on the left side.

In this case, you can call the fitContent method that adjust the view and fits all data within the chart.

If calling fitContent has no effect, it might be due to how the library displays data.

The library allocates specific width for each data point to maintain consistency between different chart types. For example, for line series, the plot point is placed at the center of this allocated space, while candlestick series use most of the width for the candle body. The allocated space for each data point is proportional to the chart width. As a result, series with fewer data points may have a small margin on both sides.

You can specify the logical range with the setVisibleLogicalRange method to display the series exactly to the edges. For example, the code sample below adjusts the range by half a bar-width on both sides.

**Examples:**

Example 1 (javascript):
```javascript
chart.timeScale().resetTimeScale();
```

Example 2 (javascript):
```javascript
chart.timeScale().fitContent();
```

Example 3 (javascript):
```javascript
const vr = chart.timeScale().getVisibleLogicalRange();chart.timeScale().setVisibleLogicalRange({ from: vr.from + 0.5, to: vr.to - 0.5 });
```

---

## Time zones

**URL:** https://tradingview.github.io/lightweight-charts/docs/time-zones

**Contents:**
- Time zones
- Overview​
- Approaches​
  - Using pure JavaScript​
  - Using the date-fns-tz library​
  - Using the IANA time zone database​
- Why are time zones not supported?​

Lightweight Charts™ does not natively support time zones. If necessary, you should handle time zone adjustments manually.

The library processes all date and time values in UTC. To support time zones, adjust each bar's timestamp in your dataset based on the appropriate time zone offset. Therefore, a UTC timestamp should correspond to the local time in the target time zone.

Consider the example. A data point has the 2021-01-01T10:00:00.000Z timestamp in UTC. You want to display it in the Europe/Moscow time zone, which has the UTC+03:00 offset according to the IANA time zone database. To do this, adjust the original UTC timestamp by adding 3 hours. Therefore, the new timestamp should be 2021-01-01T13:00:00.000Z.

When converting time zones, consider the following: Adding a time zone offset could change not only the time but the date as well. An offset may vary due to DST (Daylight Saving Time) or other regional adjustments. If your data is measured in business days and does not include a time component, in most cases, you should not adjust it to a time zone.

Consider the approaches below to convert time values to the required time zone.

For more information on this approach, refer to StackOverflow.

If you only need to support a client (local) time zone, you can use the following function:

You can use the utcToZonedTime function from the date-fns-tz library as follows:

If you process a large dataset and approaches above do not meet your performance requirements, consider using the tzdata.

This approach can significantly improve performance for the following reasons:

The approaches above were not implemented in Lightweight Charts™ for the following reasons:

Since time zone support is not required for all users, it is intentionally left out of the library to maintain high performance and a lightweight package size.

**Examples:**

Example 1 (javascript):
```javascript
function timeToTz(originalTime, timeZone) {    const zonedDate = new Date(new Date(originalTime * 1000).toLocaleString('en-US', { timeZone }));    return zonedDate.getTime() / 1000;}
```

Example 2 (javascript):
```javascript
function timeToLocal(originalTime) {    const d = new Date(originalTime * 1000);    return Date.UTC(d.getFullYear(), d.getMonth(), d.getDate(), d.getHours(), d.getMinutes(), d.getSeconds(), d.getMilliseconds()) / 1000;}
```

Example 3 (javascript):
```javascript
import { utcToZonedTime } from 'date-fns-tz';function timeToTz(originalTime, timeZone) {    const zonedDate = utcToZonedTime(new Date(originalTime * 1000), timeZone);    return zonedDate.getTime() / 1000;}
```

---

## Best Practices for Pixel Perfect Rendering in Canvas Drawings

**URL:** https://tradingview.github.io/lightweight-charts/docs/plugins/pixel-perfect-rendering

**Contents:**
- Best Practices for Pixel Perfect Rendering in Canvas Drawings
- Centered Shapes​
- Dual Point Shapes​
- Default Widths​

To achieve crisp pixel perfect rendering for your plugins, it is recommended that the canvas drawings are created using bitmap coordinates. The difference between media and bitmap coordinate spaces is discussed on the Canvas Rendering Target page. Essentially, all drawing actions should use integer positions and dimensions when on the bitmap coordinate space.

To ensure consistency between your plugins and the library's built-in logic for rendering points on the chart, use of the following calculation functions.

Variable names containing media refer to positions / dimensions specified using the media coordinate space (such as the x and y coordinates provided by the library to the renderers), and names containing bitmap refer to positions / dimensions on the bitmap coordinate space (actual device screen pixels).

If you need to draw a shape which is centred on a position (for example a price or x coordinate) and has a desired width then you could use the positionsLine function presented below. This can be used for drawing a horizontal line at a specific price, or a vertical line aligned with the centre of series point.

If you need to draw a shape between two coordinates (for example, y coordinates for a high and low price) then you can use the positionsBox function as presented below.

Please refer to the following pages for functions defining the default widths of shapes drawn by the library:

**Examples:**

Example 1 (typescript):
```typescript
interface BitmapPositionLength {    /** coordinate for use with a bitmap rendering scope */    position: number;    /** length for use with a bitmap rendering scope */    length: number;}function centreOffset(lineBitmapWidth: number): number {    return Math.floor(lineBitmapWidth * 0.5);}/** * Calculates the bitmap position for an item with a desired length (height or width), and centred according to * a position coordinate defined in media sizing. * @param positionMedia - position coordinate for the bar (in media coordinates) * @param pixelRatio - pixel ratio. Either horizontal for x positions, or vertical for y positions * @param desiredWidthMedia - desired width (in media coordinates) * @returns Position of the start point and length dimension. */export function positionsLine(    positionMedia: number,    pixelRatio: number,    desiredWidthMedia: number = 1,    widthIsBitmap?: boolean): BitmapPositionLength {    const scaledPosition = Math.round(pixelRatio * positionMedia);    const lineBitmapWidth = widthIsBitmap        ? desiredWidthMedia        : Math.round(desiredWidthMedia * pixelRatio);    const offset = centreOffset(lineBitmapWidth);    const position = scaledPosition - offset;    return { position, length: lineBitmapWidth };}
```

Example 2 (typescript):
```typescript
/** * Determines the bitmap position and length for a dimension of a shape to be drawn. * @param position1Media - media coordinate for the first point * @param position2Media - media coordinate for the second point * @param pixelRatio - pixel ratio for the corresponding axis (vertical or horizontal) * @returns Position of the start point and length dimension. */export function positionsBox(    position1Media: number,    position2Media: number,    pixelRatio: number): BitmapPositionLength {    const scaledPosition1 = Math.round(pixelRatio * position1Media);    const scaledPosition2 = Math.round(pixelRatio * position2Media);    return {        position: Math.min(scaledPosition1, scaledPosition2),        length: Math.abs(scaledPosition2 - scaledPosition1) + 1,    };}
```

---

## Custom Series Types

**URL:** https://tradingview.github.io/lightweight-charts/docs/plugins/custom_series

**Contents:**
- Custom Series Types
- Defining a Custom Series​
  - Renderer​
  - Update​
  - Price Value Builder​
  - Whitespace​
  - Default Options​
  - Destroy​

Custom series allow developers to create new types of series with their own data structures, and rendering logic (implemented using CanvasRenderingContext2D methods). These custom series extend the current capabilities of our built-in series, providing a consistent API which mirrors the built-in chart types.

These series are expected to have a uniform width for each data point, which ensures that the chart maintains a consistent look and feel across all series types. The only restriction on the data structure is that it should extend the CustomData interface (have a valid time property for each data point).

A custom series should implement the ICustomSeriesPaneView interface. The interface defines the basic functionality and structure required for creating a custom series view.

It includes the following methods and properties:

This method should return a renderer which implements the ICustomSeriesPaneRenderer interface and is used to draw the series data on the main chart pane.

The draw method of the renderer is evoked whenever the chart needs to draw the series.

The PriceToCoordinateConverter provided as the 2nd argument to the draw method is a convenience function for changing prices into vertical coordinate values. It is provided since the series' original data will most likely be defined in price values, and the renderer needs to draw with coordinates. The values returned by the converter will be defined in mediaSize (unscaled by devicePixelRatio).

CanvasRenderingTarget2D provided within the draw function is explained in more detail on the Canvas Rendering Target page.

This method will be called with the latest data for the renderer to use during the next paint.

The update method is evoked with two parameters: data (discussed below), and seriesOptions. seriesOptions is a reference to the currently applied options for the series

The PaneRendererCustomData interface provides the data that can be used within the renderer for drawing the series data. It includes the following properties:

A function for interpreting the custom series data and returning an array of numbers representing the prices values for the item, specifically the equivalent highest, lowest, and current price values for the data item.

These price values are used by the chart to determine the auto-scaling (to ensure the items are in view) and the crosshair and price line positions. The largest and smallest values in the array will be used to specify the visible range of the painted item, and the last value will be used for the crosshair and price line position.

A function used by the library to determine which data points provided by the user should be considered Whitespace. The method should return true when the data point is Whitespace. Data points which are whitespace data won't be provided to the renderer, or the priceValueBuilder.

The default options to be used for the series. The user can override these values using the options argument in addCustomSeries, or via the applyOptions method on the ISeriesAPI.

This method will be evoked when the series has been removed from the chart. This method should be used to clean up any objects, references, and other items that could potentially cause memory leaks.

This method should contain all the necessary code to clean up the object before it is removed from memory. This includes removing any event listeners or timers that are attached to the object, removing any references to other objects, and resetting any values or properties that were modified during the lifetime of the object.

---

## Full Bar Width Calculations

**URL:** https://tradingview.github.io/lightweight-charts/docs/plugins/pixel-perfect-rendering/widths/full-bar-width

**Contents:**
- Full Bar Width Calculations

It is recommend that you first read the Pixel Perfect Rendering page.

The following functions can be used to get the calculated width that the library would use for the full width of a bar (data point) at a specific bar spacing and device pixel ratio. This can be used when you would like to use the full width available for each data point on the x axis, and don't want any gaps to be visible.

**Examples:**

Example 1 (typescript):
```typescript
interface BitmapPositionLength {    /** coordinate for use with a bitmap rendering scope */    position: number;    /** length for use with a bitmap rendering scope */    length: number;}/** * Calculates the position and width which will completely full the space for the bar. * Useful if you want to draw something that will not have any gaps between surrounding bars. * @param xMedia - x coordinate of the bar defined in media sizing * @param halfBarSpacingMedia - half the width of the current barSpacing (un-rounded) * @param horizontalPixelRatio - horizontal pixel ratio * @returns position and width which will completely full the space for the bar */export function fullBarWidth(    xMedia: number,    halfBarSpacingMedia: number,    horizontalPixelRatio: number): BitmapPositionLength {    const fullWidthLeftMedia = xMedia - halfBarSpacingMedia;    const fullWidthRightMedia = xMedia + halfBarSpacingMedia;    const fullWidthLeftBitmap = Math.round(        fullWidthLeftMedia * horizontalPixelRatio    );    const fullWidthRightBitmap = Math.round(        fullWidthRightMedia * horizontalPixelRatio    );    const fullWidthBitmap = fullWidthRightBitmap - fullWidthLeftBitmap;    return {        position: fullWidthLeftBitmap,        length: fullWidthBitmap,    };}
```

---

## Canvas Rendering Target

**URL:** https://tradingview.github.io/lightweight-charts/docs/plugins/canvas-rendering-target

**Contents:**
- Canvas Rendering Target
- Using CanvasRenderingTarget2D​
- Difference between Bitmap and Media​
  - Bitmap Coordinate Space​
    - Bitmap Coordinate Space Usage​
  - Media Coordinate Space​
    - Media Coordinate Space Usage​
- General Tips​

The renderer functions used within the plugins (both Custom Series, and Drawing Primitives) are provided with a CanvasRenderingTarget2D interface on which the drawing logic (using the Browser's 2D Canvas API) should be executed. CanvasRenderingTarget2D is provided by the Fancy Canvas library.

The typescript definitions can be viewed here: fancy-canvas on npmjs.com and specifically the definition for CanvasRenderingTarget2D can be viewed here: canvas-rendering-target.d.ts

and specifically the definition for CanvasRenderingTarget2D can be viewed here: canvas-rendering-target.d.ts

CanvasRenderingTarget2D provides two rendering scope which you can use:

Bitmap sizing represents the actual physical pixels on the device's screen, while the media size represents the size of a pixel according to the operating system (and browser) which is generally an integer representing the ratio of actual physical pixels are used to render a media pixel. This integer ratio is referred to as the device pixel ratio.

Using the bitmap sizing allows for more control over the drawn image to ensure that the graphics are crisp and pixel perfect, however this generally means that the code will contain a lot multiplication of coordinates by the pixel ratio. In cases where you don't need to draw using the bitmap sizing then it is easier to use media sizing as you don't need to worry about the devices pixel ratio.

useBitmapCoordinateSpace can be used to if you would like draw using the actual devices pixels as the coordinate sizing. The provided scope (of type BitmapCoordinatesRenderingScope) contains readonly values for the following:

useMediaCoordinateSpace can be used to if you would like draw using the media dimensions as the coordinate sizing. The provided scope (of type MediaCoordinatesRenderingScope) contains readonly values for the following:

It is recommended that rendering functions should save and restore the canvas context before and after all the rendering logic to ensure that the canvas state is the same as when the renderer function was evoked. To handle the case when an error in the code might prevent the restore function from being evoked, you should use the try - finally code block to ensure that the context is correctly restored in all cases.

Note that useBitmapCoordinateSpace and useMediaCoordinateSpace will automatically save and restore the canvas context for the logic defined within them. This tip for your additional rendering functions within the use*CoordinateSpace.

**Examples:**

Example 1 (javascript):
```javascript
// target is an instance of CanvasRenderingTarget2Dtarget.useBitmapCoordinateSpace(scope => {    // scope is an instance of BitmapCoordinatesRenderingScope    // example of drawing a filled rectangle which fills the canvas    scope.context.beginPath();    scope.context.rect(0, 0, scope.bitmapSize.width, scope.bitmapSize.height);    scope.context.fillStyle = 'rgba(100, 200, 50, 0.5)';    scope.context.fill();});
```

Example 2 (javascript):
```javascript
// target is an instance of CanvasRenderingTarget2Dtarget.useMediaCoordinateSpace(scope => {    // scope is an instance of BitmapCoordinatesRenderingScope    // example of drawing a filled rectangle which fills the canvas    scope.context.beginPath();    scope.context.rect(0, 0, scope.mediaSize.width, scope.mediaSize.height);    scope.context.fillStyle = 'rgba(100, 200, 50, 0.5)';    scope.context.fill();});
```

Example 3 (javascript):
```javascript
function myRenderingFunction(scope) {    const ctx = scope.context;    // save the current state of the context to the stack    ctx.save();    try {        // example code        scope.context.beginPath();        scope.context.rect(0, 0, scope.mediaSize.width, scope.mediaSize.height);        scope.context.fillStyle = 'rgba(100, 200, 50, 0.5)';        scope.context.fill();    } finally {        // restore the saved context from the stack        ctx.restore();    }}target.useMediaCoordinateSpace(scope => {    myRenderingFunction(scope);    myOtherRenderingFunction(scope);    /* ... */});
```

---

## Candlestick Width Calculations

**URL:** https://tradingview.github.io/lightweight-charts/docs/plugins/pixel-perfect-rendering/widths/candlestick

**Contents:**
- Candlestick Width Calculations

It is recommend that you first read the Pixel Perfect Rendering page.

The following functions can be used to get the calculated width that the library would use for a candlestick at a specific bar spacing and device pixel ratio.

Below a bar spacing of 4, the library will attempt to use as large a width as possible without the possibility of overlapping, whilst above 4 then the width will start to trend towards an 80% width of the available space.

It is expected that candles can overlap slightly at smaller bar spacings (more pronounced on lower resolution devices). This produces a more readable chart. If you need to ensure that bars can never overlap then rather use the widths for Columns or the full bar width calculation.

**Examples:**

Example 1 (typescript):
```typescript
function optimalCandlestickWidth(    barSpacing: number,    pixelRatio: number): number {    const barSpacingSpecialCaseFrom = 2.5;    const barSpacingSpecialCaseTo = 4;    const barSpacingSpecialCaseCoeff = 3;    if (barSpacing >= barSpacingSpecialCaseFrom && barSpacing <= barSpacingSpecialCaseTo) {        return Math.floor(barSpacingSpecialCaseCoeff * pixelRatio);    }    // coeff should be 1 on small barspacing and go to 0.8 while bar spacing grows    const barSpacingReducingCoeff = 0.2;    const coeff =        1 -        (barSpacingReducingCoeff *            Math.atan(                Math.max(barSpacingSpecialCaseTo, barSpacing) - barSpacingSpecialCaseTo            )) /            (Math.PI * 0.5);    const res = Math.floor(barSpacing * coeff * pixelRatio);    const scaledBarSpacing = Math.floor(barSpacing * pixelRatio);    const optimal = Math.min(res, scaledBarSpacing);    return Math.max(Math.floor(pixelRatio), optimal);}/** * Calculates the candlestick width that the library would use for the current * bar spacing. * @param barSpacing - bar spacing in media coordinates * @param horizontalPixelRatio - horizontal pixel ratio * @returns The width (in bitmap coordinates) that the chart would use to draw a candle body */export function candlestickWidth(    barSpacing: number,    horizontalPixelRatio: number): number {    let width = optimalCandlestickWidth(barSpacing, horizontalPixelRatio);    if (width >= 2) {        const wickWidth = Math.floor(horizontalPixelRatio);        if (wickWidth % 2 !== width % 2) {            width--;        }    }    return width;}
```

---
