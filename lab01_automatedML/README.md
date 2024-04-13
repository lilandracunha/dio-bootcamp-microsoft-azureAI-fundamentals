## Azure Machine Learning

<br>
<p align = "justify">Descrição: Neste LAB, vamos aprender a criar nossa conta no Azure e explorar as capacidades de Machine Learning da plataforma para desenvolver nossa primeira automação prática. Ao aplicar implementações e soluções escaláveis de aprendizado de máquina na nuvem da Microsoft, adquiriremos conhecimentos valiosos e experiência na construção de soluções eficientes.
<br><br>

> [!NOTE] As etapas descritas neste laboratório foram executadas a partir das orientações presentes na documentação oficial do Microsoft Azure: <a href = "https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html">Explore Automated Machine Learning in Azure Machine Learning</a>

## Criando o espaço de trabalho no Azure AI
Após criar uma conta no portal Azure, o primeiro passo para atuar com os serviços de machine learning disponibilizados é seguir para a configuração do espaço de trabalho:
1. Acesse o <a href = "https://portal.azure.com">portal Azure</a> e faça o login através da conta cadastrada;

2. Ao acessar a página inicial do portal, selecione a opção "+ Create a resource/+ Criar um recurso", pesquise por Azure Machine Learning e, ao localizá-lo, clique em "Create/Criar":
<img src = "azureHomeCreateResource"> <img src = "createAzureMLResource">

A opção selecionada acima irá carregar a página de criação do recurso Azure Machine Learning, cujas configurações podem ser visualizadas a seguir: 
<img src = "createAzureMLResourceSettings">

  - Subscription/Assinatura: Sua assinatura do Azure - Por padrão, este campo já estará preenchido conforme a assinatura vigente na conta;
    - Resource group/Grupo de recursos: Um grupo é uma coleção de recursos que compartilham o mesmo ciclo de vida, permissões e políticas - Selecione um criado anteriormente ou crie um novo grupo de recursos; 
  - Name/Nome: Insira um nome exclusivo;
  - Region/Região: Certifique-se de que a região selecionada tenha a série de máquinas virtuais necessária para os destinos do seu espaço de trabalho - Para os laboratórios será utilizada a região East US;
  - Storage account/Conta de armazenamento: Conta utilizada como armazenamento de dados padrão para o workspace; é possível criar um novo recurso de armazenamento ou selecionar um existente - Será definido um valor padrão para esse campo;
  - Key vault/Cofre de chaves: Um "cofre de chaves" é usado para armazenar segredos e outras informações confidenciais necessárias ao espaço de trabalho - Será definido um valor padrão para esse campo;
  - Application insights/Insights de aplicação: O espaço de trabalho utiliza o Azure Application Insights para armazenar informações de monitorização sobre os modelos implantados - Será definido um valor padrão para esse campo;
  - Container registry/Registro de contêiner: Um registro de contêiner é usado para registrar imagens do Docker usadas em treinamento e implantações; para minimizar custos, um novo recurso do registro é criado apenas depois de construir a sua primeira imagem. Alternativamente, você pode optar por criar o recurso agora ou selecionar um existente em sua assinatura - Iremos preencher este campo como None/Nenhum (será criado automaticamente na primeira vez que você implantar um modelo em um contêiner).

3. Selecione "Review + create/Revisar + criar" e depois "Create/Criar" e aguarde a conclusão do deploy.
<img src = "deployAzureMLResourceSettings">

> [!NOTE] Existem outras abas na criação do workspace, porém para o laboratório não iremos aplicar alterações para elas.

## Utilizando aprendizado de máquina automatizado para treinar um modelo
<p align = "justify">O aprendizado de máquina automatizado permite que sejam aplicados vários algoritmos e parâmetros para treinar modelos e identificar o que melhor se aplica aos seus objetivos. 
<p align = "justify">Para este laboratório será utilizado um <a href = "https://capitalbikeshare.com/system-data">dataset de detalhes históricos de aluguel de bicicletas</a> e o objetivo é treinar um modelo que prevê a quantidade de aluguéis esperada em um determinado dia com base em características sazonais e meteorológicas.

1. No <a href = "https://ml.azure.com/?azure-portal=true">estúdio Azure Machine Learning</a>, dentro do workspace criado, acesse a opção ML Automatizado;
<img src = "createNewAutomatedML">

