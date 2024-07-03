# Utilizando LIKE e BETWEEN com PySpark

Neste documento, abordaremos como utilizar os operadores `LIKE` e `BETWEEN` para realizar filtros em um DataFrame utilizando PySpark. Serão apresentados métodos tanto com SQL quanto com comandos nativos do PySpark.

## Preparação do DataFrame

Primeiro, vamos criar e preparar um DataFrame a partir de um arquivo CSV:

```python
# Importando os módulos necessários
from pyspark.sql.functions import col, regexp_replace
from pyspark.sql.types import IntegerType, DoubleType

# Lendo o arquivo CSV e criando o DataFrame
df_carros = spark.read.format("csv").option("header", True).load("caminho/do/arquivo.csv")

# Removendo o símbolo '$' da coluna 'preco' e alterando os tipos das colunas
df_carros = df_carros.withColumn(
    "preco",
    regexp_replace(col("preco"), "\\$", "").cast(DoubleType())
).select(
    col("id_carro").cast(IntegerType()),
    "modelo_carro",
    col("preco"),
    col("cod_marca").cast(IntegerType())
)

# Exibindo o DataFrame preparado
display(df_carros)
```

## Criação de uma Tabela Temporária

Para utilizar comandos SQL, primeiro precisamos criar uma visão temporária do DataFrame:

```python
# Criando uma visão temporária chamada "carro"
df_carros.createOrReplaceTempView("carro")
```

## Filtros com LIKE

### Filtros com LIKE utilizando SQL

O operador `LIKE` é utilizado para buscar padrões em strings. Aqui estão alguns exemplos de como utilizá-lo:

```python
# Exemplos de filtros com LIKE utilizando SQL
%sql
SELECT
    *
FROM
    carro
WHERE
    modelo_carro LIKE "%alo%" -- Resultados com 'alo' no meio: ex. Avalon

SELECT
    *
FROM
    carro
WHERE
    modelo_carro LIKE "%rt" -- Resultados com 'rt' no final: ex. Escort, Sport

SELECT
    *
FROM
    carro
WHERE
    modelo_carro LIKE "Go%" -- Resultados com 'Go' no início: ex. Gol, Golf
```

Para armazenar o resultado em um novo DataFrame:

```python
# Executando filtros com LIKE e armazenando o resultado em um novo DataFrame
df_carros_sql_like = spark.sql("""
SELECT
    *
FROM
    carro
WHERE
    modelo_carro LIKE "%alo%" -- Resultados com 'alo' no meio: ex. Avalon
OR
    modelo_carro LIKE "%rt" -- Resultados com 'rt' no final: ex. Escort, Sport
OR
    modelo_carro LIKE "Go%" -- Resultados com 'Go' no início: ex. Gol, Golf
""")

# Exibindo o DataFrame resultante
display(df_carros_sql_like)
```

### Filtros com LIKE utilizando PySpark

Para aplicar filtros utilizando o operador `LIKE` com comandos nativos do PySpark:

```python
# Aplicando filtros com LIKE utilizando PySpark
df_carros_spark_like = df_carros.where(
    col("modelo_carro").like("%alo%")
)

# Exibindo o DataFrame resultante
display(df_carros_spark_like)
```

## Filtros com BETWEEN

### Filtros com BETWEEN utilizando SQL

O operador `BETWEEN` é utilizado para buscar valores dentro de um intervalo especificado:

```python
# Exemplo de filtro com BETWEEN utilizando SQL
%sql
SELECT
    *
FROM
    carro
WHERE
    preco BETWEEN 50000 AND 75000
```

Para armazenar o resultado em um novo DataFrame:

```python
# Executando o filtro com BETWEEN e armazenando o resultado em um novo DataFrame
df_carros_sql_between = spark.sql("""
SELECT
    *
FROM
    carro
WHERE
    preco BETWEEN 50000 AND 75000
""")

# Exibindo o DataFrame resultante
display(df_carros_sql_between)
```

### Filtros com BETWEEN utilizando PySpark

Para aplicar filtros utilizando o operador `BETWEEN` com comandos nativos do PySpark:

```python
# Aplicando filtros com BETWEEN utilizando PySpark
df_carros_spark_between = df_carros.where(
    col("preco").between(50000, 75000)
)

# Exibindo o DataFrame resultante
display(df_carros_spark_between)
```

---

### Considerações finais

Com esses exemplos, você pode utilizar tanto comandos SQL quanto comandos nativos do PySpark para aplicar filtros em DataFrames utilizando os operadores `LIKE` e `BETWEEN`, permitindo flexibilidade e precisão na busca por padrões e intervalos de valores.
