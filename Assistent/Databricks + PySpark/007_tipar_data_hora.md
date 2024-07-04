# Tipagem de Data e Hora em PySpark

Neste documento, abordaremos como converter strings em tipos de dados de data e hora em um DataFrame utilizando PySpark. Serão apresentados métodos tanto com SQL quanto com comandos nativos do PySpark.

## Criação dos DataFrames

Primeiro, vamos criar três DataFrames com diferentes formatos de data e hora:

```python
# Criando DataFrames com diferentes formatos de data e hora
df_datas_1 = spark.createDataFrame(
    ["2021-07-05T10:00:00.000+0000", "2020-12-05T00:09:00.000+0000", "2017-02-23T16:23:00.000+0000"],
    "string"
).toDF("datas")

df_datas_2 = spark.createDataFrame(
    ["2021-07-05 10:00", "2020-12-05 00:09", "2017-02-23 16:23"],
    "string"
).toDF("datas")

df_datas_3 = spark.createDataFrame(
    ["05/07/2021 10:21", "05/12/2020 09:11", "23/02/2017 14:30"],
    "string"
).toDF("datas")

# Exibindo os DataFrames criados
display(df_datas_1)
display(df_datas_2)
display(df_datas_3)
```

## Criação de Tabelas Temporárias

Para utilizar comandos SQL, primeiro precisamos criar visões temporárias dos DataFrames:

```python
# Criando visões temporárias
df_datas_1.createOrReplaceTempView("datas_1")
df_datas_2.createOrReplaceTempView("datas_2")
df_datas_3.createOrReplaceTempView("datas_3")
```

## Conversão de Data e Hora com SQL

### Exemplo de Conversão para Data com SQL

```python
# Exemplo de conversão para data utilizando SQL
%sql
SELECT
    CAST(datas AS DATE)
FROM
    datas_1
```

### Exemplo de Conversão para Timestamp com SQL

```python
# Exemplo de conversão para timestamp utilizando SQL
%sql
SELECT
    TO_TIMESTAMP(datas)
FROM
    datas_1
```

Para armazenar o resultado em um novo DataFrame:

```python
# Executando a conversão para timestamp e armazenando o resultado em um novo DataFrame
df_datas_1_tip = spark.sql("""
SELECT
    TO_TIMESTAMP(datas) AS datas
FROM
    datas_1
""")

# Exibindo o DataFrame resultante
display(df_datas_1_tip)
```

### Conversão de Data e Hora em Formato Brasileiro com SQL

Para o terceiro DataFrame, onde as datas estão no formato brasileiro (dd/MM/yyyy HH:mm):

```python
# Exemplo de conversão para timestamp utilizando SQL com formato brasileiro
%sql
SELECT
    TO_TIMESTAMP(datas, "dd/MM/yyyy HH:mm")
FROM
    datas_3
```

Para armazenar o resultado em um novo DataFrame:

```python
# Executando a conversão para timestamp com formato brasileiro e armazenando o resultado em um novo DataFrame
df_datas_3_tip = spark.sql("""
SELECT
    TO_TIMESTAMP(datas, "dd/MM/yyyy HH:mm") AS datas
FROM
    datas_3
""")

# Exibindo o DataFrame resultante
display(df_datas_3_tip)
```

## Conversão de Data e Hora com PySpark

### Conversão de Data e Hora com PySpark

Para aplicar a conversão utilizando comandos nativos do PySpark:

```python
# Importando as funções necessárias
from pyspark.sql.functions import to_timestamp, to_date

# Conversão de datas no primeiro DataFrame
df_datas_1_spark = df_datas_1.withColumn("datas", to_timestamp("datas"))

# Conversão de datas no segundo DataFrame
df_datas_2_spark = df_datas_2.withColumn("datas", to_date("datas"))

# Conversão de datas no terceiro DataFrame com formato brasileiro
df_datas_3_spark = df_datas_3.withColumn("datas", to_timestamp("datas", "dd/MM/yyyy HH:mm"))

# Exibindo os DataFrames resultantes
display(df_datas_1_spark)
display(df_datas_2_spark)
display(df_datas_3_spark)
```

### Explicação das Funções

- `to_timestamp(column, format=None)`: Converte uma coluna de string em um timestamp. Se um formato específico for fornecido, a função usará esse formato.
- `to_date(column, format=None)`: Converte uma coluna de string em uma data. Similarmente, utiliza um formato específico se fornecido.
- `CAST(column AS TYPE)`: Converte uma coluna para um tipo de dado específico utilizando SQL.

---

### Considerações finais

Com esses exemplos, você pode converter strings em tipos de dados de data e hora em DataFrames utilizando tanto SQL quanto comandos nativos do PySpark, permitindo manipulação flexível e precisa de dados temporais.
