## Utilizando AI Search para indexação e consulta de dados

<br>
<p align = "justify"><b>Descrição</b>: Neste LAB, aplicaremos técnicas de organização de documentos e conduziremos pesquisas eficientes através da ingestão de dados, seguindo três passos essenciais: ingestão de conhecimento de IA, criação do índice correspondente e exploração dessas funcionalidades. Ao final, ganharemos uma visão prática das potencialidades oferecidas por essas ferramentas na mineração de conhecimento.
<br><br>

> [!NOTE]
> As etapas descritas neste laboratório foram executadas a partir das orientações presentes na documentação oficial do Microsoft Azure: 
> <a href = "https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/11-ai-search.html">Explore an Azure AI Search index (UI)</a>

<br><br>

## Criando os recursos necessários
<p align = "justify">Para atingir a proposta deste laboratório, primeiro será necessária a criação de dois recursos: (1) <b>Azure AI Search</b>, que gerenciará a indexação e a consulta dos dados e (2) <b>Azure AI services</b>, que fornece serviços capazes de enriquecer os dados em sua fonte com insights gerados por IA.

### Criando o recurso no Azure AI Search
1. Acesse o <a href = "https://portal.azure.com/">portal Azure</a> e faça o login através da conta cadastrada;
2. Ao acessar a página inicial do portal, selecione a opção "<i>+ Create a resource</i>", pesquise por <i>Azure AI Search</i> e, ao localizá-lo, clique em "<i>Create</i>":
<br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/selectAISearchResource.png" align = "center"/>
<br>
A opção selecionada acima irá carregar a página de criação do recurso <i><b>Azure AI Search</b></i>, cujas configurações que serão preenchidas podem ser visualizadas a seguir:

  - <b><i>Subscription</i></b>: Sua assinatura do Azure;
  - <b><i>Resource group</i></b>: Selecione um criado anteriormente ou crie um novo grupo de recursos;
  - <b><i>Service name</i></b>: Insira um nome exclusivo contendo apenas letras minúsculas;
  - <b><i>Location</i></b>: ```East US```;
  - <b><i>Pricing tier</i></b>: ```Basic```;

3. Selecione "Review + create", depois "Create" e aguarde a conclusão do <i>deploy</i>.


### Criando o recurso no Azure AI Services
<p align = "justify">Visto que este recurso foi criado nos laboratórios anteriores, irei incluir apenas a descrição dos campos a serem preenchidos, porém caso tenha dúvidas quanto ao caminho que deve seguir, é possível consultar maiores detalhes no <a href = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/tree/main/lab02_visionStudio">laboratório 02</a>.

1. Novamente no <a href = "https://portal.azure.com/">portal Azure</a>, selecione a opção "<i>+ Create a resource</i>", pesquise por <i>Azure AI services</i> e, ao localizá-lo, clique em "<i>Create</i>":
    A opção selecionada acima irá carregar a página de criação do recurso <i><b>Azure AI services</b></i>, cujas configurações que serão preenchidas podem ser visualizadas a seguir:
  - <b><i>Subscription</i></b>: Sua assinatura do Azure;
  - <b><i>Resource group</i></b>: Um grupo é uma coleção de recursos que compartilham o mesmo ciclo de vida, permissões e políticas - Selecione um criado anteriormente ou crie um novo grupo de recursos;
  - <b><i>Region</i></b>: ```East US```;
  - <b><i>Name</i></b>: Insira um nome exclusivo;
  - <b><i>Pricing tier</i></b>: ```Standard S0```;
  - <b><i>By checking this box I acknowledge that I have read and understood all the terms below</i></b>: Marque esta opção.

3. Selecione "Review + create" e depois "Create" e aguarde a conclusão do <i>deploy</i>.

### Criando uma conta de armazenamento
1. Retorne para a página inicial do Azure portal e busque pela opção <i><b>Storage accounts</b></i> (caso esta opção não apareça no início, é possível selecionar "<i>+ Create a resource</i>" para buscá-la): <br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/selectStorageAccounts.png" align = "center"/> 

