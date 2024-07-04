# Utilizando `substring`, `right` e `left` com PySpark

Neste documento, abordaremos como utilizar as funções `substring`, `right` e `left` para manipulação de strings em um DataFrame utilizando PySpark. Serão apresentados métodos tanto com SQL quanto com comandos nativos do PySpark.

## Preparação do DataFrame

Primeiro, vamos criar e preparar um DataFrame a partir de um arquivo CSV:

```python
# Importando os módulos necessários
from pyspark.sql.functions import col, expr, substring
from pyspark.sql.types import IntegerType, DoubleType

# Lendo o arquivo CSV e criando o DataFrame
df_carros = spark.read.format("csv").option("header", True).load("/caminho/do/arquivo.csv")

# Exibindo o DataFrame inicial
display(df_carros)
```

## Criação de uma Tabela Temporária

Para utilizar comandos SQL, primeiro precisamos criar uma visão temporária do DataFrame:

```python
# Criando uma visão temporária chamada "carros"
df_carros.createOrReplaceTempView("carros")
```

## Uso de `substring`, `right` e `left` com SQL

As funções `substring`, `right` e `left` são usadas para extrair partes específicas de uma string. Aqui estão exemplos de como utilizá-las com SQL:

### Exemplos de Uso com SQL

```python
# Exemplos de uso das funções substring, right e left utilizando SQL
%sql
SELECT
    modelo_carro,
    SUBSTRING(modelo_carro, 2, 3) AS modelo_sub,  -- Extrai 3 caracteres a partir da posição 2
    LEFT(modelo_carro, 2) AS modelo_left,  -- Extrai os 2 primeiros caracteres
    RIGHT(modelo_carro, 2) AS modelo_right  -- Extrai os 2 últimos caracteres
FROM
    carros
```

Para armazenar o resultado em um novo DataFrame:

```python
# Executando as funções e armazenando o resultado em um novo DataFrame
df_carro_sql = spark.sql("""
SELECT
    modelo_carro,
    SUBSTRING(modelo_carro, 2, 3) AS modelo_sub,  -- Extrai 3 caracteres a partir da posição 2
    LEFT(modelo_carro, 2) AS modelo_left,  -- Extrai os 2 primeiros caracteres
    RIGHT(modelo_carro, 2) AS modelo_right  -- Extrai os 2 últimos caracteres
FROM
    carros
""")

# Exibindo o DataFrame resultante
display(df_carro_sql)
```

## Uso de `substring`, `right` e `left` com PySpark

### Exemplos de Uso com PySpark

Para aplicar as funções `substring`, `right` e `left` utilizando comandos nativos do PySpark:

```python
# Aplicando as funções substring, right e left utilizando PySpark
df_carros_pyspark = df_carros.withColumn(
    "modelo_sub", substring(col("modelo_carro"), 2, 3)  -- Extrai 3 caracteres a partir da posição 2
).withColumn(
    "modelo_left", expr("LEFT(modelo_carro, 2)")  -- Extrai os 2 primeiros caracteres
).withColumn(
    "modelo_right", expr("RIGHT(modelo_carro, 2)")  -- Extrai os 2 últimos caracteres
)

# Exibindo o DataFrame resultante
display(df_carros_pyspark)
```

### Explicação das Funções

- `substring(column, pos, len)`: Extrai uma substring da coluna especificada, começando na posição `pos` e com comprimento `len`. A contagem de posição começa em 1.
- `left(column, len)`: Extrai os `len` primeiros caracteres da coluna especificada.
- `right(column, len)`: Extrai os `len` últimos caracteres da coluna especificada.

---

### Considerações finais

Com esses exemplos, você pode utilizar tanto comandos SQL quanto comandos nativos do PySpark para manipular strings em DataFrames, garantindo flexibilidade e precisão na extração de substrings e partes específicas de strings.
