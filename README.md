# DAX
## Obliczanie dni roboczych pomiędzy dwiema datami, bez dni świątecznych
<li> średnia

```
AvgDelivery:=
AVERAGEX ( sales, INT ( sales[delivery date] - sales[order date] ) + 1 )
```

<li> średnia dni robocze
    
```
Avg_Delivery_WD :=
AVERAGEX (
    sales,
    VAR RagneOfDates =
        DATESBETWEEN ( 'calendar1'[Date], [order date], [delivery date] )
    VAR WorkingDates =
        FILTER ( RagneOfDates, NOT ( WEEKDAY ( 'calendar1'[Date] ) IN { 1, 7 } ) )
    VAR NumberOfWorkingDays =
        COUNTROWS ( WorkingDates )
    RETURN
        NumberOfWorkingDays
)
```
Na podstawie dataset KajoData i Kompletny przewodnik po DAX, Marco Russo, Alberto Ferrari.

![Liczba dni roboczych pomiędzy datami bez świąt](https://github.com/user-attachments/assets/d39c2b7a-5b6a-4c92-89b0-f969c989174b)

