# Power BI Sales Dashboard

## Introduction and Data Overview

In this project, my goal is to make a dashboard representing the sales of a retail company that has operations in Portugal! It has 5 different departments that feed certain districts in a specific manner. Our goal is to have a general view of the sales of the company, by product, by segment, by district and by department. This data is fictional and created by myself. 
Our source table is the result of the merge of 4 different tables: the sales table, the products table, the department table and the customers table. The schema of the sales table is:
-	Cliente: (the client that the invoice is related to);
-	Produto: (the product invoiced in that transaction);
-	Janeiro – Dezembro: 12 Different columns for the months with the Net Sales as the values (First operation was to unpivot all these column into Attribute = “Month” and Values = Sum of “Net Sales”.);
-	Ano: A column that reports the year in which that transaction was made , since it has some history values about other years. It can assume values “YTD”, which refers to those year’s sales, FY24, sales of fiscal year 2024, FRCST, forecasted values for sales this year, and OBJ25, goal values for this fiscal year;

Customer table Schema:
-	Cliente (Client Name);
-	Distrito (District where the customer is from)

Product Table Schema:
-	Produto;
-	Segmento Produto (the segment that a certain product is in)

Department Table Schema:
-	Departamento (Departmant Name, which is basically the district where it is located);
-	Distrito (The districts that that specific department makes sales to. No district has more that one department but each department feeds more than one district generally)

## Data Transformation

Since I’ll be doing this project with Power BI, basically I have two options to merge all the available data: either load all tables into Power BI and merged them based on the common columns, or work it with Power Query (inside or outside the Power BI desktop, since it has Power Query incorporated in it). 
Since I need to make a few transformations in the data that will require the use of Power Query, like the unpivoting of the Months, like stated previously, I will be making all the operations with it and turn it into a single table with all the columns that we need.
So first of all let’s load all of our four tables into Power Query. Then, let’s start by the sales table. Like previously said, we need to unpivot the months columns because we don’t want a different column for each month, but instead one for all of them, with an adjacent column with the net sales values for each month, client, year, etc.

So for that, we basically just need to select all the columns, then go to Transform  Unpivot Columns. Now, we also want to add some more values to the “Ano” column, because we actually want to create variables to compare our sales from current year to our objectives, to what we had forecasted and to the sales from the previous year. 
To do that, we need to do the reverse action we made for months, as we actually need to pivot the “Ano” column for the different values of it to become separate columns. The final goal is to, when they are separated, the perform a subtraction operation of one and other. After pivoting, for the column where we compare the current year with previous year (let’s call it YTD – FY24), we will go to Add Column  Custom Column. The formula for this column will be pretty straight forward: [YTD] - [FY24]. 

We can do this for all the variables we want, since the calculations are always a simple subtraction with YTD and the variable we want to compare it with. Then, only need to unpivot all those column again into Attribute = Ano and Values = Net Sales, without forgetting to aggregate with Sum.
And with this the first part is finished, since the original columns of the sales   table are now disposed in a way that will be easier to work. Now what we need to do is to join the rest of the columns that are not native to this table, like the district, product segment, department, etc.

This is actually going to be quite simple. What we need to do first is to join the customer table with the department table, since there are no common column in department and sales table. To do that, we do a left join with the customer table as our left table and department table as our right table, on customer.distrito = department.distrito. Now we can expand the columns Department Name from the result of our merged queries, which basically did this: If this client is in this district, which is supplied by this department, then the supplier of that client is this department.
Now we can merge our sales table with the result of these merged tables based on column Cliente they have in common and expand the columns we want, which are Distrito and Department. Let’s also join table product on column Produto, expand Segmento and we now have all the columns we need in our sales table so we can actually start building the dashboard!

 ![image](https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Dashboard/Images/1.png)

And here it is! Quite fast, right?? I think there is no need to go into detail on the operations performed to get this final result because in general, Power BI is quite intuitive and simple. So, in summary, only a few adaptations to get the final result as we want it: change the Distrito column into a geographical type to be able to display it as the map we have there in the middle, a few conditional formatting to display the negative values as red and positive as green like you can see in the table. I don’t really think there is much more to say about this part, only pick the right type of chart to display your data in the most clear possible way and it’s good to go! So, in general, what we have is a display of the sales data by month, by quarter, by value of column Year, about the product segment, clients and departments. So everything we have in our table is now displayed in this dashboard. Power BI has a feature that filters data when you click the different features of the dashboard. So, by chance, if you click on the bar of Lisboa department, all the data that will be shown highlighted is relative to the Lisboa department, so there is no need to set filters or turn on the use as filter option like you’d have to do if you were working with tableau. 

