### Trigger
```
get_variable("BTC_LASTSELL") == 1
```
### Action 1   // Reset expected balance
```
Set Variable
```
```
BTC_SELLBAG
```
```
9999999999
```
### Action 2   // Reset rule 1/2 variable
```
Set Variable
```
```
BTC_LASTSELL
```
```
0
```
### Action 3   // Update total balance
```
Set Variable
```
```
!BTC_Total
```
```
balance("BTC")+get_variable("!BTC_Nexo")+get_variable("!BTC_Kucoin")+get_variable("!BTC_Wallets")
```