## Análise de imagem 4.0 com o AI Vision Service

<br>
<p align = "justify">Descrição: Neste LAB, iremos praticar a criação de reconhecimento facial, identificação de dados em documentos e também o reconhecimento de elementos em imagens. Através desses exercícios, aprimoraremos nossas habilidades na aplicação prática de tecnologias de reconhecimento, proporcionando uma compreensão mais profunda e prática desses conceitos essenciais.
<br><br>

> [!NOTE]
> As etapas descritas neste laboratório foram executadas a partir das orientações presentes nas documentações oficiais do Microsoft Azure: 
> <a href = "https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/03-image-analysis.html">Analyze images in Vision Studio</a>
> <a href = "https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/04-face.html">Detect faces in Vision Studio</a>
> <a href = "https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/05-ocr.html">Read text in Vision Studio</a>

<br><br>

## Criando o recurso no Azure AI
1. Acesse o <a href = "https://portal.azure.com/">portal Azure</a> e faça o login através da conta cadastrada;
2. Ao acessar a página inicial do portal, selecione a opção "<i>+ Create a resource</i>", pesquise por <i>Azure AI services</i> e, ao localizá-lo, clique em "<i>Create</i>":
    <br>
    <img src = "createAzureAIServices" align = "center"/><br>
    A opção selecionada acima irá carregar a página de criação do recurso <i><b>Azure AI services</b></i>, cujas configurações que serão preenchidas podem ser visualizadas a seguir:
    <br>
    <img src = "createAzureAIServicesSettings" align = "center"/>

  - <b><i>Subscription</i></b>: Sua assinatura do Azure - Por padrão, este campo já estará preenchido conforme a assinatura vigente na conta;
  - <b><i>Resource group</i></b>: Um grupo é uma coleção de recursos que compartilham o mesmo ciclo de vida, permissões e políticas - Selecione um criado anteriormente ou crie um novo grupo de recursos;
  - <b><i>Region</i></b>: ```East US```;
  - <b><i>Name</i></b>: Insira um nome exclusivo;
  - <b><i>Pricing tier</i></b>: ```Standard S0```;
  - <b><i>By checking this box I acknowledge that I have read and understood all the terms below</i></b>: Marque esta opção.

3. Selecione "Review + create" e depois "Create" e aguarde a conclusão do <i>deploy</i>.

<br>

> [!IMPORTANT]
> Existem outras abas na criação do workspace, porém para o laboratório não iremos aplicar alterações para elas.

<br><br>

## Conectando o recurso Azure AI service ao Vision Studio
1. Após a criação do recurso, acesse o <a href = "https://portal.vision.cognitive.azure.com">Vision Studio</a> e faça login com a sua conta. Neste momento, teremos uma visualização semelhante a imagem a seguir, onde deve ser localizada a opção "<i>View all resources</i>: <br>
<img src = "getStartedWithVision" align = "center"/>

2. Na tela seguinte serão apresentados todos os recursos criados em sua assinatura e faz-se necessário selecionar o que deseja utilizar. Ao selecionar o recurso a ser utilizado, será habilitada a opção "<i>Select as default resource</i>" e não serão percebidas alterações na tela, porém basta fechar a seleção e seguir com as próximas etapas. <br>
<img src = "selectResource" align = "center"/> 

<p align = "justify">Após fechar a tela de seleção do recurso, podemos iniciar as tratativas do Vision Studio. 

## Gerando legendas para uma imagem
Na tela inicial do <a href = "https://portal.vision.cognitive.azure.com">Vision Studio</a>, busque a aba "<i><b>Image analysis</b></i>" e selecione a opção "<i>Add captions to images</i>": <br>
<img src = "selectAddCaptions" align = "center"/> 

2. Ao clicar na inclusão de legendas para imagens, marque a opção logo abaixo de "<i>Try it out</i>" concordando que entende a política de uso do recurso selecionado previamente;

3. O próprio Vision Studio fornece algumas opções de imagens para teste e ao clicar em cada uma delas serão trazidas informações de acordo com o que está pré-definido: <br>
<img src = "addCaptionsSample" align = "center"/>

<p align = "justify">Para testar mais um pouco, pesquisei em mecanismos de busca imagens sinalizadas com o filtro "Licenças Creative Commons", selecionei uma paisagem com poucos detalhes e adicionei ao serviço: <br>
<img src = "addCaptionsChina" align = "center"/>

> [!NOTE]
> O retorno JSON dos testes acima pode ser consultado na pasta outputs: <a href = "addCaptionsSample">Gerar legenda 1</a> | <a href = "addCaptionsChina">Gerar legenda 2</a> 

## Detectando rostos no Vision Studio
1. Na tela inicial do <a href = "https://portal.vision.cognitive.azure.com">Vision Studio</a>, busque a aba "<i><b>Face</b></i>" e selecione a opção "<i>Detect Faces in an image</i>": <br>
<img src = "selectDetectFaces" align = "center"/> 

2. Ao clicar na detecção de rostos, marque a opção logo abaixo de "<i>Try it out</i>" concordando que entende a política de uso do recurso selecionado previamente (caso esta não esteja selecionada);

3. O próprio Vision Studio fornece algumas opções de imagens para teste e ao clicar em cada uma delas serão trazidas informações de acordo com o que está pré-definido: <br>
<img src = "detectFacesSample" align = "center"/>

<p align = "justify">Para testar mais um pouco, pesquisei em mecanismos de busca imagens sinalizadas com o filtro "Licenças Creative Commons", selecionei uma que se enquadrou no parâmetro utilizado no modelo pré-definido e adicionei ao serviço. O retorno foi capaz de apontar detalhes do uso da máscara, porém vemos que o reconhecimento para o rosto 2 não foi completamente satisfatório: <br>
<img src = "detectFacesMask" align = "center"/>

> [!NOTE]
> O retorno JSON dos testes acima pode ser consultado na pasta outputs: <a href = "detectFacesSample">Detecção de Rosto 1</a> | <a href = "detectFacesMask">Detecção de Rosto 2</a> 

## Extraindo texto a partir de imagens
1. Na tela inicial do <a href = "https://portal.vision.cognitive.azure.com">Vision Studio</a>, busque a aba "<i><b>Optical character recognition</b></i>" e selecione a opção <i>Extract text from images</i>: <br>
<img src = "selectExtractText" align = "center"/>

2. Ao clicar na extração de texto, marque a opção logo abaixo de "<i>Try it out</i>" concordando que entende a política de uso do recurso selecionado previamente (caso esta não esteja selecionada);

3. O próprio Vision Studio fornece algumas opções de imagens para teste e ao clicar em cada uma delas serão trazidas informações de acordo com o que está pré-definido: <br>
<img src = "extractTextSample" align = "center"/>

<p align = "justify">Para novos testes, inclui uma imagem própria para validação do retorno:<br>
<img src = "extractTextCocaCola" align = "center"/>

> [!NOTE]
> O retorno JSON dos testes acima pode ser consultado na pasta outputs: <a href = "extractTextSample">Extrair texto 1</a> | <a href = "extractTextCocaCola">Extrair texto 2</a> 
