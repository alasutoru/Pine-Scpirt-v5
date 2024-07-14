//@version=5
indicator("Multiple MAs as Support/Resistance", overlay=true)

ma20 = ta.sma(close, 20)
ma50 = ta.sma(close, 50)
ma200 = ta.sma(close, 200)

plot(ma20, color=color.blue, title="20 SMA")
plot(ma50, color=color.yellow, title="50 SMA")
plot(ma200, color=color.red, title="200 SMA")