2. Dentro de <i>Storage accounts</i>, selecione "<i>+Create</i>" e note que serão apresentados campos para preenchimento iguais aos vistos nos demais recursos já criados, sendo:

  - <b><i>Subscription</i></b>: Sua assinatura do Azure;
  - <b><i>Resource group</i></b>: Selecione um criado anteriormente ou crie um novo grupo de recursos;
  - <b><i>Storage account name</i></b>: Insira um nome exclusivo contendo apenas letras minúsculas;
  - <b><i>Performance</i></b>: ```Standard```;
  - <b><i>Pricing tier</i></b>: ```Basic```;
  - <b><i>Redundancy</i></b>: ```Locally redundant storage (LRS)```

> [!NOTE]
> Existem outras opções de configurações para a criação da conta de armazenamento, porém para o laboratório seguiremos as orientações da documentação. Desta forma, não iremos alterar nada além dos pontos descritos acima. 

3. Uma vez que o <i>deploy</i> tenha sido finalizado, acesse o recurso criado; 

4. O <i>Storage account</i> possui regras de segurança que devem ser configuradas para a execução deste laboratório. Para isto, procure a opção "<i>Configuration</i>" no menu esquerdo: <br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/storageAccountView.png" align = "center"/>

5. Altere para ```Enabled``` a opção "<i>Allow Blob anonymous access</i>" e clique em "<i>Save</i>". <br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/enableAllowBlob.png" align = "center"/>


## Carregando documentos no Azure Storage
1. Sem precisar sair da tela em que estávamos na etapa anterior, procure pela opção "<i>Containers</i>" no menu esquerdo: <br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/selectContainers.png" align = "center"/>

2. Na criação do <i>container</i>, clique em "<i>+ Container</i>" e adicione as seguintes informações: 

  - <b><i>Name</i></b>: ```coffee-reviews```;
  - <b><i>Public access level</i></b>: ```Container (anonymous read access for containers and blobs)```;
  - <b><i>Advanced</i></b>: Não é necessário alterar. <br> 
  <img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/newContainer.png" align = "center"/>

3. Clique em "<i>Create</i>";

4. Após a criação, acesse a documentação para fazer o download dos <a href = "https://aka.ms/mslearn-coffee-reviews">documentos</a> que serão adicionados ao <i>container</i> criado. Concluído o download, descompacte a pasta com as <i>reviews</i>;

5. Na página <i>Containers</i>, selecione "<i>coffee-reviews</i>" e faça upload dos arquivos de <i>review</i> da pasta baixada: <br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/uploadReviews.png" align = "center"/>

## Extraindo insights dos documentos
<p align = "justify">Agora que os documentos estão armazenados no Azure Storage, iremos começar a extrair informações deles. 

1. Acesse o <i><b>Azure AI Search</b></i> - O recurso pode ser acessado pelo portal Azure ou pela busca na página em que finalizamos a última etapa executada: <br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/aiSearch.png" align = "center"/>

2. Selecione o recurso criado para o AI Search e clique em "<i>Import data</i>": <br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/importData.png" align = "center"/>

3. Em <i>Import data</i> serão apresentadas algumas abas. Iremos na guia "<i>Connect to your data</i>" e selecionaremos "<i>Azure Blob Storage</i>" para "<i><b>Data Source</b></i>". Ao selecionar o <i>data source</i>, serão preenchidos os valores:

  - <b><i>Data Source</i></b>: ```Azure Blob Storage```;
  - <b><i>Data source name</i></b>: ```coffee-customer-data```;
  - <b><i>Data to extract</i></b>: ```Content and metadata```; 
  - <b><i>Parsing mode</i></b>: ```Default```; 
  - <b><i>Connection string</i></b>: ```Choose an existing connection```, selecione a conta de armazenamento e o <i>container</i> ```coffee-reviews```. Clique em "<i>Select</i>";
  - <b><i>Managed identity authentication</i></b>: ```None```; 
  - <b><i>Container name</i></b>: Este campo será preenchido automaticamente depois que definirmos a conexão; 
  - <b><i>Blob folder</i></b>: Deixar em branco; 
  - <b><i>Description</i></b>: ```Reviews for Fourth Coffee shops.```

