### Trigger   // once a day
```
hour == 10 and minute == 30
```
### Action 1
```
Pause Rule
```
```
Current Rule
```
```
1 | Hours
```
### Action 2
```
Set Variable
```
```
%BTC_takeprof
```
```
max(0.05,get_variable("%BTC_takeprof")*(1-get_variable("!DCA_Speed")))
```
### Action 3
```
Set Variable
```
```
%MATIC_takeprof
```
```
max(0.1,get_variable("%MATIC_takeprof")*(1-get_variable("!DCA_Speed")))
```