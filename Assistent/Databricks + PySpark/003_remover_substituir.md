# Remoção de Linhas Duplicadas e Substituição de Valores com PySpark

Neste documento, abordaremos como remover linhas duplicadas e substituir valores em colunas de um DataFrame utilizando PySpark. Serão apresentados métodos tanto com SQL quanto com comandos nativos do PySpark.

## Criação de um DataFrame

Primeiro, vamos criar um DataFrame a partir de um arquivo CSV:

```python
# Lendo o arquivo CSV e criando o DataFrame
df_carros = spark.read.format("csv").option("header", True).load("caminho/do/arquivo/csv")

# Exibindo as linhas onde id_carro é igual a '1'
display(
    df_carros.where("id_carro = '1'")
)
```

## Criação de uma Visão Temporária

Para utilizar comandos SQL, primeiro precisamos criar uma visão temporária do DataFrame:

```python
# Criando uma visão temporária chamada "carros"
df_carros.createOrReplaceTempView("carros")
```

## Contagem de Linhas

Para contar o número total de linhas e o número de linhas distintas no DataFrame, podemos utilizar comandos SQL:

```python
# Contagem do número total de linhas
%sql
SELECT COUNT(*) FROM carros

# Contagem do número de linhas distintas
%sql
SELECT COUNT(DISTINCT *) FROM carros
```

## Substituição de Valores em Colunas

### Substituição com Comandos SQL

Podemos utilizar comandos SQL para substituir valores em uma coluna. Neste exemplo, vamos remover o símbolo '$' da coluna `preco`:

```python
# Exemplo de substituição de valores com SQL
%sql
SELECT
    REPLACE(preco, '$', '') AS preco -- Substitui '$' por uma string vazia
FROM
    carros
```

Para armazenar o resultado em um novo DataFrame:

```python
# Executando a substituição e armazenando o resultado em um novo DataFrame
df_carros_sql = spark.sql("""
SELECT
    REPLACE(preco, '$', '') AS preco -- Substitui '$' por uma string vazia
FROM
    carros
""")

# Exibindo o DataFrame resultante
display(df_carros_sql)
```

### Substituição com Comandos PySpark

Para realizar a substituição de valores utilizando comandos nativos do PySpark:

```python
# Importando a função regexp_replace para substituir valores
from pyspark.sql.functions import regexp_replace

# Substituindo '$' por uma string vazia na coluna 'preco'
df_carros_pyspark_substituir = df_carros.withColumn("preco", regexp_replace('preco', '\\$', ''))

# Exibindo o DataFrame resultante
display(df_carros_pyspark_substituir)
```

## Remoção de Linhas Duplicadas

### Remoção com Comandos SQL

Para remover linhas duplicadas com comandos SQL, podemos utilizar a cláusula `DISTINCT`:

```python
# Exemplo de remoção de duplicadas com SQL
%sql
SELECT DISTINCT * FROM carros
```

### Remoção com Comandos PySpark

Para remover linhas duplicadas utilizando comandos nativos do PySpark:

```python
# Removendo linhas duplicadas com o método distinct
df_carros_pyspark_distinct = df_carros.distinct()

# Contando o número de linhas no DataFrame resultante
print(df_carros_pyspark_distinct.count())

# Outra forma de remover duplicatas utilizando o método dropDuplicates
df_carros_pyspark_drop = df_carros.dropDuplicates()

# Exibindo o DataFrame sem duplicatas
display(df_carros_pyspark_drop)
```

---

### Considerações finais
Com esses exemplos, você pode utilizar tanto comandos SQL quanto comandos nativos do PySpark para remover linhas duplicadas e substituir valores em colunas de DataFrames, garantindo flexibilidade e eficiência no processamento de dados.