4. Selecione "<i>Next: Add cognitive skills (Optional)</i>";

5. Seguindo a documentação, na tela <i>Add cognitive skills (Optional)</i>, selecione o seu recurso <i>Azure AI services</i> na opção <i><b>Attach Cognitive Services</b></i>;

6. Em <i><b>Add enrichments</b></i> preencha os seguintes campos:

  - <b><i>Skillset name</i></b>: ```coffee-skillset```;
  - <b><i>Enable OCR and merge all text into merged_content field</i></b>: Habilite esta opção;
  - <b><i>Source data field</i></b>: ```merged_content```; 
  - <b><i>Enrichment granularity level</i></b>: ```Pages (5000 character chunks)```; 
  - <b><i>Enable incremental enrichment</i></b>: <b>Não</b> marque esta opção;
  - Marque os campos:
    - Extract location names (locations); 
    - Extract key phrase (keyphrases);  
    - Detect sentiment (sentiment);
    - Generate tags from images (imageTags);
    - Generate captions from images (imageCaption)
  - Siga para <i><b>Save enrichments to a knowledge store</b></i> e marque as opções:
    - Image projections
    - Documents (Automaticamente serão selecionadas as demais opções: Pages, Key phrases, Entities, Image details e Image references). 
  
  > [!NOTE]
  > Neste ponto, assim que selecionei a opção <i>Image projections</i>, um aviso em vermelho apareceu na tela. 
  > <br><br>
  > <img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/chooseConnection.png" aling = "center"/>
  > <br><br>
  > Caso seja apresentado o mesmo alerta, clique em "<i>Choose an existing connection</i>", selecione a sua conta de armazenamento > "<i>+ Container</i>" > <b><i>Name</i></b>: ```knowledge-store```, <b><i>Anonymous access level</i></b>: ```Private``` e clique em "<i>Create</i>"; <br>
  > Selecione o <i>container</i> criado e clique em "<i>Select</i>". 


7. Selecione <b><i>Azure blob projections</i></b>: Document e não altere o nome do <i>container</i>;

8. Siga para "<i>Next: Customize target index</i>" e valide as seguintes opções:
  - <b><i>Index name</i></b>: ```coffee-index```;
  - <b><i>Key</i></b>: ```metadata_storage_path```;
  - <b><i>Suggester name</i></b>: Deixe em branco;
  - <b><i>Search mode</i></b>: Mantenha o valor pré-definido;

9. Marque a opção "<b><i>filterable</i></b>" para todos os campos selecionados automaticamente;

10. Siga para "<i>Next: Create an indexer</i>" e preencha: 
  - <b><i>Indexer name</i></b>: ```coffee-indexer```;
  - <b><i>Schedule</i></b>: ```Once```;
  <p align = "justify">Nas opções avançadas, atente-se a:

  - <b><i>Base-64 Encode Keys</i></b>: Mantenha esta opção selecionada;

11. Selecione "<b><i>Submit</i></b>" no final da página. O indexador será gerado e executará automaticamente o pipeline de indexação, que:
 - Extrai os campos de metadados do documento e o conteúdo da fonte de dados;
 - Executa o conjunto de habilidades cognitivas para gerar campos mais enriquecidos;
 - Mapeia os campos extraídos para o índice.

12. Volte para a página do recurso Azure AI Search criado para este laboratório. No menu esquerdo, procure pela opção "<b><i>Indexers</i></b>: <br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/selectIndexers.png" aling = "center"/>

13. Na tela apresentada, selecione o indexador ```coffee-indexer```: <br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/coffeeIndexer.png" aling = "center"/>

> [!NOTE]
> Caso a seleção do indexer retorno erro, clique em <i>Refresh</i> até que seja retornado sucesso na execução. 

