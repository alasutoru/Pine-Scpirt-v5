//@version=5
indicator("KDV3", overlay=false)

// 定義參數
kLength = input.int(14, title="K Length")
dLength = input.int(3, title="D Length")
overboughtLevel = input.int(80, title="超買水平", minval=50, maxval=100)
oversoldLevel = input.int(20, title="超賣水平", minval=0, maxval=50)

// 計算 KD 值
lowestLow = ta.lowest(low, kLength)
highestHigh = ta.highest(high, kLength)
K = 100 * (close - lowestLow) / (highestHigh - lowestLow)
D = ta.sma(K, dLength)

// 畫出 %K 和 %D 線
plot(K, color=color.blue, title="%K", linewidth=1)
plot(D, color=color.orange, title="%D", linewidth=1)

// 添加超買和超賣區域
h1 = hline(overboughtLevel, "超買", color=color.red, linestyle=hline.style_dotted, linewidth=1)
h2 = hline(oversoldLevel, "超賣", color=color.green, linestyle=hline.style_dotted, linewidth=1)

// 填充超買超賣區域
fill(h1, h2, color=color.new(color.gray, 90), title="超買/超賣區域")

// 計算超買和超賣部分的值
overboughtPart = math.max(K - overboughtLevel, 0)
oversoldPart = math.max(oversoldLevel - K, 0)

// 繪製柱狀圖
plot(overboughtPart, title="超買柱狀圖", style=plot.style_columns, color=color.new(color.red, 50))
plot(oversoldPart, title="超賣柱狀圖", style=plot.style_columns, color=color.new(color.green, 50))

// 背景顏色提示（可選，如果與柱狀圖衝突可以註釋掉）
// bgcolor(K > overboughtLevel ? color.new(color.red, 90) : na)
// bgcolor(K < oversoldLevel ? color.new(color.green, 90) : na)

var float prevK = na
isStrong = na(prevK) ? false : (prevK <= overboughtLevel and K > overboughtLevel)
isWeak = na(prevK) ? false : (prevK >= oversoldLevel and K < oversoldLevel)

// 更新 prevK
prevK := K
