---
name: lightweight-charts
description: TradingView Lightweight Charts - A free, lightweight, and performant financial charting library for creating interactive stock/crypto charts
version: 5.1
---

# TradingView Lightweight Charts Skill

A free, lightweight (~45kb gzipped), and performant financial charting library for creating interactive stock, crypto, and forex charts. Built by TradingView.

## When to Use This Skill

Use this skill when:
- Creating financial charts (candlestick, line, area, bar, histogram, baseline)
- Building trading dashboards or portfolio visualizations
- Implementing real-time price charts with live data updates
- Adding technical analysis visualizations
- Customizing chart appearance, scales, and interactions
- Creating custom series types or chart plugins/primitives
- Working with time scale, price scale, or chart events

## Installation

```bash
npm install --save lightweight-charts
```

## Quick Start

```javascript
import { createChart, CandlestickSeries } from 'lightweight-charts';

// Create chart
const chart = createChart(document.getElementById('container'), {
    layout: {
        textColor: 'black',
        background: { type: 'solid', color: 'white' }
    }
});

// Add candlestick series
const candlestickSeries = chart.addSeries(CandlestickSeries, {
    upColor: '#26a69a',
    downColor: '#ef5350',
    borderVisible: false,
    wickUpColor: '#26a69a',
    wickDownColor: '#ef5350'
});

// Set data
candlestickSeries.setData([
    { time: '2018-12-22', open: 75.16, high: 82.84, low: 36.16, close: 45.72 },
    { time: '2018-12-23', open: 45.12, high: 53.90, low: 45.12, close: 48.09 },
    // ... more data
]);

// Fit content to view
chart.timeScale().fitContent();
```

## API Quick Reference

### Core Functions

| Function | Description |
|----------|-------------|
| `createChart(container, options)` | Create a standard time-based chart |
| `createYieldCurveChart(container, options)` | Create a yield curve chart |
| `createOptionsChart(container, options)` | Create an options chart (price-based x-axis) |
| `createChartEx(container, horzScaleBehavior, options)` | Create chart with custom horizontal scale |

### Series Types (Variables)

| Variable | Description |
|----------|-------------|
| `AreaSeries` | Area chart with filled region below line |
| `BarSeries` | OHLC bar chart |
| `BaselineSeries` | Two-color area chart split at baseline |
| `CandlestickSeries` | Japanese candlestick chart |
| `HistogramSeries` | Vertical histogram bars |
| `LineSeries` | Simple line chart |

### Key Interfaces

| Interface | Description |
|-----------|-------------|
| `IChartApi` | Main chart control interface |
| `ISeriesApi` | Series control interface (data, options, price lines) |
| `ITimeScaleApi` | Time scale control (visible range, scrolling) |
| `IPriceScaleApi` | Price scale control (left/right axis) |
| `IPaneApi` | Pane control for multi-pane charts |

### Enumerations

| Enumeration | Values |
|-------------|--------|
| `LineStyle` | Solid, Dotted, Dashed, LargeDashed, SparseDotted |
| `LineType` | Simple, WithSteps, Curved |
| `CrosshairMode` | Normal, Magnet, Hidden |
| `PriceScaleMode` | Normal, Logarithmic, Percentage, IndexedTo100 |
| `ColorType` | Solid, VerticalGradient |

## Common Patterns

### Creating Different Series Types

```javascript
import { createChart, LineSeries, AreaSeries, CandlestickSeries } from 'lightweight-charts';

const chart = createChart(container);

// Line series
const lineSeries = chart.addSeries(LineSeries, { color: '#2962FF' });
lineSeries.setData([
    { time: '2018-12-12', value: 24.11 },
    { time: '2018-12-13', value: 31.74 },
]);

// Area series
const areaSeries = chart.addSeries(AreaSeries, {
    lineColor: '#2962FF',
    topColor: '#2962FF',
    bottomColor: 'rgba(41, 98, 255, 0.28)',
});

// Candlestick series
const candleSeries = chart.addSeries(CandlestickSeries, {
    upColor: '#26a69a',
    downColor: '#ef5350',
});
candleSeries.setData([
    { time: '2018-12-19', open: 141.77, high: 170.39, low: 120.25, close: 145.72 },
]);
```

### Real-Time Data Updates

```javascript
// Update the latest bar or add new bar
series.update({ time: '2018-12-31', value: 25 });

// For OHLC data
candlestickSeries.update({
    time: '2018-12-31',
    open: 109.87,
    high: 114.69,
    low: 85.66,
    close: 112
});
```

