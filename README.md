[![Author](https://img.shields.io/badge/author-Gabriel_Freitas-purple.svg)](https://www.linkedin.com/in/gabrielsfreitas/) [![](https://img.shields.io/badge/python-3.7+-blue.svg)](https://www.python.org/downloads/) [![](https://img.shields.io/badge/Microsoft-Power_BI-yellow.svg)](https://powerbi.microsoft.com/pt-br/downloads/) [![](https://img.shields.io/badge/Oracle-SQL-orange.svg)](https://www.mysql.com/downloads/) [![](https://img.shields.io/badge/Apache-Spark-green.svg)]([https://powerbi.microsoft.com/pt-br/downloads/](https://spark.apache.org/)) [![](https://img.shields.io/badge/Mongo-DB-blue.svg)](https://www.mongodb.com/)

<p align="center">
  <img src="banner.jpg" >
</p>

# ETL e Data Crawler - Glue e Athena

## Conteúdo
* IAM para o Glue
* Criar buckets e pastas
* Executar o Crawler
* Criar um Job
* Executar e avaliar o resultado
* Realizar consultas com o Athena

## AIM para o Glue
O IAM (Identity and Access Management) é uma ferramenta para gerenciamento de recursos da AWS.

1. Em soluções, procure por IAM
2. Em gerenciamento de acesso, vá até a sessão *funções*
3. Crie um perfil com o tipo de identidade *serviços AWS* e procure por Glue
4. Conceda a função de *AdministratorAccess* para este serviço

## Criar buckets e pastas
Crie um bucket no Amazon S3, um serviço de armazenamento escalável na nuvem da AWS.

1. Crie um bucket no S3
2. Em seguida, crie outras 5 sub pastas para armaenar os logs, arquivos temporários e as fontes de dados.

![image](https://github.com/user-attachments/assets/0617f1ed-43d8-4e59-83cd-8de0f33aa167)


3. 
