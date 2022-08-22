### Trigger 

```
true
```
### Action 1  // once an hour
```
Pause Rule
```
```
Current Rule
```
```
1 | Hours
```
### Action 2  // Compute dry powder amount
```
Set Variable
```
```
!USD_Total
```
```
floor(balance("USD")+get_variable("!USD_Nexo")+get_variable("!USD_Kucoin")+balance("EUR")*price("EUR/USD")+balance("USDT")+balance("PAXG")*price("PAXG/USD"))
```