# Lightweight-Charts - Api

**Pages:** 207

---

## Interface: IRange<T>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/IRange

**Contents:**
- Interface: IRange<T>
- Type parameters​
- Properties​
  - from​
  - to​

Represents a generic range from one value to another.

The from value. The start of the range.

The to value. The end of the range.

---

## Interface: ISeriesApi<TSeriesType, HorzScaleItem, TData, TOptions, TPartialOptions>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/ISeriesApi

**Contents:**
- Interface: ISeriesApi<TSeriesType, HorzScaleItem, TData, TOptions, TPartialOptions>
- Type parameters​
- Methods​
  - priceFormatter()​
    - Returns​
  - priceToCoordinate()​
    - Parameters​
    - Returns​
  - coordinateToPrice()​
    - Parameters​

Represents the interface for interacting with series.

• TSeriesType extends SeriesType

• HorzScaleItem = Time

• TData = SeriesDataItemTypeMap<HorzScaleItem>[TSeriesType]

• TOptions = SeriesOptionsMap[TSeriesType]

• TPartialOptions = SeriesPartialOptionsMap[TSeriesType]

priceFormatter(): IPriceFormatter

Returns current price formatter

Interface to the price formatter object that can be used to format prices in the same way as the chart does

priceToCoordinate(price): Coordinate

Converts specified series price to pixel coordinate according to the series price scale

Input price to be converted

Pixel coordinate of the price level on the chart

coordinateToPrice(coordinate): BarPrice

Converts specified coordinate to price value according to the series price scale

Input coordinate to be converted

Price value of the coordinate on the chart

barsInLogicalRange(range): BarsInfo<HorzScaleItem>

Returns bars information for the series in the provided logical range or null, if no series data has been found in the requested range. This method can be used, for instance, to implement downloading historical data while scrolling to prevent a user from seeing empty space.

• range: IRange<number>

The logical range to retrieve info for.

BarsInfo<HorzScaleItem>

The bars info for the given logical range.

applyOptions(options): void

Applies new options to the existing series You can set options initially when you create series or use the applyOptions method of the series to change the existing options. Note that you can only pass options you want to change.

• options: TPartialOptions

Any subset of options.

options(): Readonly<TOptions>

Returns currently applied options

Full set of currently applied options, including defaults

priceScale(): IPriceScaleApi

Returns the API interface for controlling the price scale that this series is currently attached to.

IPriceScaleApi An interface for controlling the price scale (axis component) currently used by this series

Important: The returned PriceScaleApi is bound to the specific price scale (by ID and pane) that the series is using at the time this method is called. If you later move the series to a different pane or attach it to a different price scale (e.g., from 'right' to 'left'), the previously returned PriceScaleApi will NOT follow the series. It will continue to control the original price scale it was created for.

To control the new price scale after moving a series, you must call this method again to get a fresh PriceScaleApi instance for the current price scale.

Sets or replaces series data.

Ordered (earlier time point goes first) array of data items. Old data is fully replaced with the new one.

update(bar, historicalUpdate?): void

Adds new data item to the existing set (or updates the latest item if times of the passed/latest items are equal).

A single data item to be added. Time of the new item must be greater or equal to the latest existing time point. If the new item's time is equal to the last existing item's time, then the existing item is replaced with the new one.

• historicalUpdate?: boolean

If true, allows updating an existing data point that is not the latest bar. Default is false. Updating older data using historicalUpdate will be slower than updating the most recent data point.

Removes one or more data items from the end of the series.

The number of data items to remove.

The removed data items.

dataByIndex(logicalIndex, mismatchDirection?): TData

Returns a bar data by provided logical index.

• logicalIndex: number

• mismatchDirection?: MismatchDirection

Search direction if no data found at provided logical index.

Original data item provided via setData or update methods.

data(): readonly TData[]

Returns all the bar data for the series.

Original data items provided via setData or update methods.

subscribeDataChanged(handler): void

Subscribe to the data changed event. This event is fired whenever the update or setData method is evoked on the series.

• handler: DataChangedHandler

Handler to be called on a data changed event.

unsubscribeDataChanged(handler): void

Unsubscribe a handler that was previously subscribed using subscribeDataChanged.

• handler: DataChangedHandler

Previously subscribed handler

createPriceLine(options): IPriceLine

Creates a new price line

• options: CreatePriceLineOptions

Any subset of options, however price is required.

removePriceLine(line): void

Removes the price line that was created before.

priceLines(): IPriceLine[]

Returns an array of price lines.

seriesType(): TSeriesType

Return current series type.

lastValueData(globalLast): LastValueDataResult

Return the last value data of the series.

• globalLast: boolean

If false, get the last value in the current visible range. Otherwise, fetch the absolute last value

The last value data of the series.

attachPrimitive(primitive): void

Attaches additional drawing primitive to the series

• primitive: ISeriesPrimitive<HorzScaleItem>

any implementation of ISeriesPrimitive interface

detachPrimitive(primitive): void

Detaches additional drawing primitive from the series

• primitive: ISeriesPrimitive<HorzScaleItem>

implementation of ISeriesPrimitive interface attached before Does nothing if specified primitive was not attached

moveToPane(paneIndex): void

Move the series to another pane.

If the pane with the specified index does not exist, the pane will be created.

The index of the pane. Should be a number between 0 and the total number of panes.

seriesOrder(): number

Gets the zero-based index of this series within the list of all series on the current pane.

The current index of the series in the pane's series collection.

setSeriesOrder(order): void

Sets the zero-based index of this series within the pane's series collection, thereby adjusting its rendering order.

The desired zero-based index to set for this series within the pane.

getPane(): IPaneApi<HorzScaleItem>

Returns the pane to which the series is currently attached.

IPaneApi<HorzScaleItem>

Pane API object to control the pane

**Examples:**

Example 1 (javascript):
```javascript
const barsInfo = series.barsInLogicalRange(chart.timeScale().getVisibleLogicalRange());console.log(barsInfo);
```

Example 2 (javascript):
```javascript
function onVisibleLogicalRangeChanged(newVisibleLogicalRange) {    const barsInfo = series.barsInLogicalRange(newVisibleLogicalRange);    // if there less than 50 bars to the left of the visible area    if (barsInfo !== null && barsInfo.barsBefore < 50) {        // try to load additional historical data and prepend it to the series data    }}chart.timeScale().subscribeVisibleLogicalRangeChange(onVisibleLogicalRangeChanged);
```

Example 3 (css):
```css
lineSeries.setData([    { time: '2018-12-12', value: 24.11 },    { time: '2018-12-13', value: 31.74 },]);
```

Example 4 (css):
```css
barSeries.setData([    { time: '2018-12-19', open: 141.77, high: 170.39, low: 120.25, close: 145.72 },    { time: '2018-12-20', open: 145.72, high: 147.99, low: 100.11, close: 108.19 },]);
```

---

## Enumeration: MarkerSign

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/enumerations/MarkerSign

**Contents:**
- Enumeration: MarkerSign
- Enumeration Members​
  - Negative​
  - Neutral​
  - Positive​

Enumeration representing the sign of a marker.

Represents a negative change (-1)

Represents no change (0)

Represents a positive change (1)

---

## Type alias: AreaSeriesOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/AreaSeriesOptions

**Contents:**
- Type alias: AreaSeriesOptions

AreaSeriesOptions: SeriesOptions <AreaStyleOptions>

Represents area series options.

---

## Interface: Point

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/Point

**Contents:**
- Interface: Point
- Properties​
  - x​
  - y​

Represents a point on the chart.

readonly x: Coordinate

readonly y: Coordinate

---

## Type alias: MouseEventHandler()<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/MouseEventHandler

**Contents:**
- Type alias: MouseEventHandler()<HorzScaleItem>
- Type parameters​
- Parameters​
- Returns​

MouseEventHandler<HorzScaleItem>: (param) => void

A custom function use to handle mouse events.

• param: MouseEventParams<HorzScaleItem>

---

## Type alias: TickMarkFormatter()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/TickMarkFormatter

**Contents:**
- Type alias: TickMarkFormatter()
- Example​
- Parameters​
- Returns​

TickMarkFormatter: (time, tickMarkType, locale) => string | null

The TickMarkFormatter is used to customize tick mark labels on the time scale.

This function should return time as a string formatted according to tickMarkType type (year, month, etc) and locale.

Note that the returned string should be the shortest possible value and should have no more than 8 characters. Otherwise, the tick marks will overlap each other.

If the formatter function returns null then the default tick mark formatter will be used as a fallback.

• tickMarkType: TickMarkType

**Examples:**

Example 1 (javascript):
```javascript
const customFormatter = (time, tickMarkType, locale) => {    // your code here};
```

---

## Interface: LastValueDataResultWithData

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/LastValueDataResultWithData

**Contents:**
- Interface: LastValueDataResultWithData
- Properties​
  - noData​
  - price​
  - color​

Represents last value data result of a series for plugins when there is data

Indicates if the series has data.

The last price of the series.

The color of the last value.

---

## Interface: CustomStyleOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/CustomStyleOptions

**Contents:**
- Interface: CustomStyleOptions
- Properties​
  - color​

Represents style options for a custom series.

Color used for the price line and price scale label.

---

## Interface: IPriceScaleApi

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/IPriceScaleApi

**Contents:**
- Interface: IPriceScaleApi
- Methods​
  - applyOptions()​
    - Parameters​
    - Returns​
  - options()​
    - Returns​
  - width()​
    - Returns​
  - setVisibleRange()​

Interface to control chart's price scale

applyOptions(options): void

Applies new options to the price scale

• options: DeepPartial <PriceScaleOptions>

Any subset of options.

options(): Readonly <PriceScaleOptions>

Returns currently applied options of the price scale

Readonly <PriceScaleOptions>

Full set of currently applied options, including defaults

Returns a width of the price scale if it's visible or 0 if invisible.

setVisibleRange(range): void

Sets the visible range of the price scale.

• range: IRange<number>

The visible range to set, with from and to properties.

getVisibleRange(): IRange<number>

Returns the visible range of the price scale.

The visible range of the price scale, or null if the range is not set.

setAutoScale(on): void

Sets the auto scale mode of the price scale.

If true, enables auto scaling; if false, disables it.

---

## Interface: PaneSize

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/PaneSize

**Contents:**
- Interface: PaneSize
- Properties​
  - height​
  - width​

Dimensions of the Chart Pane (the main chart area which excludes the time and price scales).

Height of the Chart Pane (pixels)

Width of the Chart Pane (pixels)

---

## Interface: PriceScaleMargins

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/PriceScaleMargins

**Contents:**
- Interface: PriceScaleMargins
- Properties​
  - top​
  - bottom​

Defines margins of the price scale.

Top margin in percentages. Must be greater or equal to 0 and less than 1.

Bottom margin in percentages. Must be greater or equal to 0 and less than 1.

---

## Type alias: HistogramSeriesPartialOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/HistogramSeriesPartialOptions

**Contents:**
- Type alias: HistogramSeriesPartialOptions

HistogramSeriesPartialOptions: SeriesPartialOptions <HistogramStyleOptions>

Represents histogram series options where all properties are optional.

---

## Interface: GridOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/GridOptions

**Contents:**
- Interface: GridOptions
- Properties​
  - vertLines​
  - horzLines​

Structure describing grid options.

vertLines: GridLineOptions

Vertical grid line options.

horzLines: GridLineOptions

Horizontal grid line options.

---

## Interface: SeriesMarkerPrice<TimeType>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/SeriesMarkerPrice

**Contents:**
- Interface: SeriesMarkerPrice<TimeType>
- Extends​
- Type parameters​
- Properties​
  - time​
    - Inherited from​
  - shape​
    - Inherited from​
  - color​
    - Inherited from​

Represents a series marker.

The time of the marker.

SeriesMarkerBase . time

shape: SeriesMarkerShape

The shape of the marker.

SeriesMarkerBase . shape

The color of the marker.

SeriesMarkerBase . color

The ID of the marker.

SeriesMarkerBase . id

optional text: string

The optional text of the marker.

SeriesMarkerBase . text

optional size: number

The optional size of the marker.

SeriesMarkerBase . size

position: SeriesMarkerPricePosition

The position of the marker.

SeriesMarkerBase . position

The price value for exact Y-axis positioning.

Required when using SeriesMarkerPricePosition position type.

SeriesMarkerBase . price

---

## Interface: IPriceFormatter

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/IPriceFormatter

**Contents:**
- Interface: IPriceFormatter
- Methods​
  - format()​
    - Parameters​
    - Returns​
  - formatTickmarks()​
    - Parameters​
    - Returns​

Interface to be implemented by the object in order to be used as a price formatter

format(price): string

Original price to be formatted

formatTickmarks(prices): string[]

A formatting function for price scale tick marks. Use this function to define formatting rules based on all provided price values.

• prices: readonly number[]

Prices to be formatted

---

## Interface: CrosshairOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/CrosshairOptions

**Contents:**
- Interface: CrosshairOptions
- Properties​
  - mode​
    - Default Value​
  - vertLine​
  - horzLine​
  - doNotSnapToHiddenSeriesIndices​
    - Default Value​

Structure describing crosshair options

vertLine: CrosshairLineOptions

Vertical line options.

horzLine: CrosshairLineOptions

Horizontal line options.

doNotSnapToHiddenSeriesIndices: boolean

If set to true, the crosshair will not snap to the data points of hidden series.

**Examples:**

Example 1 (json):
```json
{@link CrosshairMode.Magnet}
```

---

## Interface: IPanePrimitivePaneView

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/IPanePrimitivePaneView

**Contents:**
- Interface: IPanePrimitivePaneView
- Methods​
  - zOrder()?​
    - Returns​
    - See​
  - renderer()​
    - Returns​

This interface represents the primitive for one of the pane of the chart (main chart area, time scale, price scale).

optional zOrder(): PrimitivePaneViewZOrder

Defines where in the visual layer stack the renderer should be executed. Default is 'normal'.

PrimitivePaneViewZOrder

the desired position in the visual layer stack.

PrimitivePaneViewZOrder

renderer(): IPrimitivePaneRenderer

This method returns a renderer - special object to draw data

IPrimitivePaneRenderer

an renderer object to be used for drawing, or null if we have nothing to draw.

---

## Interface: KineticScrollOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/KineticScrollOptions

**Contents:**
- Interface: KineticScrollOptions
- Properties​
  - touch​
    - Default Value​
  - mouse​
    - Default Value​

Represents options for enabling or disabling kinetic scrolling with mouse and touch gestures.

Enable kinetic scroll with touch gestures.

Enable kinetic scroll with the mouse.

---

## Type alias: HorzScaleItemConverterToInternalObj()<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/HorzScaleItemConverterToInternalObj

**Contents:**
- Type alias: HorzScaleItemConverterToInternalObj()<HorzScaleItem>
- Type parameters​
- Parameters​
- Returns​

HorzScaleItemConverterToInternalObj<HorzScaleItem>: (time) => InternalHorzScaleItem

Function for converting a horizontal scale item to an internal item.

• time: HorzScaleItem

InternalHorzScaleItem

---

## Interface: SeriesDefinition<T>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/SeriesDefinition

**Contents:**
- Interface: SeriesDefinition<T>
- Type parameters​
- Properties​
  - type​
  - isBuiltIn​
  - defaultOptions​

Series definition interface.

• T extends SeriesType

readonly isBuiltIn: boolean

Indicates if the series is built-in.

readonly defaultOptions: SeriesStyleOptionsMap[T]

Default series options.

---

## Type alias: Mutable<T>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/Mutable

**Contents:**
- Type alias: Mutable<T>
- Type parameters​

Mutable<T>: { -readonly [P in keyof T]: T[P] }

Removes "readonly" from all properties

---

## Type alias: SeriesPartialOptions<T>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/SeriesPartialOptions

**Contents:**
- Type alias: SeriesPartialOptions<T>
- Type parameters​

SeriesPartialOptions<T>: DeepPartial<T & SeriesOptionsCommon>

Represents a SeriesOptions where every property is optional.

---

## Type alias: PriceToCoordinateConverter()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/PriceToCoordinateConverter

**Contents:**
- Type alias: PriceToCoordinateConverter()
- Parameters​
- Returns​

PriceToCoordinateConverter: (price) => Coordinate | null

Converter function for changing prices into vertical coordinate values.

This is provided as a convenience function since the series original data will most likely be defined in price values, and the renderer needs to draw with coordinates. This returns the same values as directly using the series' priceToCoordinate method.

---

## Type alias: UTCTimestamp

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/UTCTimestamp

**Contents:**
- Type alias: UTCTimestamp
- Example​

UTCTimestamp: Nominal<number, "UTCTimestamp">

Represents a time as a UNIX timestamp.

If your chart displays an intraday interval you should use a UNIX Timestamp.

Note that JavaScript Date APIs like Date.now return a number of milliseconds but UTCTimestamp expects a number of seconds.

Note that to prevent errors, you should cast the numeric type of the time to UTCTimestamp type from the package (value as UTCTimestamp) in TypeScript code.

**Examples:**

Example 1 (typescript):
```typescript
const timestamp = 1529899200 as UTCTimestamp; // Literal timestamp representing 2018-06-25T04:00:00.000Zconst timestamp2 = (Date.now() / 1000) as UTCTimestamp;
```

---

## Interface: SeriesUpDownMarker<T>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/SeriesUpDownMarker

**Contents:**
- Interface: SeriesUpDownMarker<T>
- Type parameters​
- Properties​
  - time​
  - value​
  - sign​

Represents a marker drawn above or below a data point to indicate a price change update.

The type of the time value, defaults to Time.

The point on the horizontal scale.

The price value for the data point.

The direction of the price change.

---

## Interface: PriceFormatBuiltIn

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/PriceFormatBuiltIn

**Contents:**
- Interface: PriceFormatBuiltIn
- Examples​
- Properties​
  - type​
  - precision​
    - Default Value​
  - minMove​
    - Default Value​
  - base?​

Represents series value formatting options. The precision and minMove properties allow wide customization of formatting.

type: "percent" | "price" | "volume"

Built-in price formats:

Number of digits after the decimal point. If it is not set, then its value is calculated automatically based on minMove.

2 if both minMove and precision are not provided, calculated automatically based on minMove otherwise.

The minimum possible step size for price value movement. This value shouldn't have more decimal digits than the precision.

optional base: number

The base value for the price format. It should equal to 1 / minMove. If this option is specified, we ignore the minMove option. It can be useful for cases with very small price movements like 1e-18 where we can reach limitations of floating point precision.

**Examples:**

Example 1 (unknown):
```unknown
`minMove=0.01`, `precision` is not specified - prices will change like 1.13, 1.14, 1.15 etc.
```

Example 2 (unknown):
```unknown
`minMove=0.01`, `precision=3` - prices will change like 1.130, 1.140, 1.150 etc.
```

Example 3 (unknown):
```unknown
`minMove=0.05`, `precision` is not specified - prices will change like 1.10, 1.15, 1.20 etc.
```

---

## Interface: HandleScaleOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/HandleScaleOptions

**Contents:**
- Interface: HandleScaleOptions
- Properties​
  - mouseWheel​
    - Default Value​
  - pinch​
    - Default Value​
  - axisPressedMouseMove​
  - axisDoubleClickReset​

Represents options for how the chart is scaled by the mouse and touch gestures.

Enable scaling with the mouse wheel.

Enable scaling with pinch/zoom gestures.

axisPressedMouseMove: boolean | AxisPressedMouseMoveOptions

Enable scaling the price and/or time scales by holding down the left mouse button and moving the mouse.

axisDoubleClickReset: boolean | AxisDoubleClickOptions

Enable resetting scaling by double-clicking the left mouse button.

---

## Interface: LocalizationOptions<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/LocalizationOptions

**Contents:**
- Interface: LocalizationOptions<HorzScaleItem>
- Extends​
- Extended by​
- Type parameters​
- Properties​
  - timeFormatter?​
    - Default Value​
  - dateFormat​
    - Default Value​
  - locale​

Represents options for formatting dates, times, and prices according to a locale.

optional timeFormatter: TimeFormatterFn<HorzScaleItem>

Override formatting of the time scale crosshair label.

Date formatting string.

Can contain yyyy, yy, MMMM, MMM, MM and dd literals which will be replaced with corresponding date's value.

Ignored if timeFormatter has been specified.

Current locale used to format dates. Uses the browser's language settings by default.

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl#Locale_identification_and_negotiation

LocalizationOptionsBase . locale

optional priceFormatter: PriceFormatterFn

Override formatting of the price scale tick marks, labels and crosshair labels. Can be used for cases that can't be covered with built-in price formats.

LocalizationOptionsBase . priceFormatter

optional tickmarksPriceFormatter: TickmarksPriceFormatterFn

Overrides the formatting of price scale tick marks. Use this to define formatting rules based on all provided price values.

LocalizationOptionsBase . tickmarksPriceFormatter

optional percentageFormatter: PercentageFormatterFn

Overrides the formatting of percentage scale tick marks.

LocalizationOptionsBase . percentageFormatter

optional tickmarksPercentageFormatter: TickmarksPercentageFormatterFn

Override formatting of the percentage scale tick marks. Can be used if formatting should be adjusted based on all the values being formatted

LocalizationOptionsBase . tickmarksPercentageFormatter

---

## Function: createChartEx()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/functions/createChartEx

**Contents:**
- Function: createChartEx()
- Type parameters​
- Parameters​
- Returns​

createChartEx<HorzScaleItem, THorzScaleBehavior>(container, horzScaleBehavior, options?): IChartApiBase<HorzScaleItem>

This function is the main entry point of the Lightweight Charting Library. If you are using time values for the horizontal scale then it is recommended that you rather use the createChart function.

type of points on the horizontal scale

• THorzScaleBehavior extends IHorzScaleBehavior<HorzScaleItem>

type of horizontal axis strategy that encapsulate all the specific behaviors of the horizontal scale type

• container: string | HTMLElement

ID of HTML element or element itself

• horzScaleBehavior: THorzScaleBehavior

Horizontal scale behavior

• options?: DeepPartial<ReturnType<THorzScaleBehavior["options"]>>

Any subset of options to be applied at start.

IChartApiBase<HorzScaleItem>

An interface to the created chart

---

## Interface: TimeScaleOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/TimeScaleOptions

**Contents:**
- Interface: TimeScaleOptions
- Extends​
- Properties​
  - rightOffset​
    - Default Value​
    - Inherited from​
  - rightOffsetPixels?​
    - Default Value​
    - Inherited from​
  - barSpacing​

Extended time scale options for time-based horizontal scale

The margin space in bars from the right side of the chart.

HorzScaleOptions . rightOffset

optional rightOffsetPixels: number

The margin space in pixels from the right side of the chart. This option has priority over rightOffset.

HorzScaleOptions . rightOffsetPixels

The space between bars in pixels.

HorzScaleOptions . barSpacing

minBarSpacing: number

The minimum space between bars in pixels.

HorzScaleOptions . minBarSpacing

maxBarSpacing: number

The maximum space between bars in pixels.

Has no effect if value is set to 0.

HorzScaleOptions . maxBarSpacing

