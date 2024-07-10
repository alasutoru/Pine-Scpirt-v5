```
//@version=5
indicator("MACD with DIF Line", shorttitle="MACD DIF", overlay=false)

// 定義參數
fastLength = input.int(12, title="Fast Length")
slowLength = input.int(26, title="Slow Length")
signalSmoothing = input.int(9, title="Signal Smoothing")
difSmoothing = input.int(9, title="DIF Smoothing")
src = input(close, title="Source")

// 計算快線和慢線的EMA
fastMA = ta.ema(src, fastLength)
slowMA = ta.ema(src, slowLength)

// 計算MACD線和信號線
macdLine = fastMA - slowMA
signalLine = ta.ema(macdLine, signalSmoothing)

// 計算DIF線
difLine = ta.ema(macdLine - signalLine, difSmoothing)

// 繪製MACD線、信號線和DIF線
plot(difLine, title="DIF Line", color=color.blue, linewidth=2)
plot(signalLine, title="Signal Line", color=color.orange, linewidth=2)
plot(macdLine, title="MACD Line", color=color.red, linewidth=2)

// 計算柱狀圖
histogram = macdLine - signalLine

// 計算histogram的EMA
histogramEma = ta.ema(histogram, 5)
```
// 根據強弱分顏色
color_histogram = (histogram > 0) ? ((histogram > histogramEma) ? color.green : color.lime) : ((histogram < histogramEma) ? color.red : color.maroon)

// 繪製根據強弱分顏色的柱狀圖
plot(histogram, title="Histogram", color=color_histogram, style=plot.style_histogram, linewidth=1)
