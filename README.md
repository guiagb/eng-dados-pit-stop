# Análise de Pit Stops na Fórmula 1

Este projeto tem como objetivo aplicar os principais conceitos de engenharia de dados em um cenário real utilizando dados históricos de pit stops na Fórmula 1. A proposta foi desenvolvida como parte de um MVP para fins acadêmicos, explorando desde a coleta até a análise de dados estruturados.

---

## Descrição do Projeto

Utilizando um dataset aberto do Kaggle sobre paradas nos boxes da F1, foi construída uma pipeline completa que envolve:

- **Modelagem dimensional** com base em um esquema estrela
- **Transformação e limpeza dos dados brutos**
- **Carga em banco de dados PostgreSQL local**
- **Consultas SQL para análise exploratória**
- **Visualizações com Python para apoio à interpretação dos resultados**

---

## Estrutura do Modelo

O modelo segue a abordagem de **Data Warehouse** com uma tabela fato central (`fato_pitstop`) e duas tabelas dimensão (`dim_corrida` e `dim_piloto`):

### `dim_corrida`

| Coluna  | Tipo    | Descrição                 |
|---------|---------|---------------------------|
| season  | INTEGER | Ano da temporada          |
| round   | INTEGER | Rodada do campeonato      |
| circuit | TEXT    | Nome do circuito          |

### `dim_piloto`

| Coluna      | Tipo  | Descrição             |
|-------------|-------|-----------------------|
| driver      | TEXT  | Nome do piloto        |
| constructor | TEXT  | Nome da equipe        |

### `fato_pitstop`

| Coluna             | Tipo     | Descrição                                 |
|--------------------|----------|-------------------------------------------|
| season             | INTEGER  | Chave estrangeira para `dim_corrida`      |
| round              | INTEGER  | Chave estrangeira para `dim_corrida`      |
| driver             | TEXT     | Chave estrangeira para `dim_piloto`       |
| constructor        | TEXT     | Chave estrangeira para `dim_piloto`       |
| laps               | INTEGER  | Total de voltas completadas               |
| position           | INTEGER  | Posição final do piloto                   |
| total_pitstops     | INTEGER  | Número total de pit stops                 |
| avg_pitstop_time   | NUMERIC  | Tempo médio de parada (em segundos)       |
| pitstops           | TEXT     | Lista de pit stops (volta e tempo)        |

---

## Pipeline de ETL

A extração foi feita a partir de um arquivo `.csv`, seguido por tratamento dos dados, como:

- Remoção de espaços em nomes de colunas
- Conversão de dados faltantes em `0` ou `null` de acordo com o tipo
- Padronização para letras minúsculas
- Separação lógica entre dados de dimensão e fato
- Carga no PostgreSQL com suporte a chaves primárias e estrangeiras

---

## Análises Desenvolvidas

As análises se concentraram em entender o comportamento dos pit stops ao longo dos anos, com foco especial a partir de 2011, quando os dados passaram a ser mais completos.

Entre os insights:

- Evolução do número médio de pit stops por temporada
- Circuitos com maiores médias de paradas
- Comparativo entre médias com e sem zeros
- Identificação de valores ausentes e distorções

As análises foram feitas tanto em SQL quanto em Python com a biblioteca `matplotlib`.

---

## Qualidade dos Dados

Durante o processo, foi identificada uma limitação importante: as temporadas anteriores a 2011 possuem registros de pit stop ausentes ou zerados, o que afeta o cálculo de médias.

Também foi feita uma análise atributo por atributo para garantir consistência e detectar possíveis outliers.

---

## Dados Utilizados

- [Formula 1 Pit Stop Dataset (Kaggle)](https://www.kaggle.com/datasets/akashrane2609/formula-1-pit-stop-dataset)