Prevent scrolling to the left of the first bar.

HorzScaleOptions . fixLeftEdge

fixRightEdge: boolean

Prevent scrolling to the right of the most recent bar.

HorzScaleOptions . fixRightEdge

lockVisibleTimeRangeOnResize: boolean

Prevent changing the visible time range during chart resizing.

HorzScaleOptions . lockVisibleTimeRangeOnResize

rightBarStaysOnScroll: boolean

Prevent the hovered bar from moving when scrolling.

HorzScaleOptions . rightBarStaysOnScroll

borderVisible: boolean

Show the time scale border.

HorzScaleOptions . borderVisible

The time scale border color.

HorzScaleOptions . borderColor

HorzScaleOptions . visible

Show the time, not just the date, in the time scale and vertical crosshair label.

HorzScaleOptions . timeVisible

secondsVisible: boolean

Show seconds in the time scale and vertical crosshair label in hh:mm:ss format for intraday data.

HorzScaleOptions . secondsVisible

shiftVisibleRangeOnNewBar: boolean

Shift the visible range to the right (into the future) by the number of new bars when new data is added.

Note that this only applies when the last bar is visible.

HorzScaleOptions . shiftVisibleRangeOnNewBar

allowShiftVisibleRangeOnWhitespaceReplacement: boolean

Allow the visible range to be shifted to the right when a new bar is added which is replacing an existing whitespace time point on the chart.

Note that this only applies when the last bar is visible & shiftVisibleRangeOnNewBar is enabled.

HorzScaleOptions . allowShiftVisibleRangeOnWhitespaceReplacement

ticksVisible: boolean

Draw small vertical line on time axis labels.

HorzScaleOptions . ticksVisible

optional tickMarkMaxCharacterLength: number

Maximum tick mark label length. Used to override the default 8 character maximum length.

HorzScaleOptions . tickMarkMaxCharacterLength

uniformDistribution: boolean

Changes horizontal scale marks generation. With this flag equal to true, marks of the same weight are either all drawn or none are drawn at all.

HorzScaleOptions . uniformDistribution

minimumHeight: number

Define a minimum height for the time scale. Note: This value will be exceeded if the time scale needs more space to display it's contents.

Setting a minimum height could be useful for ensuring that multiple charts positioned in a horizontal stack each have an identical time scale height, or for plugins which require a bit more space within the time scale pane.

HorzScaleOptions . minimumHeight

allowBoldLabels: boolean

Allow major time scale labels to be rendered in a bolder font weight.

HorzScaleOptions . allowBoldLabels

ignoreWhitespaceIndices: boolean

Ignore time scale points containing only whitespace (for all series) when drawing grid lines, tick marks, and snapping the crosshair to time scale points.

For the yield curve chart type it defaults to true.

HorzScaleOptions . ignoreWhitespaceIndices

enableConflation: boolean

Enable data conflation for performance optimization when bar spacing is very small. When enabled, multiple data points are automatically combined into single points when they would be rendered in less than 0.5 pixels of screen space. This significantly improves rendering performance for large datasets when zoomed out.

HorzScaleOptions . enableConflation

optional conflationThresholdFactor: number

Smoothing factor for conflation thresholds. Controls how aggressively conflation is applied. This can be used to create smoother-looking charts, especially useful for sparklines and small charts.

Higher values result in fewer data points being displayed, creating smoother but less detailed charts. This is particularly useful for sparklines and small charts where smooth appearance is prioritized over showing every data point.

Note: Should be used with continuous series types (line, area, baseline) for best visual results. Candlestick and bar series may look less natural with high smoothing factors.

HorzScaleOptions . conflationThresholdFactor

precomputeConflationOnInit: boolean

Precompute conflation chunks for common levels right after data load. When enabled, the system will precompute conflation data in the background, which improves performance when zooming out but increases initial load time and memory usage.

Recommended for: Large datasets (>10K points) on machines with sufficient memory

HorzScaleOptions . precomputeConflationOnInit

precomputeConflationPriority: "background" | "user-visible" | "user-blocking"

Priority used for background precompute tasks when the Prioritized Task Scheduling API is available.

Recommendation: Use 'background' for most cases to avoid impacting user experience. Only use higher priorities if conflation is critical for your application's functionality.

HorzScaleOptions . precomputeConflationPriority

optional tickMarkFormatter: TickMarkFormatter

Tick marks formatter can be used to customize tick marks labels on the time axis.

**Examples:**

Example 1 (unknown):
```unknown
'background'
```

---

## Interface: CustomConflationContext<HorzScaleItem, TData>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/CustomConflationContext

**Contents:**
- Interface: CustomConflationContext<HorzScaleItem, TData>
- Type parameters​
- Properties​
  - data​
  - index​
  - originalTime​
  - time​
  - priceValues​

Context object provided to custom series conflation reducers. This wraps the internal SeriesPlotRow data while providing a user-friendly interface.

• HorzScaleItem = Time

• TData extends CustomData<HorzScaleItem> = CustomData<HorzScaleItem>

The original custom data item provided by the user.

readonly index: number

The time index of the data point in the series.

readonly originalTime: HorzScaleItem

The original time value provided by the user.

readonly time: unknown

The internal time point object.

readonly priceValues: CustomSeriesPricePlotValues

The computed price values for this data point (as returned by priceValueBuilder). The last value in this array is used as the current price.

---

## Interface: TextWatermarkLineOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/TextWatermarkLineOptions

**Contents:**
- Interface: TextWatermarkLineOptions
- Properties​
  - color​
    - Default Value​
  - text​
    - Default Value​
  - fontSize​
    - Default Value​
  - lineHeight?​
    - Default Value​

Text of the watermark. Word wrapping is not supported.

optional lineHeight: number

Line height in pixels.

-apple-system, BlinkMacSystemFont, 'Trebuchet MS', Roboto, Ubuntu, sans-serif

---

## Type alias: TimeRangeChangeEventHandler()<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/TimeRangeChangeEventHandler

**Contents:**
- Type alias: TimeRangeChangeEventHandler()<HorzScaleItem>
- Type parameters​
- Parameters​
- Returns​

TimeRangeChangeEventHandler<HorzScaleItem>: (timeRange) => void

A custom function used to handle changes to the time scale's time range.

• timeRange: IRange<HorzScaleItem> | null

---

## Interface: TrackingModeOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/TrackingModeOptions

**Contents:**
- Interface: TrackingModeOptions
- Properties​
  - exitMode​
    - Default Value​

Represent options for the tracking mode's behavior.

Mobile users will not have the ability to see the values/dates like they do on desktop. To see it, they should enter the tracking mode. The tracking mode will deactivate the scrolling and make it possible to check values and dates.

exitMode: TrackingModeExitMode

Determine how to exit the tracking mode.

By default, mobile users will long press to deactivate the scroll and have the ability to check values and dates. Another press is required to activate the scroll, be able to move left/right, zoom, etc.

**Examples:**

Example 1 (json):
```json
{@link TrackingModeExitMode.OnNextTap}
```

---

## Interface: CrosshairLineOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/CrosshairLineOptions

**Contents:**
- Interface: CrosshairLineOptions
- Properties​
  - color​
    - Default Value​
  - width​
    - Default Value​
  - style​
    - Default Value​
  - visible​
    - See​

Structure describing a crosshair line (vertical or horizontal)

Crosshair line color.

Crosshair line width.

Crosshair line style.

Display the crosshair line.

Note that disabling crosshair lines does not disable crosshair marker on Line and Area series. It can be disabled by using crosshairMarkerVisible option of a relevant series.

labelVisible: boolean

Display the crosshair label on the relevant scale.

labelBackgroundColor: string

Crosshair label background color.

**Examples:**

Example 1 (json):
```json
{@link LineStyle.LargeDashed}
```

---

## Interface: ICustomSeriesPaneRenderer

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/ICustomSeriesPaneRenderer

**Contents:**
- Interface: ICustomSeriesPaneRenderer
- Methods​
  - draw()​
    - Parameters​
    - Returns​

Renderer for the custom series. This paints on the main chart pane.

draw(target, priceConverter, isHovered, hitTestData?): void

Draw function for the renderer.

• target: CanvasRenderingTarget2D

canvas context to draw on, refer to FancyCanvas library for more details about this class.

• priceConverter: PriceToCoordinateConverter

converter function for changing prices into vertical coordinate values.

Whether the series is hovered.

• hitTestData?: unknown

Optional hit test data for the series.

---

## Type alias: Background

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/Background

**Contents:**
- Type alias: Background

Background: SolidColor | VerticalGradientColor

Represents the background color of the chart.

---

## Type alias: RedComponent

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/RedComponent

**Contents:**
- Type alias: RedComponent

RedComponent: Nominal<number, "RedComponent">

Red component of the RGB color value The valid values are integers in range [0, 255]

---

## Type alias: TickMarkWeightValue

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/TickMarkWeightValue

**Contents:**
- Type alias: TickMarkWeightValue
- See​

TickMarkWeightValue: Nominal<number, "TickMarkWeightValue">

Weight of the tick mark.

---

## Interface: TimeChartOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/TimeChartOptions

**Contents:**
- Interface: TimeChartOptions
- Extends​
- Properties​
  - width​
    - Default Value​
    - Inherited from​
  - height​
    - Default Value​
    - Inherited from​
  - autoSize​

Options for chart with time at the horizontal scale

Width of the chart in pixels

If 0 (default) or none value provided, then a size of the widget will be calculated based its container's size.

ChartOptionsImpl . width

Height of the chart in pixels

If 0 (default) or none value provided, then a size of the widget will be calculated based its container's size.

ChartOptionsImpl . height

Setting this flag to true will make the chart watch the chart container's size and automatically resize the chart to fit its container whenever the size changes.

This feature requires ResizeObserver class to be available in the global scope. Note that calling code is responsible for providing a polyfill if required. If the global scope does not have ResizeObserver, a warning will appear and the flag will be ignored.

Please pay attention that autoSize option and explicit sizes options width and height don't conflict with one another. If you specify autoSize flag, then width and height options will be ignored unless ResizeObserver has failed. If it fails then the values will be used as fallback.

The flag autoSize could also be set with and unset with applyOptions function.

ChartOptionsImpl . autoSize

layout: LayoutOptions

ChartOptionsImpl . layout

leftPriceScale: PriceScaleOptions

Left price scale options

ChartOptionsImpl . leftPriceScale

rightPriceScale: PriceScaleOptions

Right price scale options

ChartOptionsImpl . rightPriceScale

overlayPriceScales: OverlayPriceScaleOptions

Overlay price scale options

ChartOptionsImpl . overlayPriceScales

crosshair: CrosshairOptions

The crosshair shows the intersection of the price and time scale values at any point on the chart.

ChartOptionsImpl . crosshair

A grid is represented in the chart background as a vertical and horizontal lines drawn at the levels of visible marks of price and the time scales.

ChartOptionsImpl . grid

handleScroll: boolean | HandleScrollOptions

Scroll options, or a boolean flag that enables/disables scrolling

ChartOptionsImpl . handleScroll

handleScale: boolean | HandleScaleOptions

Scale options, or a boolean flag that enables/disables scaling

ChartOptionsImpl . handleScale

kineticScroll: KineticScrollOptions

Kinetic scroll options

ChartOptionsImpl . kineticScroll

trackingMode: TrackingModeOptions

Represent options for the tracking mode's behavior.

Mobile users will not have the ability to see the values/dates like they do on desktop. To see it, they should enter the tracking mode. The tracking mode will deactivate the scrolling and make it possible to check values and dates.

ChartOptionsImpl . trackingMode

addDefaultPane: boolean

Whether to add a default pane to the chart Disable this option when you want to create a chart with no panes and add them manually

ChartOptionsImpl . addDefaultPane

localization: LocalizationOptions <Time>

Localization options.

ChartOptionsImpl . localization

timeScale: TimeScaleOptions

Extended time scale options with option to override tickMarkFormatter

ChartOptionsImpl . timeScale

**Examples:**

Example 1 (css):
```css
const chart = LightweightCharts.createChart(document.body, {    autoSize: true,});
```

---

## Type alias: BarSeriesOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/BarSeriesOptions

**Contents:**
- Type alias: BarSeriesOptions

BarSeriesOptions: SeriesOptions <BarStyleOptions>

Represents bar series options.

---

## Interface: HistogramData<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/HistogramData

**Contents:**
- Interface: HistogramData<HorzScaleItem>
- Extends​
- Type parameters​
- Properties​
  - color?​
  - time​
    - Inherited from​
  - value​
    - Inherited from​
  - customValues?​

Structure describing a single item of data for histogram series

• HorzScaleItem = Time

optional color: string

Optional color value for certain data item. If missed, color from options is used

The time of the data.

SingleValueData . time

Price value of the data.

SingleValueData . value

optional customValues: Record<string, unknown>

Additional custom values which will be ignored by the library, but could be used by plugins.

SingleValueData . customValues

---

## Interface: SeriesMarkersOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/SeriesMarkersOptions

**Contents:**
- Interface: SeriesMarkersOptions
- Properties​
  - autoScale​
    - Default Value​
  - zOrder​
    - Default Value​

Configuration options for the series markers plugin. These options affect all markers managed by the plugin.

Specifies whether the auto-scaling calculation should expand to include the size of markers.

When true, the auto-scale feature will adjust the price scale's range to ensure series markers are fully visible and not cropped by the chart's edges.

When false, the scale will only fit the series data points, which may cause markers to be partially hidden.

Note: This option only has an effect when auto-scaling is enabled for the price scale.

zOrder: SeriesMarkerZOrder

Defines the stacking order of the markers relative to the series and other primitives.

---

## Interface: CandlestickStyleOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/CandlestickStyleOptions

**Contents:**
- Interface: CandlestickStyleOptions
- Properties​
  - upColor​
    - Default Value​
  - downColor​
    - Default Value​
  - wickVisible​
    - Default Value​
  - borderVisible​
    - Default Value​

Represents style options for a candlestick series.

Color of rising candles.

Color of falling candles.

Enable high and low prices candle wicks.

borderVisible: boolean

Enable candle borders.

borderUpColor: string

Border color of rising candles.

borderDownColor: string

Border color of falling candles.

Wick color of rising candles.

wickDownColor: string

Wick color of falling candles.

---

## Type alias: PriceFormatterFn()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/PriceFormatterFn

**Contents:**
- Type alias: PriceFormatterFn()
- Parameters​
- Returns​

PriceFormatterFn: (priceValue) => string

A function used to format a BarPrice as a string.

• priceValue: BarPrice

---

## Function: isBusinessDay()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/functions/isBusinessDay

**Contents:**
- Function: isBusinessDay()
- Parameters​
- Returns​

isBusinessDay(time): time is BusinessDay

Check if a time value is a business day object.

true if time is a BusinessDay object, false otherwise.

---

## Interface: PaneRendererCustomData<HorzScaleItem, TData>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/PaneRendererCustomData

**Contents:**
- Interface: PaneRendererCustomData<HorzScaleItem, TData>
- Type parameters​
- Properties​
  - bars​
  - barSpacing​
  - visibleRange​
  - conflationFactor​

Data provide to the custom series pane view which can be used within the renderer for drawing the series data.

• TData extends CustomData<HorzScaleItem>

bars: readonly CustomBarItemData<HorzScaleItem, TData>[]

List of all the series' items and their x coordinates.

Spacing between consecutive bars.

visibleRange: IRange<number>

The current visible range of items on the chart.

conflationFactor: number

Current conflation factor. The value represents how many data points have been combined to form this conflated data point. This can be used to calculate the effective bar spacing until the next data point. effectiveBarSpacing = conflationFactor * barSpacing. If you are rendering a non-continuous series (like a Candlestick instead of Line) then you likely would want to use the effectiveBarSpacing value for your width calculations.

---

## Type alias: Coordinate

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/Coordinate

**Contents:**
- Type alias: Coordinate

Coordinate: Nominal<number, "Coordinate">

Represents a coordiate as a number.

---

## Interface: PriceLineOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/PriceLineOptions

**Contents:**
- Interface: PriceLineOptions
- Properties​
  - id?​
  - price​
    - Default Value​
  - color​
    - Default Value​
  - lineWidth​
    - Default Value​
  - lineStyle​

Represents a price line options.

The optional ID of this price line.

Price line's width in pixels.

axisLabelVisible: boolean

Display the current price value in on the price scale.

Price line's on the chart pane.

axisLabelColor: string

Background color for the axis label. Will default to the price line color if unspecified.

axisLabelTextColor: string

Text color for the axis label.

**Examples:**

Example 1 (json):
```json
{@link LineStyle.Solid}
```

---

## Enumeration: CrosshairMode

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/enumerations/CrosshairMode

**Contents:**
- Enumeration: CrosshairMode
- Enumeration Members​
  - Normal​
  - Magnet​
  - Hidden​
  - MagnetOHLC​

Represents the crosshair mode.

This mode allows crosshair to move freely on the chart.

This mode sticks crosshair's horizontal line to the price value of a single-value series or to the close price of OHLC-based series.

This mode disables rendering of the crosshair.

This mode sticks crosshair's horizontal line to the price value of a single-value series or to the open/high/low/close price of OHLC-based series.

---

## Interface: CustomBarItemData<HorzScaleItem, TData>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/CustomBarItemData

**Contents:**
- Interface: CustomBarItemData<HorzScaleItem, TData>
- Type parameters​
- Properties​
  - x​
  - time​
  - originalData​
  - barColor​

Renderer data for an item within the custom series.

• TData extends CustomData<HorzScaleItem> = CustomData<HorzScaleItem>

Horizontal coordinate for the item. Measured from the left edge of the pane in pixels.

Time scale index for the item. This isn't the timestamp but rather the logical index.

Original data for the item.

Color assigned for the item, typically used for price line and price scale label.

---

## Interface: SeriesPartialOptionsMap

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/SeriesPartialOptionsMap

**Contents:**
- Interface: SeriesPartialOptionsMap
- Properties​
  - Bar​
  - Candlestick​
  - Area​
  - Baseline​
  - Line​
  - Histogram​
  - Custom​

Represents the type of partial options for each series type.

For example a bar series has options represented by BarSeriesPartialOptions.

Bar: DeepPartial <BarStyleOptions & SeriesOptionsCommon>

The type of bar series partial options.

Candlestick: DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon>

The type of candlestick series partial options.

Area: DeepPartial <AreaStyleOptions & SeriesOptionsCommon>

The type of area series partial options.

Baseline: DeepPartial <BaselineStyleOptions & SeriesOptionsCommon>

The type of baseline series partial options.

Line: DeepPartial <LineStyleOptions & SeriesOptionsCommon>

The type of line series partial options.

Histogram: DeepPartial <HistogramStyleOptions & SeriesOptionsCommon>

The type of histogram series partial options.

Custom: DeepPartial <CustomStyleOptions & SeriesOptionsCommon>

The type of a custom series partial options.

---

## Interface: BaselineData<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/BaselineData

**Contents:**
- Interface: BaselineData<HorzScaleItem>
- Extends​
- Type parameters​
- Properties​
  - topFillColor1?​
  - topFillColor2?​
  - topLineColor?​
  - bottomFillColor1?​
  - bottomFillColor2?​
  - bottomLineColor?​

Structure describing a single item of data for baseline series

• HorzScaleItem = Time

optional topFillColor1: string

Optional top area top fill color value for certain data item. If missed, color from options is used

optional topFillColor2: string

Optional top area bottom fill color value for certain data item. If missed, color from options is used

optional topLineColor: string

Optional top area line color value for certain data item. If missed, color from options is used

optional bottomFillColor1: string

Optional bottom area top fill color value for certain data item. If missed, color from options is used

optional bottomFillColor2: string

Optional bottom area bottom fill color value for certain data item. If missed, color from options is used

optional bottomLineColor: string

Optional bottom area line color value for certain data item. If missed, color from options is used

The time of the data.

SingleValueData . time

Price value of the data.

SingleValueData . value

optional customValues: Record<string, unknown>

Additional custom values which will be ignored by the library, but could be used by plugins.

SingleValueData . customValues

---

## Type alias: DataItem<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/DataItem

**Contents:**
- Type alias: DataItem<HorzScaleItem>
- Type parameters​

DataItem<HorzScaleItem>: SeriesDataItemTypeMap<HorzScaleItem>[SeriesType]

Represents the type of data that a series contains.

---

## Interface: PriceRange

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/PriceRange

**Contents:**
- Interface: PriceRange
- Properties​
  - minValue​
  - maxValue​

Represents a price range.

Maximum value in the range.

Minimum value in the range.

---

## Interface: CustomData<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/CustomData

**Contents:**
- Interface: CustomData<HorzScaleItem>
- Extends​
- Type parameters​
- Properties​
  - color?​
  - time​
    - Inherited from​
  - customValues?​
    - Inherited from​

Base structure describing a single item of data for a custom series.

This type allows for any properties to be defined within the interface. It is recommended that you extend this interface with the required data structure.

• HorzScaleItem = Time

optional color: string

If defined then this color will be used for the price line and price scale line for this specific data item of the custom series.

The time of the data.

CustomSeriesWhitespaceData . time

optional customValues: Record<string, unknown>

Additional custom values which will be ignored by the library, but could be used by plugins.

CustomSeriesWhitespaceData . customValues

---

## Type alias: LineSeriesPartialOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/LineSeriesPartialOptions

**Contents:**
- Type alias: LineSeriesPartialOptions

LineSeriesPartialOptions: SeriesPartialOptions <LineStyleOptions>

Represents line series options where all properties are optional.

---

## Interface: DrawingUtils

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/DrawingUtils

**Contents:**
- Interface: DrawingUtils
- Properties​
  - setLineStyle()​
    - Parameters​
    - Returns​

Helper drawing utilities exposed by the library to a Primitive (a.k.a plugin).

readonly setLineStyle: (ctx, lineStyle) => void

Drawing utility to change the line style on the canvas context to one of the built-in line styles.

• ctx: CanvasRenderingContext2D

2D rendering context for the target canvas.

• lineStyle: LineStyle

Built-in LineStyle to set on the canvas context.

---

## Type alias: SeriesType

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/SeriesType

**Contents:**
- Type alias: SeriesType
- See​

SeriesType: keyof SeriesOptionsMap

Represents a type of series.

---

## Interface: ICustomSeriesPaneView<HorzScaleItem, TData, TSeriesOptions>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/ICustomSeriesPaneView

**Contents:**
- Interface: ICustomSeriesPaneView<HorzScaleItem, TData, TSeriesOptions>
- Type parameters​
- Methods​
  - renderer()​
    - Returns​
  - update()​
    - Parameters​
    - Returns​
  - priceValueBuilder()​
    - Parameters​

This interface represents the view for the custom series

• HorzScaleItem = Time

• TData extends CustomData<HorzScaleItem> = CustomData<HorzScaleItem>

• TSeriesOptions extends CustomSeriesOptions = CustomSeriesOptions

renderer(): ICustomSeriesPaneRenderer

This method returns a renderer - special object to draw data for the series on the main chart pane.

ICustomSeriesPaneRenderer

