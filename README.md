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
  Contém informações únicas sobre os clientes.
  - id_cliente (surrogate key)
  - nome_cliente

- **dim_produto**  
  Armazena detalhes sobre os produtos vendidos.
  - id_produto (surrogate key)
  - nome_produto
  - nome_categoria
  - nome_fabricante

- **dim_data**  
  Derivada da coluna `data`, com decomposição em componentes temporais.
  - id_data (surrogate key)
  - data
  - ano
  - mes
  - dia
  - dia_semana

- **dim_local**  
  Contém informações geográficas relacionadas às vendas.
  - id_local (surrogate key)
  - nome_cidade
  - nome_estado

### Tabela Fato:

- **fato_vendas**  
  Tabela principal contendo as medidas de negócio e chaves para as dimensões.
  - id_venda (surrogate key)
  - id_cliente (foreign key)
  - id_produto (foreign key)
  - id_local (foreign key)
  - id_data (foreign key)
  - qtd_vendida (medida)
  - valor_total (medida)

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