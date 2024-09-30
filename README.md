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

3. Na pasta *source*, carregue os arquivos `.csv` que servirão como fonte de dados no projeto.

## Executar o Crawler
O crawler se conecta a um armazenamento de dados, avança por uma lista priorizada de classificadores para determinar o schema dos seus dados e, em seguida, cria tabelas de metadados no seu catálogo de dados. Por sua vez, o Glue é um serviço de integração de dados serverless.

1. No console da AWS, procure por Glue e crie um database - um agrupamento lógico de tabelas
2. Crie um crawler. Defina o Amazon S3 como fonte de dados e passe o caminho até a `/source`
3. Para que o crwaler possa detectar o schema, é preciso conceder os devidos acessos. Logo, atribua o perfil criado anteriormente para o crawler
4. Atribua o database criado como target
5. Após alguns instantes de ser criado, execute o crawler

![image](https://github.com/user-attachments/assets/6c2ec906-ab1a-40e8-88b6-dcc587257d5f)

6. Na aba tables, perceba que após alguns minutos, as tabelas foram criadas já com o schema configurado

![image](https://github.com/user-attachments/assets/a2188562-842e-41f3-94fb-eaf7504623a6)

## Criar um Job
O AWS Glue Studio é uma interface visual que facilita a criação, execução e monitoramento de ETL (Extract, Transform, Load) jobs na AWS Glue, sem a necessidade de escrever código.

1. Ainda no serviço Glue, na aba de Data Integration, crie um ETL Visual
2. Selecione a fonte de dados como sendo AWS Glue Data Catalog e o target como sendo um bucket do Amazon S3
3. De acordo com o schema das tabelas disponibilizadas, altere o nome dos ids das tabelas em que serão feitas os joins para não haver conflito com as Foreign Keys
4. Utilize o componente de join para fazer a junção entre as tabelas através das chaves.

<details>
  <summary>Clique aqui para ver a configuração em JSON</summary>

	```"dag": {
		"node-1727651267184": {
			"classification": "DataSource",
			"type": "Catalog",
			"name": "vendas",
			"inputs": [],
			"database": "vendas",
			"table": "vendas_csv",
			"runtimeParameters": [],
			"generatedNodeName": "vendas_node1727651267184",
			"codeGenVersion": 2
		},
		"node-1727651375897": {
			"classification": "Transform",
			"type": "ApplyMapping",
			"name": "VendasMapping",
			"inputs": [
				"node-1727651267184"
			],
			"mapping": [
				{
					"toKey": "idvenda",
					"fromPath": [
						"idvenda"
					],
					"toType": "long",
					"fromType": "long",
					"dropped": false
				},
				{
					"toKey": "idvendedor_vendas",
					"fromPath": [
						"idvendedor"
					],
					"toType": "long",
					"fromType": "long",
					"dropped": false
				},
				{
					"toKey": "idcliente_vendas",
					"fromPath": [
						"idcliente"
					],
					"toType": "long",
					"fromType": "long",
					"dropped": false
				},
				{
					"toKey": "data",
					"fromPath": [
						"data"
					],
					"toType": "string",
					"fromType": "string",
					"dropped": false
				},
				{
					"toKey": "total",
					"fromPath": [
						"total"
					],
					"toType": "double",
					"fromType": "double",
					"dropped": false
				}
			],
			"generatedNodeName": "VendasMapping_node1727651375897",
			"codeGenVersion": 2
		},
		"node-1727651507323": {
			"classification": "DataSource",
			"type": "Catalog",
			"name": "itens-venda",
			"inputs": [],
			"database": "vendas",
			"table": "itensvenda_csv",
			"runtimeParameters": [],
			"generatedNodeName": "itensvenda_node1727651507323",
			"codeGenVersion": 2
		},
		"node-1727651564714": {
			"classification": "Transform",
			"type": "ApplyMapping",
			"name": "ItensVendasMapping",
			"inputs": [
				"node-1727651507323"
			],
			"mapping": [
				{
					"toKey": "idproduto_itensvenda",
					"fromPath": [
						"idproduto"
					],
					"toType": "long",
					"fromType": "long",
					"dropped": false
				},
				{
					"toKey": "idvenda_itensvenda",
					"fromPath": [
						"idvenda"
					],
					"toType": "long",
					"fromType": "long",
					"dropped": false
				},
				{
					"toKey": "quantidade",
					"fromPath": [
						"quantidade"
					],
					"toType": "long",
					"fromType": "long",
					"dropped": false
				},
				{
					"toKey": "valorunitario",
					"fromPath": [
						"valorunitario"
					],
					"toType": "double",
					"fromType": "double",
					"dropped": false
				},
				{
					"toKey": "valortotal",
					"fromPath": [
						"valortotal"
					],
					"toType": "double",
					"fromType": "double",
					"dropped": false
				},
				{
					"toKey": "desconto",
					"fromPath": [
						"desconto"
					],
					"toType": "double",
					"fromType": "double",
					"dropped": false
				}
			],
			"generatedNodeName": "ItensVendasMapping_node1727651564714",
			"codeGenVersion": 2
		},
		"node-1727651677757": {
			"classification": "Transform",
			"type": "Join",
			"name": "JoinVendas-ItensVendas",
			"inputs": [
				"node-1727651375897",
				"node-1727651564714"
			],
			"joinType": "equijoin",
			"columns": [
				{
					"from": "node-1727651375897",
					"keys": [
						"idvenda"
					]
				},
				{
					"from": "node-1727651564714",
					"keys": [
						"idvenda_itensvenda"
					]
				}
			],
			"columnConditions": [
				"="
			],
			"generatedNodeName": "JoinVendasItensVendas_node1727651677757",
			"codeGenVersion": 2
		},
		"node-1727651837108": {
			"classification": "DataSource",
			"type": "Catalog",
			"name": "clientes",
			"inputs": [],
			"database": "vendas",
			"table": "clientes_csv",
			"runtimeParameters": [],
			"generatedNodeName": "clientes_node1727651837108",
			"codeGenVersion": 2
		},
		"node-1727651861798": {
			"classification": "Transform",
			"type": "Join",
			"name": "JoinClientes",
			"inputs": [
				"node-1727651837108",
				"node-1727651677757"
			],
			"joinType": "equijoin",
			"columns": [
				{
					"from": "node-1727651837108",
					"keys": [
						"idcliente"
					]
				},
				{
					"from": "node-1727651677757",
					"keys": [
						"idcliente_vendas"
					]
				}
			],
			"columnConditions": [
				"="
			],
			"generatedNodeName": "JoinClientes_node1727651861798",
			"codeGenVersion": 2
		},
		"node-1727651934162": {
			"classification": "DataSource",
			"type": "Catalog",
			"name": "produtos",
			"inputs": [],
			"database": "vendas",
			"table": "produtos_csv",
			"runtimeParameters": [],
			"generatedNodeName": "produtos_node1727651934162",
			"codeGenVersion": 2
		},
		"node-1727651947481": {
			"classification": "Transform",
			"type": "Join",
			"name": "JoinProdutos",
			"inputs": [
				"node-1727651934162",
				"node-1727651861798"
			],
			"joinType": "equijoin",
			"columns": [
				{
					"from": "node-1727651934162",
					"keys": [
						"idproduto"
					]
				},
				{
					"from": "node-1727651861798",
					"keys": [
						"idproduto_itensvenda"
					]
				}
			],
			"columnConditions": [
				"="
			],
			"generatedNodeName": "JoinProdutos_node1727651947481",
			"codeGenVersion": 2
		},
		"node-1727651994859": {
			"classification": "DataSource",
			"type": "Catalog",
			"name": "vendedores",
			"inputs": [],
			"database": "vendas",
			"table": "vendedores_csv",
			"runtimeParameters": [],
			"generatedNodeName": "vendedores_node1727651994859",
			"codeGenVersion": 2
		},
		"node-1727652016164": {
			"classification": "Transform",
			"type": "Join",
			"name": "JoinVendedores",
			"inputs": [
				"node-1727651994859",
				"node-1727651947481"
			],
			"joinType": "equijoin",
			"columns": [
				{
					"from": "node-1727651994859",
					"keys": [
						"idvendedor"
					]
				},
				{
					"from": "node-1727651947481",
					"keys": [
						"idvendedor_vendas"
					]
				}
			],
			"columnConditions": [
				"="
			],
			"generatedNodeName": "JoinVendedores_node1727652016164",
			"codeGenVersion": 2
		},
		"node-1727652124861": {
			"classification": "Transform",
			"type": "ApplyMapping",
			"name": "ColunasFinais",
			"inputs": [
				"node-1727652016164"
			],
			"mapping": [
				{
					"toKey": "idvendedor",
					"fromPath": [
						"idvendedor"
					],
					"toType": "long",
					"fromType": "long",
					"dropped": true
				},
				{
					"toKey": "nome",
					"fromPath": [
						"nome"
					],
					"toType": "string",
					"fromType": "string",
					"dropped": false
				},
				{
					"toKey": "idproduto",
					"fromPath": [
						"idproduto"
					],
					"toType": "long",
					"fromType": "long",
					"dropped": true
				},
				{
					"toKey": "produto",
					"fromPath": [
						"produto"
					],
					"toType": "string",
					"fromType": "string",
					"dropped": false
				},
				{
					"toKey": "preco",
					"fromPath": [
						"preco"
					],
					"toType": "double",
					"fromType": "double",
					"dropped": false
				},
				{
					"toKey": "idcliente",
					"fromPath": [
						"idcliente"
					],
					"toType": "long",
					"fromType": "long",
					"dropped": true
				},
				{
					"toKey": "cliente",
					"fromPath": [
						"cliente"
					],
					"toType": "string",
					"fromType": "string",
					"dropped": false
				},
				{
					"toKey": "estado",
					"fromPath": [
						"estado"
					],
					"toType": "string",
					"fromType": "string",
					"dropped": false
				},
				{
					"toKey": "sexo",
					"fromPath": [
						"sexo"
					],
					"toType": "string",
					"fromType": "string",
					"dropped": false
				},
				{
					"toKey": "status",
					"fromPath": [
						"status"
					],
					"toType": "string",
					"fromType": "string",
					"dropped": false
				},
				{
					"toKey": "idvenda",
					"fromPath": [
						"idvenda"
					],
					"toType": "long",
					"fromType": "long",
					"dropped": true
				},
				{
					"toKey": "idvendedor_vendas",
					"fromPath": [
						"idvendedor_vendas"
					],
					"toType": "long",
					"fromType": "long",
					"dropped": true
				},
				{
					"toKey": "idcliente_vendas",
					"fromPath": [
						"idcliente_vendas"
					],
					"toType": "long",
					"fromType": "long",
					"dropped": true
				},
				{
					"toKey": "data",
					"fromPath": [
						"data"
					],
					"toType": "string",
					"fromType": "string",
					"dropped": false
				},
				{
					"toKey": "total",
					"fromPath": [
						"total"
					],
					"toType": "double",
					"fromType": "double",
					"dropped": false
				},
				{
					"toKey": "idproduto_itensvenda",
					"fromPath": [
						"idproduto_itensvenda"
					],
					"toType": "long",
					"fromType": "long",
					"dropped": true
				},
				{
					"toKey": "idvenda_itensvenda",
					"fromPath": [
						"idvenda_itensvenda"
					],
					"toType": "long",
					"fromType": "long",
					"dropped": true
				},
				{
					"toKey": "quantidade",
					"fromPath": [
						"quantidade"
					],
					"toType": "long",
					"fromType": "long",
					"dropped": false
				},
				{
					"toKey": "valorunitario",
					"fromPath": [
						"valorunitario"
					],
					"toType": "double",
					"fromType": "double",
					"dropped": false
				},
				{
					"toKey": "valortotal",
					"fromPath": [
						"valortotal"
					],
					"toType": "double",
					"fromType": "double",
					"dropped": false
				},
				{
					"toKey": "desconto",
					"fromPath": [
						"desconto"
					],
					"toType": "double",
					"fromType": "double",
					"dropped": false
				}
			],
			"generatedNodeName": "ColunasFinais_node1727652124861",
			"codeGenVersion": 2
		},
		"node-1727652261958": {
			"classification": "DataSink",
			"type": "S3",
			"name": "Datalake",
			"inputs": [
				"node-1727652124861"
			],
			"format": "glueparquet",
			"compression": "snappy",
			"path": "s3://datalake-dataeng/datalake/",
			"partitionKeys": [
				"status"
			],
			"updateCatalogOptions": "none",
			"schemaChangePolicy": {
				"enableUpdateCatalog": false
			},
			"autoDataQuality": {
				"isEnabled": false
			},
			"additionalOptions": {},
			"generatedNodeName": "Datalake_node1727652261958",
			"codeGenVersion": 2
		}
	},
	"jobConfig": {
		"command": "glueetl",
		"description": "",
		"role": "arn:aws:iam::514552919279:role/glue-access-dataeng",
		"scriptName": "dataeng-job.py",
		"version": "4.0",
		"language": "python-3",
		"scriptLocation": "s3://datalake-dataeng/script/",
		"temporaryDirectory": "s3://datalake-dataeng/temp/",
		"timeout": 2880,
		"maxConcurrentRuns": 1,
		"workerType": "G.1X",
		"numberOfWorkers": 10,
		"maxRetries": 0,
		"metrics": true,
		"observabilityMetrics": true,
		"security": "none",
		"bookmark": "job-bookmark-disable",
		"logging": true,
		"spark": true,
		"sparkConfiguration": "standard",
		"sparkPath": "s3://datalake-dataeng/log/",
		"serverEncryption": false,
		"glueHiveMetastore": true,
		"etlAutoScaling": false,
		"etlAutoTuning": false,
		"jobParameters": [],
		"tags": [],
		"connectionsList": [],
		"jobMode": "VISUAL_MODE",
		"name": "dataeng-job",
		"pythonPath": ""
	},
	"hasBeenSaved": false
}</details>```
	
![image](https://github.com/user-attachments/assets/801b7e3f-4936-4094-9278-77f5af349a00)

5. Ao final, execute o Job para que ele possa unificar as tabelas e inserir no Datalake (target pré-definido)

## Realizar consultas com o Athena
O Athena é um serviço de análise interativa sem a utilização de um servidor

1. No console AWS procure por Athena
2. Configure a fonte de dados para AWS Data Catalog e o database target em que se encontra a tabela unificada
3. Em configurações, atribua um bucket do S3 para salvar os resultados da consulta de forma temporária

![image](https://github.com/user-attachments/assets/0dd05e4c-79bb-4834-b5f3-6cac7a36cf9f)

![image](https://github.com/user-attachments/assets/f78647e7-6fdf-42e1-8d75-3f4913cfde8b)

