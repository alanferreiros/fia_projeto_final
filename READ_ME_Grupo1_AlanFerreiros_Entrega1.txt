ENTREGA 1

Projeto Final
FIA - MBA - Analytics in Big Data

|**Author** | Alan Ferreirós          |
|**Date**   | 04-May-2020             |
|**e-mail** | alan.ferreiros@gmail.com|

GitHub: (https://github.com/alanferreiros/fia_projeto_final)

========= OBSERVAÇÕES =========

Fiz os notebook em inglês, porque dependendo dos resultados gostaria de utilizar como exemplo no meu trabalho e gostaria de publicar no GitHub e talvez no Kaggle.

Caso isto seja um impeditivo, posso passar para português nas próximas entregas.

========= INSTRUÇÕES =========

As análises estão divididas em 3 notebooks:

1. "Grupo1_setup": Importa os CSVs para dataframes. Também faz "display" de cada um para verificar a importação.

2. "Grupo1_exploratoria_part1": Início da análise exploratória. Se fizer um "Run All", ele automaticamente executa o notebook Grupo1_setup e importa os dataframes para este notebook.

3. "Grupo1_exploratoria_part2": Continuação do "part1". Da mesma forma, se fizer um "Run All" deste notebook ele executa o "part1" automaticamente.
(precisei criar 2 notebooks para a análise exploratória porque o controle de versão do Databricks começou a falhar devido ao tamanho)

========= CONTEÚDO =========

"Grupo1_setup"
- "mount" do S3 no sistema do Databricks;
- Verificação do conteúdo disponibilizado no S3;
- Criação de dataframes a partir dos CSVs;
- Verificação dos dataframes ("display")


"Grupo1_exploratoria_part1"
- Execução do "Grupo1_setup" (comando %run)
-- Summary of dataframes
--- Challenge on Keagle (resumo das informações divulgadas no Keagle)

-- Raw Database Analysis
    Descrição das variáveis de todos os dataframes
    Classificação de variáveis (ID, categóricas ou quantitativas)
    Tipos de variáveis (string, int, timestamp...)
    Amostras
    Quantidade de "Not Availables" e porcentagens
    Número de valores distintos para cada variável

-- Initial Analysis/Study proposal
    Proposta de problema a ser resolvido utilizando a base
    Resumo: Encontrei uma oportunidade de melhorar a estimativa de prazo de entrega. Observei que o prazo existente é muito mais longo do que precisava ser, e isso pode dificultar as vendas de forma desnecessária.

-- Dataframes preparation
--- Initial dataframe definition
     Segregação de camada raw da camada de análise
     Seleção dos dataframes de interesse para o problema em questão
--- Check integrity of id columns
     Verifica se os IDs dos dataframes estão consistentes entre eles
---- Orders without items
      Nota-se que alguns pedidos não têm itens associados
      Executa o tratamento adequado
---- Sellers without geolocation
      Nota-se que o CEP de alguns vendedores (seller) não possuem correspondência na tabela de geolocalização
      Adiciona-se manualmente os CEPs faltantes
---- Repeated zip codes
      Existem CEPs repetidos.
      Faz o tratamento adequado.
--- Adjustments of column types
     Ajustes nas colunas de "timestamp" e "date"
--- Treatment of null values
     Levantamento de valores nulos (faltantes) nos dataframes.
---- Nulls in df_orders
      Executa o tratamento de nulos no dataframe de pedidos.
      - order_delivered_customer_date
---- Nulls in df_products
      Executa o tratamento de nulos no dataframe de produtos.
      (algumas características dos produtos não estavam preenchidas).
      Estratégia:
       Produtos sem categoria:
         Categoria foi inferida com base na principal categoria que o vendedor trabalha.
       Produtos sem características (tamanho, peso...):
         Características foram inferidas com base na mediana dos produtos daquela categoria daquele vendedor.
       Produtos em que a categoria não pôde ser inferida:
         Uma nova categoria foi criada para este caso: 'UNKNOWN_CATEGORY'
         Características foram inferidas como a mediana dos produtos da base.
---- Relevant columns
      Levantamento de quais colunas serão relevantes para este estudo.
---- Create target
      Definição de uma variável target (objetivo) para ser inferida pelos modelos.
      (no caso, será a diferença entre a data de aprovação do pedido e a entrega)
--- Get only orders with 1 item
     Visto que 90% da base possui apenas 1 item no pedido, foi adotado que somente estes serão utilizados na previsão.
     Pedidos com mais de 1 item podem complicar muito a análise, visto que não se sabe qual dos itens foi responsável pelo prazo de entrega.

O resultado de cada uma das etapas acima foi o dataframe df_analysis_v6, que foi utilizado no notebook seguinte.

"Grupo1_exploratoria_part2"
- Execução do "Grupo1_exploratoria_part1" (comando %run)
-- Metrics
--- Categorical variables
     - Lista de variáveis categóricas (qualitativas)
     - Levantamento de quantidades de categorias em cada uma
     - Verificação de que não existem mais valores nuloes entre elas
     - Frequências de cada uma delas
     - Análise da influência entre estado de entrega/envio e o tempo de entrega
     Nota: Muitas das variáveis categóricas possuem milhares de valores possíveis, por conta disso as frequências em geral foram mostrada em tabelas. Quando possível, foram montados gráficos de barra.
     
---- Check if orders are concentrated in a few categories
      Apesar de existirem milhares de categorias, talvez os pedidos se concentrem em apenas algumas delas.
      Este estudo verifica estas características e sugere tratamentos posteriores e substituição por outros valores (feature engineering)

--- Quantitative variables
     - Lista de variáveis quantitativas
     - Levantamento de medidas de posição
     - Box plots
       Nota: foram constatados muitos outliers. Porém, no fim se demonstra que eles podem ser explicados ao fazer análises bi-variadas.
     - Levantamento de medidas de dispersão
     - Análise de distribuição de valores de datas.
     - Análise da distribuição de clientes e vendedores (latitude e longitude)
     
     
     

