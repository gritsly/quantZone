### Trigger
```
get_variable("MATIC_LASTSELL") == 1
```
### Action 1   // Reset expected balance
```
Set Variable
```
```
MATIC_SELLBAG
```
```
9999999999
```
### Action 2   // Reset rule 1/2 variable
```
Set Variable
```
```
MATIC_LASTSELL
```
```
0
```
### Action 3   // Update total balance
```
Set Variable
```
```
!MATIC_Total
```
```
balance("MATIC")+get_variable("!MATIC_Nexo")+get_variable("!MATIC_Kucoin")+get_variable("!MATIC_Wallets")
```