//@version=5
indicator("KDJ K High Price Divergence Signals", shorttitle="KDJ K High Divergence", overlay=true)

// KDJ parameters
length = input.int(14, title="Length")
smoothK = input.int(3, title="K Smoothing")
smoothD = input.int(3, title="D Smoothing")

// Calculate KDJ
lowestLow = ta.lowest(low, length)
highestHigh = ta.highest(high, length)
rsv = (close - lowestLow) / (highestHigh - lowestLow) * 100
K = ta.sma(rsv, smoothK)
D = ta.sma(K, smoothD)
J = 3 * K - 2 * D

// Detect divergence based on K high price
prev_highest_high = ta.valuewhen(high == ta.highest(high, length), high, 1)
prev_K = ta.valuewhen(high == ta.highest(high, length), K, 1)

divergence_up = high > prev_highest_high and K < prev_K
divergence_down = high < prev_highest_high and K > prev_K

// Plotting signals
plotshape(series=divergence_up, location=location.belowbar, color=color.green, style=shape.triangleup, size=size.small, title="Bullish Divergence")
plotshape(series=divergence_down, location=location.abovebar, color=color.red, style=shape.triangledown, size=size.small, title="Bearish Divergence")

// Remove KDJ plots
// hline(20, "20 Line", color=color.gray)
// hline(80, "80 Line", color=color.gray)
// plot(K, color=color.blue, title="%K")
// plot(D, color=color.orange, title="%D")
// plot(J, color=color.purple, title="%J")