### Time Scale Control

```javascript
const timeScale = chart.timeScale();

// Fit all data in view
timeScale.fitContent();

// Reset to default view
timeScale.resetTimeScale();

// Set visible range
timeScale.setVisibleRange({
    from: '2018-01-01',
    to: '2018-12-31'
});

// Get visible logical range
const logicalRange = timeScale.getVisibleLogicalRange();

// Subscribe to visible range changes
timeScale.subscribeVisibleLogicalRangeChange((newRange) => {
    console.log('Visible range changed:', newRange);
});
```

### Price Scale Configuration

```javascript
// Configure left/right price scales
const chart = createChart(container, {
    leftPriceScale: { visible: true },
    rightPriceScale: { visible: true },
});

// Apply options to series price scale
series.priceScale().applyOptions({
    scaleMargins: { top: 0.1, bottom: 0.1 },
    autoScale: true,
});
```

### Adding Price Lines

```javascript
const priceLine = series.createPriceLine({
    price: 80.0,
    color: '#3179F5',
    lineWidth: 2,
    lineStyle: LineStyle.Dotted,
    axisLabelVisible: true,
    title: 'Target Price',
});

// Remove price line
series.removePriceLine(priceLine);
```

### Mouse/Click Event Handling

```javascript
chart.subscribeClick((param) => {
    if (param.time) {
        console.log('Clicked time:', param.time);
        const price = param.seriesData.get(series);
        console.log('Price data:', price);
    }
});

chart.subscribeCrosshairMove((param) => {
    if (param.point) {
        console.log('Crosshair position:', param.point.x, param.point.y);
    }
});
```

### Multi-Pane Charts

```javascript
// Series automatically creates new pane when moved
series.moveToPane(1);

// Get pane reference
const pane = chart.panes()[0];

// Attach pane primitive
pane.attachPrimitive(myPanePrimitive);
```

### Custom Series Plugins

```javascript
class MyCustomSeries {
    // Implement ICustomSeriesPaneView interface
    renderer() {
        return {
            draw: (target, priceConverter) => {
                target.useBitmapCoordinateSpace(scope => {
                    // Custom drawing logic using CanvasRenderingContext2D
                });
            }
        };
    }
    
    update(data, options) {
        // Handle data updates
    }
    
    priceValueBuilder(plotRow) {
        return [plotRow.value]; // Return price values for autoscaling
    }
}

const customSeries = chart.addCustomSeries(new MyCustomSeries(), {
    // Custom options
});
```

### Series Primitives (Drawing Tools)

```javascript
class MyPrimitive {
    // Implement ISeriesPrimitive interface
    paneViews() {
        return [{
            renderer: () => ({
                draw: (target) => {
                    target.useMediaCoordinateSpace(scope => {
                        // Draw on the chart
                    });
                }
            })
        }];
    }
    
    attached({ chart, series, requestUpdate }) {
        this._requestUpdate = requestUpdate;
    }
}

// Attach to series
series.attachPrimitive(new MyPrimitive());
```

## Data Formats

### Time Format
Time can be specified as:
- Unix timestamp (seconds): `1642425322`
- ISO date string: `'2018-12-22'`
- Business day object: `{ year: 2018, month: 12, day: 22 }`

### Series Data Types

```typescript
// Line/Area/Histogram
{ time: Time, value: number }

// Candlestick/Bar (OHLC)
{ time: Time, open: number, high: number, low: number, close: number }

// Baseline
{ time: Time, value: number }  // Uses baseValue option for split
```

## Reference Files

Detailed API documentation is available in `references/`:

- **api.md** (207 pages) - Complete API reference: interfaces, types, enumerations, functions
- **guides.md** (16 pages) - Series types, chart types, scales, panes, plugins
- **other.md** (13 pages) - Migrations, platform-specific guides, release notes

## Resources

- [Official Documentation](https://tradingview.github.io/lightweight-charts/docs)
- [API Reference](https://tradingview.github.io/lightweight-charts/docs/api)
- [GitHub Repository](https://github.com/tradingview/lightweight-charts)
- [Tutorials](https://tradingview.github.io/lightweight-charts/tutorials)

## License Note

The Lightweight Charts license requires attribution to TradingView. Add the following to your public pages:
- Attribution notice from the NOTICE file
- Link to https://www.tradingview.com
