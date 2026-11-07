You are a professional PineScript version=6 developer.
You know how to code indicators and strategies and you also know their differences in code.
I need your help to turn a TradingView indicator into a strategy please.

Go Long when….
Close Long when…

Respect these instructions:
- Convert all Indicator specific code to Strategy specific code. Don't use any code that a TradingView Strategy won't support. Especially timeframes and gaps. Define those in code so they are semantically the same as before.

- Preserve the timeframe logic if there is one. Fill gaps.

- If the indicator is plotting something, the strategy code shall plot the same thing as well so the visuals are preserved. Set plot offsets to 0.

- Don't trigger a short. Simply go Long and Flat.

- Use this strategy code line. It contains all default parameters you need:
strategy("NAME", overlay=true, calc_on_every_tick=false, initial_capital=1000, default_qty_type=strategy.percent_of_equity, default_qty_value=100, commission_type=strategy.commission.percent, commission_value=0.1, slippage=3, pyramiding=1, margin_long=0, margin_short=0, fill_orders_on_standard_ohlc=true)

- strategy.commission.percent and strategy.slippage don't exist in PineScript. Please avoid this mistake. Set those variables in the strategy() function when initiating the strategy.

- Never make line breaks when calling functions, in IFs, Loops or when defining Variables. It would cause syntax errors. Of course you do need to make line breaks when defining new functions. Search the web for the PineScript documentation to be sure how it’s done.

- Leave all other strategy settings to default values (aka. don't set them at all).

- Never use lookahead_on because that’s cheating. 

- Add Start Date and End Date inputs/filters so the user can choose from when to when to execute trades. Start with 1st January 2018 and go to 31st December 2069.

- When setting the title of the strategy, add "AI - " at the start of the name and then continue with the name of the strategy.

This is the code of the Indicator you shall migrate to a TradingView Strategy:
//@version=6
indicator(title="Williams alligator", shorttitle="Alligator", overlay=true, timeframe="", timeframe_gaps=true)
smma(src, length) =>
	smma =  0.0
	smma := na(smma[1]) ? ta.sma(src, length) : (smma[1] * (length - 1) + src) / length
	smma
jawLength = input.int(13, minval=1, title="Jaw Length")
teethLength = input.int(8, minval=1, title="Teeth Length")
lipsLength = input.int(5, minval=1, title="Lips Length")
jawOffset = input(8, title="Jaw Offset")
teethOffset = input(5, title="Teeth Offset")
lipsOffset = input(3, title="Lips Offset")
jaw = smma(hl2, jawLength)
teeth = smma(hl2, teethLength)
lips = smma(hl2, lipsLength)
plot(jaw, "Jaw", offset = jawOffset, color=#2962FF)
plot(teeth, "Teeth", offset = teethOffset, color=#E91E63)
plot(lips, "Lips", offset = lipsOffset, color=#66BB6A)



