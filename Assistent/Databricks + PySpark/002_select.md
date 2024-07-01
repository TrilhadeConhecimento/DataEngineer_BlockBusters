# Métodos de Select e Filtros com PySpark

Este documento demonstra como realizar operações de seleção de dados e aplicar filtros em um DataFrame utilizando PySpark, tanto com comandos SQL quanto com comandos nativos do PySpark.

## Criação de um DataFrame

Primeiro, vamos criar um DataFrame a partir de um arquivo CSV:

```python
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

## Aplicação de Filtros

### Filtros com Comandos SQL

Para aplicar filtros utilizando comandos SQL:

```python
# Exemplo de filtro com SQL
%sql
SELECT * FROM carros
WHERE id_carro = 1
AND modelo_carro = "Golf" -- Pode-se usar AND ou OR
```

Para armazenar o resultado filtrado em um novo DataFrame:

```python
# Executando a consulta SQL com filtros e armazenando o resultado em um novo DataFrame
df_carros_sql_filtrado = spark.sql("""
SELECT * FROM carros
WHERE id_carro = 1
OR modelo_carro = "Golf" -- Pode-se usar AND ou OR
""")

# Exibindo o DataFrame filtrado
display(df_carros_sql_filtrado)
```

### Filtros com Comandos PySpark

Para aplicar filtros diretamente com comandos nativos do PySpark:

```python
# Utilizando o método where
display(
    df_carros.where("id_carro = '1'")
)

# Utilizando o método filter
display(
    df_carros.filter("id_carro = '1'")
)

# Utilizando lógicas (AND, OR, NOT) - não esqueça de importar 'col'
from pyspark.sql.functions import col

display(
    df_carros.where(
        (col("id_carro") == "1") | (col("modelo_carro") == "Golf")
    )
)

# Outras formas de filtrar
display(
    df_carros[df_carros.id_carro == '1']
)

display(
    df_carros[df_carros["id_carro"] == '1']
)
```

Para criar um DataFrame a partir dos resultados filtrados:

```python
# Filtrando e criando um novo DataFrame
df_carros_spark_filtrado = df_carros.where("id_carro = '1'")

# Exibindo o DataFrame filtrado
display(df_carros_spark_filtrado)
```

---

### Considerações finais

Com esses exemplos, você pode utilizar tanto comandos SQL quanto comandos nativos do PySpark para selecionar e filtrar dados em DataFrames, permitindo flexibilidade e eficiência no processamento de dados
