# Métodos de Select com PySpark

Este documento demonstra como realizar operações de seleção de dados em um DataFrame utilizando PySpark, tanto com comandos SQL quanto com comandos nativos do PySpark.

## Criação de um DataFrame

Primeiro, vamos criar um DataFrame a partir de um arquivo CSV:

```python
# Importando o módulo necessário
from pyspark.sql import SparkSession

# Lendo o arquivo CSV e criando o DataFrame
df_carros = spark.read.format("csv").option("header", True).load("Caminho/do/arquivo.csv")

# Exibindo o DataFrame
display(df_carros)
```

## Seleção com Comando SQL

Para utilizar comandos SQL, primeiro precisamos criar uma visão temporária do DataFrame:

```python
# Criando uma visão temporária chamada "carros"
df_carros.createOrReplaceTempView("carros")
```

Podemos então executar uma consulta SQL para selecionar dados:

```python
# Exemplo de consulta SQL
%sql
SELECT
  * -- O * seleciona todas as colunas, pode-se especificar colunas desejadas e usar alias com AS
FROM
  carros
```

Para armazenar o resultado da consulta em um novo DataFrame:

```python
# Executando a consulta SQL e armazenando o resultado em um novo DataFrame
df_carros_sql = spark.sql("""
SELECT
  * -- O * seleciona todas as colunas, pode-se especificar colunas desejadas e usar alias com AS
FROM
  carros
""")

# Exibindo o novo DataFrame
display(df_carros_sql)
```

## Seleção com Comando PySpark

Para realizar uma seleção utilizando comandos nativos do PySpark:

```python
# Importando a função col para selecionar colunas
from pyspark.sql.functions import col

# Selecionando colunas específicas e renomeando-as com alias
df_carros_spark = df_carros.select(
    col("modelo_carro").alias("modelo"), 
    col("id_carro").alias("Id")
)

# Exibindo o DataFrame resultante
display(df_carros_spark)
```