an renderer object to be used for drawing.

update(data, seriesOptions): void

This method will be called with the latest data for the renderer to use during the next paint.

• data: PaneRendererCustomData<HorzScaleItem, TData>

• seriesOptions: TSeriesOptions

priceValueBuilder(plotRow): CustomSeriesPricePlotValues

A function for interpreting the custom series data and returning an array of numbers representing the price values for the item. These price values are used by the chart to determine the auto-scaling (to ensure the items are in view) and the crosshair and price line positions. The last value in the array will be used as the current value. You shouldn't need to have more than 3 values in this array since the library only needs a largest, smallest, and current value.

CustomSeriesPricePlotValues

isWhitespace(data): data is CustomSeriesWhitespaceData<HorzScaleItem>

A function for testing whether a data point should be considered fully specified, or if it should be considered as whitespace. Should return true if is whitespace.

• data: TData | CustomSeriesWhitespaceData<HorzScaleItem>

data point to be tested

data is CustomSeriesWhitespaceData<HorzScaleItem>

defaultOptions(): TSeriesOptions

optional destroy(): void

This method will be evoked when the series has been removed from the chart. This method should be used to clean up any objects, references, and other items that could potentially cause memory leaks.

This method should contain all the necessary code to clean up the object before it is removed from memory. This includes removing any event listeners or timers that are attached to the object, removing any references to other objects, and resetting any values or properties that were modified during the lifetime of the object.

optional conflationReducer(item1, item2): TData

Optional reducer used for conflation of custom data points. Given exactly 2 custom data contexts, should return a single aggregated item. Each context provides access to the original data plus metadata needed for conflation.

• item1: CustomConflationContext<HorzScaleItem, TData>

• item2: CustomConflationContext<HorzScaleItem, TData>

---

## Interface: HistogramStyleOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/HistogramStyleOptions

**Contents:**
- Interface: HistogramStyleOptions
- Properties​
  - color​
    - Default Value​
  - base​
    - Default Value​

Represents style options for a histogram series.

Initial level of histogram columns.

---

## Interface: IPriceLine

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/IPriceLine

**Contents:**
- Interface: IPriceLine
- Methods​
  - applyOptions()​
    - Parameters​
    - Returns​
    - Example​
  - options()​
    - Returns​

Represents the interface for interacting with price lines.

applyOptions(options): void

Apply options to the price line.

• options: Partial <PriceLineOptions>

Any subset of options.

options(): Readonly <PriceLineOptions>

Get the currently applied options.

Readonly <PriceLineOptions>

**Examples:**

Example 1 (css):
```css
priceLine.applyOptions({    price: 90.0,    color: 'red',    lineWidth: 3,    lineStyle: LightweightCharts.LineStyle.Dashed,    axisLabelVisible: false,    title: 'P/L 600',});
```

---

## Type alias: VisiblePriceScaleOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/VisiblePriceScaleOptions

**Contents:**
- Type alias: VisiblePriceScaleOptions
- See​

VisiblePriceScaleOptions: PriceScaleOptions

Represents a visible price scale's options.

---

## Function: createYieldCurveChart()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/functions/createYieldCurveChart

**Contents:**
- Function: createYieldCurveChart()
- Parameters​
- Returns​

createYieldCurveChart(container, options?): IYieldCurveChartApi

Creates a yield curve chart with the specified options.

A yield curve chart differs from the default chart type in the following ways:

• container: string | HTMLElement

ID of HTML element or element itself

• options?: DeepPartial <YieldCurveChartOptions>

The yield chart options.

An interface to the created chart

---

## Type alias: AreaSeriesPartialOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/AreaSeriesPartialOptions

**Contents:**
- Type alias: AreaSeriesPartialOptions

AreaSeriesPartialOptions: SeriesPartialOptions <AreaStyleOptions>

Represents area series options where all properties are optional.

---

## Type alias: TickmarksPriceFormatterFn()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/TickmarksPriceFormatterFn

**Contents:**
- Type alias: TickmarksPriceFormatterFn()
- Parameters​
- Returns​

TickmarksPriceFormatterFn: (priceValue) => string[]

• priceValue: BarPrice[]

---

## Interface: BaselineStyleOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/BaselineStyleOptions

**Contents:**
- Interface: BaselineStyleOptions
- Properties​
  - baseValue​
    - Default Value​
  - relativeGradient​
    - Default Value​
  - topFillColor1​
    - Default Value​
  - topFillColor2​
    - Default Value​

Represents style options for a baseline series.

baseValue: BaseValuePrice

Base value of the series.

{ type: 'price', price: 0 }

relativeGradient: boolean

Gradient is relative to the base value and the currently visible range. If it is false, the gradient is relative to the top and bottom of the chart.

topFillColor1: string

The first color of the top area.

'rgba(38, 166, 154, 0.28)'

topFillColor2: string

The second color of the top area.

'rgba(38, 166, 154, 0.05)'

The line color of the top area.

'rgba(38, 166, 154, 1)'

bottomFillColor1: string

The first color of the bottom area.

'rgba(239, 83, 80, 0.05)'

bottomFillColor2: string

The second color of the bottom area.

'rgba(239, 83, 80, 0.28)'

bottomLineColor: string

The line color of the bottom area.

'rgba(239, 83, 80, 1)'

pointMarkersVisible: boolean

Show circle markers on each point.

optional pointMarkersRadius: number

Circle markers radius in pixels.

crosshairMarkerVisible: boolean

Show the crosshair marker.

crosshairMarkerRadius: number

Crosshair marker radius in pixels.

crosshairMarkerBorderColor: string

Crosshair marker border color. An empty string falls back to the color of the series under the crosshair.

crosshairMarkerBackgroundColor: string

The crosshair marker background color. An empty string falls back to the color of the series under the crosshair.

crosshairMarkerBorderWidth: number

Crosshair marker border width in pixels.

lastPriceAnimation: LastPriceAnimationMode

Last price animation mode.

**Examples:**

Example 1 (json):
```json
{@link LineStyle.Solid}
```

Example 2 (json):
```json
{@link LineType.Simple}
```

Example 3 (json):
```json
{@link LastPriceAnimationMode.Disabled}
```

---

## Type alias: PercentageFormatterFn()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/PercentageFormatterFn

**Contents:**
- Type alias: PercentageFormatterFn()
- Parameters​
- Returns​

PercentageFormatterFn: (percentageValue) => string

A function used to format a percentage value as a string.

• percentageValue: number

---

## Type alias: DataChangedScope

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/DataChangedScope

**Contents:**
- Type alias: DataChangedScope

DataChangedScope: "full" | "update"

The extent of the data change.

---

## Interface: BarStyleOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/BarStyleOptions

**Contents:**
- Interface: BarStyleOptions
- Properties​
  - upColor​
    - Default Value​
  - downColor​
    - Default Value​
  - openVisible​
    - Default Value​
  - thinBars​
    - Default Value​

Represents style options for a bar series.

Color of rising bars.

Color of falling bars.

Show open lines on bars.

---

## Interface: LayoutPanesOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/LayoutPanesOptions

**Contents:**
- Interface: LayoutPanesOptions
- Properties​
  - enableResize​
    - Default Value​
  - separatorColor​
    - Default Value​
  - separatorHoverColor​
    - Default Value​

Represents panes customizations.

enableResize: boolean

Enable panes resizing

separatorColor: string

Color of pane separator

separatorHoverColor: string

Color of pane separator background applied on hover

rgba(178, 181, 189, 0.2)

---

## Interface: TimeScalePoint

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/TimeScalePoint

**Contents:**
- Interface: TimeScalePoint
- Properties​
  - timeWeight​
  - time​
    - [species]​
  - originalTime​

Represents a point on the time scale

readonly timeWeight: TickMarkWeightValue

readonly time: object

[species]: "InternalHorzScaleItem"

The 'name' or species of the nominal.

readonly originalTime: unknown

Original time for the point

---

## Interface: OhlcData<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/OhlcData

**Contents:**
- Interface: OhlcData<HorzScaleItem>
- Extends​
- Extended by​
- Type parameters​
- Properties​
  - time​
    - Overrides​
  - open​
  - high​
  - low​

Represents a bar with a Time and open, high, low, and close prices.

• HorzScaleItem = Time

WhitespaceData . time

optional customValues: Record<string, unknown>

Additional custom values which will be ignored by the library, but could be used by plugins.

WhitespaceData . customValues

---

## Interface: AutoScaleMargins

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/AutoScaleMargins

**Contents:**
- Interface: AutoScaleMargins
- Properties​
  - below​
  - above​

Represents the margin used when updating a price scale.

The number of pixels for bottom margin

The number of pixels for top margin

---

## Type alias: DataChangedHandler()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/DataChangedHandler

**Contents:**
- Type alias: DataChangedHandler()
- Parameters​
- Returns​

DataChangedHandler: (scope) => void

A custom function use to handle data changed events.

• scope: DataChangedScope

---

## Type alias: Nominal<T, Name>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/Nominal

**Contents:**
- Type alias: Nominal<T, Name>
- Examples​
- Type declaration​
  - [species]​
- Type parameters​

Nominal<T, Name>: T & object

This is the generic type useful for declaring a nominal type, which does not structurally matches with the base type and the other types declared over the same base type

The 'name' or species of the nominal.

• Name extends string

**Examples:**

Example 1 (typescript):
```typescript
type Index = Nominal<number, 'Index'>;// let i: Index = 42; // this fails to compilelet i: Index = 42 as Index; // OK
```

Example 2 (typescript):
```typescript
type TagName = Nominal<string, 'TagName'>;
```

---

## Enumeration: TrackingModeExitMode

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/enumerations/TrackingModeExitMode

**Contents:**
- Enumeration: TrackingModeExitMode
- Enumeration Members​
  - OnTouchEnd​
  - OnNextTap​

Determine how to exit the tracking mode.

By default, mobile users will long press to deactivate the scroll and have the ability to check values and dates. Another press is required to activate the scroll, be able to move left/right, zoom, etc.

Tracking Mode will be deactivated on touch end event.

Tracking Mode will be deactivated on the next tap event.

---

## Type alias: VertAlign

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/VertAlign

**Contents:**
- Type alias: VertAlign

VertAlign: "top" | "center" | "bottom"

Represents a vertical alignment.

---

## Interface: IPaneApi<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/IPaneApi

**Contents:**
- Interface: IPaneApi<HorzScaleItem>
- Type parameters​
- Methods​
  - getHeight()​
    - Returns​
  - setHeight()​
    - Parameters​
    - Returns​
  - moveTo()​
    - Parameters​

Represents the interface for interacting with a pane in a lightweight chart.

Retrieves the height of the pane in pixels.

The height of the pane in pixels.

setHeight(height): void

Sets the height of the pane.

The number of pixels to set as the height of the pane.

moveTo(paneIndex): void

Moves the pane to a new position.

The target index of the pane. Should be a number between 0 and the total number of panes - 1.

Retrieves the index of the pane.

The index of the pane. It is a number between 0 and the total number of panes - 1.

getSeries(): ISeriesApi<keyof SeriesOptionsMap, HorzScaleItem, AreaData<HorzScaleItem> | WhitespaceData<HorzScaleItem> | BarData<HorzScaleItem> | CandlestickData<HorzScaleItem> | BaselineData<HorzScaleItem> | LineData<HorzScaleItem> | HistogramData<HorzScaleItem> | CustomData<HorzScaleItem> | CustomSeriesWhitespaceData<HorzScaleItem>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>[]

Retrieves the array of series for the current pane.

ISeriesApi<keyof SeriesOptionsMap, HorzScaleItem, AreaData<HorzScaleItem> | WhitespaceData<HorzScaleItem> | BarData<HorzScaleItem> | CandlestickData<HorzScaleItem> | BaselineData<HorzScaleItem> | LineData<HorzScaleItem> | HistogramData<HorzScaleItem> | CustomData<HorzScaleItem> | CustomSeriesWhitespaceData<HorzScaleItem>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>[]

getHTMLElement(): HTMLElement

Retrieves the HTML element of the pane.

The HTML element of the pane or null if pane wasn't created yet.

attachPrimitive(primitive): void

Attaches additional drawing primitive to the pane

• primitive: IPanePrimitive<HorzScaleItem>

any implementation of IPanePrimitive interface

detachPrimitive(primitive): void

Detaches additional drawing primitive from the pane

• primitive: IPanePrimitive<HorzScaleItem>

implementation of IPanePrimitive interface attached before Does nothing if specified primitive was not attached

priceScale(priceScaleId): IPriceScaleApi

Returns the price scale with the given id.

• priceScaleId: string

ID of the price scale to find

If the price scale with the given id is not found in this pane

setPreserveEmptyPane(preserve): void

Sets whether to preserve the empty pane

Whether to preserve the empty pane

preserveEmptyPane(): boolean

Returns whether to preserve the empty pane

Whether to preserve the empty pane

getStretchFactor(): number

Returns the stretch factor of the pane. Stretch factor determines the relative size of the pane compared to other panes.

The stretch factor of the pane. Default is 1

setStretchFactor(stretchFactor): void

Sets the stretch factor of the pane. When you creating a pane, the stretch factor is 1 by default. So if you have three panes, and you want to make the first pane twice as big as the second and third panes, you can set the stretch factor of the first pane to 2000. Example:

• stretchFactor: number

The stretch factor of the pane.

addCustomSeries<TData, TOptions, TPartialOptions>(customPaneView, customOptions?): ISeriesApi<"Custom", HorzScaleItem, WhitespaceData<HorzScaleItem> | TData, TOptions, TPartialOptions>

Creates a custom series with specified parameters.

A custom series is a generic series which can be extended with a custom renderer to implement chart types which the library doesn't support by default.

• TData extends CustomData<HorzScaleItem>

• TOptions extends CustomSeriesOptions

• TPartialOptions extends DeepPartial<TOptions & SeriesOptionsCommon> = DeepPartial<TOptions & SeriesOptionsCommon>

• customPaneView: ICustomSeriesPaneView<HorzScaleItem, TData, TOptions>

A custom series pane view which implements the custom renderer.

• customOptions?: DeepPartial<TOptions & SeriesOptionsCommon>

Customization parameters of the series being created.

ISeriesApi<"Custom", HorzScaleItem, WhitespaceData<HorzScaleItem> | TData, TOptions, TPartialOptions>

addSeries<T>(definition, options?): ISeriesApi<T, HorzScaleItem, SeriesDataItemTypeMap<HorzScaleItem>[T], SeriesOptionsMap[T], SeriesPartialOptionsMap[T]>

Creates a series with specified parameters.

• T extends keyof SeriesOptionsMap

• definition: SeriesDefinition<T>

• options?: SeriesPartialOptionsMap[T]

Customization parameters of the series being created.

ISeriesApi<T, HorzScaleItem, SeriesDataItemTypeMap<HorzScaleItem>[T], SeriesOptionsMap[T], SeriesPartialOptionsMap[T]>

**Examples:**

Example 1 (javascript):
```javascript
const pane1 = chart.addPane();const pane2 = chart.addPane();const pane3 = chart.addPane();pane1.setStretchFactor(0.2);pane2.setStretchFactor(0.3);pane3.setStretchFactor(0.5);// Now the first pane will be 20% of the total height, the second pane will be 30% of the total height, and the third pane will be 50% of the total height.// Note: if you have one pane with default stretch factor of 1 and set other pane's stretch factor to 50,// library will try to make second pane 50 times smaller than the first pane
```

Example 2 (javascript):
```javascript
const series = pane.addCustomSeries(myCustomPaneView);
```

Example 3 (css):
```css
const series = pane.addSeries(LineSeries, { lineWidth: 2 });
```

---

## Function: createChart()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/functions/createChart

**Contents:**
- Function: createChart()
- Parameters​
- Returns​

createChart(container, options?): IChartApi

This function is the simplified main entry point of the Lightweight Charting Library with time points for the horizontal scale.

• container: string | HTMLElement

ID of HTML element or element itself

• options?: DeepPartial <TimeChartOptions>

Any subset of options to be applied at start.

An interface to the created chart

---

## Interface: LastValueDataResultWithoutData

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/LastValueDataResultWithoutData

**Contents:**
- Interface: LastValueDataResultWithoutData
- Properties​
  - noData​

Represents last value data result of a series for plugins when there is no data

Indicates if the series has data.

---

## Interface: AutoscaleInfo

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/AutoscaleInfo

**Contents:**
- Interface: AutoscaleInfo
- Properties​
  - priceRange​
  - margins?​

Represents information used to update a price scale.

priceRange: PriceRange

optional margins: AutoScaleMargins

---

## Type alias: ChartOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/ChartOptions

**Contents:**
- Type alias: ChartOptions

ChartOptions: TimeChartOptions

Structure describing options of the chart with time points at the horizontal scale. Series options are to be set separately

---

## Type alias: CustomSeriesPartialOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/CustomSeriesPartialOptions

**Contents:**
- Type alias: CustomSeriesPartialOptions

CustomSeriesPartialOptions: SeriesPartialOptions <CustomStyleOptions>

Represents a custom series options where all properties are optional.

---

## Interface: SeriesMarkerBar<TimeType>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/SeriesMarkerBar

**Contents:**
- Interface: SeriesMarkerBar<TimeType>
- Extends​
- Type parameters​
- Properties​
  - position​
    - Overrides​
  - time​
    - Inherited from​
  - shape​
    - Inherited from​

Represents a series marker.

position: SeriesMarkerBarPosition

The position of the marker.

SeriesMarkerBase . position

The time of the marker.

SeriesMarkerBase . time

shape: SeriesMarkerShape

The shape of the marker.

SeriesMarkerBase . shape

The color of the marker.

SeriesMarkerBase . color

The ID of the marker.

SeriesMarkerBase . id

optional text: string

The optional text of the marker.

SeriesMarkerBase . text

optional size: number

The optional size of the marker.

SeriesMarkerBase . size

optional price: number

The price value for exact Y-axis positioning.

Required when using SeriesMarkerPricePosition position type.

SeriesMarkerBase . price

---

## Interface: TimeMark

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/TimeMark

**Contents:**
- Interface: TimeMark
- Properties​
  - needAlignCoordinate​
  - coord​
  - label​
  - weight​

Represents a tick mark on the horizontal (time) scale.

needAlignCoordinate: boolean

Does time mark need to be aligned

Coordinate for the time mark

Display label for the time mark

weight: TickMarkWeightValue

Weight of the time mark

---

## Function: createSeriesMarkers()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/functions/createSeriesMarkers

**Contents:**
- Function: createSeriesMarkers()
- Type parameters​
- Parameters​
- Returns​
- Example​

createSeriesMarkers<HorzScaleItem>(series, markers?, options?): ISeriesMarkersPluginApi<HorzScaleItem>

A function to create a series markers primitive.

• series: ISeriesApi<keyof SeriesOptionsMap, HorzScaleItem, AreaData<HorzScaleItem> | WhitespaceData<HorzScaleItem> | BarData<HorzScaleItem> | CandlestickData<HorzScaleItem> | BaselineData<HorzScaleItem> | LineData<HorzScaleItem> | HistogramData<HorzScaleItem> | CustomData<HorzScaleItem> | CustomSeriesWhitespaceData<HorzScaleItem>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>

The series to which the primitive will be attached.

• markers?: SeriesMarker<HorzScaleItem>[]

An array of markers to be displayed on the series.

• options?: DeepPartial <SeriesMarkersOptions>

Options for the series markers plugin.

ISeriesMarkersPluginApi<HorzScaleItem>

**Examples:**

Example 1 (sql):
```sql
import { createSeriesMarkers } from 'lightweight-charts';    const seriesMarkers = createSeriesMarkers(        series,        [            {                color: 'green',                position: 'inBar',                shape: 'arrowDown',                time: 1556880900,            },        ]    ); // and then you can modify the markers // set it to empty array to remove all markers seriesMarkers.setMarkers([]); // `seriesMarkers.markers()` returns current markers
```

---

## Interface: SeriesMarkerBase<TimeType>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/SeriesMarkerBase

**Contents:**
- Interface: SeriesMarkerBase<TimeType>
- Extended by​
- Type parameters​
- Properties​
  - time​
  - position​
  - shape​
  - color​
  - id?​
  - text?​

Represents a series marker.

The time of the marker.

position: SeriesMarkerPosition

The position of the marker.

shape: SeriesMarkerShape

The shape of the marker.

The color of the marker.

The ID of the marker.

optional text: string

The optional text of the marker.

optional size: number

The optional size of the marker.

optional price: number

The price value for exact Y-axis positioning.

Required when using SeriesMarkerPricePosition position type.

---

## Interface: SeriesDataItemTypeMap<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/SeriesDataItemTypeMap

**Contents:**
- Interface: SeriesDataItemTypeMap<HorzScaleItem>
- Type parameters​
- Properties​
  - Bar​
  - Candlestick​
  - Area​
  - Baseline​
  - Line​
  - Histogram​
  - Custom​

Represents the type of data that a series contains.

For example a bar series contains BarData or WhitespaceData.

• HorzScaleItem = Time

Bar: WhitespaceData<HorzScaleItem> | BarData<HorzScaleItem>

The types of bar series data.

Candlestick: WhitespaceData<HorzScaleItem> | CandlestickData<HorzScaleItem>

The types of candlestick series data.

Area: AreaData<HorzScaleItem> | WhitespaceData<HorzScaleItem>

The types of area series data.

Baseline: WhitespaceData<HorzScaleItem> | BaselineData<HorzScaleItem>

The types of baseline series data.

Line: WhitespaceData<HorzScaleItem> | LineData<HorzScaleItem>

The types of line series data.

Histogram: WhitespaceData<HorzScaleItem> | HistogramData<HorzScaleItem>

The types of histogram series data.

Custom: CustomData<HorzScaleItem> | CustomSeriesWhitespaceData<HorzScaleItem>

The base types of an custom series data.

---

## Interface: YieldCurveChartOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/YieldCurveChartOptions

**Contents:**
- Interface: YieldCurveChartOptions
- Extends​
- Properties​
  - width​
    - Default Value​
    - Inherited from​
  - height​
    - Default Value​
    - Inherited from​
  - autoSize​

Extended chart options that include yield curve specific options. This interface combines the standard chart options with yield curve options.

Width of the chart in pixels

If 0 (default) or none value provided, then a size of the widget will be calculated based its container's size.

ChartOptionsImpl . width

Height of the chart in pixels

If 0 (default) or none value provided, then a size of the widget will be calculated based its container's size.

ChartOptionsImpl . height

Setting this flag to true will make the chart watch the chart container's size and automatically resize the chart to fit its container whenever the size changes.

This feature requires ResizeObserver class to be available in the global scope. Note that calling code is responsible for providing a polyfill if required. If the global scope does not have ResizeObserver, a warning will appear and the flag will be ignored.

Please pay attention that autoSize option and explicit sizes options width and height don't conflict with one another. If you specify autoSize flag, then width and height options will be ignored unless ResizeObserver has failed. If it fails then the values will be used as fallback.

The flag autoSize could also be set with and unset with applyOptions function.

ChartOptionsImpl . autoSize

layout: LayoutOptions

