# FTX quant zone weighted DCA / Take profits

This strategy is not reliant on any predictions of price. Just math - weighted DCA and take profits when available (also weighted).

!DISCLAIMER - FTX Quant zone is very vulnerable to mistakes, rules will run indefinitely when not stopped etc. Use at your own risk, remember to set everything exactly as provided, and test first on low volumes of coins/USD!

The following quant zones rules are based on DCA Bot from rimko: https://wiki.rimko.xyz/index.php/DCA_Bot

This strategy expands on the bot with following features:
- modifies DCA bot to buy more when price is less than moving average (& reverse), capped at 2x.
- after each buy recomputes the average buy price and saves in @COIN_avbp variable (used later in take profits rule)
- allows for transfer of coins outside of FTX, uses variable !COIN_Total and !USD_Total to keep track (In the examples I use variables for Nexo, Kucoin, and 'Wallets'. Modify accordingly)
- implements new pair of rules to take **some** profits when available

## How to start DCA bot?
- For each coin you have to initialize vars:
  - "-COIN_Minimal_trad_size" - from the market pair dashboard
  - "-COIN_Order_size" - how many % of your portfolio you want to buy this coin, f.eg, BTC 80% + MATIC 20% = -BTC_Order_size:0.8,-MATIC_Order_size:0.2
  - "*COIN_OS" - 0 - internal variable for bot
  - "!COIN_Total" - your total amount of coin, ftx or otherwise you want to track
  - "COIN_BAG" - 9999999999 - very high amount above your holdings
- Initialize global vars:
  - "!USD_Total" - your total amount of dry powder (recomputed every hour by attached initVar rule)
  - "!DCA_Speed" - how much of your dry powder you want to spend daily - f.eg 3% = !DCA_Speed:0.03
- For each coin you want to DCA you have to implement attached DCA rules

### Notes:
  - in BTC I am doing ^3 in price multiplier computation, in other coins ^2, as BTC has less price volatility, feel free to set it however you want, just remember to modify it in all the actions
  - in coins that have higher minimal trade size, or you will buy very low amounts, you might need to set the bot to buy once a week, in this case you need to add day_of_week to triggers in dca1_2 and *7 to **EACH** !DCA_Speed in its actions - MATIC is the example
  - if you don't want to track coins outside FTX you can just skip computing the !USD_Total and !COIN_Total variables and use balance("USD") & balance("COIN")
  - moving average used is "average price from last month" - hardcoded here: min(2,(average_price("BTC/USD", 40320). 40320 minutes = 28 days.
  
## How to start take profits bot?
- set up reduceTakeProfits rule with an action for each coin
- initialize vars for each coin:
  - "COIN_LASTSELL" - 0 - internal variable for bot
  - "COIN_SELLBAG" - 9999999999 - very high amount above your holdings
  - "%COIN_takeprof" - initialized in reduceTakeProfits rule
- (I assume the DCA bot is running, as it uses the variables from there)

- set up SELL rules for each coin.

### Notes:
  - in BTC i take first profits at 10% level, in ETH at 15% level, in other altcoins (like MATIC) - 20% level. It is configurable in reduceTakeProfits rule
  - the amount of coin i sell is set at takeProfitslevel/2, so when BTC gets 10% pump above !BTC_avbp variable, it will sell 5% of BTC, and double the takeProfitslevel to 20%.
  - then if BTC pumps another 10%, and reaches takeProfitsLevel=20%, it will sell 10% of BTC, and set the level at 40%. That is how the weighted take profits are implemented.
  - the sell amount is capped at 0.25 of total balance, configurable in sell rules
  - every day the takeProfitsLevel for every coin are lowered by !DCA_Speed amount, until reach basic level / sells again and get multiplied.


