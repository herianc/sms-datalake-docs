# Acessando por outras plataformas

## **Sumário**

Nesta seção você aprenderá a acessar o Data Lake através das seguintes feramentas:

- [Python](./outras-formas.md#python)
- [R](./outras-formas.md#r)
- [Google Colab](./outras-formas.md#google-colab)
- [Looker](./outras-formas.md#looker)



## **Python**

!!! warning "Importante"
    Para conectar o Python você precisará de uma **conta de serviço**, este é um acesso diferente daquele que é dado por padrão ao Data Lake. 
    
    ??? abstract "`credential.json`"
        Este acesso tem a forma de um arquivo `.json` com o conteúdo similar a este:
        ```{.json}
        {
            "type": "service_account",
            "project_id": "rj-sms",
            "private_key_id": "XXXXXX",
            "private_key": "-----BEGIN PRIVATE KEY------
            [CHAVE] ----END PRIVATE KEY-----\n",
            "client_email": "email@dominio.com",
            "client_id": "0000000",
            "auth_uri": "https://accounts.google.com/o/oauth2/auth",
            "token_uri": "https://oauth2.googleapis.com/token",
            "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
            "client_x509_cert_url": ""
        }
        ```
    Caso não tenha, solicite sua conta de serviço.

### Passo a passo para conectar

#### 1. Instale a biblioteca do Google Cloud para o Python

```{.sh}
pip install google-cloud-bigquery
```

#### 2. Autentique no Google Cloud
Para autenticar no Google Cloud, a biblioteca busca uma variável de ambiente chamada `GOOGLE_APPLICATION_CREDENTIALS` que contém o caminho o json da conta de serviço.

A forma mais simples de injetar as credenciais no ambiente é através deste comando no terminal:

```{.sh}
export GOOGLE_APPLICATION_CREDENTIALS="/caminho/para/seu/arquivo-de-credenciais.json"
```
Caso queria fazer direto via python, a injeção das credenciais pode ser feita desta forma:

```{.py3}
import os

credentials_path = "/path/to/your/gcp/credentials.json"
os.environ["GOOGLE_APPLICATION_CREDENTIALS"] = credentials_path
```

#### 3. Importe a biblioteca e crie um cliente do BigQuery

No seu script Python, importe a classe bigquery do módulo google.cloud.bigquery. Em seguida, crie uma instância de Client para se conectar ao BigQuery:

```{.py3}
from google.cloud import bigquery

# Crie um cliente do BigQuery
client = bigquery.Client()
```
#### 4. Execute consultas SQL

```{.py3}
import pandas as pd
from google.cloud import bigquery

client = bigquery.Client()

# Escreva sua consulta SQL
query = """
SELECT *
FROM `seu-projeto.seu_dataset.sua_tabela`
"""

# Execute a consulta
query_job = client.query(query)

# Carregue a consulta em um dataframe
df = query_job.to_dataframe()
```

--- 

## **R**

### Passo a passo para conectar

#### 1. Instale a biblioteca do Google Cloud para o R

```{.sh}
install.packages("bigrquery")
```

#### 2. Autentique no Google Cloud

Para autenticar no Google Cloud, basta chamar uma conexão pela primeira vez que você será direcionado para autenticar pelo navegador: 

```{.r}
library(bigrquery)
library(DBI)

# Crie uma conexão para se conectar ao BigQuery
con <- dbConnect(
  bigrquery::bigquery(),
  project = "rj-sms"
)
```

Ainda nesta primeira vez que chamar a conexão, é possível que o R peça para instalar uma biblioteca adicional, proceda com a instalação para que a autenticação funcione. 

Uma vez instalado, você será redirecionado para o navegador onde permitirá a leitura das suas credenciais do Google:

![Página de autenticação](https://t9013004335.p.clickup-attachments.com/t9013004335/82471af0-264c-420c-891d-d41819fa5a4e/image.png)

Selecione o usuário que tem acesso ao BigQuery, permita o acesso e feche a aba do navegador.

#### 3. Execute consultas SQL

Com o cliente BigQuery configurado, você pode usar métodos como query  para enviar consultas SQL e obter resultados:

```{.r}
# Escreva sua consulta SQL
query <- "
SELECT *
FROM `seu-projeto.seu_dataset.sua_tabela`
"

# Execute a consulta
result <- dbGetQuery(con, query)
```

---

## **Google Colab**

### Passo a passo para conectar

#### 1. Crie um novo notebook e importe as bibliotecas necessárias

Após selecionar um **Novo Notebook**, importe as bibliotecas necessárias para se conectar ao BigQuery. 

```{.py3}
from google.cloud import bigquery
from google.colab import auth
auth.authenticate_user()
```
!!! note "Nota"
    `auth.authenticate_user()` irá abrir um pop-out para logar com a sua conta Google com acesso ao BigQuery.



<figure markdown="span">
  ![Página de Autenticação - Colab](../assets/colab-auth1.png)
  <figcaption> </figcaption>
</figure>


<figure markdown="span">
  ![Pop-out de Autenticação - Colab](../assets/colab-auth2.png)
  <figcaption> </figcaption>
</figure>


#### 2. Execute consultas SQL

```{.py3}
query = """
SELECT *
FROM `projeto.dataset.tabela`
LIMIT 10
"""

df = client.query(query).to_dataframe()
df.head()
```

---

## **Looker**

!!! tip "Dica"
    O Looker e o BigQuery, utilizam a mesma autenticação. Caso você já tenha acesso ao BigQuery da SMS, basta acessar o [link](https://lookerstudio.google.com/) da ferramenta e seguir os passo abaixo para conectar seus dados.

### Passo a passo para conectar

#### 1. Configure uma conexão no Looker
Faça login no Looker com as mesmas credenciais do BigQuery 
Na tela inicial, comece um novo modelo, clicando em Relatório em branco. 


![Página Looker](https://t9013004335.p.clickup-attachments.com/t9013004335/4becca94-c94f-4a08-b6a8-af792c911b0d/Capturar7.PNG)

#### 2. No menu de Adicionar dados ao relatório , navegue até "Adicionar Dados" e selecione o Google conector "BigQuery".

![BigQuery-Connector](https://t9013004335.p.clickup-attachments.com/t9013004335/dea20064-7633-4980-b0cf-f59efeb5fa4a/Capturar3.PNG)

#### 3. Selecione o projeto

![Projeto](https://t9013004335.p.clickup-attachments.com/t9013004335/113f403c-4b9f-4feb-aa07-7660cdaa4538/Capturar6.PNG)

#### 4. Preencha o campo "Inserir o ID do projeto manualmente", com o ambiente no qual foi solicitado o acesso. Para o ambiente produtivo do Lake, o projeto é `rj-sms`.

#### 5. Em seguida escolha o conjunto de dados para inserir ao seu relatório.

