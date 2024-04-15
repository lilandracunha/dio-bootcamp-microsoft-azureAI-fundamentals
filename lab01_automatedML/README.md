# Azure Machine Learning

<br>
<p align = "justify"><b>Descrição</b>: Neste LAB, vamos aprender a criar nossa conta no Azure e explorar as capacidades de Machine Learning da plataforma para desenvolver nossa primeira automação prática. Ao aplicar implementações e soluções escaláveis de aprendizado de máquina na nuvem da Microsoft, adquiriremos conhecimentos valiosos e experiência na construção de soluções eficientes.
<br><br>

> [!NOTE]
> As etapas descritas neste laboratório foram executadas a partir das orientações presentes na documentação oficial do Microsoft Azure: <a href = "https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html">Explore Automated Machine Learning in Azure Machine Learning</a>

<br><br>

## Criando o espaço de trabalho no Azure AI
Após criar uma conta no portal Azure, o primeiro passo para atuar com os serviços de <i>machine learning</i> disponibilizados é seguir para a configuração do espaço de trabalho:
1. Acesse o <a href = "https://portal.azure.com">portal Azure</a> e faça o login através da conta cadastrada;

2. Ao acessar a página inicial do portal, selecione a opção "<i>+ Create a resource</i>/+ Criar um recurso", pesquise por Azure Machine Learning e, ao localizá-lo, clique em "<i>Create</i>/Criar":
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab01_automatedML/assets/createAzureMLResource.png" align = "center"/>
<br>
A opção selecionada acima irá carregar a página de criação do recurso <i><b>Azure Machine Learning</b></i>, cujas configurações que serão preenchidas podem ser visualizadas a seguir:
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab01_automatedML/assets/createAzureMLResourceSettings.png" align = "center"/>

  - <b><i>Subscription</i>/Assinatura</b>: Sua assinatura do Azure - Por padrão, este campo já estará preenchido conforme a assinatura vigente na conta;
    - <b><i>Resource group</i>/Grupo de recursos</b>: Um grupo é uma coleção de recursos que compartilham o mesmo ciclo de vida, permissões e políticas - Selecione um criado anteriormente ou crie um novo grupo de recursos; 
  - <b><i>Name</i>/Nome</b>: Insira um nome exclusivo;
  - <b><i>Region</i>/Região</b>: Certifique-se de que a região selecionada tenha a série de máquinas virtuais necessária para os destinos do seu espaço de trabalho - Para os laboratórios será utilizada a região ```East US```;
  - <b><i>Storage account</i>/Conta de armazenamento</b>: Conta utilizada como armazenamento de dados padrão para o workspace; é possível criar um novo recurso de armazenamento ou selecionar um existente - Será definido um valor padrão para esse campo;
  - <b><i>Key vault</i>/Cofre de chaves</b>: Um "cofre de chaves" é usado para armazenar segredos e outras informações confidenciais necessárias ao espaço de trabalho - Será definido um valor padrão para esse campo;
  - <b><i>Application insights</i>/Insights de aplicação</b>: O espaço de trabalho utiliza o <i>Azure Application Insights</i> para armazenar informações de monitorização sobre os modelos implantados - Será definido um valor padrão para esse campo;
  - <b><i>Container registry</i>/Registro de contêiner</b>: Um registro de contêiner é usado para registrar imagens do Docker usadas em treinamento e implantações; para minimizar custos, um novo recurso do registro é criado apenas depois de construir a sua primeira imagem. Alternativamente, você pode optar por criar o recurso agora ou selecionar um existente em sua assinatura - Iremos preencher este campo como None/Nenhum (será criado automaticamente na primeira vez que você implantar um modelo em um contêiner).

3. Selecione "<i>Review + create</i>/Revisar + criar" e depois "<i>Create</i>/Criar" e aguarde a conclusão do <i>deploy</i>.

<br>

> [!IMPORTANT]
> Existem outras abas na criação do workspace, porém para o laboratório não iremos aplicar alterações para elas.

