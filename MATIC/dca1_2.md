### Trigger   // At specific hour, until expected balance met
```
day_of_week == 1 and hour == 9 and balance("MATIC") < get_variable("MATIC_BAG")
```
### Action 1   // Set expected balance. Take dry powder level, multiply by DCA_speed variable (ex. 2%/day), multiply by MATIC_Order_size variable (ex. set MATIC to be 0.25 of your portfolio), multiply by modifier computed from relation of price to average price of last month to the power of 2 (or 3), capped at 2 (in short - if below montly moving average, buy more, if above, buy less).
```
Set Variable
```
```
MATIC_BAG
```
```
balance("MATIC") + ((ceil(((get_variable("!DCA_Speed")*7*get_variable("!USD_Total")*get_variable("-MATIC_Order_size")*min(2,(average_price("MATIC/USD", 40320)/price("MATIC/USD"))**2))/price("MATIC/USD"))*(1/get_variable("-MATIC_Minimal_trad_size")))/(1/get_variable("-MATIC_Minimal_trad_size")))) if get_variable("*MATIC_OS") == 0 else get_variable("MATIC_BAG")
```
### Action 2   // Place buy order
```
Place Custom Order
```
```
Limit Order | Buy | MATIC_USD
```
```
ceil(((get_variable("!DCA_Speed")*7*get_variable("!USD_Total")*get_variable("-MATIC_Order_size")*min(2,(average_price("MATIC/USD", 40320)/price("MATIC/USD"))**2))/price("MATIC/USD"))*(1/get_variable("-MATIC_Minimal_trad_size")))/(1/get_variable("-MATIC_Minimal_trad_size")) if get_variable("*MATIC_OS") == 0 else get_variable("MATIC_BAG") - balance("MATIC")
```
```
bid_price("MATIC/USD")
```
```
+postOnly | +cancelAndPlaceANewOrder
```
### Action 3   // Provide a way to distinguish first run from consecutive runs 
```
Set Variable
```
```
*MATIC_OS
```
```
1
```
### Action 4   // Update average buy price (add USD and MATIC to USD/MATIC equation)
```
Set Variable
```
```
@MATIC_avbp
```
```
(get_variable("!MATIC_Total")*get_variable("@MATIC_avbp")+(get_variable("!DCA_Speed")*7*get_variable("!USD_Total")*get_variable("-MATIC_Order_size")*min(2,(average_price("MATIC/USD",40320)/price("MATIC/USD"))**2)))/(get_variable("!MATIC_Total")+((get_variable("!DCA_Speed")*7*get_variable("!USD_Total")*get_variable("-MATIC_Order_size")*min(2,(average_price("MATIC/USD", 40320)/price("MATIC/USD"))**2))/price("MATIC/USD"))) if get_variable("*MATIC_OS") == 0 else get_variable("@MATIC_avbp")
```