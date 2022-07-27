### Trigger   // Run when price goes above the average buy price * take profits modifier | repeat until sold
```
get_variable("MATIC_SELLBAG") < balance("MATIC") or price("MATIC/USD") > get_variable("@MATIC_avbp")*(1+get_variable("%MATIC_takeprof"))
```
### Action 1   // Sell x% of total, where 'x' == take profits modifier / 2, cap at 25%. Add one minimal_trade_size to sell below the blockfolio profits made during rule run.
```
Place Custom Order
```
```
Limit Order | Sell | MATIC/USD
```
```
ceil(((get_variable("!MATIC_Total")*min(0.25,get_variable("%MATIC_takeprof")/2)))*(1/get_variable("-MATIC_Minimal_trad_size")))/(1/get_variable("-MATIC_Minimal_trad_size")) if (get_variable("MATIC_LASTSELL") == 0) else balance("MATIC") - get_variable("MATIC_SELLBAG") + get_variable("-MATIC_Minimal_trad_size")
```
```
offer_price("MATIC/USD")
```
```
+postOnly +cancelAndPlaceANewOrder
```
### Action 2   // Provide a way to distinguish first run from consecutive runs
```
Set Variable
```
```
MATIC_LASTSELL
```
```
1
```
### Action 3   // Set expected coin balance after rule end
```
Set Variable
```
```
MATIC_SELLBAG
```
```
max(get_variable("-MATIC_Minimal_trad_size")*5,balance("MATIC") - floor(((get_variable("!MATIC_Total")*min(0.25,get_variable("%MATIC_takeprof")/2)))*(1/get_variable("-MATIC_Minimal_trad_size")))/(1/get_variable("-MATIC_Minimal_trad_size"))) if (get_variable("MATIC_LASTSELL") == 0) else get_variable("MATIC_SELLBAG")
```
### Action 4   // Pause variable reset rule
```
Pause Rule
```
```
MATIC_SELL 2/2
```
```
1 | Minutes
```
### Action 5   // Double take profits modifier
```
Set Variable
```
```
%MATIC_takeprof
```
```
get_variable("%MATIC_takeprof")*2 if get_variable("MATIC_LASTSELL") == 0 else get_variable("%MATIC_takeprof")
```