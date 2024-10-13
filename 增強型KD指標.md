//@version=5
indicator("增強型KD指標", overlay=false)

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

// 1. 畫出 %K 和 %D 線
plot(K, color=color.blue, title="%K", linewidth=2)
plot(D, color=color.orange, title="%D", linewidth=2)

// 2. 添加超買和超賣區域
h1 = hline(overboughtLevel, "超買", color=color.red, linestyle=hline.style_dotted, linewidth=1)
h2 = hline(oversoldLevel, "超賣", color=color.green, linestyle=hline.style_dotted, linewidth=1)
fill(h1, h2, color=color.new(color.gray, 90), title="超買/超賣區域")

// 3. 柱狀圖
overboughtPart = math.max(K - overboughtLevel, 0)
oversoldPart = math.max(oversoldLevel - K, 0)
plot(overboughtPart, title="超買柱狀圖", style=plot.style_columns, color=color.new(color.red, 50))
plot(oversoldPart, title="超賣柱狀圖", style=plot.style_columns, color=color.new(color.green, 50))

// 4. 背景色漸變
getColor(k) =>
    c = color.from_gradient(k, oversoldLevel, overboughtLevel, color.green, color.red)
    color.new(c, 70)

bgcolor(getColor(K))

// 5. 箭頭指示
plotarrow(K > overboughtLevel ? 1 : K < oversoldLevel ? -1 : 0, title="買賣超箭頭", colorup=color.new(color.red, 0), colordown=color.new(color.green, 0))

// 6. 圓點指示
plotshape(K > overboughtLevel, title="超買點", location=location.top, color=color.new(color.red, 0), style=shape.circle, size=size.tiny)
plotshape(K < oversoldLevel, title="超賣點", location=location.bottom, color=color.new(color.green, 0), style=shape.circle, size=size.tiny)

// 7. 聲音提醒（僅在實時圖表上有效）
alertcondition(ta.cross(K, overboughtLevel), title="突破超買", message="K值突破超買水平！")
alertcondition(ta.cross(oversoldLevel, K), title="突破超賣", message="K值突破超賣水平！")

// 8. 顯示數值標籤
var label kLabel = na
var label dLabel = na
label.delete(kLabel)
label.delete(dLabel)
kLabel := label.new(bar_index, K, str.tostring(K, "#.##"), color=color.new(color.blue, 100), textcolor=color.white, style=label.style_none, size=size.normal)
dLabel := label.new(bar_index, D, str.tostring(D, "#.##"), color=color.new(color.orange, 100), textcolor=color.white, style=label.style_none, size=size.normal)