## Analysis

However this may be, there are other things that we can still do, some questions we might have by any chance and for that we can use a different tool of Power BI, DAX! Eventhough we already have created a column that compares our sales of current year with the objectives set, let’s create a variable that we can use to compare how sales are going according to what was initially set by the company by brand, by segment and anything else we want to, and just leave our dashboard as it is. To do that, we will create a new measure called “Objetivos” with the following formula:

### DAX:
```
Objetivos = 
VAR TotalRealCY =
CALCULATE(
    SUM('Vendas[NS]),
    'Vendas[Ano]="YTD_25",
    'Vendas[Mês.1]<6
    )
VAR ObjetivosSoFar = 
CALCULATE(
    SUM('Vendas[NS]),
    'Vendas[Mês.1]<6,
    'Vendas[Ano]="OBJ_25"
    )
RETURN
    TotalRealCY – ObjetivosSoFar
```
    
Pretty much just creating two variables that sum our sales this year, the objectives initially set and the subtract them! Only need to specify in the formula that the months we are looking at have got to have value less than 6, since we only have data for 5 months because the current year is not yet finished, however, the objectives were set to all year and so we need to make that specification. So let’s use this measure to look at different things that might pop up, like the Products:

![image](https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Dashboard/Images/2.png)
 
Now we can simply use this to guide ourselves in terms of so far this year was not as expected at first, and we can apply this to different parameters such as the departments, the client, you name it! By creating different visuals or simply toggling the parameter you want to check instead of Brands. So let’s go ahead and create more of these metrics that are quite useful to analyze your business results! Let’s create one that allows to see the growth, in percentage, from our current year so far to last year at the same stage:

### DAX:

```
Crescimento NetSales % = 
VAR NetSales25 =
    CALCULATE(
        SUM('Vendas[NS]), 
        'Vendas[Ano]="YTD_25",
        'Vendas[Mês.1]<6
        )
VAR NetSales24 =
    CALCULATE(
        SUM('Vendas[NS]), 
        'Vendas[Ano]="FY24",
        'Vendas[Mês.1]<6
        )
RETURN
    IF(
        NetSales24 <>0,
        (NetSales25-NetSales24)/NetSales24,
        BLANK()
    )
```

![image](https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Dashboard/Images/3.png)
 

Like we can see, the growth by department, which will of course affect the overall results of the company, is actually negative, which is of course bad since we were aiming to obtain better results than last year, and not the other way around. So based on that, let’s try to predict how the rest of the year will be in terms of performance, based on the first 5 months that we already have.
First of all, we have to keep in mind that there might be seasonality associated to the business, so let’s use the last year, since we have data to the whole year, to try to create a seasonality index:

### DAX:

```
IIndiceSazonalidade = 
VAR TotalVendas2024 =
CALCULATE(
    SUM(Vendas[NS]),
    Vendas[Ano]="FY24",
    ALL(Vendas[Mês.1])
)
VAR VendasMes2024 =
CALCULATE(
    SUM(Vendas[NS]),
    Vendas[Ano]="FY24"
)
VAR Season_Index =
DIVIDE(VendasMes2024,TotalVendas2024)
RETURN Season_Index
```

So basically just dividing each month by the total sum of sales from last year, and our seasonality index looks something like this:

![image](https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Dashboard/Images/4.png)
 
Now based on this season index we can calculate a projection of net sales for all the months of our current fiscal year. Since we already have the first 5 months’ values, what we are going to do is: Calculate the sum of net sales so far, divide it by the sum of index for the months already past (0.4) to estimate how much it will be at the end of a full year based on the first 5 months, and then multiply that value for the season index of each month to get the projection of net sales for each month! 

![image](https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Dashboard/Images/5.png)

On the left you can see the projected net sales for each month based on the metric defined earlier and the comparison with the actual net sales so far. As you can see, there are some innacuricies when comparing the values, which might be explained by the seasonality index being calculated using only one year, that is clearly not so representative of how a fiscal year’s sales are distributed. 
Now let’s do something different and create a new variable that categorizes clients. We already have something similar for products, but not for clients. So let’s go ahead and create a new measure:

### DAX:

```
TabelaClientes = 
VAR TotalVendas2024 = CALCULATE(
    SUM(Vendas[NS]),
    Vendas[Ano]="FY24",
    ALL(Vendas)
)

VAR TotalClientes = CALCULATE(DISTINCTCOUNT(Vendas[Clientes.Personalizado.1]),Vendas[Ano] = "FY24",ALL(Vendas))
VAR Media2024 = TotalVendas2024 / TotalClientes
VAR VendasPorCliente = CALCULATE(SUM(Vendas[NS]),Vendas[Ano] = "FY24", ALLEXCEPT(Vendas,Vendas[Clientes.Personalizado.1]))
VAR TotalVendasPorCliente = CALCULATE(
    SUM(Vendas[NS]),
    Vendas[Ano] = "FY24",
    ALLEXCEPT(Vendas,Vendas[Clientes.Personalizado.1])
)
VAR DESVIO = STDEV.P(SomaCliente2024[Vendas])
RETURN 
IF(VendasPorCliente > DESVIO + Media2024, "Top",
IF(TotalVendasPorCliente < Media2024 ,"Budget","Mid"))
```

So basically what we did was calculating the average sales for the year FY24, which consists of summing all the sales for this year (VAR TotalVendas2024) and divide it by the number of companies that made sales in that year (VAR TotalClientes). Then we create a table with the sum of net sales, because we cannot apply the standard deviation formula to a measure, we have to apply it to a table. The table is SomaCliente2024. Then the criteria is : if the sales of a cliente in that year was superior to Average Sales 2024 + Standard Deviation 2024, then it  is a “top” client. If it is less than Average – Standard Deviation, then is is Budget, else is “Mid”. Here’s a clip of the final result:

![image](https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Dashboard/Images/6.png)

However, there’s a problem with this approach: the fluctuation between sales of different customers was far too great (we have a client with sales going up to 12M, as well as clients with sales a lot lower, at 80k this year), which makes it so that the standard deviation of sales is actually a higher value than the average sales per client for that year, which means that no client would land in the “budget” category, since they’d have to register negative values for sales for that to happen. With that in mind, let’s make a few adjustments and tweak the formula  a bit, so just add this to the RETURN  clause, and, while we were at it we took the chance to create more layers for our categorization measure: 

### DAX:
```
RETURN SWITCH(
    TRUE(),
    VendasPorCliente >= Media + 1.5*DESVIO, "Premium",
    VendasPorCliente >= Media + DESVIO, "Top",
    Media < VendasPorCliente < Media + DESVIO, "Importante",
    VendasPorCliente >= Media - 0.5 * DESVIO, "Ramp Up",
    "Cliente Ocasional"
)
```

![image](https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Dashboard/Images/7.png)

Basically, we’re still using the  Average and Standard Deviation values for our sales in FY24 but adapting the calculations to the context. Now that I’m satisfied with the customer categorization, I’ll actually ad this as a new column to our Sales table so just copied the measure formula to the add column option and called it Segmento_Cliente, which is now a part of ou sales table if we want to use that information in the future. Let’s also add this to our dashboard if we need to check out something quickly about this variable:

![image](https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Dashboard/Images/8.png)

By having this in our dashboard and by simply clicking it we can do a lot of quick checks, for example, the ramp up clients represented about 25% of sales last year (like seen above in the picture) but this year so far it represents 59% of the net sales, which can give valuable intel on the differences when comparing to last fiscal year. We could even use this new variable along with the growing percentage from last year that we created earlier in this project to have a more in depth look of what is going on month by month to the months we have so far this year. But what really stands out in the opposite side of things is that the sales for “Top” customers had a variation of -300% when comparing to the same segment of last year! Let’s use the dashboard again to check that out:

![image](https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Dashboard/Images/9.png)
 
Two things that really stand out are:
1 – All the customers of the “Top” Segment are located in Santarém district, so, in a real scenario, we could narrow it down to this specific district and try to understand what could’ve happened, perhaps a new competitor in that district with more aggressive prices to similar products?
2 – One thing that is obvious from the table in the bottom right, is that almost 95% of this variation from last year is explained by the products of only 2 brands: coca cola and nestle!
So let’s imagine this scenario: Our sales team contacted these clients and actually found out that this fall in sales for these specific brands’ products was in fact caused by a new competitor with prices lower than ours, as far as 40% lower. The clients do prefer our products, and so do their customers, but the difference in price is too big to be ignored by both parties. Because of this, one of the customers’ compromised that, since the demand for these products have neither increased nor decreased, if our company lowers the prices in 25%, they would reach the same amount of orders as of last year (of course it’s never as simple as this, but let’s move on). The other client of Santarém district agreed on the same terms but only for the Nestlé products.