ChartOptionsImpl . layout

leftPriceScale: PriceScaleOptions

Left price scale options

ChartOptionsImpl . leftPriceScale

rightPriceScale: PriceScaleOptions

Right price scale options

ChartOptionsImpl . rightPriceScale

overlayPriceScales: OverlayPriceScaleOptions

Overlay price scale options

ChartOptionsImpl . overlayPriceScales

timeScale: HorzScaleOptions

ChartOptionsImpl . timeScale

crosshair: CrosshairOptions

The crosshair shows the intersection of the price and time scale values at any point on the chart.

ChartOptionsImpl . crosshair

A grid is represented in the chart background as a vertical and horizontal lines drawn at the levels of visible marks of price and the time scales.

ChartOptionsImpl . grid

handleScroll: boolean | HandleScrollOptions

Scroll options, or a boolean flag that enables/disables scrolling

ChartOptionsImpl . handleScroll

handleScale: boolean | HandleScaleOptions

Scale options, or a boolean flag that enables/disables scaling

ChartOptionsImpl . handleScale

kineticScroll: KineticScrollOptions

Kinetic scroll options

ChartOptionsImpl . kineticScroll

trackingMode: TrackingModeOptions

Represent options for the tracking mode's behavior.

Mobile users will not have the ability to see the values/dates like they do on desktop. To see it, they should enter the tracking mode. The tracking mode will deactivate the scrolling and make it possible to check values and dates.

ChartOptionsImpl . trackingMode

addDefaultPane: boolean

Whether to add a default pane to the chart Disable this option when you want to create a chart with no panes and add them manually

ChartOptionsImpl . addDefaultPane

localization: LocalizationOptions<number>

Localization options.

ChartOptionsImpl . localization

yieldCurve: YieldCurveOptions

Yield curve specific options. This object contains all the settings related to how the yield curve is displayed and behaves.

**Examples:**

Example 1 (css):
```css
const chart = LightweightCharts.createChart(document.body, {    autoSize: true,});
```

---

## Enumeration: LastPriceAnimationMode

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/enumerations/LastPriceAnimationMode

**Contents:**
- Enumeration: LastPriceAnimationMode
- Enumeration Members​
  - Disabled​
  - Continuous​
  - OnDataUpdate​

Represents the type of the last price animation for series such as area or line.

Animation is always disabled

Animation is always enabled.

Animation is active after new data.

---

## Interface: SeriesOptionsCommon

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/SeriesOptionsCommon

**Contents:**
- Interface: SeriesOptionsCommon
- Properties​
  - lastValueVisible​
    - Default Value​
  - title​
    - Default Value​
  - priceScaleId?​
    - Default Value​
  - visible​
    - Default Value​

Represents options common for all types of series

lastValueVisible: boolean

Visibility of the label with the latest visible price on the price scale.

true, false for yield curve charts

You can name series when adding it to a chart. This name will be displayed on the label next to the last value label.

optional priceScaleId: string

Target price scale to bind new series to.

'right' if right scale is visible and 'left' otherwise

Visibility of the series. If the series is hidden, everything including price lines, baseline, price labels and markers, will also be hidden. Please note that hiding a series is not equivalent to deleting it, since hiding does not affect the timeline at all, unlike deleting where the timeline can be changed (some points can be deleted).

priceLineVisible: boolean

Show the price line. Price line is a horizontal line indicating the last price of the series.

true, false for yield curve charts

priceLineSource: PriceLineSource

The source to use for the value of the price line.

priceLineWidth: LineWidth

Width of the price line.

priceLineColor: string

Color of the price line. By default, its color is set by the last bar color (or by line color on Line and Area charts).

priceLineStyle: LineStyle

priceFormat: PriceFormat

{ type: 'price', precision: 2, minMove: 0.01 }

baseLineVisible: boolean

Visibility of base line. Suitable for percentage and IndexedTo100 scales.

baseLineColor: string

Color of the base line in IndexedTo100 mode.

baseLineWidth: LineWidth

Base line width. Suitable for percentage and IndexedTo10 scales.

baseLineStyle: LineStyle

Base line style. Suitable for percentage and indexedTo100 scales.

optional autoscaleInfoProvider: AutoscaleInfoProvider

Override the default AutoscaleInfo provider. By default, the chart scales data automatically based on visible data range. However, for some reasons one could require overriding this behavior.

optional conflationThresholdFactor: number

Conflation smoothing factor for this series. Overrides the global time scale option. Controls how aggressively conflation is applied specifically to this series.

Higher values result in fewer data points being displayed for this series, creating smoother but less detailed charts. This allows different series on the same chart to have different conflation levels.

undefined (uses global time scale option)

**Examples:**

Example 1 (json):
```json
{@link PriceLineSource.LastBar}
```

Example 2 (json):
```json
{@link LineStyle.Dashed}
```

Example 3 (json):
```json
{@link LineStyle.Solid}
```

Example 4 (javascript):
```javascript
const firstSeries = chart.addSeries(LineSeries, {    autoscaleInfoProvider: () => ({        priceRange: {            minValue: 0,            maxValue: 100,        },    }),});
```

---

## Interface: IChartApi

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/IChartApi

**Contents:**
- Interface: IChartApi
- Extends​
- Methods​
  - applyOptions()​
    - Parameters​
    - Returns​
    - Overrides​
  - remove()​
    - Returns​
    - Inherited from​

The main interface of a single chart using time for horizontal scale.

applyOptions(options): void

Applies new options to the chart

• options: DeepPartial <TimeChartOptions>

Any subset of options.

IChartApiBase . applyOptions

Removes the chart object including all DOM elements. This is an irreversible operation, you cannot do anything with the chart after removing it.

IChartApiBase . remove

resize(width, height, forceRepaint?): void

Sets fixed size of the chart. By default chart takes up 100% of its container.

If chart has the autoSize option enabled, and the ResizeObserver is available then the width and height values will be ignored.

Target width of the chart.

Target height of the chart.

• forceRepaint?: boolean

True to initiate resize immediately. One could need this to get screenshot immediately after resize.

IChartApiBase . resize

addCustomSeries<TData, TOptions, TPartialOptions>(customPaneView, customOptions?, paneIndex?): ISeriesApi<"Custom", Time, TData | WhitespaceData <Time>, TOptions, TPartialOptions>

Creates a custom series with specified parameters.

A custom series is a generic series which can be extended with a custom renderer to implement chart types which the library doesn't support by default.

• TData extends CustomData <Time>

• TOptions extends CustomSeriesOptions

• TPartialOptions extends DeepPartial<TOptions & SeriesOptionsCommon> = DeepPartial<TOptions & SeriesOptionsCommon>

• customPaneView: ICustomSeriesPaneView <Time, TData, TOptions>

A custom series pane view which implements the custom renderer.

• customOptions?: DeepPartial<TOptions & SeriesOptionsCommon>

Customization parameters of the series being created.

ISeriesApi<"Custom", Time, TData | WhitespaceData <Time>, TOptions, TPartialOptions>

IChartApiBase . addCustomSeries

addSeries<T>(definition, options?, paneIndex?): ISeriesApi<T, Time, SeriesDataItemTypeMap <Time>[T], SeriesOptionsMap[T], SeriesPartialOptionsMap[T]>

Creates a series with specified parameters.

• T extends keyof SeriesOptionsMap

• definition: SeriesDefinition<T>

• options?: SeriesPartialOptionsMap[T]

Customization parameters of the series being created.

An index of the pane where the series should be created.

ISeriesApi<T, Time, SeriesDataItemTypeMap <Time>[T], SeriesOptionsMap[T], SeriesPartialOptionsMap[T]>

IChartApiBase . addSeries

removeSeries(seriesApi): void

Removes a series of any type. This is an irreversible operation, you cannot do anything with the series after removing it.

• seriesApi: ISeriesApi<keyof SeriesOptionsMap, Time, CustomData <Time> | WhitespaceData <Time> | AreaData <Time> | BarData <Time> | CandlestickData <Time> | BaselineData <Time> | LineData <Time> | HistogramData <Time> | CustomSeriesWhitespaceData <Time>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>

IChartApiBase . removeSeries

subscribeClick(handler): void

Subscribe to the chart click event.

• handler: MouseEventHandler <Time>

Handler to be called on mouse click.

IChartApiBase . subscribeClick

unsubscribeClick(handler): void

Unsubscribe a handler that was previously subscribed using subscribeClick.

• handler: MouseEventHandler <Time>

Previously subscribed handler

IChartApiBase . unsubscribeClick

subscribeDblClick(handler): void

Subscribe to the chart double-click event.

• handler: MouseEventHandler <Time>

Handler to be called on mouse double-click.

IChartApiBase . subscribeDblClick

unsubscribeDblClick(handler): void

Unsubscribe a handler that was previously subscribed using subscribeDblClick.

• handler: MouseEventHandler <Time>

Previously subscribed handler

IChartApiBase . unsubscribeDblClick

subscribeCrosshairMove(handler): void

Subscribe to the crosshair move event.

• handler: MouseEventHandler <Time>

Handler to be called on crosshair move.

IChartApiBase . subscribeCrosshairMove

unsubscribeCrosshairMove(handler): void

Unsubscribe a handler that was previously subscribed using subscribeCrosshairMove.

• handler: MouseEventHandler <Time>

Previously subscribed handler

IChartApiBase . unsubscribeCrosshairMove

priceScale(priceScaleId, paneIndex?): IPriceScaleApi

Returns API to manipulate a price scale.

• priceScaleId: string

ID of the price scale.

Index of the pane (default: 0)

IChartApiBase . priceScale

timeScale(): ITimeScaleApi <Time>

Returns API to manipulate the time scale

IChartApiBase . timeScale

options(): Readonly <ChartOptionsImpl <Time>>

Returns currently applied options

Readonly <ChartOptionsImpl <Time>>

Full set of currently applied options, including defaults

IChartApiBase . options

takeScreenshot(addTopLayer?, includeCrosshair?): HTMLCanvasElement

Make a screenshot of the chart with all the elements excluding crosshair.

• addTopLayer?: boolean

if true, the top layer and primitives will be included in the screenshot (default: false)

• includeCrosshair?: boolean

works only if addTopLayer is enabled. If true, the crosshair will be included in the screenshot (default: false)

A canvas with the chart drawn on. Any Canvas methods like toDataURL() or toBlob() can be used to serialize the result.

IChartApiBase . takeScreenshot

addPane(preserveEmptyPane?): IPaneApi <Time>

Add a pane to the chart

• preserveEmptyPane?: boolean

Whether to preserve the empty pane

IChartApiBase . addPane

panes(): IPaneApi <Time>[]

Returns array of panes' API

IChartApiBase . panes

removePane(index): void

Removes a pane with index

the pane to be removed

IChartApiBase . removePane

swapPanes(first, second): void

swap the position of two panes.

IChartApiBase . swapPanes

autoSizeActive(): boolean

Returns the active state of the autoSize option. This can be used to check whether the chart is handling resizing automatically with a ResizeObserver.

Whether the autoSize option is enabled and the active.

IChartApiBase . autoSizeActive

chartElement(): HTMLDivElement

Returns the generated div element containing the chart. This can be used for adding your own additional event listeners, or for measuring the elements dimensions and position within the document.

generated div element containing the chart.

IChartApiBase . chartElement

setCrosshairPosition(price, horizontalPosition, seriesApi): void

Set the crosshair position within the chart.

Usually the crosshair position is set automatically by the user's actions. However in some cases you may want to set it explicitly.

For example if you want to synchronise the crosshairs of two separate charts.

The price (vertical coordinate) of the new crosshair position.

• horizontalPosition: Time

The horizontal coordinate (time by default) of the new crosshair position.

• seriesApi: ISeriesApi<keyof SeriesOptionsMap, Time, CustomData <Time> | WhitespaceData <Time> | AreaData <Time> | BarData <Time> | CandlestickData <Time> | BaselineData <Time> | LineData <Time> | HistogramData <Time> | CustomSeriesWhitespaceData <Time>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>

IChartApiBase . setCrosshairPosition

clearCrosshairPosition(): void

Clear the crosshair position within the chart.

IChartApiBase . clearCrosshairPosition

paneSize(paneIndex?): PaneSize

Returns the dimensions of the chart pane (the plot surface which excludes time and price scales). This would typically only be useful for plugin development.

The index of the pane

Dimensions of the chart pane

IChartApiBase . paneSize

horzBehaviour(): IHorzScaleBehavior <Time>

Returns the horizontal scale behaviour.

IHorzScaleBehavior <Time>

IChartApiBase . horzBehaviour

**Examples:**

Example 1 (javascript):
```javascript
const series = chart.addCustomSeries(myCustomPaneView);
```

Example 2 (css):
```css
const series = chart.addSeries(LineSeries, { lineWidth: 2 });
```

Example 3 (unknown):
```unknown
chart.removeSeries(series);
```

Example 4 (javascript):
```javascript
function myClickHandler(param) {    if (!param.point) {        return;    }    console.log(`Click at ${param.point.x}, ${param.point.y}. The time is ${param.time}.`);}chart.subscribeClick(myClickHandler);
```

---

## Interface: SeriesAttachedParameter<HorzScaleItem, TSeriesType>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/SeriesAttachedParameter

**Contents:**
- Interface: SeriesAttachedParameter<HorzScaleItem, TSeriesType>
- Type parameters​
- Properties​
  - chart​
  - series​
  - requestUpdate()​
    - Returns​
  - horzScaleBehavior​

Object containing references to the chart and series instances, and a requestUpdate method for triggering a refresh of the chart.

• HorzScaleItem = Time

• TSeriesType extends SeriesType = keyof SeriesOptionsMap

chart: IChartApiBase<HorzScaleItem>

series: ISeriesApi<TSeriesType, HorzScaleItem, SeriesDataItemTypeMap<HorzScaleItem>[TSeriesType], SeriesOptionsMap[TSeriesType], SeriesPartialOptionsMap[TSeriesType]>

Series to which the Primitive is attached.

requestUpdate: () => void

Request an update (redraw the chart)

horzScaleBehavior: IHorzScaleBehavior<HorzScaleItem>

Horizontal Scale Behaviour for the chart.

---

## Type alias: CustomSeriesPricePlotValues

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/CustomSeriesPricePlotValues

**Contents:**
- Type alias: CustomSeriesPricePlotValues

CustomSeriesPricePlotValues: number[]

Price values for the custom series. This list should include the largest, smallest, and current price values for the data point. The last value in the array will be used for the current value. You shouldn't need to have more than 3 values in this array since the library only needs a largest, smallest, and current value.

---

## Interface: UpDownMarkersPluginOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/UpDownMarkersPluginOptions

**Contents:**
- Interface: UpDownMarkersPluginOptions
- Properties​
  - positiveColor​
  - negativeColor​
  - updateVisibilityDuration​

Configuration options for the UpDownMarkers plugin.

positiveColor: string

The color used for markers indicating a positive price change. This color will be applied to markers shown above data points where the price has increased.

negativeColor: string

The color used for markers indicating a negative price change. This color will be applied to markers shown below data points where the price has decreased.

updateVisibilityDuration: number

The duration (in milliseconds) for which update markers remain visible on the chart. After this duration, the markers will automatically disappear. Set to 0 for markers to remain indefinitely until the next update.

---

## Type alias: ISeriesPrimitive<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/ISeriesPrimitive

**Contents:**
- Type alias: ISeriesPrimitive<HorzScaleItem>
- Type parameters​

ISeriesPrimitive<HorzScaleItem>: ISeriesPrimitiveBase <SeriesAttachedParameter<HorzScaleItem, SeriesType>>

Interface for series primitives. It must be implemented to add some external graphics to series.

• HorzScaleItem = Time

---

## Type alias: TimeFormatterFn()<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/TimeFormatterFn

**Contents:**
- Type alias: TimeFormatterFn()<HorzScaleItem>
- Type parameters​
- Parameters​
- Returns​

TimeFormatterFn<HorzScaleItem>: (time) => string

A custom function used to override formatting of a time to a string.

• HorzScaleItem = Time

• time: HorzScaleItem

---

## Type alias: BarSeriesPartialOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/BarSeriesPartialOptions

**Contents:**
- Type alias: BarSeriesPartialOptions

BarSeriesPartialOptions: SeriesPartialOptions <BarStyleOptions>

Represents bar series options where all properties are options.

---

## Interface: YieldCurveOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/YieldCurveOptions

**Contents:**
- Interface: YieldCurveOptions
- Properties​
  - baseResolution​
    - Default Value​
  - minimumTimeRange​
    - Default Value​
  - startTimeRange​
    - Default Value​
  - formatTime()?​
    - Parameters​

Options specific to yield curve charts.

baseResolution: number

The smallest time unit for the yield curve, typically representing one month. This value determines the granularity of the time scale.

minimumTimeRange: number

The minimum time range to be displayed on the chart, in units of baseResolution. This ensures that the chart always shows at least this much time range, even if there's less data.

startTimeRange: number

The starting time value for the chart, in units of baseResolution. This determines where the time scale begins.

optional formatTime: (months) => string

Optional custom formatter for time values on the horizontal axis. If not provided, a default formatter will be used.

The number of months (or baseResolution units) to format

**Examples:**

Example 1 (unknown):
```unknown
120 (10 years)
```

---

## Type alias: UpDownMarkersSupportedSeriesTypes

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/UpDownMarkersSupportedSeriesTypes

**Contents:**
- Type alias: UpDownMarkersSupportedSeriesTypes

UpDownMarkersSupportedSeriesTypes: "Line" | "Area"

Defines the supported series types for up down markers primitive plugin.

---

## Interface: VerticalGradientColor

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/VerticalGradientColor

**Contents:**
- Interface: VerticalGradientColor
- Properties​
  - type​
  - topColor​
  - bottomColor​

Represents a vertical gradient of two colors.

type: VerticalGradient

---

## Type alias: HorzAlign

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/HorzAlign

**Contents:**
- Type alias: HorzAlign

HorzAlign: "left" | "center" | "right"

Represents a horizontal alignment.

---

## Interface: ChartOptionsImpl<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/ChartOptionsImpl

**Contents:**
- Interface: ChartOptionsImpl<HorzScaleItem>
- Extends​
- Extended by​
- Type parameters​
- Properties​
  - width​
    - Default Value​
    - Inherited from​
  - height​
    - Default Value​

Structure describing options of the chart. Series options are to be set separately

Width of the chart in pixels

If 0 (default) or none value provided, then a size of the widget will be calculated based its container's size.

ChartOptionsBase . width

Height of the chart in pixels

If 0 (default) or none value provided, then a size of the widget will be calculated based its container's size.

ChartOptionsBase . height

Setting this flag to true will make the chart watch the chart container's size and automatically resize the chart to fit its container whenever the size changes.

This feature requires ResizeObserver class to be available in the global scope. Note that calling code is responsible for providing a polyfill if required. If the global scope does not have ResizeObserver, a warning will appear and the flag will be ignored.

Please pay attention that autoSize option and explicit sizes options width and height don't conflict with one another. If you specify autoSize flag, then width and height options will be ignored unless ResizeObserver has failed. If it fails then the values will be used as fallback.

The flag autoSize could also be set with and unset with applyOptions function.

ChartOptionsBase . autoSize

layout: LayoutOptions

ChartOptionsBase . layout

leftPriceScale: PriceScaleOptions

Left price scale options

ChartOptionsBase . leftPriceScale

rightPriceScale: PriceScaleOptions

Right price scale options

ChartOptionsBase . rightPriceScale

overlayPriceScales: OverlayPriceScaleOptions

Overlay price scale options

ChartOptionsBase . overlayPriceScales

timeScale: HorzScaleOptions

ChartOptionsBase . timeScale

crosshair: CrosshairOptions

The crosshair shows the intersection of the price and time scale values at any point on the chart.

ChartOptionsBase . crosshair

A grid is represented in the chart background as a vertical and horizontal lines drawn at the levels of visible marks of price and the time scales.

ChartOptionsBase . grid

handleScroll: boolean | HandleScrollOptions

Scroll options, or a boolean flag that enables/disables scrolling

ChartOptionsBase . handleScroll

handleScale: boolean | HandleScaleOptions

Scale options, or a boolean flag that enables/disables scaling

ChartOptionsBase . handleScale

kineticScroll: KineticScrollOptions

Kinetic scroll options

ChartOptionsBase . kineticScroll

trackingMode: TrackingModeOptions

Represent options for the tracking mode's behavior.

Mobile users will not have the ability to see the values/dates like they do on desktop. To see it, they should enter the tracking mode. The tracking mode will deactivate the scrolling and make it possible to check values and dates.

ChartOptionsBase . trackingMode

addDefaultPane: boolean

Whether to add a default pane to the chart Disable this option when you want to create a chart with no panes and add them manually

ChartOptionsBase . addDefaultPane

localization: LocalizationOptions<HorzScaleItem>

Localization options.

ChartOptionsBase . localization

**Examples:**

Example 1 (css):
```css
const chart = LightweightCharts.createChart(document.body, {    autoSize: true,});
```

---

## Interface: ISeriesUpDownMarkerPluginApi<HorzScaleItem, TData>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/ISeriesUpDownMarkerPluginApi

**Contents:**
- Interface: ISeriesUpDownMarkerPluginApi<HorzScaleItem, TData>
- Extends​
- Type parameters​
- Properties​
  - detach()​
    - Returns​
    - Inherited from​
  - getSeries()​
    - Returns​
    - Inherited from​

UpDownMarkersPrimitive Plugin for showing the direction of price changes on the chart. This plugin can only be used with Line and Area series types.

Use setData and update from this primitive instead of the those on the series to let the primitive handle the creation of price change markers automatically.

• TData extends SeriesDataItemTypeMap<HorzScaleItem>[UpDownMarkersSupportedSeriesTypes] = SeriesDataItemTypeMap<HorzScaleItem>["Line"]

Detaches the plugin from the series.

ISeriesPrimitiveWrapper . detach

getSeries: () => ISeriesApi<keyof SeriesOptionsMap, HorzScaleItem, WhitespaceData<HorzScaleItem> | LineData<HorzScaleItem> | AreaData<HorzScaleItem> | BarData<HorzScaleItem> | CandlestickData<HorzScaleItem> | BaselineData<HorzScaleItem> | HistogramData<HorzScaleItem> | CustomData<HorzScaleItem> | CustomSeriesWhitespaceData<HorzScaleItem>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>

Returns the current series.

ISeriesApi<keyof SeriesOptionsMap, HorzScaleItem, WhitespaceData<HorzScaleItem> | LineData<HorzScaleItem> | AreaData<HorzScaleItem> | BarData<HorzScaleItem> | CandlestickData<HorzScaleItem> | BaselineData<HorzScaleItem> | HistogramData<HorzScaleItem> | CustomData<HorzScaleItem> | CustomSeriesWhitespaceData<HorzScaleItem>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>

ISeriesPrimitiveWrapper . getSeries

applyOptions: (options) => void

Applies new options to the plugin.

• options: Partial <UpDownMarkersPluginOptions>

Partial options to apply.

