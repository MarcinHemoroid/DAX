# DAX
## Obliczanie dni roboczych pomiędzy dwiema datami, bez dni świątecznych
<li> średnia liczba dni

```
AvgDelivery:=
AVERAGEX ( sales, INT ( sales[delivery date] - sales[order date] ) + 1 )
```

<li> średnia liczba dni, dni robocze
    
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


![Liczba dni roboczych pomiędzy datami bez świąt](https://github.com/user-attachments/assets/d39c2b7a-5b6a-4c92-89b0-f969c989174b)

<li> średnia liczba dni, dni robocze z uwzględnieniem świąt
    
```
M5_AvgDelivery WD DT =
AVERAGEX (
    sales,
    VAR RangeOfDates =
        DATESBETWEEN ( calendar1[date], sales[order date], sales[delivery date] )
    VAR NumberOfWorkingDays =
        CALCULATE (
            COUNTROWS ( calendar1 ),
            RangeOfDates,
            NOT ( WEEKDAY ( calendar1[date] ) IN { 1, 7 } ),
            calendar1[is holiday] = 0
        )
    RETURN
        NumberOfWorkingDays
)

```

![Liczba dni roboczych pomiędzy datami z uwzględnieniem świąt](https://github.com/user-attachments/assets/98eaf19c-a7c5-4c72-bd97-e9613b5604e0)


Na podstawie dataset KajoData i Kompletny przewodnik po DAX, Marco Russo, Alberto Ferrari.

## Pokazywanie łącznie danych budżetu i sprzedaży

<li> Pytanie biznesowe: jaka powinna być skorygowana prognoza budżetu znając rzeczywistą sprzedaż od początku roku do 15 sierpnia i znając założenia budżetowe do końca roku.
    
```
AjustedBudgetOptimized =
VAR LastDatewithSales =
    CALCULATE ( MAX ( sale[OrderDateKey] ), ALL ( sale ) )
VAR SalesAmount = [Sale Amnt]
VAR BudgetAmount =
    CALCULATE ( [Budget Amnt], KEEPFILTERS ( 'date'[DateKey] > LastDatewithSales ) )
VAR AdjustedBudget = SalesAmount + BudgetAmount
RETURN
    AdjustedBudget
```

![screen pokazywanie łącznie danych budżetu i sprzedaży](https://github.com/user-attachments/assets/48c55c79-0484-4c2b-9c9a-198fc091aa51)

Na podstawie dataset Kompletny przewodnik po DAX, Marco Russo, Alberto Ferrari.





