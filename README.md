# Projeto: Análise exploratória e Descritiva de Dados de crédito com SQL

### Este repositório

Neste repositório há um arquivo em SQL do qual as mesma queries que foram tratadas pelo AWS- Athena, há também o repositório do notebook Kaggle onde é possivel visualizar o projeto completo pelo link:
##### :pushpin: [Análise exploratória e descritiva de dados de crédito com SQL](https://www.kaggle.com/code/leticialavieri/eda-de-cr-dito-com-sql)

<br>

### Sobre o projeto

A exploração de dados (EDA) é uma etapa crucial na análise de dados, permitindo uma compreensão profunda da natureza dos dados, a identificação de padrões e a revelação de tendências.
Este trabalho abrange conceitos e técnicas essenciais para a compreensão e manipulação eficaz dos dados.

Para este projeto, utilizei o AWS S3-bucket e o Athena para manipular dados em SQL e para a visualização gráfica foi utilizado o Excel. 
O AWS S3-bucket é um serviço de armazenamento em nuvem que permite armazenar e acessar grandes volumes de dados de maneira segura e escalável, ideal para armazenar datasets que podem ser acessados e analisados conforme necessário. O Athena é um serviço de consulta interativo que facilita a análise de dados diretamente no S3 usando SQL. 
Com o Athena, não é necessário carregar dados em um banco de dados separado, permitindo consultas rápidas e eficientes diretamente sobre os dados armazenados no S3. Juntas, essas ferramentas possibilitam uma manipulação ágil e flexível dos dados, otimizando o processo de análise.

O objetivo desta análise é identificar padrões e tendências nos dados de crédito, fornecer insights sobre o comportamento dos clientes e auxiliar na tomada de decisões estratégicas. 
Foi explorado diversas ideias de insights, incluindo a análise do perfil de crédito dos clientes, a identificação de fatores que influenciam a aprovação ou rejeição de crédito e a previsão do comportamento futuro dos clientes com base nos dados históricos.

<br>

### Informações sobre o Dataset

<br>
Os dados representam informações de clientes de um banco e contam com as seguintes colunas:

idade = idade do cliente <br>
sexo = sexo do cliente (F ou M) <br>
dependentes = número de dependentes do cliente <br>
escolaridade = nível de escolaridade do clientes <br>
salario_anual = faixa salarial do cliente <br>
tipo_cartao = tipo de cartao do cliente <br>
qtd_produtos = quantidade de produtos comprados nos últimos 12 meses <br>
iteracoes_12m = quantidade de iterações/transacoes nos ultimos 12 meses <br>
meses_inativo_12m = quantidade de meses que o cliente ficou inativo <br>
limite_credito = limite de credito do cliente <br>
valor_transacoes_12m = valor das transações dos ultimos 12 meses<br>
qtd_transacoes_12m = quantidade de transacoes dos ultimos 12 meses<br>

<br>
A tabela foi criada no AWS Athena junto com o S3 Bucket, seguindo a Query:

CREATE EXTERNAL TABLE IF NOT EXISTS default.credito (<br>
idade INT,<br>
sexo STRING,<br>
dependentes INT,<br>
escolaridade STRING,<br>
estado_civil STRING,<br>
salario_anual STRING,<br>
tipo_cartao STRING,<br>
qtd_produtos BIGINT,<br>
iteracoes_12m INT,<br>
meses_inativo_12m INT,<br>
limite_credito FLOAT,<br>
valor_transacoes_12m FLOAT,<br>
qtd_transacoes_12m INT<br>
)<br>

ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'
WITH SERDEPROPERTIES ('serialization.format' = ',', 'field.delim' = ',') LOCATION 's3://bucket-leticialavieri-ebac/'
TBLPROPERTIES ('has_encrypted_data'='false');


O dataset esta em formato CSV e contem 172KB de tamanho.
Existe uma versão do dataser dos dados disponibilizados em: https://github.com/andre-marcos-perez/ebac-course-utils/tree/main/dataset

<br> 

#### Imagens do projeto:

![image](https://github.com/LeticiaLavieri/EDA-credito-SQL/assets/165159056/a2ac0e4d-d1dc-41ea-b85f-55d5f6055423)
![image](https://github.com/LeticiaLavieri/EDA-credito-SQL/blob/main/Imagens/Imagens%20do%20projeto%202.PNG?raw=true)
![image](https://github.com/LeticiaLavieri/EDA-credito-SQL/blob/main/Imagens/Imagens%20do%20projeto%203.PNG?raw=true)


```sql
SELECT 
	COUNT(*) AS Quantidade_linhas
FROM creditoSQL;


SELECT *
FROM creditoSQL
LIMIT 10;


SELECT DISTINCT escolaridade FROM creditoSQL;
SELECT DISTINCT estado_civil FROM creditoSQL;
SELECT DISTINCT salario_anual FROM creditoSQL;
SELECT DISTINCT tipo_cartao FROM creditoSQL;


SELECT 
    COUNT(*) AS Quantidade, salario_anual
FROM creditoSQL
    GROUP BY salario_anual;
    
SELECT 
	COUNT(*) AS Quantidade_clientes, 
    sexo 
FROM creditoSQL 
GROUP BY sexo;


SELECT 
	COUNT(*) AS Quantidade,
	sexo,
	salario_anual
FROM creditoSQL
GROUP BY sexo,
	salario_anual;


SELECT 
	MIN(idade) AS idade_minima,
	MAX(idade) AS idade_maxima
FROM creditoSQL;


SELECT 
    MAX(limite_credito) AS limite_maximo,
    escolaridade,
    tipo_cartao,
    sexo
FROM creditoSQL 
WHERE
    escolaridade != 'na' AND 
    tipo_cartao != 'na'
GROUP BY 
    escolaridade,
    tipo_cartao,
    sexo 
ORDER BY 
    limite_maximo DESC LIMIT 10;


SELECT
    MAX(limite_credito) AS limite_minimo,
    escolaridade,
    tipo_cartao,
    sexo 
FROM creditoSQL
WHERE escolaridade != 'na' AND tipo_cartao != 'na' 
GROUP BY 
    escolaridade, 
    tipo_cartao,
    sexo 
ORDER BY limite_minimo ASC LIMIT 10;
```
<br>

#### Ferramentas utilizadas:
![SQLite](https://img.shields.io/badge/Sqlite-003B57?style=for-the-badge&logo=sqlite&logoColor=white) ![Excel](https://img.shields.io/badge/Microsoft_Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white) ![AWS](https://img.shields.io/badge/Amazon_AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