ISeriesPrimitiveWrapper . applyOptions

setData: (data) => void

Sets the data for the series and manages data points for marker updates.

Array of data points to set.

update: (data, historicalUpdate?) => void

Updates a single data point and manages marker updates for existing data points.

The data point to update.

• historicalUpdate?: boolean

Optional flag for historical updates.

markers: () => readonly SeriesUpDownMarker<HorzScaleItem>[]

Retrieves the current markers on the chart.

readonly SeriesUpDownMarker<HorzScaleItem>[]

setMarkers: (markers) => void

Manually sets markers on the chart.

• markers: SeriesUpDownMarker<HorzScaleItem>[]

Array of SeriesUpDownMarker to set.

clearMarkers: () => void

Clears all markers from the chart.

---

## Interface: IPrimitivePaneView

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/IPrimitivePaneView

**Contents:**
- Interface: IPrimitivePaneView
- Methods​
  - zOrder()?​
    - Returns​
    - See​
  - renderer()​
    - Returns​

This interface represents the primitive for one of the pane of the chart (main chart area, time scale, price scale).

optional zOrder(): PrimitivePaneViewZOrder

Defines where in the visual layer stack the renderer should be executed. Default is 'normal'.

PrimitivePaneViewZOrder

the desired position in the visual layer stack.

PrimitivePaneViewZOrder

renderer(): IPrimitivePaneRenderer

This method returns a renderer - special object to draw data

IPrimitivePaneRenderer

an renderer object to be used for drawing, or null if we have nothing to draw.

---

## Type alias: InternalHorzScaleItemKey

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/InternalHorzScaleItemKey

**Contents:**
- Type alias: InternalHorzScaleItemKey

InternalHorzScaleItemKey: Nominal<number, "InternalHorzScaleItemKey">

Index key for a horizontal scale item.

---

## Type alias: Rgba

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/Rgba

**Contents:**
- Type alias: Rgba

Rgba: [RedComponent, GreenComponent, BlueComponent, AlphaComponent]

---

## Enumeration: PriceScaleMode

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/enumerations/PriceScaleMode

**Contents:**
- Enumeration: PriceScaleMode
- Enumeration Members​
  - Normal​
  - Logarithmic​
  - Percentage​
  - IndexedTo100​

Represents the price scale mode.

Price scale shows prices. Price range changes linearly.

Price scale shows prices. Price range changes logarithmically.

Price scale shows percentage values according the first visible value of the price scale. The first visible value is 0% in this mode.

The same as percentage mode, but the first value is moved to 100.

---

## Interface: ITimeScaleApi<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/ITimeScaleApi

**Contents:**
- Interface: ITimeScaleApi<HorzScaleItem>
- Type parameters​
- Methods​
  - scrollPosition()​
    - Returns​
  - scrollToPosition()​
    - Parameters​
    - Returns​
  - scrollToRealTime()​
    - Returns​

Interface to chart time scale

scrollPosition(): number

Return the distance from the right edge of the time scale to the lastest bar of the series measured in bars.

scrollToPosition(position, animated): void

Scrolls the chart to the specified position.

Setting this to true makes the chart scrolling smooth and adds animation

scrollToRealTime(): void

Restores default scroll position of the chart. This process is always animated.

getVisibleRange(): IRange<HorzScaleItem>

Returns current visible time range of the chart.

Note that this method cannot extrapolate time and will use the only currently existent data. To get complete information about current visible range, please use getVisibleLogicalRange and ISeriesApi.barsInLogicalRange.

IRange<HorzScaleItem>

Visible range or null if the chart has no data at all.

setVisibleRange(range): void

Sets visible range of data.

Note that this method cannot extrapolate time and will use the only currently existent data. Thus, for example, if currently a chart doesn't have data prior 2018-01-01 date and you set visible range with from date 2016-01-01, it will be automatically adjusted to 2018-01-01 (and the same for to date).

But if you can approximate indexes on your own - you could use setVisibleLogicalRange instead.

• range: IRange<HorzScaleItem>

Target visible range of data.

getVisibleLogicalRange(): LogicalRange

Returns the current visible logical range of the chart as an object with the first and last time points of the logical range, or returns null if the chart has no data.

Visible range or null if the chart has no data at all.

setVisibleLogicalRange(range): void

Sets visible logical range of data.

• range: IRange<number>

Target visible logical range of data.

resetTimeScale(): void

Restores default zoom level and scroll position of the time scale.

Automatically calculates the visible range to fit all data from all series.

logicalToCoordinate(logical): Coordinate

Converts a logical index to local x coordinate.

Logical index needs to be converted

x coordinate of that time or null if the chart doesn't have data

coordinateToLogical(x): Logical

Converts a coordinate to logical index.

Coordinate needs to be converted

Logical index that is located on that coordinate or null if the chart doesn't have data

timeToIndex(time, findNearest?): TimePointIndex

Converts a time to local x coordinate.

• time: HorzScaleItem

Time needs to be converted

• findNearest?: boolean

X coordinate of that time or null if no time found on time scale

timeToCoordinate(time): Coordinate

Converts a time to local x coordinate.

• time: HorzScaleItem

Time needs to be converted

X coordinate of that time or null if no time found on time scale

coordinateToTime(x): HorzScaleItem

Converts a coordinate to time.

Coordinate needs to be converted.

Time of a bar that is located on that coordinate or null if there are no bars found on that coordinate.

Returns a width of the time scale.

Returns a height of the time scale.

subscribeVisibleTimeRangeChange(handler): void

Subscribe to the visible time range change events.

The argument passed to the handler function is an object with from and to properties of type Time, or null if there is no visible data.

• handler: TimeRangeChangeEventHandler<HorzScaleItem>

Handler (function) to be called when the visible indexes change.

unsubscribeVisibleTimeRangeChange(handler): void

Unsubscribe a handler that was previously subscribed using subscribeVisibleTimeRangeChange.

• handler: TimeRangeChangeEventHandler<HorzScaleItem>

Previously subscribed handler

subscribeVisibleLogicalRangeChange(handler): void

Subscribe to the visible logical range change events.

The argument passed to the handler function is an object with from and to properties of type number, or null if there is no visible data.

• handler: LogicalRangeChangeEventHandler

Handler (function) to be called when the visible indexes change.

unsubscribeVisibleLogicalRangeChange(handler): void

Unsubscribe a handler that was previously subscribed using subscribeVisibleLogicalRangeChange.

• handler: LogicalRangeChangeEventHandler

Previously subscribed handler

subscribeSizeChange(handler): void

Adds a subscription to time scale size changes

• handler: SizeChangeEventHandler

Handler (function) to be called when the time scale size changes

unsubscribeSizeChange(handler): void

Removes a subscription to time scale size changes

• handler: SizeChangeEventHandler

Previously subscribed handler

applyOptions(options): void

Applies new options to the time scale.

• options: DeepPartial <HorzScaleOptions>

Any subset of options.

options(): Readonly <HorzScaleOptions>

Returns current options

Readonly <HorzScaleOptions>

Currently applied options

**Examples:**

Example 1 (css):
```css
chart.timeScale().setVisibleRange({    from: (new Date(Date.UTC(2018, 0, 1, 0, 0, 0, 0))).getTime() / 1000,    to: (new Date(Date.UTC(2018, 1, 1, 0, 0, 0, 0))).getTime() / 1000,});
```

Example 2 (css):
```css
chart.timeScale().setVisibleLogicalRange({ from: 0, to: 10 });
```

Example 3 (javascript):
```javascript
function myVisibleTimeRangeChangeHandler(newVisibleTimeRange) {    if (newVisibleTimeRange === null) {        // handle null    }    // handle new logical range}chart.timeScale().subscribeVisibleTimeRangeChange(myVisibleTimeRangeChangeHandler);
```

Example 4 (unknown):
```unknown
chart.timeScale().unsubscribeVisibleTimeRangeChange(myVisibleTimeRangeChangeHandler);
```

---

## Variable: customSeriesDefaultOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/variables/customSeriesDefaultOptions

**Contents:**
- Variable: customSeriesDefaultOptions

const customSeriesDefaultOptions: CustomSeriesOptions

---

## Interface: BaseValuePrice

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/BaseValuePrice

**Contents:**
- Interface: BaseValuePrice
- Properties​
  - type​
  - price​

Represents a type of priced base value of baseline series type.

Distinguished type value.

---

## Interface: TouchMouseEventData

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/TouchMouseEventData

**Contents:**
- Interface: TouchMouseEventData
- Properties​
  - clientX​
  - clientY​
  - pageX​
  - pageY​
  - screenX​
  - screenY​
  - localX​
  - localY​

The TouchMouseEventData interface represents events that occur due to the user interacting with a pointing device (such as a mouse). See MouseEvent

readonly clientX: Coordinate

The X coordinate of the mouse pointer in local (DOM content) coordinates.

readonly clientY: Coordinate

The Y coordinate of the mouse pointer in local (DOM content) coordinates.

readonly pageX: Coordinate

The X coordinate of the mouse pointer relative to the whole document.

readonly pageY: Coordinate

The Y coordinate of the mouse pointer relative to the whole document.

readonly screenX: Coordinate

The X coordinate of the mouse pointer in global (screen) coordinates.

readonly screenY: Coordinate

The Y coordinate of the mouse pointer in global (screen) coordinates.

readonly localX: Coordinate

The X coordinate of the mouse pointer relative to the chart / price axis / time axis canvas element.

readonly localY: Coordinate

The Y coordinate of the mouse pointer relative to the chart / price axis / time axis canvas element.

readonly ctrlKey: boolean

Returns a boolean value that is true if the Ctrl key was active when the key event was generated.

readonly altKey: boolean

Returns a boolean value that is true if the Alt (Option or ⌥ on macOS) key was active when the key event was generated.

readonly shiftKey: boolean

Returns a boolean value that is true if the Shift key was active when the key event was generated.

readonly metaKey: boolean

Returns a boolean value that is true if the Meta key (on Mac keyboards, the ⌘ Command key; on Windows keyboards, the Windows key (⊞)) was active when the key event was generated.

---

## Interface: IChartApiBase<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/IChartApiBase

**Contents:**
- Interface: IChartApiBase<HorzScaleItem>
- Extended by​
- Type parameters​
- Methods​
  - remove()​
    - Returns​
  - resize()​
    - Parameters​
    - Returns​
  - addCustomSeries()​

The main interface of a single chart.

• HorzScaleItem = Time

Removes the chart object including all DOM elements. This is an irreversible operation, you cannot do anything with the chart after removing it.

resize(width, height, forceRepaint?): void

Sets fixed size of the chart. By default chart takes up 100% of its container.

If chart has the autoSize option enabled, and the ResizeObserver is available then the width and height values will be ignored.

Target width of the chart.

Target height of the chart.

• forceRepaint?: boolean

True to initiate resize immediately. One could need this to get screenshot immediately after resize.

addCustomSeries<TData, TOptions, TPartialOptions>(customPaneView, customOptions?, paneIndex?): ISeriesApi<"Custom", HorzScaleItem, TData | WhitespaceData<HorzScaleItem>, TOptions, TPartialOptions>

Creates a custom series with specified parameters.

A custom series is a generic series which can be extended with a custom renderer to implement chart types which the library doesn't support by default.

• TData extends CustomData<HorzScaleItem>

• TOptions extends CustomSeriesOptions

• TPartialOptions extends DeepPartial<TOptions & SeriesOptionsCommon> = DeepPartial<TOptions & SeriesOptionsCommon>

• customPaneView: ICustomSeriesPaneView<HorzScaleItem, TData, TOptions>

A custom series pane view which implements the custom renderer.

• customOptions?: DeepPartial<TOptions & SeriesOptionsCommon>

Customization parameters of the series being created.

ISeriesApi<"Custom", HorzScaleItem, TData | WhitespaceData<HorzScaleItem>, TOptions, TPartialOptions>

addSeries<T>(definition, options?, paneIndex?): ISeriesApi<T, HorzScaleItem, SeriesDataItemTypeMap<HorzScaleItem>[T], SeriesOptionsMap[T], SeriesPartialOptionsMap[T]>

Creates a series with specified parameters.

• T extends keyof SeriesOptionsMap

• definition: SeriesDefinition<T>

• options?: SeriesPartialOptionsMap[T]

Customization parameters of the series being created.

An index of the pane where the series should be created.

ISeriesApi<T, HorzScaleItem, SeriesDataItemTypeMap<HorzScaleItem>[T], SeriesOptionsMap[T], SeriesPartialOptionsMap[T]>

removeSeries(seriesApi): void

Removes a series of any type. This is an irreversible operation, you cannot do anything with the series after removing it.

• seriesApi: ISeriesApi<keyof SeriesOptionsMap, HorzScaleItem, CustomData<HorzScaleItem> | WhitespaceData<HorzScaleItem> | AreaData<HorzScaleItem> | BarData<HorzScaleItem> | CandlestickData<HorzScaleItem> | BaselineData<HorzScaleItem> | LineData<HorzScaleItem> | HistogramData<HorzScaleItem> | CustomSeriesWhitespaceData<HorzScaleItem>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>

subscribeClick(handler): void

Subscribe to the chart click event.

• handler: MouseEventHandler<HorzScaleItem>

Handler to be called on mouse click.

unsubscribeClick(handler): void

Unsubscribe a handler that was previously subscribed using subscribeClick.

• handler: MouseEventHandler<HorzScaleItem>

Previously subscribed handler

subscribeDblClick(handler): void

Subscribe to the chart double-click event.

• handler: MouseEventHandler<HorzScaleItem>

Handler to be called on mouse double-click.

unsubscribeDblClick(handler): void

Unsubscribe a handler that was previously subscribed using subscribeDblClick.

• handler: MouseEventHandler<HorzScaleItem>

Previously subscribed handler

subscribeCrosshairMove(handler): void

Subscribe to the crosshair move event.

• handler: MouseEventHandler<HorzScaleItem>

Handler to be called on crosshair move.

unsubscribeCrosshairMove(handler): void

Unsubscribe a handler that was previously subscribed using subscribeCrosshairMove.

• handler: MouseEventHandler<HorzScaleItem>

Previously subscribed handler

priceScale(priceScaleId, paneIndex?): IPriceScaleApi

Returns API to manipulate a price scale.

• priceScaleId: string

ID of the price scale.

Index of the pane (default: 0)

timeScale(): ITimeScaleApi<HorzScaleItem>

Returns API to manipulate the time scale

ITimeScaleApi<HorzScaleItem>

applyOptions(options): void

Applies new options to the chart

• options: DeepPartial <ChartOptionsImpl<HorzScaleItem>>

Any subset of options.

options(): Readonly <ChartOptionsImpl<HorzScaleItem>>

Returns currently applied options

Readonly <ChartOptionsImpl<HorzScaleItem>>

Full set of currently applied options, including defaults

takeScreenshot(addTopLayer?, includeCrosshair?): HTMLCanvasElement

Make a screenshot of the chart with all the elements excluding crosshair.

• addTopLayer?: boolean

if true, the top layer and primitives will be included in the screenshot (default: false)

• includeCrosshair?: boolean

works only if addTopLayer is enabled. If true, the crosshair will be included in the screenshot (default: false)

A canvas with the chart drawn on. Any Canvas methods like toDataURL() or toBlob() can be used to serialize the result.

addPane(preserveEmptyPane?): IPaneApi<HorzScaleItem>

Add a pane to the chart

• preserveEmptyPane?: boolean

Whether to preserve the empty pane

IPaneApi<HorzScaleItem>

panes(): IPaneApi<HorzScaleItem>[]

Returns array of panes' API

IPaneApi<HorzScaleItem>[]

removePane(index): void

Removes a pane with index

the pane to be removed

swapPanes(first, second): void

swap the position of two panes.

autoSizeActive(): boolean

Returns the active state of the autoSize option. This can be used to check whether the chart is handling resizing automatically with a ResizeObserver.

Whether the autoSize option is enabled and the active.

chartElement(): HTMLDivElement

Returns the generated div element containing the chart. This can be used for adding your own additional event listeners, or for measuring the elements dimensions and position within the document.

generated div element containing the chart.

setCrosshairPosition(price, horizontalPosition, seriesApi): void

Set the crosshair position within the chart.

Usually the crosshair position is set automatically by the user's actions. However in some cases you may want to set it explicitly.

For example if you want to synchronise the crosshairs of two separate charts.

The price (vertical coordinate) of the new crosshair position.

• horizontalPosition: HorzScaleItem

The horizontal coordinate (time by default) of the new crosshair position.

• seriesApi: ISeriesApi<keyof SeriesOptionsMap, HorzScaleItem, CustomData<HorzScaleItem> | WhitespaceData<HorzScaleItem> | AreaData<HorzScaleItem> | BarData<HorzScaleItem> | CandlestickData<HorzScaleItem> | BaselineData<HorzScaleItem> | LineData<HorzScaleItem> | HistogramData<HorzScaleItem> | CustomSeriesWhitespaceData<HorzScaleItem>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>

clearCrosshairPosition(): void

Clear the crosshair position within the chart.

paneSize(paneIndex?): PaneSize

Returns the dimensions of the chart pane (the plot surface which excludes time and price scales). This would typically only be useful for plugin development.

The index of the pane

Dimensions of the chart pane

horzBehaviour(): IHorzScaleBehavior<HorzScaleItem>

Returns the horizontal scale behaviour.

IHorzScaleBehavior<HorzScaleItem>

**Examples:**

Example 1 (javascript):
```javascript
const series = chart.addCustomSeries(myCustomPaneView);
```

Example 2 (css):
```css
const series = chart.addSeries(LineSeries, { lineWidth: 2 });
```

Example 3 (unknown):
```unknown
chart.removeSeries(series);
```

Example 4 (javascript):
```javascript
function myClickHandler(param) {    if (!param.point) {        return;    }    console.log(`Click at ${param.point.x}, ${param.point.y}. The time is ${param.time}.`);}chart.subscribeClick(myClickHandler);
```

---

## Interface: IHorzScaleBehavior<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/IHorzScaleBehavior

**Contents:**
- Interface: IHorzScaleBehavior<HorzScaleItem>
- Type parameters​
- Methods​
  - options()​
    - Returns​
  - setOptions()​
    - Parameters​
    - Returns​
  - preprocessData()​
    - Parameters​

Class interface for Horizontal scale behavior

options(): ChartOptionsImpl<HorzScaleItem>

Structure describing options of the chart.

ChartOptionsImpl<HorzScaleItem>

setOptions(options): void

Set the chart options. Note that this is different to applyOptions since the provided options will overwrite the current options instead of merging with the current options.

• options: ChartOptionsImpl<HorzScaleItem>

Chart options to be set

preprocessData(data): void

Method to preprocess the data.

• data: DataItem<HorzScaleItem> | DataItem<HorzScaleItem>[]

Data items for the series

convertHorzItemToInternal(item): object

Convert horizontal scale item into an internal horizontal scale item.

• item: HorzScaleItem

InternalHorzScaleItem

[species]: "InternalHorzScaleItem"

The 'name' or species of the nominal.

createConverterToInternalObj(data): HorzScaleItemConverterToInternalObj<HorzScaleItem>

Creates and returns a converter for changing series data into internal horizontal scale items.

• data: (AreaData<HorzScaleItem> | WhitespaceData<HorzScaleItem> | BarData<HorzScaleItem> | CandlestickData<HorzScaleItem> | BaselineData<HorzScaleItem> | LineData<HorzScaleItem> | HistogramData<HorzScaleItem> | CustomData<HorzScaleItem> | CustomSeriesWhitespaceData<HorzScaleItem>)[]

HorzScaleItemConverterToInternalObj<HorzScaleItem>

HorzScaleItemConverterToInternalObj

key(internalItem): InternalHorzScaleItemKey

Returns the key for the specified horizontal scale item.

• internalItem: HorzScaleItem | object

horizontal scale item for which the key should be returned

InternalHorzScaleItemKey

InternalHorzScaleItemKey

cacheKey(internalItem): number

Returns the cache key for the specified horizontal scale item.

horizontal scale item for which the cache key should be returned

• internalItem.[species]: "InternalHorzScaleItem"

The 'name' or species of the nominal.

updateFormatter(options): void

Update the formatter with the localization options.

• options: LocalizationOptions<HorzScaleItem>

formatHorzItem(item): string

Format the horizontal scale item into a display string.

horizontal scale item to be formatted as a string

• item.[species]: "InternalHorzScaleItem"

The 'name' or species of the nominal.

formatTickmark(item, localizationOptions): string

Format the horizontal scale tickmark into a display string.

• localizationOptions: LocalizationOptions<HorzScaleItem>

maxTickMarkWeight(marks): TickMarkWeightValue

Returns the maximum tickmark weight value for the specified tickmarks on the time scale.

fillWeightsForPoints(sortedTimePoints, startIndex): void

Fill the weights for the sorted time scale points.

• sortedTimePoints: readonly Mutable <TimeScalePoint>[]

sorted time scale points

optional shouldResetTickmarkLabels(tickMarks): boolean

If returns true, then the tick mark formatter will be called for all the visible tick marks even if the formatter has previously been called for a specific tick mark. This allows you to change the formatting on all the tick marks.

• tickMarks: readonly TickMark[]

---

## Interface: TextWatermarkOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/TextWatermarkOptions

**Contents:**
- Interface: TextWatermarkOptions
- Properties​
  - visible​
    - Default Value​
  - horzAlign​
    - Default Value​
  - vertAlign​
    - Default Value​
  - lines​
    - Default Value​

Display the watermark.

Horizontal alignment inside the chart area.

Vertical alignment inside the chart area.

lines: TextWatermarkLineOptions[]

Text to be displayed within the watermark. Each item in the array is treated as new line.

---

## Type alias: SeriesMarkerZOrder

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/SeriesMarkerZOrder

**Contents:**
- Type alias: SeriesMarkerZOrder

SeriesMarkerZOrder: "top" | "aboveSeries" | "normal"

The visual stacking order for the markers within the chart.

---

## Interface: IYieldCurveChartApi

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/IYieldCurveChartApi

**Contents:**
- Interface: IYieldCurveChartApi
- Extends​
- Methods​
  - remove()​
    - Returns​
    - Inherited from​
  - resize()​
    - Parameters​
    - Returns​
    - Inherited from​

The main interface of a single yield curve chart.

Removes the chart object including all DOM elements. This is an irreversible operation, you cannot do anything with the chart after removing it.

resize(width, height, forceRepaint?): void

Sets fixed size of the chart. By default chart takes up 100% of its container.

If chart has the autoSize option enabled, and the ResizeObserver is available then the width and height values will be ignored.

Target width of the chart.

Target height of the chart.

• forceRepaint?: boolean

True to initiate resize immediately. One could need this to get screenshot immediately after resize.

addCustomSeries<TData, TOptions, TPartialOptions>(customPaneView, customOptions?, paneIndex?): ISeriesApi<"Custom", number, WhitespaceData<number> | TData, TOptions, TPartialOptions>

Creates a custom series with specified parameters.

A custom series is a generic series which can be extended with a custom renderer to implement chart types which the library doesn't support by default.

