//@version=5
indicator("MACD with Strength", overlay=false)

// MACD settings
fastLength = input.int(12, title="Fast Length")
slowLength = input.int(26, title="Slow Length")
signalSmoothing = input.int(9, title="Signal Smoothing")

// Calculate MACD line and Signal line
[macdLine, signalLine, _] = ta.macd(close, fastLength, slowLength, signalSmoothing)

// Calculate MACD Histogram
macdHist = macdLine - signalLine

// Define MACD strength thresholds (you can adjust these thresholds based on your preference)
strongThreshold = input.float(0.5, title="Strong Threshold")
weakThreshold = input.float(0.1, title="Weak Threshold")

// Determine the color of the MACD Histogram
var color histColor = color.gray // Default color to avoid NA value

if macdHist > 0
    if macdHist > strongThreshold
        histColor := color.red
    else
        histColor := color.new(color.red, 50)  // Adjust the transparency as needed
else
    if macdHist < -strongThreshold
        histColor := color.green
    else
        histColor := color.new(color.green, 50)  // Adjust the transparency as needed

// Plot MACD line, Signal line, and Histogram
plot(macdLine, color=color.blue, title="MACD Line")
plot(signalLine, color=color.orange, title="Signal Line")
plot(macdHist, color=histColor, style=plot.style_histogram, title="MACD Histogram")
