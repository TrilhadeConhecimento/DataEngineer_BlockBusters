# Transformação e Tipagem de Colunas com PySpark

Neste documento, abordaremos como transformar e tipar colunas de um DataFrame utilizando PySpark. Serão apresentados métodos tanto com SQL quanto com comandos nativos do PySpark.

## Criação e Transformação de um DataFrame

Primeiro, vamos criar um DataFrame a partir de um arquivo CSV e realizar uma transformação para remover o símbolo '$' da coluna `preco`:

```python
# Importando os módulos necessários
from pyspark.sql.functions import col, regexp_replace

# Lendo o arquivo CSV e criando o DataFrame
df_carros = spark.read.format("csv").option("header", True).load("/caminho/do/arquivo.csv")

# Removendo o símbolo '$' da coluna 'preco'
df_carros = df_carros.withColumn(
    "preco",
    regexp_replace(col("preco"), "\\$", "")
)

# Exibindo o DataFrame transformado
display(df_carros)
```

## Criação de uma Visão Temporária

Para utilizar comandos SQL, primeiro precisamos criar uma visão temporária do DataFrame:

```python
# Criando uma visão temporária chamada "carros"
df_carros.createOrReplaceTempView("carros")
```

## Tipagem de Colunas com SQL

Para alterar o tipo de dados das colunas utilizando comandos SQL:

```python
# Exemplo de tipagem de colunas com SQL
%sql
SELECT
    CAST(id_carro AS INT) AS id_carro,
    modelo_carro,
    CAST(preco AS DOUBLE) AS preco,
    CAST(cos_marca AS INT) AS cos_marca
FROM
    carros
```

Para armazenar o resultado em um novo DataFrame:

```python
# Executando a tipagem de colunas e armazenando o resultado em um novo DataFrame
df_carros_sql = spark.sql("""
SELECT
    CAST(id_carro AS INT) AS id_carro,
    modelo_carro,
    CAST(preco AS DOUBLE) AS preco,
    CAST(cos_marca AS INT) AS cos_marca
FROM
    carros
""")

# Exibindo o DataFrame resultante
display(df_carros_sql)
```

Para verificar a estrutura do DataFrame e confirmar os tipos de dados:

```python
# Verificando a estrutura do DataFrame
df_carros_sql.printSchema()
```

## Tipagem de Colunas com PySpark

Para alterar o tipo de dados das colunas utilizando comandos nativos do PySpark:

### Primeira Maneira

```python
# Alterando o tipo de dados das colunas utilizando withColumn
df_carros_pyspark = df_carros.withColumn(
    "id_carro",
    col("id_carro").cast("int")
).withColumn(
    "preco",
    col("preco").cast("double")
).withColumn(
    "cos_marca",
    col("cos_marca").cast("int")
)

# Exibindo o DataFrame resultante
display(df_carros_pyspark)
```

### Segunda Maneira

```python
# Importando os tipos de dados necessários
from pyspark.sql.types import IntegerType, DoubleType

# Alterando o tipo de dados das colunas utilizando select e cast
df_carros_pyspark = df_carros.select(
    col("id_carro").cast(IntegerType()).alias("id_carro"),
    col("modelo_carro"),
    col("preco").cast(DoubleType()).alias("preco"),
    col("cos_marca").cast(IntegerType()).alias("cos_marca")
)

# Exibindo o DataFrame resultante
display(df_carros_pyspark)
```

---

### Considerações finais

Com esses exemplos, você pode utilizar tanto comandos SQL quanto comandos nativos do PySpark para transformar e tipar colunas em DataFrames, garantindo flexibilidade e precisão no processamento de dados.
