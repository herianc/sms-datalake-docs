
# Campos

## Quais variáveis manter, quais adicionar e quais remover

Mantemos nossas tabelas parcialmente **normalizadas**, e temos regras para quais variáveis incluirmos em produção. Elas são:

* Remover variáveis de nomes de entidades que já estão no Data Lake. Exemplo: retirar `nome_estabelecimento` da tabela que já inclui `id_cnes`.

* Remover variáveis já utilizadas como partição. Exemplo: remover `ano` e `data` se a tabela é particionada nessas duas dimensões.

* Manter todas as chaves primárias que já vem com a tabela, mas (1) adicionar chaves estrangeiras relevantes (ex.  `id_municipio`) e (2) retirar chaves  estrangeiras irrelevantes (e.g. `regiao`).


## Nomeação & Ordenamento dos Campos

### Ordem das colunas

A ordem das colunas em tabelas é padronizada para manter uma consistência no Data Lake. Nossas regras são:
* As chaves sempre são as primeiras colunas: primeiro a chave primária, depois as chaves estrangeira em ordem descendente de abrangência. 
    * Exemplo de ordem para uma tabela de atendimento: `id_atendimento`, `id_uf`, `id_municipio`, `id_cnes`, `paciente_cpf`.
* Agrupar e ordenar colunas por importância ou temas.

### Nomeação das colunas

Os campos, de forma geral, deve seguir o seguinte Padrão:

`[id_][<entidade>_][<dimensão>][_<unidade>]`

### Regras gerais

Nomes de variáveis devem respeitar algumas regras:

* Usar ao máximo nomes já presentes no repositório. Exemplos: estabelecimento, material, profissional_saude, episodio_assistencial

* Ser o mais intuitivo, claro e extenso possível.

* Ter todas letras minúsculas (inclusive siglas), sem acentos, conectados por _.

* Não incluir conectores como de, da, dos, e, a, em, etc.

#### `id_`

Como regra geral, as colunas de chave primária e estrangeiras devem começar com o prefixo id  a fim de mantermos todas as colunas de chave agrupadas ao trabalhar com qualquer ferramenta de viz.
Ex. `id_cnes`, `id_material`, `id_municipio`, `id_cid`

Só devem ter o prefixo id_ quando a variável representar chaves primárias de entidades no Data Lake (que tem ou que eventualmente terão uma tabela no lake).

Alguns casos especiais, quando o campo é notoriamente conhecido como uma chave (ex. CPF ou CNS) o uso do prefixo pode ser descartado para fim de legibilidade.

#### `<entidade>` (Prefixo opcional)

Por modelarmos os dados no estilo de fatos e dimensões, nossas tabelas não são rigorosamente normalizadas. Por isso, é comum termos na mesma tabela colunas relativas à diferentes entidades.
Por isso adotamos o seguinte tratamento: adiciona*se o prefixo de entidade quando a entidade da coluna for diferente da entidade da tabela. Exemplos: 

* numa tabela sobre a entidade estoque, uma coluna contendo a forma farmacêutica do material seria declarada assim: material_forma_farmaceutica.

* numa tabela sobre a entidade material, características do material seriam descritas como forma_farmaceutica, posologia, grupo_terapeutico, etc.

No caso específico das tabelas da camada ouro, onde as tabelas estão altamente desnormalizados, recomenda-se que todas as colunas contenham o prefixo da entidade à qual pertecem. Essa prática facilita a busca das informações nas ferramentas de BI, visto que elas, geralmente, ficam ordenadas alfabeticamente.

#### `<dimensão>` (Obrigatório)

Nomear a dimensão é algo menos estruturado e, por isso, requer bom senso. Neste quesito a única restrição é respeitar as regras gerais listadas no início desta sessão.

#### `<_unidade>` (Sufixo opcional)  

Recomenda*se a utilização de sufixos para declaração da unidade a fim de aumentar a legibilidade do campo. Lista de unidades permitidas:

* `_nome`,
* `_data`
* `_datahora`,
* `_valor`,
* `_quantidade`,
* `_proporcao` (variáveis de porcentagem 0*100%),
* `_taxa`,
* `_razao`,
* `_indicador` (variáveis do tipo booleano),
* `_tipo`,
* `_sigla`

### Representação dos Valores


#### Limpando STRINGs

A limpeza dos campos vai variar conforme o propósito de cada tabela nas camadas Gold, sendo convencionado apenas para os tiers Bronze e Prata os seguintes pontos, do mais abrangente para o mais específico:

* Todo campo string não deve iniciar com espaços ou tab (\n), esses devem ser padronizados usando a função TRIM().

* Campos de strings nullables: deverão ser padronizadas utilizando a macro process_null que garante que strings como "NaN" "None" e "" sejam transformadas em em null, segundo o formato do BigQuery.

* Campos numéricos: deverão conter limpeza de caracteres não numéricos segundo a macro clean_numeric_string.

* Campos categóricas: deverão ser padronizadas com inicial maiúscula e resto minúsculo, sem acentos seguindo macro remove_accents_upper e capitalize_first_letter nessa ordem.

* Campos de nome e endereço (ex: nome de estabelecimento, nome próprio e logradouro) devem ser padronizadas com letras maíusculas no inicio de cada nome e preposições em minúscula, sem acentos ou símbolos*. Utiliza*se as macros remove_accents_upper seguida de proper_estabelecimento ou proper_br. 

* Em campos de texto livre, mantemos igual aos dados originais.

* Analisar casos de símbolos especificos que quando retirados temos perda semântica. Ex: "AV JULIO DE AMORIM PEREIRA  S/N" → "AV JULIO DE AMORIM PEREIRA  SN" 

### Formatos de valores

* **Decimal**: formato americano, i.e `.` sempre . (ponto) ao invés de `,` (vírgula).
* **Data**: `YYYY-MM-DD`
* **Horário (24h)**: `HH:MM:SS`
* **Datetime (ISO-8601)**: `YYYY-MM-DDTHH:MM:SS.sssZ`
* **Valor nulo**: `""` (csv, Stata), `NULL` (Python), `NA` (R) ou `.`
* **Proporção/porcentagem**: entre 0*100
* **Booleano**: `false`/`true`
* **Sexo**: Masculino/Feminino

## Colunas de Particionamento da Tabela

Uma tabela particionada é uma tabela especial dividida em segmentos, chamados de partições, que facilitam o gerenciamento e a consulta de seus dados. Ao dividir uma grande tabela em partições menores, você pode melhorar o desempenho da consulta e pode controlar os custos reduzindo o número de bytes lidos por uma consulta. Por isso, sempre recomendamos que tabelas grandes sejam particionadas. Leia mais 
a respeito na documentação da Google Cloud.

Em geral, o particionamento é feito sobre o valors de uma data criando-se três colunas novas cujos nomes são dados por: `ano_particao`, `mes_particao`,`data_particao` e que possuem os respectivos formatos 'YYYY', 'MM', 'YYYY-MM-DD'. 