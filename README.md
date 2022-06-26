//╭╮╱╱╭╮╭╮╱╱╭╮
//┃╰╮╭╯┃┃┃╱╱┃┃
//╰╮┃┃╭┻╯┣╮╭┫╰━┳╮╭┳━━╮
//╱┃╰╯┃╭╮┃┃┃┃╭╮┃┃┃┃━━┫
//╱╰╮╭┫╰╯┃╰╯┃╰╯┃╰╯┣━━┃
//╱╱╰╯╰━━┻━━┻━━┻━━┻━━╯
//╭━━━┳╮╱╱╱╱╱╱╱╭╮
//┃╭━╮┃┃╱╱╱╱╱╱╱┃┃
//┃┃╱╰┫╰━┳━━┳━╮╭━╮╭━━┫┃
//┃┃╱╭┫╭╮┃╭╮┃╭╮┫╭╮┫┃━┫┃
//┃╰━╯┃┃┃┃╭╮┃┃┃┃┃┃┃┃━┫╰╮
//╰━━━┻╯╰┻╯╰┻╯╰┻╯╰┻━━┻━╯
//━╯
//Vdub Binary Options SnipperVX v1 / Vdub Rejection Spike v3 intergration -
//Signal generator for Binary Options / ©Vdubus http://www.vdubus.co.uk/
study("Vdub Binary Options SniperVX  v1", overlay=true, shorttitle="Vdub_SniperBX_v1")
//
//====================channel======================
len = input(8, minval=1)
src = input(close, title="Source")
out = sma(src, len)
last8h = highest(close, 13)
lastl8 = lowest(close, 13)
bearish = cross(close,out) == 1 and close[1] > close
bullish = cross(close,out) == 1 and close[1] < close
channel2=input(false, title="Bar Channel On/Off")
ul2=plot(channel2?last8h:last8h==nz(last8h[1])?last8h:na, color=black, linewidth=1, style=linebr, title="Candle body resistance level top", offset=0)
ll2=plot(channel2?lastl8:lastl8==nz(lastl8[1])?lastl8:na, color=black, linewidth=1, style=linebr, title="Candle body resistance level bottom", offset=0)
//
//--------------------Trend colour ema------------------------------------------------//    ema trend direction trigger For Singals 1 & 2
src0 = close, len0 = input(13, minval=1, title="Trend Change EMA")
ema0 = ema(src0, len0)
plot_color = ema0 >= ema0[2]  ? lime: ema0 < ema0[2] ? red : na
plot(ema0, title="EMA", style=line, linewidth=1, color = plot_color)
//
//--Modified vyacheslav.shindin-------------------------------------------------//           Signal 1
//Configured ema signal output
slow = 8
fast = 5
vh1 = ema(highest(avg(low, close), fast), 5)
vl1 = ema(lowest(avg(high, close), slow), 8)
//
e_ema1 = ema(close, 1)
e_ema2 = ema(e_ema1, 1)
e_ema3 = ema(e_ema2, 1)
tema = 1 * (e_ema1 - e_ema2) + e_ema3
//
e_e1 = ema(close, 8)
e_e2 = ema(e_e1, 5)
dema = 2 * e_e1 - e_e2
signal = tema > dema ? max(vh1, vl1) : min(vh1, vl1)
is_call = tema > dema and signal > low and (signal-signal[1] > signal[1]-signal[2])
is_put = tema < dema and signal < high and (signal[1]-signal > signal[2]-signal[1])
 
plotshape(is_call ? 1 : na, title="BUY ARROW", color=green, text="*BUY*", style=shape.arrowup, location=location.belowbar)
plotshape(is_put ? -1 : na, title="SELL ARROW", color=red, text="*SELL*", style=shape.arrowdown)
//
//Modified - Rajandran R Supertrend----------------------------------------------------- //       Signal 2
Factor=input(1, minval=1,maxval = 000, title="Trend Transition Signal")
Pd=input(1, minval=1,maxval = 100)
Up=hl2-(Factor*atr(Pd))
Dn=hl2+(Factor*atr(Pd))
TrendUp=close[1]>TrendUp[1]? max(Up,TrendUp[1]) : Up
TrendDown=close[1]<TrendDown[1]? min(Dn,TrendDown[1]) : Dn
Trend = close > TrendDown[1] ? 1: close< TrendUp[1]? -1: nz(Trend[1],0)
 
plotarrow(Trend == 1 and Trend[1] == -1 ? Trend : na, title="Up Entry Arrow", colorup=lime, maxheight=1000, minheight=50, transp=85)
plotarrow(Trend == -1 and Trend[1] == 1 ? Trend : na, title="Down Entry Arrow", colordown=red, maxheight=1000, minheight=50, transp=85)
// Moddified [RS]Support and Resistance V0
RST = input(title='Support / Resistance length:', type=integer, defval=21)
RSTT = valuewhen(high >= highest(high, RST), high, 0)
RSTB = valuewhen(low <= lowest(low, RST), low, 0)
RT2 = plot(RSTT, color=RSTT != RSTT[1] ? na : red, linewidth=1, offset=0)
RB2 = plot(RSTB, color=RSTB != RSTB[1] ? na : green, linewidth=1, offset=0)
//
//===============================Directional Projection=======================================//
tf2 = input('1', title="Trend Projection TF / Mins/D/W")
M2 = input('ATR')
P2 = input(13.00, type=float)
W2 = input(1)
pf2 = pointfigure(tickerid, 'close', M2, P2, W2)
spfc2 = security(pf2, tf2, close)
channel3=input(false, title="Connect Projection High/Low")
p22=plot(channel3?spfc2:spfc2==nz(spfc2[1])?spfc2:na, color=blue, linewidth=2, style=linebr, title="Directional Projection", offset=0)
//----------------------------------------------------------------------//
