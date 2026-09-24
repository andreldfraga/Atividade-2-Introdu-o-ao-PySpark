# Lista de Exercícios 1 – Introdução ao PySpark

Disciplina: **Processamento de Dados Massivos**
Curso: **Ciência de Dados e Inteligência Artificial – IESB**
Professor: **Alexandre Roriz**

---

## Questão 1 (código)

Depois de carregar o DataFrame `df`, exiba:

a) o schema inferido (tipos de cada coluna);

b) as 10 primeiras linhas;

c) o número total de linhas do dataset.

---

## Questão 2 (código)

Selecione apenas as colunas `VendorID`, `tpep_pickup_datetime`, `trip_distance`, `fare_amount` e `payment_type`, e exiba as 5 primeiras linhas do resultado.

---

## Questão 3 (código)

Filtre as corridas que atendem simultaneamente às duas condições abaixo, e exiba quantas corridas restaram:

* `trip_distance` maior que 5 milhas;
* `passenger_count` maior ou igual a 3.

---

## Questão 4 (descritiva)

No comando de preparação deste exercício, o DataFrame foi carregado com `inferSchema=True`, deixando o Spark descobrir sozinho o tipo de cada coluna. Explique como esse processo de inferência funciona, e compare com a alternativa de definir o schema manualmente usando `StructType`/`StructField` (como fizemos no notebook do Colab da aula anterior).

Quais são as vantagens e os riscos de cada abordagem, especialmente pensando num arquivo com milhões de linhas como o deste exercício?

---

## Questão 5 (código)

Agrupe as corridas por `payment_type` e calcule, para cada grupo:

* a quantidade de corridas;
* a soma total de `total_amount` (receita total).

Exiba o resultado ordenado pela receita total, da maior para a menor.

---

## Questão 6 (código)

Crie uma nova coluna chamada `hora_embarque`, extraindo apenas a hora (0 a 23) da coluna `tpep_pickup_datetime`.

Em seguida, agrupe por `hora_embarque` e calcule a tarifa média (`fare_amount`) e a distância média (`trip_distance`) para cada hora do dia.

Exiba o resultado ordenado pela hora, de 0 a 23.

---

## Questão 7 (descritiva)

No Spark, existe uma distinção entre transformações (*transformations*) e ações (*actions*).

Usando como exemplo os comandos que você utilizou nas Questões 2, 3 e 5, explique essa diferença.

Por que se diz que o Spark utiliza avaliação preguiçosa (*lazy evaluation*), e qual é a vantagem prática disso?

---

## Questão 8 (código)

Considerando apenas as corridas em que `total_amount` seja maior que zero, crie uma nova coluna chamada `percentual_gorjeta`, calculada como:

```text
(tip_amount / total_amount) * 100
```

Em seguida, exiba as 10 corridas com maior `percentual_gorjeta`, mostrando as colunas:

* `VendorID`
* `total_amount`
* `tip_amount`
* `percentual_gorjeta`

---

## Questão 9 (código)

A NYC TLC disponibiliza uma tabela de referência que traduz os códigos de zona usados em `PULocationID`/`DOLocationID` para o nome do bairro (*Borough*) e da zona (*Zone*) correspondente.

Baixe essa tabela e carregue em um novo DataFrame:

```python
!wget -q https://huggingface.co/datasets/alexvaroz/nyc_tripdata_2024_sample_4M/resolve/main/taxi_zone_lookup.csv

zonas = spark.read.csv(
    "taxi_zone_lookup.csv",
    header=True,
    inferSchema=True
)

zonas.show(5)
```

Em seguida:

a) Faça o join entre `df` e `zonas`, relacionando `df.PULocationID` com `zonas.LocationID`, para descobrir o bairro (*Borough*) de onde cada corrida partiu;

b) Agrupe o resultado por `Borough` e conte quantas corridas tiveram origem em cada um;

c) Exiba o resultado ordenado do bairro com mais corridas para o com menos.

---

## Questão 10 (descritiva)

Compare o tempo de execução do `count()` da Questão 1 (sem nenhum agrupamento) com o tempo de execução do `groupBy()` da Questão 5.

Baseando-se no conceito de *shuffle*, explique por que operações de agrupamento tendem a ser mais custosas do que operações de filtragem ou seleção de colunas, mesmo processando o mesmo volume de dados.