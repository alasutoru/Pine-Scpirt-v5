```
//@version=5
indicator("KD", overlay=false)

// 定義參數
kLength = input.int(14, title="K Length")
dLength = input.int(3, title="D Length")

// 計算最小和最大值
lowestLow = ta.lowest(low, kLength)
highestHigh = ta.highest(high, kLength)

// 計算 %K 值
K = 100 * (close - lowestLow) / (highestHigh - lowestLow)

// 計算 %D 值
D = ta.sma(K, dLength)

// 畫出 %K 和 %D 線
plot(K, color=color.blue, title="%K", linewidth=2)
plot(D, color=color.orange, title="%D", linewidth=2)

// 添加超買和超賣區域
hline(80, "Overbought", color=color.red, linestyle=hline.style_dotted, linewidth=1)
hline(20, "Oversold", color=color.green, linestyle=hline.style_dotted, linewidth=1)

// 定義強弱判斷
var float prevK = na

isStrong = na(prevK) ? false : (prevK > 80 and K > 20)
isWeak = na(prevK) ? false : (prevK < 80 and K < 20)

// 背景顏色提示
bgcolor(isStrong ? color.new(color.red, 90) : na)
bgcolor(isWeak ? color.new(color.green, 90) : na)

// 更新 prevK
prevK := K
```