To make this a more realistic scenario, let’s add the concepts of margins to our project. Let’s then create a margins column for our Vendas table and consider a GM of 50% for both Nestle and Coca Cola brands currently. Let’s say that for Coca Cola products we can’t go below a margin of 30% and nestlé 37%.
And here it is! All we had to do was creating the variables that were missing like  the Gross Margin and Minimum Gross Margin for each product and also set all the remaining rules, that are explained in more detail in the Code document. We also created a parameter called Discount that changes the results. Our results are shown in % of change from what could have been the sales based on the criteria we set and the actual net sales:

 ### DAX:

 ```
Sales_What_If_Analysis = 
VAR Produto = SELECTEDVALUE(Vendas[Brands.Personalizado])
VAR Cliente = SELECTEDVALUE(Vendas[Clientes.Personalizado.1]) ** [Guarateeing that we can apply criteria to specific clients and products] **
var UnitPrice = [UnitPrice] ## Already defined in a different measure, basically SUM(NS)/SUM(VOLUME)
VAR Volume2024 = CALCULATE(SUM(Vendas[Volume.Volume]),Vendas[Mês] IN {"JAN","FEV","OUT","NOV","DEZ"},Vendas[Ano]="FY24",ALLEXCEPT(Vendas,Vendas[Brands.Personalizado],Vendas[Clientes.Personalizado.1]))
VAR Volume2025 = CALCULATE(SUM(Vendas[Volume.Volume]),Vendas[Ano]="YTD_25",ALLEXCEPT(Vendas,Vendas[Brands.Personalizado],Vendas[Clientes.Personalizado.1]))
VAR VolumePerdido = Volume2024 - Volume2025
VAR GM = 0.5
VAR GM_MIN = 
    SWITCH(
        Produto,
        "COCA COLA", 0.3,
        "Nestlé", 0.37,
        BLANK() 
    )

VAR Recovery_Conditions = 
(Cliente ="CASA GUEDES - GUEDESPRES, LDA" && Produto IN {"Nestlé", "COCA COLA"}) ||
(Cliente = "GARRAFEIRA NACIONAL, LDA" && Produto = "Nestlé")
VAR Discount = SELECTEDVALUE('Parâmetro_Desconto'[Parâmetro_Desconto])
VAR UnitCost = [UnitPrice] * 0.5
VAR MarginWithDiscount = IF(
    Discount <> 0, 
    SWITCH(
        Produto,
        "COCA COLA", DIVIDE(
            [UnitPrice] * (1 - Discount) - UnitCost , [UnitPrice] * (1 - Discount),""),
        "Nestlé", DIVIDE(
            [UnitPrice] * (1 - Discount) - UnitCost , [UnitPrice] * (1 - Discount),"")
    ),
    GM
)

VAR NetSales2025 = CALCULATE(
        SUM(Vendas[NS]),
        Vendas[Ano] = "YTD_25",
        ALLEXCEPT(Vendas,Vendas[Clientes.Personalizado.1],Vendas[Brands.Personalizado])
)

VAR Sales_Recovery = 
    IF(
        Discount <> 0, 
        IF(
            Recovery_Conditions,
            IF(
                MarginWithDiscount >= GM_MIN,
                (Volume2025 + VolumePerdido* (Discount/0.25)) * (1-Discount) * UnitPrice ,
                "Atenção: Margem abaixo do permitido"
                ) 
        ),
       NetSales2025
    )

RETURN IFERROR(DIVIDE(Sales_Recovery - NetSales2025,NetSales2025,BLANK()),BLANK())
```

![image](https://github.com/JJLaRocha/JJLaRocha/blob/main/Projects/Dashboard/Images/10.png)
 
Our parameter was set to maximum value of 0.25, the 25% discount that returns us the same volume sold last year but with loss of 25% profit. You can see above the changes that a 5% discount would cause. The results are very different from one and other, which only tells us of how severe the fall in sales was for each specific client and products from last year to this year. As our final result, we conclude that the best option would be to advance with the 25% discount for coca cola, which would return a 26% increase in net sales, and a 20% increase for Nestlé products (we can only go as far as 20% since the Min GM Margin for this brand does not allow us to offer a 25% discount) would produce an increase of 181% for Garrafeira Nacional and 313% for Casa Guedes! All accounted, this would have produced an increase of Net Sales of 4 million euros! And of course you could change the rules a bit in case some similar situation occurs  (different customers, different products, etc), and you could use this to predict how  things would change in terms of values.
And that’s it for this project! In my opinion these visualization tools are very useful because you can get an overview of your business and then really narrow it down to what matters with further analysis based on the odd things you are noticing. I hope you liked it too!






   <div style="background-color: #FFEB3B; padding: 10px; border-radius: 5px; color: black;"><strong>⚠️ Nota Importante:</strong><br>Este código foi desenvolvido para [explicar o propósito]. Ao usá-lo, tenha em mente que [limitações, versões ou outras observações importantes].</div>






