//@version=5
indicator("Multiple MAs as Support/Resistance", overlay=true)

ma15 = ta.sma(close, 15)
ma30 = ta.sma(close, 30)
ma60 = ta.sma(close, 60)

plot(ma15, color=color.blue, title="15 SMA")
plot(ma30, color=color.yellow, title="30 SMA")
plot(ma60, color=color.red, title="60 SMA")
