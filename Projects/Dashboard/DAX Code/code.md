# VAR Objetivos - Basically just calculating the difference between objectives and actual sales for current year (net sales)

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
    ) // Ca
RETURN
    TotalRealCY – ObjetivosSoFar