• TData extends CustomData<number>

• TOptions extends CustomSeriesOptions

• TPartialOptions extends DeepPartial<TOptions & SeriesOptionsCommon> = DeepPartial<TOptions & SeriesOptionsCommon>

• customPaneView: ICustomSeriesPaneView<number, TData, TOptions>

A custom series pane view which implements the custom renderer.

• customOptions?: DeepPartial<TOptions & SeriesOptionsCommon>

Customization parameters of the series being created.

ISeriesApi<"Custom", number, WhitespaceData<number> | TData, TOptions, TPartialOptions>

removeSeries(seriesApi): void

Removes a series of any type. This is an irreversible operation, you cannot do anything with the series after removing it.

• seriesApi: ISeriesApi<keyof SeriesOptionsMap, number, WhitespaceData<number> | LineData<number> | CustomData<number> | AreaData<number> | BarData<number> | CandlestickData<number> | BaselineData<number> | HistogramData<number> | CustomSeriesWhitespaceData<number>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>

subscribeClick(handler): void

Subscribe to the chart click event.

• handler: MouseEventHandler<number>

Handler to be called on mouse click.

unsubscribeClick(handler): void

Unsubscribe a handler that was previously subscribed using subscribeClick.

• handler: MouseEventHandler<number>

Previously subscribed handler

Omit.unsubscribeClick

subscribeDblClick(handler): void

Subscribe to the chart double-click event.

• handler: MouseEventHandler<number>

Handler to be called on mouse double-click.

Omit.subscribeDblClick

unsubscribeDblClick(handler): void

Unsubscribe a handler that was previously subscribed using subscribeDblClick.

• handler: MouseEventHandler<number>

Previously subscribed handler

Omit.unsubscribeDblClick

subscribeCrosshairMove(handler): void

Subscribe to the crosshair move event.

• handler: MouseEventHandler<number>

Handler to be called on crosshair move.

Omit.subscribeCrosshairMove

unsubscribeCrosshairMove(handler): void

Unsubscribe a handler that was previously subscribed using subscribeCrosshairMove.

• handler: MouseEventHandler<number>

Previously subscribed handler

Omit.unsubscribeCrosshairMove

priceScale(priceScaleId, paneIndex?): IPriceScaleApi

Returns API to manipulate a price scale.

• priceScaleId: string

ID of the price scale.

Index of the pane (default: 0)

timeScale(): ITimeScaleApi<number>

Returns API to manipulate the time scale

ITimeScaleApi<number>

applyOptions(options): void

Applies new options to the chart

• options: DeepPartial <ChartOptionsImpl<number>>

Any subset of options.

options(): Readonly <ChartOptionsImpl<number>>

Returns currently applied options

Readonly <ChartOptionsImpl<number>>

Full set of currently applied options, including defaults

takeScreenshot(addTopLayer?, includeCrosshair?): HTMLCanvasElement

Make a screenshot of the chart with all the elements excluding crosshair.

• addTopLayer?: boolean

if true, the top layer and primitives will be included in the screenshot (default: false)

• includeCrosshair?: boolean

works only if addTopLayer is enabled. If true, the crosshair will be included in the screenshot (default: false)

A canvas with the chart drawn on. Any Canvas methods like toDataURL() or toBlob() can be used to serialize the result.

addPane(preserveEmptyPane?): IPaneApi<number>

Add a pane to the chart

• preserveEmptyPane?: boolean

Whether to preserve the empty pane

panes(): IPaneApi<number>[]

Returns array of panes' API

removePane(index): void

Removes a pane with index

the pane to be removed

swapPanes(first, second): void

swap the position of two panes.

autoSizeActive(): boolean

Returns the active state of the autoSize option. This can be used to check whether the chart is handling resizing automatically with a ResizeObserver.

Whether the autoSize option is enabled and the active.

chartElement(): HTMLDivElement

Returns the generated div element containing the chart. This can be used for adding your own additional event listeners, or for measuring the elements dimensions and position within the document.

generated div element containing the chart.

setCrosshairPosition(price, horizontalPosition, seriesApi): void

Set the crosshair position within the chart.

Usually the crosshair position is set automatically by the user's actions. However in some cases you may want to set it explicitly.

For example if you want to synchronise the crosshairs of two separate charts.

The price (vertical coordinate) of the new crosshair position.

• horizontalPosition: number

The horizontal coordinate (time by default) of the new crosshair position.

• seriesApi: ISeriesApi<keyof SeriesOptionsMap, number, WhitespaceData<number> | LineData<number> | CustomData<number> | AreaData<number> | BarData<number> | CandlestickData<number> | BaselineData<number> | HistogramData<number> | CustomSeriesWhitespaceData<number>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>

Omit.setCrosshairPosition

clearCrosshairPosition(): void

Clear the crosshair position within the chart.

Omit.clearCrosshairPosition

paneSize(paneIndex?): PaneSize

Returns the dimensions of the chart pane (the plot surface which excludes time and price scales). This would typically only be useful for plugin development.

The index of the pane

Dimensions of the chart pane

horzBehaviour(): IHorzScaleBehavior<number>

Returns the horizontal scale behaviour.

IHorzScaleBehavior<number>

addSeries<T>(definition, options?, paneIndex?): ISeriesApi<T, number, WhitespaceData<number> | LineData<number>, SeriesOptionsMap[T], SeriesPartialOptionsMap[T]>

Creates a series with specified parameters.

Note that the Yield Curve chart only supports the Area and Line series types.

• T extends YieldCurveSeriesType

• definition: SeriesDefinition<T>

A series definition for either AreaSeries or LineSeries.

• options?: SeriesPartialOptionsMap[T]

Customization parameters of the series being created.

An index of the pane where the series should be created.

ISeriesApi<T, number, WhitespaceData<number> | LineData<number>, SeriesOptionsMap[T], SeriesPartialOptionsMap[T]>

**Examples:**

Example 1 (javascript):
```javascript
const series = chart.addCustomSeries(myCustomPaneView);
```

Example 2 (unknown):
```unknown
chart.removeSeries(series);
```

Example 3 (javascript):
```javascript
function myClickHandler(param) {    if (!param.point) {        return;    }    console.log(`Click at ${param.point.x}, ${param.point.y}. The time is ${param.time}.`);}chart.subscribeClick(myClickHandler);
```

Example 4 (unknown):
```unknown
chart.unsubscribeClick(myClickHandler);
```

---

## Type alias: AlphaComponent

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/AlphaComponent

**Contents:**
- Type alias: AlphaComponent

AlphaComponent: Nominal<number, "AlphaComponent">

Alpha component of the RGBA color value The valid values are integers in range [0, 1]

---

## Type alias: TickmarksPercentageFormatterFn()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/TickmarksPercentageFormatterFn

**Contents:**
- Type alias: TickmarksPercentageFormatterFn()
- Parameters​
- Returns​

TickmarksPercentageFormatterFn: (percentageValue) => string[]

• percentageValue: number[]

---

## Type alias: GreenComponent

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/GreenComponent

**Contents:**
- Type alias: GreenComponent

GreenComponent: Nominal<number, "GreenComponent">

Green component of the RGB color value The valid values are integers in range [0, 255]

---

## Type alias: AutoscaleInfoProvider()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/AutoscaleInfoProvider

**Contents:**
- Type alias: AutoscaleInfoProvider()
- Parameters​
- Returns​

AutoscaleInfoProvider: (baseImplementation) => AutoscaleInfo | null

A custom function used to get autoscale information.

The default implementation of autoscale algorithm, you can use it to adjust the result.

---

## Type alias: DeepPartial<T>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/DeepPartial

**Contents:**
- Type alias: DeepPartial<T>
- Type parameters​

DeepPartial<T>: { [P in keyof T]?: T[P] extends (infer U)[] ? DeepPartial<U>[] : T[P] extends readonly (infer X)[] ? readonly DeepPartial<X>[] : DeepPartial<T[P]> }

Represents a type T where every property is optional.

---

## Type alias: BaselineSeriesPartialOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/BaselineSeriesPartialOptions

**Contents:**
- Type alias: BaselineSeriesPartialOptions

BaselineSeriesPartialOptions: SeriesPartialOptions <BaselineStyleOptions>

Represents baseline series options where all properties are options.

---

## Type alias: SizeChangeEventHandler()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/SizeChangeEventHandler

**Contents:**
- Type alias: SizeChangeEventHandler()
- Parameters​
- Returns​

SizeChangeEventHandler: (width, height) => void

A custom function used to handle changes to the time scale's size.

---

## Enumeration: LineStyle

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/enumerations/LineStyle

**Contents:**
- Enumeration: LineStyle
- Enumeration Members​
  - Solid​
  - Dotted​
  - Dashed​
  - LargeDashed​
  - SparseDotted​

Represents the possible line styles.

A dashed line with bigger dashes.

A dotted line with more space between dots.

---

## Type alias: LogicalRange

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/LogicalRange

**Contents:**
- Type alias: LogicalRange

LogicalRange: IRange <Logical>

A logical range is an object with 2 properties: from and to, which are numbers and represent logical indexes on the time scale.

The starting point of the time scale's logical range is the first data item among all series. Before that point all indexes are negative, starting from that point - positive.

Indexes might have fractional parts, for instance 4.2, due to the time-scale being continuous rather than discrete.

Integer part of the logical index means index of the fully visible bar. Thus, if we have 5.2 as the last visible logical index (to field), that means that the last visible bar has index 5, but we also have partially visible (for 20%) 6th bar. Half (e.g. 1.5, 3.5, 10.5) means exactly a middle of the bar.

---

## Type alias: IImageWatermarkPluginApi<T>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/IImageWatermarkPluginApi

**Contents:**
- Type alias: IImageWatermarkPluginApi<T>
- Type parameters​

IImageWatermarkPluginApi<T>: PrimitiveHasApplyOptions <IPanePrimitiveWrapper<T, ImageWatermarkOptions>>

---

## Variable: HistogramSeries

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/variables/HistogramSeries

**Contents:**
- Variable: HistogramSeries

const HistogramSeries: SeriesDefinition<"Histogram">

---

## Interface: IPanePrimitiveBase<TPaneAttachedParameters>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/IPanePrimitiveBase

**Contents:**
- Interface: IPanePrimitiveBase<TPaneAttachedParameters>
- Type parameters​
- Methods​
  - updateAllViews()?​
    - Returns​
  - paneViews()?​
    - Returns​
  - attached()?​
    - Parameters​
    - Returns​

Base interface for series primitives. It must be implemented to add some external graphics to series

• TPaneAttachedParameters = unknown

optional updateAllViews(): void

This method is called when viewport has been changed, so primitive have to recalculate / invalidate its data

optional paneViews(): readonly IPanePrimitivePaneView[]

Returns array of objects representing primitive in the main area of the chart

readonly IPanePrimitivePaneView[]

array of objects; each of then must implement IPrimitivePaneView interface

For performance reasons, the lightweight library uses internal caches based on references to arrays So, this method must return new array if set of views has changed and should try to return the same array if nothing changed

optional attached(param): void

Attached Lifecycle hook.

• param: TPaneAttachedParameters

An object containing useful references for the attached primitive to use.

optional detached(): void

Detached Lifecycle hook.

optional hitTest(x, y): PrimitiveHoveredItem

Hit test method which will be called by the library when the cursor is moved. Use this to register object ids being hovered for use within the crosshairMoved and click events emitted by the chart. Additionally, the hit test result can specify a preferred cursor type to display for the main chart pane. This method should return the top most hit for this primitive if more than one object is being intersected.

x Coordinate of mouse event

y Coordinate of mouse event

---

## Interface: BarsInfo<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/BarsInfo

**Contents:**
- Interface: BarsInfo<HorzScaleItem>
- Extends​
- Type parameters​
- Properties​
  - barsBefore​
  - barsAfter​
  - from?​
    - Inherited from​
  - to?​
    - Inherited from​

Represents a range of bars and the number of bars outside the range.

The number of bars before the start of the range. Positive value means that there are some bars before (out of logical range from the left) the IRange.from logical index in the series. Negative value means that the first series' bar is inside the passed logical range, and between the first series' bar and the IRange.from logical index are some bars.

The number of bars after the end of the range. Positive value in the barsAfter field means that there are some bars after (out of logical range from the right) the IRange.to logical index in the series. Negative value means that the last series' bar is inside the passed logical range, and between the last series' bar and the IRange.to logical index are some bars.

optional from: HorzScaleItem

The from value. The start of the range.

optional to: HorzScaleItem

The to value. The end of the range.

---

## Interface: ISeriesPrimitiveWrapper<T, Options>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/ISeriesPrimitiveWrapper

**Contents:**
- Interface: ISeriesPrimitiveWrapper<T, Options>
- Extended by​
- Type parameters​
- Properties​
  - detach()​
    - Returns​
  - getSeries()​
    - Returns​
  - applyOptions()?​
    - Parameters​

Interface for a series primitive.

Detaches the plugin from the series.

getSeries: () => ISeriesApi<keyof SeriesOptionsMap, T, AreaData<T> | WhitespaceData<T> | BarData<T> | CandlestickData<T> | BaselineData<T> | LineData<T> | HistogramData<T> | CustomData<T> | CustomSeriesWhitespaceData<T>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>

Returns the current series.

ISeriesApi<keyof SeriesOptionsMap, T, AreaData<T> | WhitespaceData<T> | BarData<T> | CandlestickData<T> | BaselineData<T> | LineData<T> | HistogramData<T> | CustomData<T> | CustomSeriesWhitespaceData<T>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>

optional applyOptions: (options) => void

Applies options to the primitive.

• options: DeepPartial<Options>

Options to apply. The options are deeply merged with the current options.

---

## Type alias: SeriesMarkerPricePosition

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/SeriesMarkerPricePosition

**Contents:**
- Type alias: SeriesMarkerPricePosition

SeriesMarkerPricePosition: "atPriceTop" | "atPriceBottom" | "atPriceMiddle"

Represents the position of a series marker relative to a specific price.

The price value should be specified in the SeriesMarker.price

---

## Function: createUpDownMarkers()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/functions/createUpDownMarkers

**Contents:**
- Function: createUpDownMarkers()
- Type parameters​
- Parameters​
- Returns​
- Example​

createUpDownMarkers<T>(series, options?): ISeriesUpDownMarkerPluginApi<T>

Creates and attaches the Series Up Down Markers Plugin.

• series: ISeriesApi<keyof SeriesOptionsMap, T, AreaData<T> | WhitespaceData<T> | BarData<T> | CandlestickData<T> | BaselineData<T> | LineData<T> | HistogramData<T> | CustomData<T> | CustomSeriesWhitespaceData<T>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>

Series to which attach the Up Down Markers Plugin

• options?: Partial <UpDownMarkersPluginOptions>

options for the Up Down Markers Plugin

ISeriesUpDownMarkerPluginApi<T>

Api for Series Up Down Marker Plugin. ISeriesUpDownMarkerPluginApi

**Examples:**

Example 1 (sql):
```sql
import { createUpDownMarkers, createChart, LineSeries } from 'lightweight-charts';const chart = createChart('container');const lineSeries = chart.addSeries(LineSeries);const upDownMarkers = createUpDownMarkers(lineSeries, {    positiveColor: '#22AB94',    negativeColor: '#F7525F',    updateVisibilityDuration: 5000,});// to add some dataupDownMarkers.setData(    [        { time: '2020-02-02', value: 12.34 },        //... more line series data    ]);// ... Update some valuesupDownMarkers.update({ time: '2020-02-02', value: 13.54 }, true);// to remove plugin from the seriesupDownMarkers.detach();
```

---

## Enumeration: ColorType

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/enumerations/ColorType

**Contents:**
- Enumeration: ColorType
- Enumeration Members​
  - Solid​
  - VerticalGradient​

Represents a type of color.

VerticalGradient: "gradient"

Vertical gradient color

---

## Interface: AxisDoubleClickOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/AxisDoubleClickOptions

**Contents:**
- Interface: AxisDoubleClickOptions
- Properties​
  - time​
    - Default Value​
  - price​
    - Default Value​

Represents options for how the time and price axes react to mouse double click.

Enable resetting scaling the time axis by double-clicking the left mouse button.

Enable reseting scaling the price axis by by double-clicking the left mouse button.

---

## Interface: SingleValueData<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/SingleValueData

**Contents:**
- Interface: SingleValueData<HorzScaleItem>
- Extends​
- Extended by​
- Type parameters​
- Properties​
  - time​
    - Overrides​
  - value​
  - customValues?​
    - Inherited from​

A base interface for a data point of single-value series.

• HorzScaleItem = Time

The time of the data.

WhitespaceData . time

Price value of the data.

optional customValues: Record<string, unknown>

Additional custom values which will be ignored by the library, but could be used by plugins.

WhitespaceData . customValues

---

## Type alias: SeriesMarkerPosition

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/SeriesMarkerPosition

**Contents:**
- Type alias: SeriesMarkerPosition

SeriesMarkerPosition: SeriesMarkerBarPosition | SeriesMarkerPricePosition

Represents the position of a series marker relative to a bar.

---

## Interface: PriceChartLocalizationOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/PriceChartLocalizationOptions

**Contents:**
- Interface: PriceChartLocalizationOptions
- Extends​
- Properties​
  - timeFormatter?​
    - Default Value​
    - Inherited from​
  - dateFormat​
    - Default Value​
    - Inherited from​
  - locale​

Extends LocalizationOptions for price-based charts. Includes settings specific to formatting price values on the horizontal scale.

optional timeFormatter: TimeFormatterFn<number>

Override formatting of the time scale crosshair label.

LocalizationOptions . timeFormatter

Date formatting string.

Can contain yyyy, yy, MMMM, MMM, MM and dd literals which will be replaced with corresponding date's value.

Ignored if timeFormatter has been specified.

LocalizationOptions . dateFormat

Current locale used to format dates. Uses the browser's language settings by default.

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl#Locale_identification_and_negotiation

LocalizationOptions . locale

optional priceFormatter: PriceFormatterFn

Override formatting of the price scale tick marks, labels and crosshair labels. Can be used for cases that can't be covered with built-in price formats.

LocalizationOptions . priceFormatter

optional tickmarksPriceFormatter: TickmarksPriceFormatterFn

Overrides the formatting of price scale tick marks. Use this to define formatting rules based on all provided price values.

LocalizationOptions . tickmarksPriceFormatter

optional percentageFormatter: PercentageFormatterFn

Overrides the formatting of percentage scale tick marks.

LocalizationOptions . percentageFormatter

optional tickmarksPercentageFormatter: TickmarksPercentageFormatterFn

Override formatting of the percentage scale tick marks. Can be used if formatting should be adjusted based on all the values being formatted

LocalizationOptions . tickmarksPercentageFormatter

The number of decimal places to display for price values on the horizontal scale.

---

## Interface: AreaStyleOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/AreaStyleOptions

**Contents:**
- Interface: AreaStyleOptions
- Properties​
  - topColor​
    - Default Value​
  - bottomColor​
    - Default Value​
  - relativeGradient​
    - Default Value​
  - invertFilledArea​
    - Default Value​

Represents style options for an area series.

Color of the top part of the area.

'rgba( 46, 220, 135, 0.4)'

Color of the bottom part of the area.

'rgba( 40, 221, 100, 0)'

relativeGradient: boolean

Gradient is relative to the base value and the currently visible range. If it is false, the gradient is relative to the top and bottom of the chart.

invertFilledArea: boolean

Invert the filled area. Fills the area above the line if set to true.

Line width in pixels.

pointMarkersVisible: boolean

Show circle markers on each point.

optional pointMarkersRadius: number

Circle markers radius in pixels.

crosshairMarkerVisible: boolean

Show the crosshair marker.

crosshairMarkerRadius: number

Crosshair marker radius in pixels.

crosshairMarkerBorderColor: string

Crosshair marker border color. An empty string falls back to the color of the series under the crosshair.

crosshairMarkerBackgroundColor: string

The crosshair marker background color. An empty string falls back to the color of the series under the crosshair.

crosshairMarkerBorderWidth: number

Crosshair marker border width in pixels.

lastPriceAnimation: LastPriceAnimationMode

Last price animation mode.

**Examples:**

Example 1 (json):
```json
{@link LineStyle.Solid}
```

Example 2 (json):
```json
{@link LineType.Simple}
```

Example 3 (json):
```json
{@link LastPriceAnimationMode.Disabled}
```

---

## Type alias: CreatePriceLineOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/CreatePriceLineOptions

**Contents:**
- Type alias: CreatePriceLineOptions

CreatePriceLineOptions: Partial <PriceLineOptions> & Pick <PriceLineOptions, "price">

Price line options for the ISeriesApi.createPriceLine method.

price is required, while the rest of the options are optional.

---

## Interface: HorzScaleOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/HorzScaleOptions

**Contents:**
- Interface: HorzScaleOptions
- Extended by​
- Properties​
  - rightOffset​
    - Default Value​
  - rightOffsetPixels?​
    - Default Value​
  - barSpacing​
    - Default Value​
  - minBarSpacing​

Options for the time scale; the horizontal scale at the bottom of the chart that displays the time of data.

The margin space in bars from the right side of the chart.

optional rightOffsetPixels: number

The margin space in pixels from the right side of the chart. This option has priority over rightOffset.

The space between bars in pixels.

minBarSpacing: number

The minimum space between bars in pixels.

maxBarSpacing: number

The maximum space between bars in pixels.

Has no effect if value is set to 0.

Prevent scrolling to the left of the first bar.

fixRightEdge: boolean

Prevent scrolling to the right of the most recent bar.

lockVisibleTimeRangeOnResize: boolean

Prevent changing the visible time range during chart resizing.

rightBarStaysOnScroll: boolean

Prevent the hovered bar from moving when scrolling.

borderVisible: boolean

Show the time scale border.

The time scale border color.

Show the time, not just the date, in the time scale and vertical crosshair label.

secondsVisible: boolean

Show seconds in the time scale and vertical crosshair label in hh:mm:ss format for intraday data.

shiftVisibleRangeOnNewBar: boolean

Shift the visible range to the right (into the future) by the number of new bars when new data is added.

Note that this only applies when the last bar is visible.

allowShiftVisibleRangeOnWhitespaceReplacement: boolean

Allow the visible range to be shifted to the right when a new bar is added which is replacing an existing whitespace time point on the chart.

Note that this only applies when the last bar is visible & shiftVisibleRangeOnNewBar is enabled.

ticksVisible: boolean

Draw small vertical line on time axis labels.

optional tickMarkMaxCharacterLength: number

Maximum tick mark label length. Used to override the default 8 character maximum length.

uniformDistribution: boolean

Changes horizontal scale marks generation. With this flag equal to true, marks of the same weight are either all drawn or none are drawn at all.

minimumHeight: number

Define a minimum height for the time scale. Note: This value will be exceeded if the time scale needs more space to display it's contents.

Setting a minimum height could be useful for ensuring that multiple charts positioned in a horizontal stack each have an identical time scale height, or for plugins which require a bit more space within the time scale pane.

allowBoldLabels: boolean

Allow major time scale labels to be rendered in a bolder font weight.

