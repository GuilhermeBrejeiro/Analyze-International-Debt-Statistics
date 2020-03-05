# Premissas

Esse projeto consiste na análise das dívidas adquiridas mundialmente. 
Países tem necessidades de impulsionar sua economia e, muitas vezes, utilizar créditos acaba sendo uma alternativa. Os dados utilizados nesse projeto foram gerados pelo Banco Mundial, que é a organização que efetua esses emprestimos. 
Para entender os motivos que levam um país a contrair uma dívida internacional, serão avaliados os seguintes pontos:
* Qual o valor total que o banco emprestou aos países desse dataset
* Qual país possue o maior débito e qual a quantia desse débito
* Quais os montantes totais por tipo de débito


# Variáveis

O arquivo analisado possui cinco colunas:
1. country_name -- Nome por extenso dos países
2. country_code -- Código de cada país
3. indicator_name -- nome do indicador (motivo da necessidade de contrair a divida) 
4. indicator_code -- código do indicador
5. debt -- valor da dívida (em dólar americano)

### Países

```
%%sql
SELECT 
     COUNT(DISTINCT country_name)  AS total_distinct_countries
FROM international_debt;
```

Existem 124 países listados no arquivo

### Indicadores

```
%%sql
SELECT
    DISTINCT indicator_code AS distinct_debt_indicators
FROM international_debt 

ORDER BY distinct_debt_indicators
```
Existem 25 diferentes indicadores

# Valor distribuído em empréstimos pelo Banco Mundial

```
%%sql
SELECT 
    ROUND(SUM(debt)/1000000, 2)  AS total_debt
FROM international_debt;
```
Para melhor visualização o valor foi dividido por milhão. O valor total de empréstimos gira em torno de 3079734 milhões

# País com maior empréstimo

```
%%sql
SELECT 
    country_name, 
    SUM(debt) AS total_debt
FROM international_debt
GROUP BY country_name
ORDER BY total_debt DESC
LIMIT 1;
```
A China tem o maior empréstimo entre todos os países análisados, um valor total de USD 285.793.494.734,20

# Principais indicadores
```
%%sql
SELECT 
    indicator_code AS debt_indicator,
    indicator_name,
    AVG(debt) AS average_debt
FROM international_debt
GROUP BY debt_indicator, indicator_name
ORDER BY average_debt DESC
LIMIT 10;
```

* Principal repayments on external debt, long-term - US$ 5161194333.812658349
* Disbursements on external debt, long-term - US$ 1958983452.859836046
* PPG, private creditors - US$ 1644024067.650806481
* PPG, bilateral - US$ 1220410844.421518983
* PPG, official creditors - US$ 1082623947.653623188


Podemos ver os principais motivos que levam os países a pedirem empréstimo. Existe uma queda abrupta entre o primeiro e o segundo indicador e do segundo para o terceiro também é uma queda considerável, indicando que esses são os principais motivos para os países contraírem dividas com o Banco Mundial.

