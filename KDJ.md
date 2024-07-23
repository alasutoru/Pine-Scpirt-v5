//@version=5
indicator(title="KDJ", shorttitle="KDJ", overlay=false)

// 計算KDJ指標的參數
length = input(9, title="Length")
smoothK = input(3, title="Smooth K")
smoothD = input(3, title="Smooth D")

// 計算RSV (Raw Stochastic Value)
highestHigh = ta.highest(high, length)
lowestLow = ta.lowest(low, length)
RSV = 100 * (close - lowestLow) / (highestHigh - lowestLow)

// 計算K線、D線和J線
K = ta.sma(RSV, smoothK)
D = ta.sma(K, smoothD)
J = 3 * K - 2 * D

// 繪製K線、D線和J線
plot(K, title="K Line", color=color.blue, linewidth=1)
plot(D, title="D Line", color=color.red, linewidth=1)
plot(J, title="J Line", color=color.green, linewidth=1)

// 添加水平線作為參考
hline(80, "Overbought", color=color.gray, linestyle=hline.style_dotted)
hline(20, "Oversold", color=color.gray, linestyle=hline.style_dotted)