ignoreWhitespaceIndices: boolean

Ignore time scale points containing only whitespace (for all series) when drawing grid lines, tick marks, and snapping the crosshair to time scale points.

For the yield curve chart type it defaults to true.

enableConflation: boolean

Enable data conflation for performance optimization when bar spacing is very small. When enabled, multiple data points are automatically combined into single points when they would be rendered in less than 0.5 pixels of screen space. This significantly improves rendering performance for large datasets when zoomed out.

optional conflationThresholdFactor: number

Smoothing factor for conflation thresholds. Controls how aggressively conflation is applied. This can be used to create smoother-looking charts, especially useful for sparklines and small charts.

Higher values result in fewer data points being displayed, creating smoother but less detailed charts. This is particularly useful for sparklines and small charts where smooth appearance is prioritized over showing every data point.

Note: Should be used with continuous series types (line, area, baseline) for best visual results. Candlestick and bar series may look less natural with high smoothing factors.

precomputeConflationOnInit: boolean

Precompute conflation chunks for common levels right after data load. When enabled, the system will precompute conflation data in the background, which improves performance when zooming out but increases initial load time and memory usage.

Recommended for: Large datasets (>10K points) on machines with sufficient memory

precomputeConflationPriority: "background" | "user-visible" | "user-blocking"

Priority used for background precompute tasks when the Prioritized Task Scheduling API is available.

Recommendation: Use 'background' for most cases to avoid impacting user experience. Only use higher priorities if conflation is critical for your application's functionality.

**Examples:**

Example 1 (unknown):
```unknown
'background'
```

---

## Type alias: PrimitiveHasApplyOptions<T>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/PrimitiveHasApplyOptions

**Contents:**
- Type alias: PrimitiveHasApplyOptions<T>
- Type parameters​

PrimitiveHasApplyOptions<T>: T & Required<Pick<T, "applyOptions">>

Primitive has applyOptions as a method for adjusting the options after creation.

---

## Interface: IPrimitivePaneRenderer

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/IPrimitivePaneRenderer

**Contents:**
- Interface: IPrimitivePaneRenderer
- Methods​
  - draw()​
    - Parameters​
    - Returns​
  - drawBackground()?​
    - Parameters​
    - Returns​

This interface represents rendering some element on the canvas

draw(target, utils?): void

Method to draw main content of the element

• target: CanvasRenderingTarget2D

canvas context to draw on, refer to FancyCanvas library for more details about this class

• utils?: DrawingUtils

exposes drawing utilities (such as setLineStyle) from the library to plugins

optional drawBackground(target, utils?): void

Optional method to draw the background. Some elements could implement this method to draw on the background of the chart. Usually this is some kind of watermarks or time areas highlighting.

• target: CanvasRenderingTarget2D

canvas context to draw on, refer FancyCanvas library for more details about this class

• utils?: DrawingUtils

exposes drawing utilities (such as setLineStyle) from the library to plugins

---

## Interface: PriceScaleOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/PriceScaleOptions

**Contents:**
- Interface: PriceScaleOptions
- Properties​
  - autoScale​
    - Default Value​
  - mode​
    - Default Value​
  - invertScale​
    - Default Value​
  - alignLabels​
    - Default Value​

Structure that describes price scale options

Autoscaling is a feature that automatically adjusts a price scale to fit the visible range of data. Note that overlay price scales are always auto-scaled.

Invert the price scale, so that a upwards trend is shown as a downwards trend and vice versa. Affects both the price scale and the data on the chart.

Align price scale labels to prevent them from overlapping.

scaleMargins: PriceScaleMargins

{ bottom: 0.1, top: 0.2 }

borderVisible: boolean

Set true to draw a border between the price scale and the chart area.

Price scale border color.

optional textColor: string

Price scale text color. If not provided LayoutOptions.textColor is used.

entireTextOnly: boolean

Show top and bottom corner labels only if entire text is visible.

Indicates if this price scale visible. Ignored by overlay price scales.

true for the right price scale and false for the left. For the yield curve chart, the default is for the left scale to be visible.

ticksVisible: boolean

Draw small horizontal line on price axis labels.

Define a minimum width for the price scale. Note: This value will be exceeded if the price scale needs more space to display it's contents.

Setting a minimum width could be useful for ensuring that multiple charts positioned in a vertical stack each have an identical price scale width, or for plugins which require a bit more space within the price scale pane.

ensureEdgeTickMarksVisible: boolean

Ensures that tick marks are always visible at the very top and bottom of the price scale, regardless of the data range. When enabled, a tick mark will be drawn at both edges of the scale, providing clear boundary indicators.

**Examples:**

Example 1 (json):
```json
{@link PriceScaleMode.Normal}
```

Example 2 (css):
```css
chart.priceScale('right').applyOptions({    scaleMargins: {        top: 0.8,        bottom: 0,    },});
```

---

## Type alias: HistogramSeriesOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/HistogramSeriesOptions

**Contents:**
- Type alias: HistogramSeriesOptions

HistogramSeriesOptions: SeriesOptions <HistogramStyleOptions>

Represents histogram series options.

---

## Interface: MouseEventParams<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/MouseEventParams

**Contents:**
- Interface: MouseEventParams<HorzScaleItem>
- Type parameters​
- Properties​
  - time?​
  - logical?​
  - point?​
  - paneIndex?​
  - seriesData​
  - hoveredSeries?​
  - hoveredObjectId?​

Represents a mouse event.

• HorzScaleItem = Time

optional time: HorzScaleItem

Time of the data at the location of the mouse event.

The value will be undefined if the location of the event in the chart is outside the range of available data.

optional logical: Logical

optional point: Point

Location of the event in the chart.

The value will be undefined if the event is fired outside the chart, for example a mouse leave event.

optional paneIndex: number

The index of the Pane

seriesData: Map <ISeriesApi<keyof SeriesOptionsMap, HorzScaleItem, AreaData<HorzScaleItem> | WhitespaceData<HorzScaleItem> | BarData<HorzScaleItem> | CandlestickData<HorzScaleItem> | BaselineData<HorzScaleItem> | LineData<HorzScaleItem> | HistogramData<HorzScaleItem> | CustomData<HorzScaleItem> | CustomSeriesWhitespaceData<HorzScaleItem>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>, BarData<HorzScaleItem> | LineData<HorzScaleItem> | HistogramData<HorzScaleItem> | CustomData<HorzScaleItem>>

Data of all series at the location of the event in the chart.

Keys of the map are ISeriesApi instances. Values are prices. Values of the map are original data items

optional hoveredSeries: ISeriesApi<keyof SeriesOptionsMap, HorzScaleItem, AreaData<HorzScaleItem> | WhitespaceData<HorzScaleItem> | BarData<HorzScaleItem> | CandlestickData<HorzScaleItem> | BaselineData<HorzScaleItem> | LineData<HorzScaleItem> | HistogramData<HorzScaleItem> | CustomData<HorzScaleItem> | CustomSeriesWhitespaceData<HorzScaleItem>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>

The ISeriesApi for the series at the point of the mouse event.

optional hoveredObjectId: unknown

The ID of the object at the point of the mouse event.

optional sourceEvent: TouchMouseEventData

The underlying source mouse or touch event data, if available

---

## Interface: AxisPressedMouseMoveOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/AxisPressedMouseMoveOptions

**Contents:**
- Interface: AxisPressedMouseMoveOptions
- Properties​
  - time​
    - Default Value​
  - price​
    - Default Value​

Represents options for how the time and price axes react to mouse movements.

Enable scaling the time axis by holding down the left mouse button and moving the mouse.

Enable scaling the price axis by holding down the left mouse button and moving the mouse.

---

## Function: version()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/functions/version

**Contents:**
- Function: version()
- Returns​

Returns the current version as a string. For example '3.3.0'.

---

## Type alias: CandlestickSeriesPartialOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/CandlestickSeriesPartialOptions

**Contents:**
- Type alias: CandlestickSeriesPartialOptions

CandlestickSeriesPartialOptions: SeriesPartialOptions <CandlestickStyleOptions>

Represents candlestick series options where all properties are optional.

---

## Type alias: LogicalRangeChangeEventHandler()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/LogicalRangeChangeEventHandler

**Contents:**
- Type alias: LogicalRangeChangeEventHandler()
- Parameters​
- Returns​

LogicalRangeChangeEventHandler: (logicalRange) => void

A custom function used to handle changes to the time scale's logical range.

• logicalRange: LogicalRange | null

---

## Interface: ISeriesMarkersPluginApi<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/ISeriesMarkersPluginApi

**Contents:**
- Interface: ISeriesMarkersPluginApi<HorzScaleItem>
- Extends​
- Type parameters​
- Properties​
  - setMarkers()​
    - Parameters​
    - Returns​
  - markers()​
    - Returns​
  - detach()​

Interface for a series markers plugin

setMarkers: (markers) => void

Set markers to the series.

• markers: SeriesMarker<HorzScaleItem>[]

An array of markers to be displayed on the series.

markers: () => readonly SeriesMarker<HorzScaleItem>[]

Returns current markers.

readonly SeriesMarker<HorzScaleItem>[]

Detaches the plugin from the series.

ISeriesPrimitiveWrapper . detach

getSeries: () => ISeriesApi<keyof SeriesOptionsMap, HorzScaleItem, AreaData<HorzScaleItem> | WhitespaceData<HorzScaleItem> | BarData<HorzScaleItem> | CandlestickData<HorzScaleItem> | BaselineData<HorzScaleItem> | LineData<HorzScaleItem> | HistogramData<HorzScaleItem> | CustomData<HorzScaleItem> | CustomSeriesWhitespaceData<HorzScaleItem>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>

Returns the current series.

ISeriesApi<keyof SeriesOptionsMap, HorzScaleItem, AreaData<HorzScaleItem> | WhitespaceData<HorzScaleItem> | BarData<HorzScaleItem> | CandlestickData<HorzScaleItem> | BaselineData<HorzScaleItem> | LineData<HorzScaleItem> | HistogramData<HorzScaleItem> | CustomData<HorzScaleItem> | CustomSeriesWhitespaceData<HorzScaleItem>, CustomSeriesOptions | AreaSeriesOptions | BarSeriesOptions | CandlestickSeriesOptions | BaselineSeriesOptions | LineSeriesOptions | HistogramSeriesOptions, DeepPartial <AreaStyleOptions & SeriesOptionsCommon> | DeepPartial <BarStyleOptions & SeriesOptionsCommon> | DeepPartial <CandlestickStyleOptions & SeriesOptionsCommon> | DeepPartial <BaselineStyleOptions & SeriesOptionsCommon> | DeepPartial <LineStyleOptions & SeriesOptionsCommon> | DeepPartial <HistogramStyleOptions & SeriesOptionsCommon> | DeepPartial <CustomStyleOptions & SeriesOptionsCommon>>

ISeriesPrimitiveWrapper . getSeries

optional applyOptions: (options) => void

Applies options to the primitive.

• options: DeepPartial<unknown>

Options to apply. The options are deeply merged with the current options.

ISeriesPrimitiveWrapper . applyOptions

---

## Interface: BarData<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/BarData

**Contents:**
- Interface: BarData<HorzScaleItem>
- Extends​
- Type parameters​
- Properties​
  - color?​
  - time​
    - Inherited from​
  - open​
    - Inherited from​
  - high​

Structure describing a single item of data for bar series

• HorzScaleItem = Time

optional color: string

Optional color value for certain data item. If missed, color from options is used

optional customValues: Record<string, unknown>

Additional custom values which will be ignored by the library, but could be used by plugins.

OhlcData . customValues

---

## Type alias: TimePointIndex

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/TimePointIndex

**Contents:**
- Type alias: TimePointIndex

TimePointIndex: Nominal<number, "TimePointIndex">

Index for a point on the horizontal (time) scale.

---

## Interface: PaneAttachedParameter<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/PaneAttachedParameter

**Contents:**
- Interface: PaneAttachedParameter<HorzScaleItem>
- Type parameters​
- Properties​
  - chart​
  - requestUpdate()​
    - Returns​

Object containing references to the chart instance, and a requestUpdate method for triggering a refresh of the chart.

• HorzScaleItem = Time

chart: IChartApiBase<HorzScaleItem>

requestUpdate: () => void

Request an update (redraw the chart)

---

## Function: createImageWatermark()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/functions/createImageWatermark

**Contents:**
- Function: createImageWatermark()
- Type parameters​
- Parameters​
- Returns​
- Example​

createImageWatermark<T>(pane, imageUrl, options): IImageWatermarkPluginApi<T>

Creates an image watermark.

• options: DeepPartial <ImageWatermarkOptions>

IImageWatermarkPluginApi<T>

Image watermark wrapper.

**Examples:**

Example 1 (sql):
```sql
import { createImageWatermark } from 'lightweight-charts';const firstPane = chart.panes()[0];const imageWatermark = createImageWatermark(firstPane, '/images/my-image.png', {  alpha: 0.5,  padding: 20,});// to change optionsimageWatermark.applyOptions({ padding: 10 });// to remove watermark from the paneimageWatermark.detach();
```

---

## Interface: PriceChartOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/PriceChartOptions

**Contents:**
- Interface: PriceChartOptions
- Extends​
- Properties​
  - width​
    - Default Value​
    - Inherited from​
  - height​
    - Default Value​
    - Inherited from​
  - autoSize​

Configuration options specific to price-based charts. Extends the base chart options and includes localization settings for price formatting.

Width of the chart in pixels

If 0 (default) or none value provided, then a size of the widget will be calculated based its container's size.

ChartOptionsImpl . width

Height of the chart in pixels

If 0 (default) or none value provided, then a size of the widget will be calculated based its container's size.

ChartOptionsImpl . height

Setting this flag to true will make the chart watch the chart container's size and automatically resize the chart to fit its container whenever the size changes.

This feature requires ResizeObserver class to be available in the global scope. Note that calling code is responsible for providing a polyfill if required. If the global scope does not have ResizeObserver, a warning will appear and the flag will be ignored.

Please pay attention that autoSize option and explicit sizes options width and height don't conflict with one another. If you specify autoSize flag, then width and height options will be ignored unless ResizeObserver has failed. If it fails then the values will be used as fallback.

The flag autoSize could also be set with and unset with applyOptions function.

ChartOptionsImpl . autoSize

layout: LayoutOptions

ChartOptionsImpl . layout

leftPriceScale: PriceScaleOptions

Left price scale options

ChartOptionsImpl . leftPriceScale

rightPriceScale: PriceScaleOptions

Right price scale options

ChartOptionsImpl . rightPriceScale

overlayPriceScales: OverlayPriceScaleOptions

Overlay price scale options

ChartOptionsImpl . overlayPriceScales

timeScale: HorzScaleOptions

ChartOptionsImpl . timeScale

crosshair: CrosshairOptions

The crosshair shows the intersection of the price and time scale values at any point on the chart.

ChartOptionsImpl . crosshair

A grid is represented in the chart background as a vertical and horizontal lines drawn at the levels of visible marks of price and the time scales.

ChartOptionsImpl . grid

handleScroll: boolean | HandleScrollOptions

Scroll options, or a boolean flag that enables/disables scrolling

ChartOptionsImpl . handleScroll

handleScale: boolean | HandleScaleOptions

Scale options, or a boolean flag that enables/disables scaling

ChartOptionsImpl . handleScale

kineticScroll: KineticScrollOptions

Kinetic scroll options

ChartOptionsImpl . kineticScroll

trackingMode: TrackingModeOptions

Represent options for the tracking mode's behavior.

Mobile users will not have the ability to see the values/dates like they do on desktop. To see it, they should enter the tracking mode. The tracking mode will deactivate the scrolling and make it possible to check values and dates.

ChartOptionsImpl . trackingMode

addDefaultPane: boolean

Whether to add a default pane to the chart Disable this option when you want to create a chart with no panes and add them manually

ChartOptionsImpl . addDefaultPane

localization: PriceChartLocalizationOptions

Localization options for formatting price values and other chart elements.

ChartOptionsImpl . localization

**Examples:**

Example 1 (css):
```css
const chart = LightweightCharts.createChart(document.body, {    autoSize: true,});
```

---

## Enumeration: PriceLineSource

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/enumerations/PriceLineSource

**Contents:**
- Enumeration: PriceLineSource
- Enumeration Members​
  - LastBar​
  - LastVisible​

Represents the source of data to be used for the horizontal price line.

Use the last bar data.

Use the last visible data of the chart viewport.

---

## Type alias: BaseValueType

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/BaseValueType

**Contents:**
- Type alias: BaseValueType

BaseValueType: BaseValuePrice

Represents a type of a base value of baseline series type.

---

## Interface: LineData<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/LineData

**Contents:**
- Interface: LineData<HorzScaleItem>
- Extends​
- Type parameters​
- Properties​
  - color?​
  - time​
    - Inherited from​
  - value​
    - Inherited from​
  - customValues?​

Structure describing a single item of data for line series

• HorzScaleItem = Time

optional color: string

Optional color value for certain data item. If missed, color from options is used

The time of the data.

SingleValueData . time

Price value of the data.

SingleValueData . value

optional customValues: Record<string, unknown>

Additional custom values which will be ignored by the library, but could be used by plugins.

SingleValueData . customValues

---

## Type alias: BarPrice

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/BarPrice

**Contents:**
- Type alias: BarPrice

BarPrice: Nominal<number, "BarPrice">

Represents a price as a number.

---

## Function: createTextWatermark()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/functions/createTextWatermark

**Contents:**
- Function: createTextWatermark()
- Type parameters​
- Parameters​
- Returns​
- Example​

createTextWatermark<T>(pane, options): ITextWatermarkPluginApi<T>

Creates an image watermark.

• options: DeepPartial <TextWatermarkOptions>

ITextWatermarkPluginApi<T>

Image watermark wrapper.

**Examples:**

Example 1 (sql):
```sql
import { createTextWatermark } from 'lightweight-charts';const firstPane = chart.panes()[0];const textWatermark = createTextWatermark(firstPane, {      horzAlign: 'center',      vertAlign: 'center',      lines: [        {          text: 'Hello',          color: 'rgba(255,0,0,0.5)',          fontSize: 100,          fontStyle: 'bold',        },        {          text: 'This is a text watermark',          color: 'rgba(0,0,255,0.5)',          fontSize: 50,          fontStyle: 'italic',          fontFamily: 'monospace',        },      ],});// to change optionstextWatermark.applyOptions({ horzAlign: 'left' });// to remove watermark from the panetextWatermark.detach();
```

---

## Type alias: PrimitivePaneViewZOrder

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/PrimitivePaneViewZOrder

**Contents:**
- Type alias: PrimitivePaneViewZOrder

PrimitivePaneViewZOrder: "bottom" | "normal" | "top"

Defines where in the visual layer stack the renderer should be executed.

---

## Type alias: BlueComponent

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/BlueComponent

**Contents:**
- Type alias: BlueComponent

BlueComponent: Nominal<number, "BlueComponent">

Blue component of the RGB color value The valid values are integers in range [0, 255]

---

## Type alias: BaselineSeriesOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/BaselineSeriesOptions

**Contents:**
- Type alias: BaselineSeriesOptions

BaselineSeriesOptions: SeriesOptions <BaselineStyleOptions>

Structure describing baseline series options.

---

## Type alias: LineWidth

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/LineWidth

**Contents:**
- Type alias: LineWidth

LineWidth: 1 | 2 | 3 | 4

Represents the width of a line.

---

## Interface: ImageWatermarkOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/ImageWatermarkOptions

**Contents:**
- Interface: ImageWatermarkOptions
- Properties​
  - maxWidth?​
    - Default Value​
  - maxHeight?​
    - Default Value​
  - padding​
    - Default Value​
  - alpha​
    - Default Value​

optional maxWidth: number

Maximum width for the image watermark.

optional maxHeight: number

Maximum height for the image watermark.

Padding to maintain around the image watermark relative to the chart pane edges.

The alpha (opacity) for the image watermark. Where 1 is fully opaque (visible) and 0 is fully transparent.

---

## Interface: ISeriesPrimitiveAxisView

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/ISeriesPrimitiveAxisView

**Contents:**
- Interface: ISeriesPrimitiveAxisView
- Methods​
  - coordinate()​
    - Returns​
  - fixedCoordinate()?​
    - Returns​
  - text()​
    - Returns​
  - textColor()​
    - Returns​

This interface represents a label on the price or time axis

The desired coordinate for the label. Note that the label will be automatically moved to prevent overlapping with other labels. If you would like the label to be drawn at the exact coordinate under all circumstances then rather use fixedCoordinate. For a price axis the value returned will represent the vertical distance (pixels) from the top. For a time axis the value will represent the horizontal distance from the left.

coordinate. distance from top for price axis, or distance from left for time axis.

optional fixedCoordinate(): number

fixed coordinate of the label. A label with a fixed coordinate value will always be drawn at the specified coordinate and will appear above any 'unfixed' labels. If you supply a fixed coordinate then you should return a large negative number for coordinate so that the automatic placement of unfixed labels doesn't leave a blank space for this label. For a price axis the value returned will represent the vertical distance (pixels) from the top. For a time axis the value will represent the horizontal distance from the left.

coordinate. distance from top for price axis, or distance from left for time axis.

text color of the label

background color of the label

optional visible(): boolean

whether the label should be visible (default: true)

optional tickVisible(): boolean

whether the tick mark line should be visible (default: true)

---

## Interface: AreaData<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/AreaData

**Contents:**
- Interface: AreaData<HorzScaleItem>
- Extends​
- Type parameters​
- Properties​
  - lineColor?​
  - topColor?​
  - bottomColor?​
  - time​
    - Inherited from​
  - value​

Structure describing a single item of data for area series

• HorzScaleItem = Time

optional lineColor: string

Optional line color value for certain data item. If missed, color from options is used

optional topColor: string

Optional top color value for certain data item. If missed, color from options is used

optional bottomColor: string

Optional bottom color value for certain data item. If missed, color from options is used

The time of the data.

SingleValueData . time

Price value of the data.

SingleValueData . value

optional customValues: Record<string, unknown>

Additional custom values which will be ignored by the library, but could be used by plugins.

SingleValueData . customValues

---

## Type alias: Logical

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/Logical

**Contents:**
- Type alias: Logical

Logical: Nominal<number, "Logical">

Represents the to or from number in a logical range.

---

## Type alias: OverlayPriceScaleOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/OverlayPriceScaleOptions

**Contents:**
- Type alias: OverlayPriceScaleOptions

OverlayPriceScaleOptions: Omit <PriceScaleOptions, "visible" | "autoScale">

Represents overlay price scale options.

---

## Interface: IPanePrimitiveWrapper<T, Options>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/IPanePrimitiveWrapper

**Contents:**
- Interface: IPanePrimitiveWrapper<T, Options>
- Type parameters​
- Properties​
  - detach()​
    - Returns​
  - getPane()​
    - Returns​
  - applyOptions()?​
    - Parameters​
    - Returns​

Interface for a pane primitive.

Detaches the plugin from the pane.

getPane: () => IPaneApi<T>

Returns the current pane.

