### Trigger   // Run when price goes above the average buy price * take profits modifier | repeat until sold
```
get_variable("BTC_SELLBAG") < balance("BTC") or price("BTC/USD") > get_variable("@BTC_avbp")*(1+get_variable("%BTC_takeprof"))
```
### Action 1   // Sell x% of total, where 'x' == take profits modifier / 2, cap at 25%. Add one minimal_trade_size to sell below the blockfolio profits made during rule run.
```
Place Custom Order
```
```
Limit Order | Sell | BTC/USD
```
```
ceil(((get_variable("!BTC_Total")*min(0.25,get_variable("%BTC_takeprof"))))*(1/get_variable("-BTC_Minimal_trad_size")))/(1/get_variable("-BTC_Minimal_trad_size")) if (get_variable("BTC_LASTSELL") == 0) else balance("BTC") - get_variable("BTC_SELLBAG") + get_variable("-BTC_Minimal_trad_size")
```
```
offer_price("BTC/USD")
```
```
+postOnly +cancelAndPlaceANewOrder
```
### Action 2   // Provide a way to distinguish first run from consecutive runs
```
Set Variable
```
```
BTC_LASTSELL
```
```
1
```
### Action 3   // Set expected coin balance after rule end
```
Set Variable
```
```
BTC_SELLBAG
```
```
max(get_variable("-BTC_Minimal_trad_size")*5,balance("BTC") - floor(((get_variable("!BTC_Total")*min(0.25,get_variable("%BTC_takeprof"))))*(1/get_variable("-BTC_Minimal_trad_size")))/(1/get_variable("-BTC_Minimal_trad_size"))) if (get_variable("BTC_LASTSELL") == 0) else get_variable("BTC_SELLBAG")
```
### Action 4   // Pause variable reset rule
```
Pause Rule
```
```
BTC_SELL 2/2
```
```
1 | Minutes
```
### Action 5   // Double take profits modifier
```
Set Variable
```
```
%BTC_takeprof
```
```
get_variable("%BTC_takeprof")*2 if get_variable("BTC_LASTSELL") == 0 else get_variable("%BTC_takeprof")
```