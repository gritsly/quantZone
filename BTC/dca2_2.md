### Trigger   // When rule 1/2 does not trigger
```
hour != 1 and get_variable("*BTC_OS") == 1
```
### Action 1   // Reset expected balance
```
Set Variable
```
```
BTC_BAG
```
```
9999999999
```
### Action 2   // Reset rule 1/2 variable
```
Set Variable
```
```
*BTC_OS
```
```
0
```
### Action 3
```
Set Variable
```
```
!BTC_Total
```
```
balance("BTC")+get_variable("!BTC_Nexo")+get_variable("!BTC_Kucoin")+get_variable("!BTC_Wallets")
```