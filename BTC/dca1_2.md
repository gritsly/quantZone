### Trigger   // At specific hour, until expected balance met
```
hour == 1 and balance("BTC") < get_variable("BTC_BAG")
```
### Action 1   // Set expected balance. Take dry powder level, multiply by DCA_speed variable (ex. 2%/day), multiply by BTC_Order_size variable (ex. set BTC to be 0.25 of your portfolio), multiply by modifier computed from relation of price to average price of last month to the power of 2 (or3), capped at x2 (in short - if below montly moving average, buy more, if above, buy less).
```
Set Variable
```
```
BTC_BAG
```
```
balance("BTC") + ((floor(((get_variable("!DCA_Speed")*get_variable("!USD_Total")*get_variable("-BTC_Order_size")*min(2,(average_price("BTC/USD", 40320)/price("BTC/USD"))**3))/price("BTC/USD"))*(1/get_variable("-BTC_Minimal_trad_size")))/(1/get_variable("-BTC_Minimal_trad_size")))) if get_variable("*BTC_OS") == 0 else get_variable("BTC_BAG")
```
### Action 2   // Place buy order
```
Place Custom Order
```
```
Limit Order | Buy | BTC_USD
```
```
floor(((get_variable("!DCA_Speed")*get_variable("!USD_Total")*get_variable("-BTC_Order_size")*min(2,(average_price("BTC/USD", 40320)/price("BTC/USD"))**3))/price("BTC/USD"))*(1/get_variable("-BTC_Minimal_trad_size")))/(1/get_variable("-BTC_Minimal_trad_size")) if get_variable("*BTC_OS") == 0 else get_variable("BTC_BAG") - balance("BTC")
```
```
bid_price("BTC/USD")
```
```
+postOnly | +cancelAndPlaceANewOrder
```
### Action 3   // Provide a way to distinguish first run from consecutive runs 
```
Set Variable
```
```
*BTC_OS
```
```
1
```
### Action 4   // Update average buy price (add USD and BTC to USD/BTC equation)
```
Set Variable
```
```
@BTC_avbp
```
```
(get_variable("!BTC_Total")*get_variable("@BTC_avbp")+(get_variable("!DCA_Speed")*get_variable("!USD_Total")*get_variable("-BTC_Order_size")*min(2,(average_price("BTC/USD",40320)/price("BTC/USD"))**3)))/(get_variable("!BTC_Total")+((get_variable("!DCA_Speed")*get_variable("!USD_Total")*get_variable("-BTC_Order_size")*min(2,(average_price("BTC/USD", 40320)/price("BTC/USD"))**3))/price("BTC/USD"))) if get_variable("*BTC_OS") == 0 else get_variable("@BTC_avbp")
```