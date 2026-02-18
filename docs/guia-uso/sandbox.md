O Sandbox, ou Caixa de Areia  , é uma área dentro do nosso BigQuery onde os usuários podem criar novos conjuntos de dados, sejam tabelas materializadas ou views, a partir de upload de arquivos, conexão com com planilhas do Google Sheets, por instrução SQL ou qualquer outra linguagem de programação a partir de bibliotecas específicas. Nele também é possível consultar e mesclar os dados que estão em produção sem o risco de qualquer tipo consequência nesse ambiente.

O ambiente do Sandbox dentro do BigQuery é destinado aos atores internos e externos à Secretaria de Saúde para testarem com autonomia e agilidade. É um espaço seguro onde os analistas e cientistas de dados podem experimentar com os dados, testar novas ideias ou modelos sem o risco de afetar os dados principais ou os sistemas de produção.

Pense nisso como um laboratório onde você pode trabalhar com cópias dos dados reais ou com dados sintéticos para desenvolver novos algoritmos, relatórios ou visualizações.

Aqui estão alguns pontos importantes sobre o Sandbox no Data Lake:

1. **Isolamento**: O Sandbox mantém suas experiências separadas do ambiente principal, garantindo que o trabalho regular não seja interrompido.
2. **Liberdade para inovar**: Como é um ambiente controlado, há mais liberdade para tentar coisas novas sem medo de cometer erros que possam ter grandes consequências.
3. **Desenvolvimento ágil**: Ajuda a acelerar o desenvolvimento de projetos de dados, pois as mudanças podem ser feitas e testadas rapidamente.
4. **Segurança dos dados**: Protege os dados sensíveis ou críticos ao permitir que os usuários trabalhem com amostras, versões anonimizadas ou sintéticas dos dados.
5. **Aprendizado e crescimento**: É um ótimo lugar para os membros da equipe melhorarem suas habilidades, pois eles podem aprender fazendo, sem pressão.

!!! warning "Importante"
    O Sandbox não é ambiente produtivo!

Por um lado isso é ótimo, vocês têm total autonomia para criar, modificar e destruir tudo dentro dos seus ambiente. Por outro lado, ele não possui as mesma proteções que o ambiente produtivo possui:
Política de Backup: inexistente neste ambiente. 
Você é responsável por garantir o backup das informações que criou.
Teste, monitoramento e notificações automáticos: inexistente neste ambiente. 
Caso ocorra alguma indisponibilidade ou erro nos dados, total ou parcial, não haverá qualquer mecanismo de prevenção ou alerta.

Pelos motivos listados acima, **não recomendamos que utilizem o Sandbox como ambiente produtivo**. Isto é, não conectem no Sandbox seus reports, dashboards ou modelos de machine learning que irão ser consumidos pelos usuários finais. Pipelines de dados são vivos, inevitavelmente quebrarão e descobrir pelo usuário é uma experiência bem ruim 🫠.
Quando acreditarem que protótipo atingiu o nível de maturidade para subir para produção. Falem conosco, iremos te ajudar a subir para o ambiente produtivo 🤠. 


## **Como acessar o Sandbox?**
Você pode acessar o Sandbox a partir deste [link](https://console.cloud.google.com/bigquery?project=rj-sms-sandbox 'console.cloud.google.com') ou direto dentro BigQuery alterando o projeto que está conectado:

1. Clicar no nome do projeto para abrir a lista de opções disponíveis

![BigQuery - Projetos](https://t9013004335.p.clickup-attachments.com/t9013004335/f718605a-4bca-4de2-a014-a11e8c5bc4f8/image.png)

2. Selecionar o projeto do Sandbox:

![BigQuery - Projetos Sandbox](https://t9013004335.p.clickup-attachments.com/t9013004335/82f313e5-c142-4127-91e6-5214b704045d/image.png)

Uma vez no Sandbox, você verá um conjunto de dados (dataset) com o respectivo nome da sua área:

![BigQuery - Dataset](https://t9013004335.p.clickup-attachments.com/t9013004335/38b8ffef-d384-405a-bbad-b0de0496b17b/image.png)

Dentro do conjunto de dados da sua área, você tem total autonomia para criar, modificar e deletar tabelas e views.

Caso você não esteja vendo o ambiente produtivo (projeto rj-sms):

<figure markdown="span">
  ![Datasets](https://t9013004335.p.clickup-attachments.com/t9013004335/ee8c837d-4d28-4e77-9568-3b3ad25d5d43/image.png)
  <figcaption> </figcaption>
</figure>

Para adicioná-lo basta seguir as orientações em Primeiros Passos do link abaixo., substituindo o projeto basedosdados por rj-sms:

![Primeiros Passos](https://t9013004335.p.clickup-attachments.com/t9013004335/a441eb37-3a3f-40e6-8fc9-4eab5db7c214/bq_access_project_new.gif)