<br><br>

## Utilizando aprendizado de máquina automatizado para treinar um modelo
<p align = "justify">O aprendizado de máquina automatizado permite que sejam aplicados vários algoritmos e parâmetros para treinar modelos e identificar o que melhor se aplica aos seus objetivos. 
<p align = "justify">Para este laboratório será utilizado um <a href = "https://capitalbikeshare.com/system-data"><i>dataset</i> de detalhes históricos de aluguel de bicicletas</a> e o objetivo é treinar um modelo que prevê a quantidade de aluguéis esperada em um determinado dia com base em características sazonais e meteorológicas.

1. No <a href = "https://ml.azure.com/?azure-portal=true">estúdio Azure Machine Learning</a>, dentro do espaço de trabalho criado, acesse a opção ML Automatizado;
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab01_automatedML/assets/createNewAutomatedML.png" align = "center"/>

2. Crie um novo serviço de ML automatizado ("<i>+ New Automated ML job</i>/+ Novo trabalho de ML automatizado") com as configurações vistas logo abaixo, clicando em "<i>Next</i>/Avançar" conforme necessário para prosseguir:\
    <b><i>Basic settings</i>/Configurações básicas:</b>
    - <b><i>Job name</i>/Nome do trabalho</b>: ```mslearn-bike-automl```;
    - <b><i>New experiment name</i>/Novo nome do experimento</b>: ```mslearn-bike-rental```;
    - <b><i>Description</i>/Descrição</b>: ```Automated machine learning for bike rental prediction```;
    - <b><i>Tags</i>/Marcas</b>: Não é necessário preencher;

    <b><i>Task type & data</i>/Tipo de tarefa e dados:</b>
    - <b><i>Select task type</i>/Selecionar tipo de tarefa</b>: ```Regression/Regressão```;
    - <b><i>Select data</i>/Selecionar os dados</b>: Clique em "<i>+ Create</i>/+ Criar" e crie um <i>dataset</i> com as configurações a seguir: 
        - <b><i>Data type</i>/Tipo de dados</b>:
            - <b><i>Name</i>/Nome</b>: ```bike-rentals```;
            - <b><i>Description</i>/Descrição</b>: ```Historic bike rental data```;
            - <b><i>Type</i>/Tipo</b>: ```Tabular```;
        - <b><i>Data source</i>/Fonte de dados</b>:
            Selecione a opção "```From web files/De arquivos da Web```";
            - <b><i>Web URL</i>/URL da Web</b>:
                - <b><i>Web URL</i></b>: ```https://aka.ms/bike-rentals```;
                - <b><i>Skip data validation</i>/Ignorar validação de dados</b>: Não ativar esta opção; como descrito, "se você optar por ignorar a validação, não validaremos seu caminho de dados ou tentaremos acessar seus dados da versão prévia e do esquema";\
            Será feita uma validação da URL informada para que o processo possa prosseguir.
        - <b><i>Settings</i>/Configurações</b>:
            - <b><i>File format</i>/Formato do arquivo</b>: ```Delimited/Delimitado```;
            - <b><i>Delimiter</i>/Delimitador</b>: ```Comma/Vírgula```;
            - <b><i>Encoding</i>/Codificação</b>: ```UTF-8```;
            - <b><i>Column headers</i>/Cabeçalhos de coluna</b>: ```Only first file has headers/Somente o primeiro possui cabeçalho```;
            - <b><i>Skip rows</i>/Ignorar linhas</b>: ```None/Nenhuma```;
            - <b><i>Dataset contains multi-line data</i>/Conjunto de dados com dados de várias linhas</b>: Não marcar esta opção;\
        Finalizando estas configurações, teremos a seguinte visualização: 
        <img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab01_automatedML/assets/createDatasetSettings.png" align = "center"/>
                
        - <b><i>Schema</i>/Esquema</b>:
            - Para esta opção iremos selecionar tudo, exceto "<i>Path</i>";
        - <b><i>Review</i>/Examinar</b>: Revise as informações inseridas;
        Após revisar as informações, selecione "<i>Create</i>/Criar" e aguarde a criação do <i>dataset</i>. Uma vez criado o <i>dataset</i>, selecione-o e clique em <i>Next</i>/Avançar para seguir para a etapa de configuração das tarefas.
    
    <b><i>Task settings</i>/Configurações de tarefas</b>:
    - <b><i>Task type</i>/Tipo de tarefa</b>: ```Regression/Regressão```;
    - <b><i>Dataset</i>/Dados</b>: ```bike-rentals```;
    - <b><i>Target column</i>/Coluna de destino</b>: ```Rentals (integer)```;
    - <b><i>Additional configuration settings</i>/Configurações adicionais</b>:
        - <b><i>Primary metric</i>/Métrica primária</b>: ```Normalized root mean squared error (raiz do erro quadrático médio normalizado)```;
        - <b><i>Explain best model</i>/Explicar o melhor modelo</b>: Desmarque esta opção;
        - <b><i>Use all supported models</i>/Usar todos os modelos suportados</b>: Desmarque esta opção. Para o laboratório iremos restringir o modelo para tentar apenas algoritmos específicos;
        - <b><i>Allowed models</i>/Modelos permitidos</b>: Selecionar apenas os modelos ```RandomForest``` e ```LightGBM```. Em outros cenários faz-se importante testar o máximo possível, porém cada modelo testado aumenta o tempo de execução do job;
        <img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab01_automatedML/assets/automatedMLTaskSettings.png" align = "center"/>
        
    - <b><i>Limits</i>/Limites (expanda esta seção para visualizar as opções)</b>:
        - <b><i>Max trials</i>/Máximo de avaliações</b>: ```3```;
        - <b><i>Max concurrent trials</i>/Máximo de avaliações simultâneas</b>: ```3```;
        - <b><i>Max nodes</i>/Máximo de nós</b>: ```3```;
        - <b><i>Metric score threshold</i>/Limite de pontuação da métrica</b>: ```0.085```;
        - <b><i>Timeout</i>/Tempo limite do experimento (minutos)</b>: ```15```;
        - <b><i>Iteration timeout</i>/Tempo limite de iteração (minutos)</b>: ```15```;
        - <b><i>Enable early termination</i>/Habilitar o encerramento antecipado</b>: Selecione esta opção;
    - <b><i>Validation and test</i>/Validar e testar</b>:
        - <b><i>Validation type</i>/Tipo de validação</b>: ```Train-validation split/Divisão de validação de treinamento```;
        - <b><i>Percentage of validation data</i>/Validação do percentual de dados</b>: ```10```;
        - <b><i>Test dataset</i>/Tipos de teste</b>: ```None/Nenhum```;\

    <b><i>Compute</i>/Computação</b>:
    - <b><i>Select compute type</i>/Selecione o tipo de computação</b>: ```Serverless/Sem servidor```;
    - <b><i>Virtual machine type</i>/Tipo de máquina virtual</b>: ```CPU```;
    - <b><i>Virtual machine tier</i>/Tipo de máquina virtual</b>: ```Dedicated```;
    - <b><i>Virtual machine size</i>/Tamanho da máquina virtual</b>: ```Standard_DS3_V2```;
    - <b><i>Number of instances</i>/Número de instâncias</b>: ```1```.
