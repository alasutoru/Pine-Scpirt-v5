//@version=5
indicator("Fibonacci Retracement", overlay=true)

lookbackPeriod = input.int(50, "Lookback Period")

high_point = ta.highest(high, lookbackPeriod)
low_point = ta.lowest(low, lookbackPeriod)

fib_23_6 = high_point - (high_point - low_point) * 0.236
fib_38_2 = high_point - (high_point - low_point) * 0.382
fib_50_0 = high_point - (high_point - low_point) * 0.5
fib_61_8 = high_point - (high_point - low_point) * 0.618

plot(fib_23_6, color=color.blue, title="23.6%")
plot(fib_38_2, color=color.green, title="38.2%")
plot(fib_50_0, color=color.yellow, title="50.0%")
plot(fib_61_8, color=color.red, title="61.8%")
