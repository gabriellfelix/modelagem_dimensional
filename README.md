# 🚀 Desafio Técnico - Engenheiro de Dados Júnior

## 🧾 Descrição
Este projeto implementa uma modelagem dimensional (esquema estrela) a partir de uma tabela única de dados brutos de vendas, produtos e clientes. A transformação foi realizada utilizando PySpark para processar os dados, criar as tabelas de dimensão e a tabela fato, permitindo análises eficientes e escaláveis.

## 📁 Estrutura dos Dados

**Arquivo de entrada:** `dados_brutos.csv`

**Colunas disponíveis:**
- nome_cliente
- cidade
- estado
- nome_produto
- categoria
- fabricante
- data
- qtd_vendida
- valor_total

## 🏗 Modelagem Criada

### Tabelas de Dimensão:

- **dim_cliente**  

Contém informações sobre os clientes.

| Coluna       | Tipo    | Descrição                            |
|--------------|---------|--------------------------------------|
| id_cliente   | Long    | Surrogate key (chave substituta)     |
| nome_cliente | String  | Nome do cliente                      |

- **dim_produto**  

Contém informações sobre os produtos vendidos.

| Coluna         | Tipo    | Descrição                            |
|----------------|---------|--------------------------------------|
| id_produto     | Long    | Surrogate key (chave substituta)     |
| nome_produto   | String  | Nome do produto                      |
| nome_categoria | String  | Categoria do produto                 |
| nome_fabricante| String  | Fabricante do produto                |

- **dim_data**  

Dimensão temporal com hierarquias e atributos descritivos.

| Coluna       | Tipo    | Descrição                            |
|--------------|---------|--------------------------------------|
| id_data      | Long    | Surrogate key (chave substituta)     |
| data         | Date    | Data completa                        |
| ano          | Integer | Ano da data                          |
| mes          | Integer | Mês da data                          |
| dia          | Integer | Dia do mês                           |
| dia_semana   | String  | Nome do dia da semana                |

- **dim_local**  

Contém informações sobre localizações geográficas.

| Coluna       | Tipo    | Descrição                            |
|--------------|---------|--------------------------------------|
| id_local     | Long    | Surrogate key (chave substituta)     |
| nome_cidade  | String  | Nome da cidade                       |
| nome_estado  | String  | Sigla do estado                      |

### Tabela Fato:

- **fato_vendas**  

A tabela fato central que contém métricas de vendas e referências para as dimensões.

| Coluna       | Tipo    | Descrição                             |
|--------------|---------|---------------------------------------|
| id_venda     | Long    | Chave primária da tabela fato         |
| id_cliente   | Long    | Chave estrangeira para dim_cliente    |
| id_produto   | Long    | Chave estrangeira para dim_produto    |
| id_local     | Long    | Chave estrangeira para dim_local      |
| id_data      | Long    | Chave estrangeira para dim_data       |
| qtd_vendida  | Integer | Quantidade de itens vendidos          |
| valor_total  | Double  | Valor total da venda                  |

## 🛠 Tecnologias Utilizadas

- **PySpark**: Framework de processamento distribuído para transformação de dados em larga escala
- **Spark SQL**: Para consultas aos dados
- **Python**: Linguagem de programação principal
- **Jupyter Notebook**: Ambiente de desenvolvimento interativo
- **Git e GitHub**: Para Versionamento

## 📂 Arquivos no Repositório

- `Modelagem Dimensional.ipynb`: Notebook com todo o código de transformação e modelagem
- `dados/entrada/dados_brutos.csv`: Arquivo com os dados brutos de entrada
- `dados/saida/`: Diretório contendo as tabelas dimensionais e fato exportadas
  - `dim_cliente.csv`
  - `dim_produto.csv`
  - `dim_data.csv`
  - `dim_local.csv`
  - `fato_vendas.csv`
- `diagrama.png`: Representação visual do modelo dimensional implementado

## ▶️ Como Executar

1. Clone este repositório para sua máquina local
2. Instale as dependências necessárias:
   ```
   pip install pyspark jupyter
   ```
3. Execute o notebook Jupyter:
   ```
   jupyter notebook "Modelagem Dimensional.ipynb"
   ```
4. Os resultados serão salvos automaticamente no diretório `dados/saida/`

## 📊 Decisões de Modelagem

1. **Chaves substitutas (surrogate keys)**: Utilizei `monotonically_increasing_id()` para gerar chaves únicas em todas as dimensões, garantindo integridade referencial e melhor desempenho em consultas.

2. **Normalização de dados**: Implementei uma padronização no casing dos campos de texto (`initcap` para nomes, `upper` para siglas) para evitar duplicidades nas dimensões.

3. **Dimensão data**: Extraí componentes temporais (ano, mês, dia, dia da semana) para facilitar análises por período e sazonalidade.

4. **Dimensão local**: Agrupei cidade e estado em uma única dimensão, seguindo o padrão geográfico de hierarquia natural.

5. **Tabela fato**: Mantive a granularidade no nível de transação individual de venda, preservando as medidas de quantidade e valor total.

6. **Validação de qualidade**: Implementei verificações de valores nulos e inconsistências antes da criação do modelo dimensional.