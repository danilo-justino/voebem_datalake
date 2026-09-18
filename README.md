# Data Lake Voe Bem - Dados da Aviação Civil (2024 – 07/2026)

Este repositório contém a arquitetura, notebooks e pipelines de transformação para o projeto de **Data Lake de Aviação Civil**, cobrindo o período de **janeiro de 2024 a julho de 2026**.

O projeto realiza a ingestão e processamento de dados de voos, cruzando informações operacionais com bases de referência de companhias aéreas (nacionais e estrangeiras) e a lista cadastral de aeroportos, utilizando a arquitetura Medallion (Bronze, Silver e Gold) no **Databricks** com **Apache Spark**.

---

## 📐 Arquitetura dos Dados (Medallion Architecture)

O fluxo de transformação do Data Lake foi estruturado em três camadas principais dentro do Databricks:

* **🟤 Camada BRONZE (Ingestão Raw)**
  * Leitura dos arquivos CSV brutos (2024 a 07/2026) sem perda de dados.
  * Preservação da tipagem original (tudo como `string`).
  * Adição de colunas de auditoria (`_ingestion_file` e `_ingestion_timestamp`).
  * Ingestão idempotente em formato **Delta Lake**.

* **⚪ Camada SILVER (Limpeza & Enriquecimento)**
  * Limpeza de nulos, remoção de duplicatas e conversão de tipos de dados (datas, inteiros, decimais).
  * Enriquecimento da base operacional de voos com o cruzamento das tabelas de referência de **Companhias Aéreas** e **Aeroportos**.

* **🟡 Camada GOLD (Agregação & Negócio)**
  * Construção de Data Marts e tabelas agregadas prontas para consumo por ferramentas de Analytics/BI.
  * KPIs de volume de voos, pontualidade, rotas mais movimentadas e análise de operadoras.

---

## 📁 Fonte de Dados

Os dados originais foram carregados via upload de arquivos **CSV** diretamente no Databricks:

1. **Dados Operacionais de Voos (2024 – Julho/2026):** Registros detalhados de partidas, chegadas e status operacionais.
2. **Referência de Companhias Aéreas:** Cadastro completo com identificadores das empresas aéreas nacionais e estrangeiras.
3. **Referência de Aeroportos:** Base cadastral com informações de aeroportos (códigos IATA/ICAO, municípios, estados e localizações).

---

## 🛠️ Tecnologias Utilizadas

* **Databricks:** Ambiente para processamento e orquestração.
* **Apache Spark (PySpark):** Engine de processamento distribuído.
* **Delta Lake:** Armazenamento em camadas que garante consistência ACID, governança e alta performance.
* **Git / GitHub:** Controle de versão e gestão de branches por camada do projeto.

---

## 📁 Estrutura do Repositório

```text
├── README.md
└── notebooks/
    ├── 01_bronze/
    │   ├── 01_ingest_flights.py
    │   ├── 02_ingest_airlines_ref.py
    │   └── 03_ingest_airports_ref.py
    ├── 02_silver/
    │   ├── 01_transform_flights.py
    │   └── 02_enrich_ref_data.py
    └── 03_gold/
        ├── 01_flight_performance_kpis.py
        └── 02_airport_flow_analytics.py
