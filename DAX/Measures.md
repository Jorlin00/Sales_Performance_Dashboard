DAX Measures Used

Total Sales =SUM(FSales[SalesAmount])
Total Profit =SUM(FSales[Profit])
Profit Margin % =DIVIDE([Total Profit],[Total Sales],0)
Total Orders =COUNTROWS(FSales)
Total Customers =DISTINCTCOUNT(DCustomer[CustomerKey])
Return Rate % =DIVIDE(SUM(FReturns[ReturnQuantity]),SUM(FSales[OrderQuantity]),0)

Previous Year Sales =CALCULATE([Total Sales],SAMEPERIODLASTYEAR(DCalendar[Date]))

Total Products =DISTINCTCOUNT(DProduct[ProductKey])
Total Quantity Sold =SUM(FSales[OrderQuantity])
Average Selling Price =DIVIDE([Total Sales],[Total Quantity Sold],0)

Orders per Customer =DIVIDE([Total Orders],[Total Customers],0)
Average Revenue per Customer =DIVIDE([Total Sales],[Total Customers],0)

Total Regions =DISTINCTCOUNT(DTerritory[Country])

DCalendar =
VAR MinDate =MINX (UNION
(
SELECTCOLUMNS ( FSales, "Date", FSales[OrderDate] ),
SELECTCOLUMNS ( FReturns, "Date", FReturns[ReturnDate] )
),
[Date]
)
VAR MaxDate =MAXX (UNION
(
SELECTCOLUMNS ( FSales, "Date", FSales[OrderDate] ),
SELECTCOLUMNS ( FReturns, "Date", FReturns[ReturnDate] )
),
[Date]
)
RETURN
ADDCOLUMNS (CALENDAR ( MinDate, MaxDate ),
"Year", YEAR ([Date]),
"Quarter", "Q" & QUARTER ([Date]),
"Month", FORMAT ([Date], "MMM"),
"Month No", MONTH ([Date]),
"Year Month", FORMAT ([Date], "MMMYY"),
"Year Month Sort", YEAR ([Date]) * 100 + MONTH ([Date])
)

Dynamic Title =
VAR _Year =IF(HASONEVALUE(DCalendar[Year]),SELECTEDVALUE(DCalendar[Year]),BLANK())
VAR _Category=IF(HASONEVALUE(DProductCategory[CategoryName]),SELECTEDVALUE(DProductCategory[CategoryName]),BLANK())
VAR _Result =CONCATENATEX(FILTER(
{
("Year", _Year),
("Category", _Category)
},
[Value2] <> BLANK()),
[Value2],
" • ")
RETURN IF(Result = "","Showing: All Data","Showing: " & _Result)

