### Trigger   // When rule 1/2 does not trigger
```
hour != 9 and get_variable("*MATIC_OS") == 1
```
### Action 1   // Reset expected balance
```
Set Variable
```
```
MATIC_BAG
```
```
9999999999
```
### Action 2   // Reset rule 1/2 variable
```
Set Variable
```
```
*MATIC_OS
```
```
0
```
### Action 3
```
Set Variable
```
```
!MATIC_Total
```
```
balance("MATIC")+get_variable("!MATIC_Nexo")+get_variable("!MATIC_Kucoin")+get_variable("!MATIC_Wallets")
```