3. Finalizando a etapa acima (<i>Compute</i>) basta clicar em <i>Next</i>/Avançar para revisar o trabalho:
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab01_automatedML/assets/submitTrainingJob.png" align = "center"/>

4. Após revisão, clicar em "<i>Submit training job</i>/Enviar trabalho de treinamento" e aguardar a conclusão do envio.

<br><br>

## Validando as métricas aplicadas
<p align = "justify">Após a conclusão do envio do trabalho é possível visualizar o resumo do que foi executado através da guia <i>Overview</i>/Visão geral. Dentro desta guia, selecione o melhor modelo retornado:
<br>  
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab01_automatedML/assets/bestModelSummary.png" align = "center"/>

<p align = "justify">Selecionando o melhor modelo serão apresentadas informações sobre ele e, neste momento, iremos acessar a guia <i>Metrics</i>/Métricas.
<p align = "justify">Através da guia <i>Metrics</i> podemos visualizar, dentre outras informações importantes, dois gráficos denominados <b><i>residuals</i></b> e <b><i>predicted_true</i></b>:
<br>  
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab01_automatedML/assets/bestModelMetrics.png" align = "center"/>

<p align = "justify">Observe os gráficos citados acima, pois eles mostram o desempenho do modelo: O gráfico <b><i>residuals</i></b> mostra os resíduos, as diferenças entre os valores previstos e os reais, enquanto que o gráfico <b><i>predicted_true</i></b> compara os valores previstos com os valores verdadeiros. 