optional applyOptions: (options) => void

Applies options to the primitive.

• options: DeepPartial<Options>

Options to apply. The options are deeply merged with the current options.

---

## Type alias: CustomSeriesOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/CustomSeriesOptions

**Contents:**
- Type alias: CustomSeriesOptions

CustomSeriesOptions: SeriesOptions <CustomStyleOptions>

Represents a custom series options.

---

## Interface: LayoutOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/LayoutOptions

**Contents:**
- Interface: LayoutOptions
- Properties​
  - background​
    - Default Value​
  - textColor​
    - Default Value​
  - fontSize​
    - Default Value​
  - fontFamily​
    - Default Value​

Represents layout options

background: Background

Chart and scales background color.

{ type: ColorType.Solid, color: '#FFFFFF' }

Color of text on the scales.

Font size of text on scales in pixels.

Font family of text on the scales.

-apple-system, BlinkMacSystemFont, 'Trebuchet MS', Roboto, Ubuntu, sans-serif

panes: LayoutPanesOptions

{ enableResize: true, separatorColor: '#2B2B43', separatorHoverColor: 'rgba(178, 181, 189, 0.2)'}

attributionLogo: boolean

Display the TradingView attribution logo on the main chart pane.

The licence for library specifies that you add the "attribution notice" from the NOTICE file to your code and a link to https://www.tradingview.com/ to the page of your website or mobile application that is available to your users. Using this attribution logo is sufficient for meeting this linking requirement. However, if you already fulfill this requirement then you can disable this attribution logo.

colorSpace: ColorSpace

Specifies the color space of the rendering context for the internal canvas elements.

Note: this option should only be specified during the chart creation and not changed at a later stage by using applyOptions.

See HTMLCanvasElement: getContext() method - Web APIs | MDN for more info

colorParsers: CustomColorParser[]

Array of custom color parser functions to handle color formats outside of standard sRGB values. Each parser function takes a string input and should return either:

Parsers are tried in order until one returns a non-null result. This allows chaining multiple parsers to handle different color space formats.

Note: this option should only be specified during the chart creation and not changed at a later stage by using applyOptions.

The library already supports these color formats by default:

Custom parsers are only needed for other color spaces like:

---

## Function: defaultHorzScaleBehavior()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/functions/defaultHorzScaleBehavior

**Contents:**
- Function: defaultHorzScaleBehavior()
- Returns​
  - Returns​

defaultHorzScaleBehavior(): () => IHorzScaleBehavior <Time>

Provides the default implementation of the horizontal scale (time-based) that can be used as a base for extending the horizontal scale with custom behavior. This allows for the introduction of custom functionality without re-implementing the entire IHorzScaleBehavior<Time> interface.

For further details, refer to the createChartEx chart constructor method.

An uninitialized class implementing the IHorzScaleBehavior<Time> interface

IHorzScaleBehavior <Time>

---

## Interface: LocalizationOptionsBase

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/LocalizationOptionsBase

**Contents:**
- Interface: LocalizationOptionsBase
- Extended by​
- Properties​
  - locale​
    - See​
    - Default Value​
  - priceFormatter?​
    - See​
    - Default Value​
  - tickmarksPriceFormatter?​

Represents basic localization options

Current locale used to format dates. Uses the browser's language settings by default.

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl#Locale_identification_and_negotiation

optional priceFormatter: PriceFormatterFn

Override formatting of the price scale tick marks, labels and crosshair labels. Can be used for cases that can't be covered with built-in price formats.

optional tickmarksPriceFormatter: TickmarksPriceFormatterFn

Overrides the formatting of price scale tick marks. Use this to define formatting rules based on all provided price values.

optional percentageFormatter: PercentageFormatterFn

Overrides the formatting of percentage scale tick marks.

optional tickmarksPercentageFormatter: TickmarksPercentageFormatterFn

Override formatting of the percentage scale tick marks. Can be used if formatting should be adjusted based on all the values being formatted

---

## Interface: PrimitiveHoveredItem

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/PrimitiveHoveredItem

**Contents:**
- Interface: PrimitiveHoveredItem
- Properties​
  - cursorStyle?​
  - externalId​
  - zOrder​
  - isBackground?​

Data representing the currently hovered object from the Hit test.

optional cursorStyle: string

CSS cursor style as defined here: MDN: CSS Cursor or undefined if you want the library to use the default cursor style instead.

Hovered objects external ID. Can be used to identify the source item within a mouse subscriber event.

zOrder: PrimitivePaneViewZOrder

The zOrder of the hovered item.

optional isBackground: boolean

Set to true if the object is rendered using drawBackground instead of draw.

---

## Type alias: SeriesMarker<TimeType>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/SeriesMarker

**Contents:**
- Type alias: SeriesMarker<TimeType>
- Type parameters​

SeriesMarker<TimeType>: SeriesMarkerBar<TimeType> | SeriesMarkerPrice<TimeType>

Represents a series marker.

---

## Interface: SeriesOptionsMap

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/SeriesOptionsMap

**Contents:**
- Interface: SeriesOptionsMap
- Properties​
  - Bar​
  - Candlestick​
  - Area​
  - Baseline​
  - Line​
  - Histogram​
  - Custom​

Represents the type of options for each series type.

For example a bar series has options represented by BarSeriesOptions.

Bar: BarSeriesOptions

The type of bar series options.

Candlestick: CandlestickSeriesOptions

The type of candlestick series options.

Area: AreaSeriesOptions

The type of area series options.

Baseline: BaselineSeriesOptions

The type of baseline series options.

Line: LineSeriesOptions

The type of line series options.

Histogram: HistogramSeriesOptions

The type of histogram series options.

Custom: CustomSeriesOptions

The type of a custom series options.

---

## Type alias: ITextWatermarkPluginApi<T>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/ITextWatermarkPluginApi

**Contents:**
- Type alias: ITextWatermarkPluginApi<T>
- Type parameters​

ITextWatermarkPluginApi<T>: PrimitiveHasApplyOptions <IPanePrimitiveWrapper<T, TextWatermarkOptions>>

---

## Type alias: InternalHorzScaleItem

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/InternalHorzScaleItem

**Contents:**
- Type alias: InternalHorzScaleItem

InternalHorzScaleItem: Nominal<unknown, "InternalHorzScaleItem">

Internal Horizontal Scale Item

---

## Interface: ChartOptionsBase

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/ChartOptionsBase

**Contents:**
- Interface: ChartOptionsBase
- Extended by​
- Properties​
  - width​
    - Default Value​
  - height​
    - Default Value​
  - autoSize​
  - layout​
  - leftPriceScale​

Represents common chart options

Width of the chart in pixels

If 0 (default) or none value provided, then a size of the widget will be calculated based its container's size.

Height of the chart in pixels

If 0 (default) or none value provided, then a size of the widget will be calculated based its container's size.

Setting this flag to true will make the chart watch the chart container's size and automatically resize the chart to fit its container whenever the size changes.

This feature requires ResizeObserver class to be available in the global scope. Note that calling code is responsible for providing a polyfill if required. If the global scope does not have ResizeObserver, a warning will appear and the flag will be ignored.

Please pay attention that autoSize option and explicit sizes options width and height don't conflict with one another. If you specify autoSize flag, then width and height options will be ignored unless ResizeObserver has failed. If it fails then the values will be used as fallback.

The flag autoSize could also be set with and unset with applyOptions function.

layout: LayoutOptions

leftPriceScale: PriceScaleOptions

Left price scale options

rightPriceScale: PriceScaleOptions

Right price scale options

overlayPriceScales: OverlayPriceScaleOptions

Overlay price scale options

timeScale: HorzScaleOptions

crosshair: CrosshairOptions

The crosshair shows the intersection of the price and time scale values at any point on the chart.

A grid is represented in the chart background as a vertical and horizontal lines drawn at the levels of visible marks of price and the time scales.

handleScroll: boolean | HandleScrollOptions

Scroll options, or a boolean flag that enables/disables scrolling

handleScale: boolean | HandleScaleOptions

Scale options, or a boolean flag that enables/disables scaling

kineticScroll: KineticScrollOptions

Kinetic scroll options

trackingMode: TrackingModeOptions

Represent options for the tracking mode's behavior.

Mobile users will not have the ability to see the values/dates like they do on desktop. To see it, they should enter the tracking mode. The tracking mode will deactivate the scrolling and make it possible to check values and dates.

localization: LocalizationOptionsBase

Basic localization options

addDefaultPane: boolean

Whether to add a default pane to the chart Disable this option when you want to create a chart with no panes and add them manually

**Examples:**

Example 1 (css):
```css
const chart = LightweightCharts.createChart(document.body, {    autoSize: true,});
```

---

## Type alias: IPanePrimitive<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/IPanePrimitive

**Contents:**
- Type alias: IPanePrimitive<HorzScaleItem>
- Type parameters​

IPanePrimitive<HorzScaleItem>: IPanePrimitiveBase <PaneAttachedParameter<HorzScaleItem>>

Interface for pane primitives. It must be implemented to add some external graphics to a pane.

• HorzScaleItem = Time

---

## Interface: CandlestickData<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/CandlestickData

**Contents:**
- Interface: CandlestickData<HorzScaleItem>
- Extends​
- Type parameters​
- Properties​
  - color?​
  - borderColor?​
  - wickColor?​
  - time​
    - Inherited from​
  - open​

Structure describing a single item of data for candlestick series

• HorzScaleItem = Time

optional color: string

Optional color value for certain data item. If missed, color from options is used

optional borderColor: string

Optional border color value for certain data item. If missed, color from options is used

optional wickColor: string

Optional wick color value for certain data item. If missed, color from options is used

optional customValues: Record<string, unknown>

Additional custom values which will be ignored by the library, but could be used by plugins.

OhlcData . customValues

---

## Enumeration: TickMarkType

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/enumerations/TickMarkType

**Contents:**
- Enumeration: TickMarkType
- Enumeration Members​
  - Year​
  - Month​
  - DayOfMonth​
  - Time​
  - TimeWithSeconds​

Represents the type of a tick mark on the time axis.

The start of the year (e.g. it's the first tick mark in a year).

The start of the month (e.g. it's the first tick mark in a month).

A time without seconds.

---

## Function: isUTCTimestamp()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/functions/isUTCTimestamp

**Contents:**
- Function: isUTCTimestamp()
- Parameters​
- Returns​

isUTCTimestamp(time): time is UTCTimestamp

Check if a time value is a UTC timestamp number.

true if time is a UTCTimestamp number, false otherwise.

---

## Type alias: SeriesMarkerBarPosition

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/SeriesMarkerBarPosition

**Contents:**
- Type alias: SeriesMarkerBarPosition

SeriesMarkerBarPosition: "aboveBar" | "belowBar" | "inBar"

Represents the position of a series marker relative to a bar.

---

## Type alias: SeriesMarkerShape

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/SeriesMarkerShape

**Contents:**
- Type alias: SeriesMarkerShape

SeriesMarkerShape: "circle" | "square" | "arrowUp" | "arrowDown"

Represents the shape of a series marker.

---

## Interface: TickMark

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/TickMark

**Contents:**
- Interface: TickMark
- Properties​
  - index​
  - time​
    - [species]​
  - weight​
  - originalTime​

Tick mark for the horizontal scale.

index: TimePointIndex

[species]: "InternalHorzScaleItem"

The 'name' or species of the nominal.

weight: TickMarkWeightValue

Weight of the tick mark

originalTime: unknown

Original value for the time property

---

## Type alias: Time

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/Time

**Contents:**
- Type alias: Time
- Example​

Time: UTCTimestamp | BusinessDay | string

The Time type is used to represent the time of data items.

Values can be a UTCTimestamp, a BusinessDay, or a business day string in ISO format.

**Examples:**

Example 1 (css):
```css
const timestamp = 1529899200; // Literal timestamp representing 2018-06-25T04:00:00.000Zconst businessDay = { year: 2019, month: 6, day: 1 }; // June 1, 2019const businessDayString = '2021-02-03'; // Business day string literal
```

---

## Type alias: LastValueDataResult

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/LastValueDataResult

**Contents:**
- Type alias: LastValueDataResult

LastValueDataResult: LastValueDataResultWithData | LastValueDataResultWithoutData

Represents last value data result of a series for plugins

---

## Type alias: CandlestickSeriesOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/CandlestickSeriesOptions

**Contents:**
- Type alias: CandlestickSeriesOptions

CandlestickSeriesOptions: SeriesOptions <CandlestickStyleOptions>

Represents candlestick series options.

---

## Enumeration: MismatchDirection

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/enumerations/MismatchDirection

**Contents:**
- Enumeration: MismatchDirection
- Enumeration Members​
  - NearestLeft​
  - None​
  - NearestRight​

Search direction if no data found at provided index

Search the nearest left item

Search the nearest right item

---

## Interface: LineStyleOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/LineStyleOptions

**Contents:**
- Interface: LineStyleOptions
- Properties​
  - color​
    - Default Value​
  - lineStyle​
    - Default Value​
  - lineWidth​
    - Default Value​
  - lineType​
    - Default Value​

Represents style options for a line series.

Line width in pixels.

pointMarkersVisible: boolean

Show circle markers on each point.

optional pointMarkersRadius: number

Circle markers radius in pixels.

crosshairMarkerVisible: boolean

Show the crosshair marker.

crosshairMarkerRadius: number

Crosshair marker radius in pixels.

crosshairMarkerBorderColor: string

Crosshair marker border color. An empty string falls back to the color of the series under the crosshair.

crosshairMarkerBackgroundColor: string

The crosshair marker background color. An empty string falls back to the color of the series under the crosshair.

crosshairMarkerBorderWidth: number

Crosshair marker border width in pixels.

lastPriceAnimation: LastPriceAnimationMode

Last price animation mode.

**Examples:**

Example 1 (json):
```json
{@link LineStyle.Solid}
```

Example 2 (json):
```json
{@link LineType.Simple}
```

Example 3 (json):
```json
{@link LastPriceAnimationMode.Disabled}
```

---

## Interface: CustomSeriesWhitespaceData<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/CustomSeriesWhitespaceData

**Contents:**
- Interface: CustomSeriesWhitespaceData<HorzScaleItem>
- Extended by​
- Type parameters​
- Properties​
  - time​
  - customValues?​

Represents a whitespace data item, which is a data point without a value.

The time of the data.

optional customValues: Record<string, unknown>

Additional custom values which will be ignored by the library, but could be used by plugins.

---

## Interface: WhitespaceData<HorzScaleItem>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/WhitespaceData

**Contents:**
- Interface: WhitespaceData<HorzScaleItem>
- Example​
- Extended by​
- Type parameters​
- Properties​
  - time​
  - customValues?​

Represents a whitespace data item, which is a data point without a value.

• HorzScaleItem = Time

The time of the data.

optional customValues: Record<string, unknown>

Additional custom values which will be ignored by the library, but could be used by plugins.

**Examples:**

Example 1 (css):
```css
const data = [    { time: '2018-12-03', value: 27.02 },    { time: '2018-12-04' }, // whitespace    { time: '2018-12-05' }, // whitespace    { time: '2018-12-06' }, // whitespace    { time: '2018-12-07' }, // whitespace    { time: '2018-12-08', value: 23.92 },    { time: '2018-12-13', value: 30.74 },];
```

---

## Interface: PriceFormatCustom

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/PriceFormatCustom

**Contents:**
- Interface: PriceFormatCustom
- Properties​
  - type​
  - formatter​
  - tickmarksFormatter?​
  - minMove​
    - Default Value​
  - base?​

Represents series value formatting options.

The custom price format.

formatter: PriceFormatterFn

Override price formatting behaviour. Can be used for cases that can't be covered with built-in price formats.

optional tickmarksFormatter: TickmarksPriceFormatterFn

Override price formatting for multiple prices. Can be used if formatter should be adjusted based of all values being formatted.

The minimum possible step size for price value movement.

optional base: number

The base value for the price format. It should equal to 1 / minMove. If this option is specified, we ignore the minMove option. It can be useful for cases with very small price movements like 1e-18 where we can reach limitations of floating point precision.

---

## Type alias: SeriesOptions<T>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/SeriesOptions

**Contents:**
- Type alias: SeriesOptions<T>
- See​
- Type parameters​

SeriesOptions<T>: T & SeriesOptionsCommon

Represents the intersection of a series type T's options and common series options.

SeriesOptionsCommon for common options.

---

## Interface: HandleScrollOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/HandleScrollOptions

**Contents:**
- Interface: HandleScrollOptions
- Properties​
  - mouseWheel​
    - Default Value​
  - pressedMouseMove​
    - Default Value​
  - horzTouchDrag​
    - Default Value​
  - vertTouchDrag​
    - Default Value​

Represents options for how the chart is scrolled by the mouse and touch gestures.

Enable scrolling with the mouse wheel.

pressedMouseMove: boolean

Enable scrolling by holding down the left mouse button and moving the mouse.

horzTouchDrag: boolean

Enable horizontal touch scrolling.

When enabled the chart handles touch gestures that would normally scroll the webpage horizontally.

vertTouchDrag: boolean

Enable vertical touch scrolling.

When enabled the chart handles touch gestures that would normally scroll the webpage vertically.

---

## Type alias: LineSeriesOptions

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/LineSeriesOptions

**Contents:**
- Type alias: LineSeriesOptions

LineSeriesOptions: SeriesOptions <LineStyleOptions>

Represents line series options.

---

## Interface: ISeriesPrimitiveBase<TSeriesAttachedParameters>

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/ISeriesPrimitiveBase

**Contents:**
- Interface: ISeriesPrimitiveBase<TSeriesAttachedParameters>
- Type parameters​
- Methods​
  - updateAllViews()?​
    - Returns​
  - priceAxisViews()?​
    - Returns​
  - timeAxisViews()?​
    - Returns​
  - paneViews()?​

Base interface for series primitives. It must be implemented to add some external graphics to series

• TSeriesAttachedParameters = unknown

optional updateAllViews(): void

This method is called when viewport has been changed, so primitive have to recalculate / invalidate its data

optional priceAxisViews(): readonly ISeriesPrimitiveAxisView[]

Returns array of labels to be drawn on the price axis used by the series

readonly ISeriesPrimitiveAxisView[]

array of objects; each of then must implement ISeriesPrimitiveAxisView interface

For performance reasons, the lightweight library uses internal caches based on references to arrays So, this method must return new array if set of views has changed and should try to return the same array if nothing changed

optional timeAxisViews(): readonly ISeriesPrimitiveAxisView[]

Returns array of labels to be drawn on the time axis

readonly ISeriesPrimitiveAxisView[]

array of objects; each of then must implement ISeriesPrimitiveAxisView interface

For performance reasons, the lightweight library uses internal caches based on references to arrays So, this method must return new array if set of views has changed and should try to return the same array if nothing changed

optional paneViews(): readonly IPrimitivePaneView[]

Returns array of objects representing primitive in the main area of the chart

readonly IPrimitivePaneView[]

array of objects; each of then must implement ISeriesPrimitivePaneView interface

For performance reasons, the lightweight library uses internal caches based on references to arrays So, this method must return new array if set of views has changed and should try to return the same array if nothing changed

optional priceAxisPaneViews(): readonly IPrimitivePaneView[]

Returns array of objects representing primitive in the price axis area of the chart

readonly IPrimitivePaneView[]

array of objects; each of then must implement ISeriesPrimitivePaneView interface

For performance reasons, the lightweight library uses internal caches based on references to arrays So, this method must return new array if set of views has changed and should try to return the same array if nothing changed

optional timeAxisPaneViews(): readonly IPrimitivePaneView[]

Returns array of objects representing primitive in the time axis area of the chart

readonly IPrimitivePaneView[]

array of objects; each of then must implement ISeriesPrimitivePaneView interface

For performance reasons, the lightweight library uses internal caches based on references to arrays So, this method must return new array if set of views has changed and should try to return the same array if nothing changed

optional autoscaleInfo(startTimePoint, endTimePoint): AutoscaleInfo

Return autoscaleInfo which will be merged with the series base autoscaleInfo. You can use this to expand the autoscale range to include visual elements drawn outside of the series' current visible price range.

Important: Please note that this method will be evoked very often during scrolling and zooming of the chart, thus it is recommended that this method is either simple to execute, or makes use of optimisations such as caching to ensure that the chart remains responsive.

• startTimePoint: Logical

start time point for the current visible range

• endTimePoint: Logical

end time point for the current visible range

optional attached(param): void

Attached Lifecycle hook.

• param: TSeriesAttachedParameters

An object containing useful references for the attached primitive to use.

optional detached(): void

Detached Lifecycle hook.

optional hitTest(x, y): PrimitiveHoveredItem

Hit test method which will be called by the library when the cursor is moved. Use this to register object ids being hovered for use within the crosshairMoved and click events emitted by the chart. Additionally, the hit test result can specify a preferred cursor type to display for the main chart pane. This method should return the top most hit for this primitive if more than one object is being intersected.

x Coordinate of mouse event

y Coordinate of mouse event

---

## Variable: CandlestickSeries

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/variables/CandlestickSeries

**Contents:**
- Variable: CandlestickSeries

const CandlestickSeries: SeriesDefinition<"Candlestick">

---

## Variable: BaselineSeries

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/variables/BaselineSeries

**Contents:**
- Variable: BaselineSeries

const BaselineSeries: SeriesDefinition<"Baseline">

---

## Function: createOptionsChart()

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/functions/createOptionsChart

**Contents:**
- Function: createOptionsChart()
- Parameters​
- Returns​

createOptionsChart(container, options?): IChartApiBase<number>

Creates an 'options' chart with price values on the horizontal scale.

This function is used to create a specialized chart type where the horizontal scale represents price values instead of time. It's particularly useful for visualizing option chains, price distributions, or any data where price is the primary x-axis metric.

• container: string | HTMLElement

The DOM element or its id where the chart will be rendered.

• options?: DeepPartial <PriceChartOptions>

Optional configuration options for the price chart.

IChartApiBase<number>

An instance of IChartApiBase configured for price-based horizontal scaling.

---

## Type alias: PriceFormat

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/type-aliases/PriceFormat

**Contents:**
- Type alias: PriceFormat

PriceFormat: PriceFormatBuiltIn | PriceFormatCustom

Represents information used to format prices.

---

## Interface: SeriesStyleOptionsMap

**URL:** https://tradingview.github.io/lightweight-charts/docs/api/interfaces/SeriesStyleOptionsMap

**Contents:**
- Interface: SeriesStyleOptionsMap
- Properties​
  - Bar​
  - Candlestick​
  - Area​
  - Baseline​
  - Line​
  - Histogram​
  - Custom​

Represents the type of style options for each series type.

For example a bar series has style options represented by BarStyleOptions.

The type of bar style options.

Candlestick: CandlestickStyleOptions

The type of candlestick style options.

Area: AreaStyleOptions

The type of area style options.

Baseline: BaselineStyleOptions

The type of baseline style options.

Line: LineStyleOptions

The type of line style options.

Histogram: HistogramStyleOptions

The type of histogram style options.

Custom: CustomStyleOptions

The type of a custom series' style options.

---