2. Crie um novo serviço de ML automatizado ("+ New Automated ML job/+ Novo trabalho de ML automatizado") com as configurações a seguir, clicando em "Next/Avançar" conforme necessário para avançar:\
    Basic settings/Configurações básicas:
    - Job name/Nome do trabalho: mslearn-bike-automl;
    - New experiment name/Novo nome do experimento: mslearn-bike-rental;
    - Description/Descrição: Automated machine learning for bike rental prediction;
    - Tags/Marcas: Não é necessário preencher;

    Task type & data/Tipo de tarefa e dados:
    - Select task type/Selecionar tipo de tarefa: Regression/Regressão - Para prever valores numéricos contínuos;
    - Select data/Selecionar os dados: Clique em "+ Create/+ Criar" e crie um dataset com as configurações a seguir: 
        - Data type/Tipo de dados:
            - Name/Nome: bike-rentals;
            - Description/Descrição: Historic bike rental data;
            - Type/Tipo: Tabular;
        - Data source/Fonte de dados:
            Selecione a opção "From web files/De arquivos da Web";
            - Web URL/URL da Web:
                - Web URL: https://aka.ms/bike-rentals
                - Skip data validation/Ignorar validação de dados: Não ativar esta opção; como descrito, "se você optar por ignorar a validação, não validaremos seu caminho de dados ou tentaremos acessar seus dados da versão prévia e do esquema";\
            Será feita uma validação da URL informada para que o processo possa prosseguir
        - Settings/Configurações:
            - File format/Formato do arquivo: Delimited/Delimitado;
            - Delimiter/Delimitador: Comma/Vírgula;
            - Encoding/Codificação: UTF-8;
            - Column headers/Cabeçalhos de coluna: Only first file has headers/Somente o primeiro possui cabeçalho;
            - Skip rows/Ignorar linhas: None/Nenhuma;
            - Dataset contains multi-line data/Conjunto de dados com dados de várias linhas: Não marcar esta opção;\
        Finalizando estas configurações, teremos a seguinte visualização: 
        <img src = "createDatasetSettings">
        - Schema/Esquema:
            - Para esta opção iremos selecionar tudo, exceto "Path";
        - Review/Examinar: Revise as informações inseridas;
        Após revisar as informações, selecione "Create/Criar" e aguarde a criação do dataset. Uma vez criado o dataset, selecione-o e clique em Next/Avançar para seguir para a etapa de configuração das tarefas.\
    
    Task settings/Configurações de tarefas:
    - Task type/Tipo de tarefa: Regression/Regressão;
    - Dataset/Dados: bike-rentals;
    - Target column/Coluna de destino: Rentals (integer);
    - Additional configuration settings/Configurações adicionais:
        - Primary metric/Métrica primária: Normalized root mean squared error (raiz do erro quadrático médio normalizado);
        - Explain best model/Explicar o melhor modelo: Desmarque esta opção;
        - Use all supported models/Usar todos os modelos suportados: Desmarque esta opção. Para o laboratório iremos restringir o modelo para tentar apenas algoritmos específicos;
        - Allowed models/Modelos permitidos: Selecionar apenas os modelos RandomForest e LightGBM. Em outros cenários faz-se importante testar o máximo possível, porém cada modelo testado aumenta o tempo de execução do job;
        <img src = "automatedMLTaskSettings">
    - Limits/Limites (expanda esta seção para visualizar as opções):
        - Max trials/Máximo de avaliações: 3;
        - Max concurrent trials/Máximo de avaliações simultâneas: 3;
        - Max nodes/Máximo de nós: 3;
        - Metric score threshold/Limite de pontuação da métrica: 0.085;
        - Timeout/Tempo limite do experimento (minutos): 15;
        - Iteration timeout/Tempo limite de iteração (minutos): 15;
        - Enable early termination/Habilitar o encerramento antecipado: Selecione esta opção;
    - Validation and test/Validar e testar:
        - Validation type/Tipo de validação: Train-validation split/Divisão de validação de treinamento;
        - Percentage of validation data/Validação do percentual de dados: 10;
        - Test dataset/Tipos de teste: None/Nenhum;\

    Compute/Computação:
    - Select compute type/Selecione o tipo de computação: Serverless/Sem servidor;
    - Virtual machine type/Tipo de máquina virtual: CPU;
    - Virtual machine tier/Tipo de máquina virtual: Dedicated;
    - Virtual machine size/Tamanho da máquina virtual: Standard_DS3_V2;
    - Number of instances/Número de instâncias: 1.
3. Finalizando a etapa acima (Compute) basta clicar em Next/Avançar para revisar o trabalho:
<img src = "submitTrainingJob">

4. Após revisão, clicar em "Submit training job/Enviar trabalho de treinamento" e aguardar a conclusão do envio.

## Validando as métricas aplicadas
<p align = "justify">Após a conclusão do envio do trabalho é possível visualizar o resumo do que foi executado através da guia Overview/Visão geral. Dentro desta guia, selecione o melhor modelo retornado:
<img src = "bestModelSummary">

<p align = "justify">Selecionando o melhor modelo serão apresentadas informações sobre ele e, neste momento, iremos acessar a guia Metrics/Métricas.
<p align = "justify">Através da guia Metrics podemos visualizar, dentre outras informações importantes, dois gráficos denominados residuals e predicted_true:
<img src = "bestModelMetrics">

<p align = "justify">Observe os gráficos citados acima, pois eles mostram o desempenho do modelo: O gráfico residuals mostra os resíduos, as diferenças entre os valores previstos e os reais, enquanto que o gráfico predicted_true compara os valores previstos com os valores verdadeiros. 

## Implantando e testando o melhor modelo
<p align = "justify">Na guia Model/Modelo do melhor modelo treinado, selecione Deploy/Implantar e use a opção Web service/Serviço Web para implantar o modelo com as configurações descritas abaixo:

- Name/Nome: predict-rentals;
- Description/Descrição: Predict cycle rentals;
- Compute type/Tipo de computação: Azure Container Instance/Instância de Contêiner do Azure;
- Enable authentication/Habilitar autenticação: Selecione esta opção, clique em Deploy/Implantar e aguarde.

## Testando a implantação
<p align = "justify">Após a finalização do deploy podemos seguir para os testes com o modelo:

1. Dentro do estúdio Azure Machine Learning, no menu esquerdo, selecione Endpoints/Pontos de extremidade e abra a tarefa implantada;
2. Na tela que será apresentada, selecione a guia Test/Testar;
3. No input dos dados, substitua o template fornecido por aquele presente na documentação (que pode ser consultado logo abaixo):
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
4. Clique no botão Test/Testar;
5. Valide os resultados do teste, que incluem um número previsto de aluguéis com base nos recursos de entrada, conforme visto a seguir: 
    ```
     {
        "Results": [
        444.27799000000000
        ]
    }
    ```
<img src = "predictRentals">