<details>
        <summary>JSON de definição do indexador</summary> 
        ```

          {
              "@odata.context": "https://diolab004.search.windows.net/$metadata#indexers/$entity",
              "@odata.etag": "\"0x8DC64E3EFB99FED\"",
              "name": "coffee-indexer",
              "description": "",
              "dataSourceName": "coffee-customer-data",
              "skillsetName": "coffee-skillset",
              "targetIndexName": "coffee-index",
              "disabled": null,
              "schedule": null,
              "parameters": {
                "batchSize": null,
                "maxFailedItems": 0,
                "maxFailedItemsPerBatch": 0,
                "base64EncodeKeys": null,
                "configuration": {
                  "dataToExtract": "contentAndMetadata",
                  "parsingMode": "default",
                  "imageAction": "generateNormalizedImages"
                }
              },
              "fieldMappings": [
                {
                  "sourceFieldName": "metadata_storage_path",
                  "targetFieldName": "metadata_storage_path",
                  "mappingFunction": {
                    "name": "base64Encode",
                    "parameters": null
                  }
                }
              ],
              "outputFieldMappings": [
                {
                  "sourceFieldName": "/document/merged_content/pages/*/locations/*",
                  "targetFieldName": "locations"
                },
                {
                  "sourceFieldName": "/document/merged_content/pages/*/keyphrases/*",
                  "targetFieldName": "keyphrases"
                },
                {
                  "sourceFieldName": "/document/merged_content/pages/*/sentiment",
                  "targetFieldName": "sentiment"
                },
                {
                  "sourceFieldName": "/document/merged_content",
                  "targetFieldName": "merged_content"
                },
                {
                  "sourceFieldName": "/document/normalized_images/*/text",
                  "targetFieldName": "text"
                },
                {
                  "sourceFieldName": "/document/normalized_images/*/layoutText",
                  "targetFieldName": "layoutText"
                },
                {
                  "sourceFieldName": "/document/normalized_images/*/imageTags/*/name",
                  "targetFieldName": "imageTags"
                },
                {
                  "sourceFieldName": "/document/normalized_images/*/imageCaption",
                  "targetFieldName": "imageCaption"
                }
              ],
              "cache": null,
              "encryptionKey": null
          }
        
        ```
</details>        

## Consultando o index criado
<p align = "justify">Usaremos o <i><b>Search Explorer</b></i> para criar consultas para o index criado - O explorer é uma ferramenta Azure que permite validar a qualidade do índex; com ele, é possível escrever consultas e revisar resultados em JSON. 

1. Dentro do serviço de pesquisa, em "<b><i>Overview</i></b>", selecione "<b><i>Search explorer</i></b>": <br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/searchExplorer.png" aling = "center"/>

2. Será apresentada a tela de pesquisa, na qual iremos selecionar a opção "<b><i>View</i></b>" e, em seguida, iremos definir a consulta para "<b><i>JSON view</i></b>"; 

3. A primeira consulta que iremos executar visa retornar um JSON com todos os documentos no index, incluindo o campo <b><i>@odata.count</i></b> que traz a contagem total destes documentos. Para exibir tal retorno, no campo <b><i>JSON query editor</i></b>, inclua a consulta a seguir e clique em "<b><i>Search</i></b>": 
  ```
    {
    "search": "*",
    "count": true
  }
  ```

  <img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/searchCount.png" aling = "center"/>

4. Após receber o retorno com todos os documentos, iremos filtrar por local. Para isto, copie a seguinte consulta no <b><i>JSON query editor</i></b> e clique em "<b><i>Search</i></b>": 
  ```
    {
      "search": "locations:'Chicago'",
      "count": true
    }
  ```

  <img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/searchChicago.png" aling = "center"/>

5. Como última consulta, iremos filtrar as avaliações negativas, copiando a consulta abaixo no <b><i>JSON query editor</i></b> e clicando em "<b><i>Search</i></b>":
  ```
    {
      "search": "sentiment:'negative'",
      "count": true
    }
  ```

  <img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab04_cognitiveSearch/assets/searchNegative.png" aling = "center"/>
