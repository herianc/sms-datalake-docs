# Operações Básicas

!!! warning "Importante"
    O Sandbox não é ambiente produtivo, ou seja, utilize apenas como ambiente de desenvolvimento e/ou teste.

## Upload de dados 

### Arquivos de texto

Para carregar com sucesso um arquivo no BigQuery é necessário respeitar as limitações da ferramenta para cada tipo de arquivo. 

Ex:

- caracteres permitidos nos nomes das colunas;
- como datas devem ser representadas;
- qual caracter deve ser utilizado para definir o início das casas decimais.

Para conhecer sobre essas limitações acessar a [documentação](https://docs.cloud.google.com/bigquery/docs/batch-loading-data?hl=pt-br#limitations-local-files "Como carregar dados em lote - BigQuery") do BigQuery, no menu da esquerda selecionar o tipo de arquivo que deseja carregar e buscar por **Limitações**. 

Para  iniciar o upload de arquivos de texto para o BigQuery nos formatos de texto CSV, JSON, e até outros formatos mais avançados (que tecnicamente não são arquivos de texto, mas binários) como AVRO, PARQUET e ORC, clique em Criar tabela.

![upload-arquivos](https://t9013004335.p.clickup-attachments.com/t9013004335/693a729b-6a11-4533-8d01-c7c42e62529b/Capturar11.png)

Na sessão Origem, selecione a opção Fazer upload em Criar tabela de. Em seguida selecione o arquivo e indique seu formato.

![upload-arquivos-menu](https://t9013004335.p.clickup-attachments.com/t9013004335/006c902e-b126-4d3a-b498-26580317338d/image.png)

Na sessão Destino, manter o Projeto como `rj-sms-sandbox` e defina um Conjunto de dados no qual você tenha acesso:


<figure markdown="span">
![upload-arquivos-destino](https://t9013004335.p.clickup-attachments.com/t9013004335/2b96b129-fbd6-4614-86ff-9b068469253d/image.png)
  <figcaption> </figcaption>
</figure>

Em seguida, insira o nome que deseja para a tabela em **Tabela**:

![upload-arquivos-tabela](https://t9013004335.p.clickup-attachments.com/t9013004335/ff72de9c-e93f-4c40-ac37-7a669032ead5/image.png)

Por último, manter a opção **Tabela nativa** em Tipo de tabela  e marque **Detectar automaticamente** o Esquema :

![upload-arquivos-esquema](https://t9013004335.p.clickup-attachments.com/t9013004335/06c89f97-d8d3-412a-9e63-8501fbaba74b/image.png)

!!! note "Nota"
    Para a detecção automática funcionar, é necessário respeitar as limitações para cada tipo de arquivo mencionadas no início desta sessão.

Feito isso só clicar em criar tabela e aguardar a mensagem de confirmação, que a tabela vai esta disponível no seu Conjunto de Dados.


<figure markdown="span">
![upload-arquivos-nova-tabela](https://t9013004335.p.clickup-attachments.com/t9013004335/b5fa535d-1e9e-4988-bac4-6685453598af/Capturar3.PNG)
  <figcaption> </figcaption>
</figure>

![upload-arquivos-bq](https://t9013004335.p.clickup-attachments.com/t9013004335/917c996b-0aed-4ef8-aedd-c804bc242d4f/Capturar4.PNG)

### Google Sheets & Planilha

Este método cria um link entre a planilha do Google Sheets e o BigQuery. Isto é, a tabela do BigQuery é um front para conteúdo que está na planilha. Qualquer modificação na planilha muda imediatamente o conteúdo do Data Lake. Este é um excelente método para armazenar dados que precisam modificar imediatamente ou com frequência.

!!! warning "Importante" 
    Planilhas do Microsoft Excel precisam ser convertidas para o formato do Google Sheets e devem estar armazenadas no Google Drive antes do upload para o BigQuery. 
    as planilhas não devem ter na primeira linha o nome das colunas. Esta informação será passada durante a etapa de conexão do BigQuery com a Planilha.
    para carregar com sucesso um arquivo no BigQuery é necessário respeitar as limitações da ferramenta para cada tipo de arquivo. A regras de representação da informação de planilhas são as mesmas dos arquivos CSV. 


O primeiro passo é clicar no botão criar tabela para iniciar o assistente

Na tela seguinte preencher os itens da sessão Origem com

- Criar Tabela de : Drive
- Selecionar URL do Driver : Informar a URL da planilha  salva no seu Google Drive
- Formato do arquivo : Arquivo do planilhas Google
- Intervalo da planilha : com o nome da aba da planilha EX:

![upload-planilha](https://t9013004335.p.clickup-attachments.com/t9013004335/830316cf-3dfb-49e4-84fc-55cff93ded68/Capturar5.PNG)

Na sessão **Destino**, manter o Projeto como `rj-sms-sandbox` e defina um **Conjunto de dados** no qual você tenha acesso:



![upload-planilha-destino](https://t9013004335.p.clickup-attachments.com/t9013004335/267e9c02-aa8f-4580-b661-c2214dd3cbbf/image.png)

Em seguida, insira o nome que deseja para a tabela em **Tabela**:

![upload-planilha-tabela](https://t9013004335.p.clickup-attachments.com/t9013004335/04af7e4e-b598-4ca5-bcfd-e81b105a8bf2/image.png)

Por último, declare ==MANUALMENTE== o o Esquema (nome das colunas, tipagem e se aceita valores vazios):

![upload-planilha-esquema](https://t9013004335.p.clickup-attachments.com/t9013004335/d7bdf7fa-cdfb-4f28-b162-6075cc0dcb38/image.png)

Após clicar em criar tabela e seus dados ficarão disponíveis dentro do seu Dataset após essa mensagem de confirmação:

<figure markdown="span">
![upload-planilha-criada](https://t9013004335.p.clickup-attachments.com/t9013004335/3e7e3289-3650-4c90-9d48-80f41222fece/Capturar7.PNG)
  <figcaption> </figcaption>
</figure>



## Criação de Views via SQL

Para criar uma tabela no BigQuery usando SQL, você pode usar a declaração `CREATE TABLE`. Segue  um exemplo básico de como criar uma tabela usando SQL dentro do BigQuery:

```sql
CREATE TABLE meu_projeto.meu_dataset.minha_tabela (
    id INT64,
    nome STRING,
    idade INT64,
    email STRING
);
```

Para criar uma view no BigQuery usando SQL, você pode usar a instrução CREATE VIEW :

```sql
CREATE VIEW `seu-projeto.seu_dataset.nome_da_view` AS
SELECT
  coluna1,
  coluna2,
  coluna3
FROM
  `seu-projeto.seu_dataset.sua_tabela`
WHERE
  condição;

```