<br><br>

## Implantando e testando o melhor modelo
<p align = "justify">Na guia <i>Model</i>/Modelo do melhor modelo treinado, selecione <i>Deploy</i>/Implantar e use a opção <i>Web service</i>/Serviço Web para implantar o modelo com as configurações descritas abaixo:

- <b><i>Name</i>/Nome</b>: ```predict-rentals```;
- <b><i>Description</i>/Descrição</b>: ```Predict cycle rentals```;
- <b><i>Compute type</i>/Tipo de computação</b>: ```Azure Container Instance/Instância de Contêiner do Azure```;
- <b><i>Enable authentication</i>/Habilitar autenticação</b>: Selecione esta opção, clique em <i>Deploy</i>/Implantar e aguarde.

<br><br>

## Testando a implantação
<p align = "justify">Após a finalização do deploy podemos seguir para os testes com o modelo:

1. Dentro do estúdio <i>Azure Machine Learning</i>, no menu esquerdo, selecione <i>Endpoints</i>/Pontos de extremidade e abra a tarefa implantada;
2. Na tela que será apresentada, selecione a guia <i>Test</i>/Testar;
3. No <i>input</i> dos dados, substitua o <i>template</i> fornecido por aquele presente na documentação (que pode ser consultado logo abaixo):
    ```
    {
    "Inputs": { 
        "data": [
        {
            "day": 1,
            "mnth": 1,   
            "year": 2022,
            "season": 2,
            "holiday": 0,
            "weekday": 1,
            "workingday": 1,
            "weathersit": 2, 
            "temp": 0.3, 
            "atemp": 0.3,
            "hum": 0.3,
            "windspeed": 0.3 
        }
        ]    
    },   
    "GlobalParameters": 1.0
    }
    ```
4. Clique no botão <i>Test</i>/Testar;
5. Valide os resultados do teste, que incluem um número previsto de aluguéis com base nos recursos de entrada, conforme visto a seguir: 
    ```
     {
        "Results": [
        444.27799000000000
        ]
    }
    ```
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab01_automatedML/assets/predictRentals.png" align = "center"/>

<br>

<p align = "justify">É possível simular outros valores, para isto, basta alterar os campos desejados no JSON do <i>input</i> apresentado na etapa 3 desta seção e testar novamente.
<p align = "justify">Após o final da criação do modelo, o Azure automaticamente gera o <i>script</i> e um <i>notebook</i> para execução que podem ser acessados no menu esquerdo através do caminho: <b>Notebooks > Users > live > job criado</b>. Caso deseje consultar os documentos criados automaticamente durante o meu teste, acesse os links a seguir:

  - <a href = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab01_automatedML/python/autoMLScript.py">Auto ML Generated Code: Script Python</a>
  - <a href = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab01_automatedML/python/autoMLScript_runNotebook.ipynb">Auto ML Generated Code: Run notebook</a>
