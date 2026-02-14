# Governança

## Visão Geral

O Data Lake da Saúde faz parte de uma iniciativa maior, capitaneada pela Prefeitura do Rio na figura do Escritório de Dados, a fim de promover iniciativas transversais entre as Secretarias, visando à melhoria da gestão pública e ao bem estar da população.

Cada Secretaria possui o seu Date Lake, mas que são mantidos por infraestrutura comum no Google Cloud. Na prática os dados das Secretarias são isoladas em ambientes exclusivos das mesmas, enquanto que a infraestrutura das camadas de orquestração e processamento das cargas de trabalho é compartilhada por toda a Prefeitura. Para mais detalhes ver Visão Geral da Infraestrutura.

![visao-geral](https://t9013004335.p.clickup-attachments.com/t9013004335/d733f9ac-0788-4f75-8c9a-4fa2baf99b40/image.png)

## Autonomia sobre os dados

Os dados de cada Secretaria são de total gestão própria. São elas que decidem o que, para quem e como compartilhar seus dados com outros atores públicos ou privados.

Porém, elas precisam seguir alguns padrões de organização da informação para garantir a interoperabilidade dos dados entre todos os demais agentes da Prefeitura.

## Interoperabilidade dos dados da Prefeitura

A fim de garantir o a interoperabilidade dos dados da Prefeitura, o Escritório de Dados também normatiza e audita como as informações devem ser organizadas dentro do Data Lake:

- Padrão de nomenclatura dos conjuntos de dados (datasets);
- Padrão de nomenclatura das tabelas;
- Padrão de nomenclatura dos campos;

Para mais detalhes ver o Manual de Estilo da Prefeitura.

## Papéis e Responsabilidades

### Prefeitura & Secretaria de Saúde
A SMS desenvolve e ser responsabiliza pelo conteúdo do Lake:

- Desenvolve e mantém as extrações e transformações dos dados (os pipelines de dados);
- Constrói e mantém os seus casos de uso (dashboards, reports, modelos de machine learning e outros produtos de dados);
- Controla os acessos para agentes internos e externos à SMS;
- Conforma e se responsabiliza perante à LGPD.

Enquanto isso, o Escritório de Dados, é responsável por fornecer e manter a infraestrutura do Data Lake no nível mais básico:

- Desenvolve sustenta os serviços onde rodam as cargas de trabalho para extração, carga e transformação dos dados (cluster em Kubernets, serviços de Containers, ...);
- Desenvolve e sustenta a camada de orquestração dos pipelines (Prefect);
- Desenvolve e sustenta as esteiras de CI/CD para deploy automático dos pipelines de dados;
- Regulamenta e garante a conformidade das informações dentro dos Data Lakes (BigQuery) das Secretarias.

### Dentro da Secretaria de Saúde

Dentro da SMS, o Comitê de Inovação e Tecnologia (CIT) é a figura que faz a interface com a Prefeitura. Suas responsabilidades são:

- Desenvolve os pipelines de dados da SMS
- Capacita outros atores dentro da SMS no desenvolvimento de novos pipelines de dados
- Monitora e garante a saúde (segurança, privacidade, acuracidade, disponibilidade e usabilidade) dos ativos de dados (tabelas do Data Lake; dashboards, reports, modelos de machine learning e outros produtos de dados conectados ao Data Lake)
- Gere os acessos ao Data Lake da SMS
- Garante a conformidade com os padrões de Governança da Prefeitura
- Apoia o Escritório de Dados no desenvolvimento e experimentação de novas features e melhorias de infraestrutura e engenharia de dados para o Data Lake

Já os demais atores da SMS:

- Consomem as informações do Data Lake e desenvolvem seus próprios reports, dashboards e modelos de machine learning.
- (Opcional) Desenvolvem novos pipelines de dados, contribuindo para expansão deste projeto (para mais detalhes ver [Contribuindo ao Projeto](./guia-desenvolvedores/contribuindo-projeto.md))