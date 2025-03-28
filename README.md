# Projeto de Análise e Consulta de Dados com PySpark e SQL

Este projeto tem como objetivo realizar a criação e consulta de uma tabela de dados utilizando PySpark e SQL, a partir de um conjunto de dados em formato CSV contendo informações sobre funcionários. Também é abordada uma simulação de consulta de dados em um sistema de aluguel de carros.

## Funcionalidades Implementadas

### 1. Criação de Tabela com PySpark SQL
- Leitura de um arquivo CSV contendo dados de funcionários (`dados_funcionarios.csv`)
- Criação de uma tabela `funcionarios2` com os seguintes campos:
  - `matricula`, `nome`, `cidade`, `estado`, `pais`, `idade`, `departamento`, `cargo`, `salario`, `escolaridade`, `nota`
- Inferência de esquema com `inferSchema` e uso do cabeçalho

```python
spark.sql('''
CREATE TABLE IF NOT EXISTS funcionarios2 (
    matricula INT,
    nome STRING,
    cidade STRING,
    estado STRING,
    pais STRING,
    idade INT,
    departamento STRING,
    cargo STRING,
    salario DOUBLE,
    escolaridade STRING,
    nota INT
)
USING CSV
OPTIONS (path '/content/dados_funcionarios.csv', header 'true', inferSchema 'true')
''')
```

### 2. Consultas SQL com Filtros e Agregações
- Seleção de nomes e salários com `salario > 20000`:

```sql
SELECT nome, salario
FROM funcionarios2
WHERE salario > 20000
```

- Média salarial por nome com filtro:

```sql
SELECT nome, AVG(salario) AS salario_medio
FROM funcionarios2
GROUP BY nome
HAVING AVG(salario) > 20000
```

### 3. Consulta Simulada: Aluguel de Carros por Mês/Ano

Consulta que retorna clientes que alugaram carros em determinado mês e ano, listando modelos e datas de aluguel:

```sql
SELECT 
    c.nome AS nome_cliente,
    car.modelo AS modelo_carro,
    a.data_aluguel
FROM 
    alugueis a
JOIN 
    clientes c ON a.cliente_id = c.id
JOIN 
    carros car ON a.carro_id = car.id
WHERE 
    MONTH(a.data_aluguel) = 3
    AND YEAR(a.data_aluguel) = 2024
ORDER BY 
    a.data_aluguel;
```

> Obs: A estrutura de tabelas é fictícia e usada para simular uma aplicação prática.

## Tecnologias Utilizadas
- Python 3
- Apache Spark (PySpark)
- SQL
- Google Colab / Jupyter Notebooks

## Estrutura Esperada do CSV
Arquivo `dados_funcionarios.csv` com colunas:
- matricula, nome, cidade, estado, pais, idade, departamento, cargo, salario, escolaridade, nota

## Como Executar
1. Suba o arquivo `dados_funcionarios.csv` no diretório `/content/`
2. Execute os blocos de código no ambiente PySpark
3. Realize consultas usando `spark.sql()`

---

Este projeto tem fins educacionais e serve como base para práticas com SQL, análise de dados e Spark.
