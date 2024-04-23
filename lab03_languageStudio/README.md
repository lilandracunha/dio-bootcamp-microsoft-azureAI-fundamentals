## Análise de sentimentos com Language Studio no Azure AI

<br>
<p align = "justify"><b>Descrição</b>: Neste LAB, exploraremos o uso do Azure Speech Studio e a análise linguística proporcionada pelo Language Studio. Durante a prática, teremos a oportunidade de aprofundar nosso entendimento sobre como aproveitar essas ferramentas avançadas da Microsoft Azure. Estaremos focados em aprimorar nossas habilidades na implementação de soluções de análise de fala e linguagem, abrindo portas para uma compreensão mais ampla e prática das capacidades oferecidas por essas tecnologias inovadoras.
<br><br>

> [!NOTE]
> As etapas descritas neste laboratório foram executadas a partir das orientações presentes nas documentações oficiais do Microsoft Azure: 
> <a href = "https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/06-text-analysis.html">Analyze text with Language Studio</a>
> <a href = "https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/09-speech.html">Explore Speech Studio</a>

<br><br>

## Criando o recurso no Azure AI
<p align = "justify">É possível criar um recurso específico para utilizar os serviços Language e Speech, porém ambos podem ser acessados através do Azure AI services (o mesmo criado no <a href = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/tree/main/lab02_visionStudio">laboratório anterior</a>). 

## Explorando o Speech Studio
<p align = "justify">Notei que mesmo a minha conta estando em inglês, o Speech Studio estava em português assim que o acessei, então as etapas a seguir encontram-se em ambos os idiomas. 

1. Acesse o <a href = "https://speech.microsoft.com/portal">Speech Studio</a> e faça login com a sua conta. Neste momento, teremos uma visualização semelhante a imagem a seguir: <br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab03_languageStudio/assets/getStartedWithSpeech.png" align = "center"/>

2. Note na imagem acima que temos a informação "<i>You don't have any recent projects yet</i>/Você ainda não tem nenhum projeto recente", sendo necessário selecionar o recurso que será utilizado (ressaltando que é possível optar pelo recurso Azure AI services). Para isto, basta selecionar as Configurações, marcar o recurso previamente criado e clicar em "<i>Use resource</i>/Usar o recurso": <br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab03_languageStudio/assets/useResource.png">

3. Feche as configurações e, na tela inicial do Speech Studio, busque por "<i>Speech to text</i>/Conversão de fala em texto", onde será selecionada a opção "<i>Real-time speech to text</i>/Conversão de fala em texto em tempo real": <br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab03_languageStudio/assets/speechToTextOption.png" align = "center"/>

4. Na tela do serviço será visualizado o campo para importação de um arquivo de áudio que será convertido em texto e a própria documentação do Speech Studio fornece um (arquivo) para teste (<a href = "https://aka.ms/mslearn-speech-files">speech.zip</a>). Baixe o arquivo da documentação e faça o upload no serviço:

> [!IMPORTANT]
> Antes de selecionar o arquivo, atente-se ao idioma do áudio, que deverá ser informado em "<i>Choose a language</i>/Escolher um idioma" 

<br>
    <img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab03_languageStudio/assets/realTimeSample.png" align = "center"/>
    <p align = "justify">Fiz mais um teste utilizando um áudio de um jogo. A frase escolhida foi "<a href = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab03_languageStudio/inputs/sennaTrueDamage.m4a"><i>It's the dream that makes us giants. Just gotta dream big enough</i></a>" e o resultado não foi totalmente preciso em relação ao "<i>gotta</i>". No primeiro teste, deixei o idioma como Inglês (Estados Unidos) e o retorno pode ser conferido na imagem <b>1</b>; no teste seguinte utilizei a mesma frase, porém alterei o idioma para Inglês (Reino Unido) e o retorno está presente na imagem <b>2</b>: </br> 
    <img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab03_languageStudio/assets/realTimePersonalTest1.png" align = "center"/> 
    <br>
    <img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab03_languageStudio/assets/realTimePersonalTest2.png" align = "center"/>

<br>

## Analisando sentimento com o Language Studio
<p align = "justify">Conforme citado no início deste guia, é possível utilizar o Language Studio através de um recurso Azure AI services, porém existem instâncias onde apenas o recurso Language Studio pode ser utilizado. Pensando nesta informação presente na documentação, iremos seguir para a criação do recurso específico:

1. Acesse o <a href = "https://portal.azure.com/">portal Azure</a> e faça o login através da conta cadastrada;
2. Ao acessar a página inicial do portal, selecione a opção "<i>+ Create a resource</i>", pesquise por <i>Language service</i> e, ao localizá-lo, clique em "<i>Create</i>":
    <br>
    <img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab03_languageStudio/assets/selectLanguageResource.png" align = "center"/>
    <br><br>
    A opção selecionada acima irá carregar a página de criação do recurso <i><b>Language service</b></i> e a primeira etapa consiste em selecionar as features adicionais que serão inclusas na criação. Podemos deixar apenas o default e clicar em "<i>Continue to create your resource</i>": <br>
    <img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab03_languageStudio/assets/resourceAdditionalFeatures.png" align = "center"/>

    Já as configuações do recurso serão preenchidas podem ser consultadas a seguir:
  - <b><i>Subscription</i></b>: Sua assinatura do Azure - Por padrão, este campo já estará preenchido conforme a assinatura vigente na conta;
  - <b><i>Resource group</i></b>: Um grupo é uma coleção de recursos que compartilham o mesmo ciclo de vida, permissões e políticas - Selecione um criado anteriormente ou crie um novo grupo de recursos;
  - <b><i>Region</i></b>: ```East US```;
  - <b><i>Name</i></b>: Insira um nome exclusivo;
  - <b><i>Pricing tier</i></b>: ```Free F0```;
  - <b><i>By checking this box I acknowledge that I have read and understood all the terms below</i></b>: Marque esta opção.

3. Selecione "Review + create" e depois "Create" e aguarde a conclusão do <i>deploy</i>; 

4. Uma vez finalizado o deploy, siga para o <a href = "https://language.cognitive.azure.com">Language Studio</a> e faça o login em sua conta. Será apresentada a tela de seleção de recurso, onde basta informar a sua assinatura (campo "<i>Subscription</i>") e o nome do recurso (<i>Name</i>): <br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab03_languageStudio/assets/selectAzureResource.png" align = "center"/>

5. Após configurar o recurso a ser utilizado, a página inicial será visualizada novamente e nela deve ser selecionada a aba "<i>Classify text</i>", onde iremos clicar em "<i>Analyze sentiment and mine opinions</i>": <br>
<img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab03_languageStudio/assets/analyzeSentimentOption.png" align = "center"/>

6. Para o exemplo disponibilizado pela documentação, iremos manter o idioma em inglês e apenas vamos copiar o texto fornecido no campo "<i>Enter your own text, upload a file, or use one of our sample texts</i>":
    - Input:
    ```
        Tired hotel with poor service
        The Royal Hotel, London, United Kingdom
        5/6/2018
        This is an old hotel (has been around since 1950's) and the room furnishings are average - becoming a bit old now and require changing. The internet didn't work and had to come to one of their office rooms to check in for my flight home. The website says it's close to the British Museum, but it's too far to walk.
    ```
    <p align = "justify">Mantenha a opção "<i>Enable opinion mining</i>" selecionada, marque a box "<i>I acknowledge that running this demo will incur usage and may incur costs to my Azure resource</i>" e clique em "<i>Run</i>": <br>
    <img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab03_languageStudio/assets/runSentimentAnalysis.png" align = "center"/> <br>
    
    - Output - Através do input, o serviço fará uma análise e trará palavras-chave que representam o sentimento apontado no que foi reportado: <br>
    <img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab03_languageStudio/assets/sentimentAnalysisSample.png" align = "center"/>
    <br><br>
    <p align = "justify">Após alguns testes, realizei uma validação com um <a href = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab03_languageStudio/inputs/resortFeedback.txt">feedback</a> criado com auxílio de IA generativa, para o qual tive o seguinte retorno: 
    <br>
    <img src = "https://github.com/lilandracunha/dio-bootcamp-microsoft-azureAI-fundamentals/blob/main/lab03_languageStudio/assets/sentimentAnalysisResortFeedback.png" align = "center"/>
    <br>
    <details>
        <summary>Retorno JSON</summary> 
        ```
        {
            "documents": [
                {
                    "id": "id__3178",
                    "sentiment": "mixed",
                    "confidenceScores": {
                        "positive": 0.41,
                        "neutral": 0.07,
                        "negative": 0.52
                    },
                    "sentences": [
                        {
                            "sentiment": "positive",
                            "confidenceScores": {
                                "positive": 1,
                                "neutral": 0,
                                "negative": 0
                            },
                            "offset": 0,
                            "length": 138,
                            "text": "Resort Maravilha do Mar Recentemente tive a oportunidade de ficar no seu resort à beira-mar e gostaria de compartilhar minha experiência. ",
                            "targets": [],
                            "assessments": []
                        },
                        {
                            "sentiment": "positive",
                            "confidenceScores": {
                                "positive": 0.88,
                                "neutral": 0.12,
                                "negative": 0
                            },
                            "offset": 138,
                            "length": 83,
                            "text": "Primeiro, deixe-me dizer que a vista do meu quarto era absolutamente deslumbrante. ",
                            "targets": [
                                {
                                    "sentiment": "positive",
                                    "confidenceScores": {
                                        "positive": 1,
                                        "negative": 0
                                    },
                                    "offset": 169,
                                    "length": 5,
                                    "text": "vista",
                                    "relations": [
                                        {
                                            "relationType": "assessment",
                                            "ref": "#/documents/0/sentences/1/assessments/0"
                                        }
                                    ]
                                }
                            ],
                            "assessments": [
                                {
                                    "sentiment": "positive",
                                    "confidenceScores": {
                                        "positive": 1,
                                        "negative": 0
                                    },
                                    "offset": 207,
                                    "length": 12,
                                    "text": "deslumbrante",
                                    "isNegated": false
                                }
                            ]
                        },
                        {
                            "sentiment": "positive",
                            "confidenceScores": {
                                "positive": 0.89,
                                "neutral": 0.05,
                                "negative": 0.06
                            },
                            "offset": 221,
                            "length": 93,
                            "text": "Acordar com o som das ondas e o sol nascente sobre o oceano foi verdadeiramente revigorante.\n",
                            "targets": [],
                            "assessments": []
                        },
                        {
                            "sentiment": "negative",
                            "confidenceScores": {
                                "positive": 0,
                                "neutral": 0.02,
                                "negative": 0.98
                            },
                            "offset": 314,
                            "length": 80,
                            "text": "No entanto, houve alguns aspectos que afetaram minha estadia de forma negativa. ",
                            "targets": [],
                            "assessments": []
                        },
                        {
                            "sentiment": "negative",
                            "confidenceScores": {
                                "positive": 0,
                                "neutral": 0,
                                "negative": 1
                            },
                            "offset": 394,
                            "length": 58,
                            "text": "Infelizmente, o controle de ruído deixou muito a desejar. ",
                            "targets": [
                                {
                                    "sentiment": "negative",
                                    "confidenceScores": {
                                        "positive": 0.01,
                                        "negative": 0.99
                                    },
                                    "offset": 411,
                                    "length": 8,
                                    "text": "controle",
                                    "relations": [
                                        {
                                            "relationType": "assessment",
                                            "ref": "#/documents/0/sentences/4/assessments/0"
                                        }
                                    ]
                                },
                                {
                                    "sentiment": "negative",
                                    "confidenceScores": {
                                        "positive": 0.01,
                                        "negative": 0.99
                                    },
                                    "offset": 423,
                                    "length": 5,
                                    "text": "ruído",
                                    "relations": [
                                        {
                                            "relationType": "assessment",
                                            "ref": "#/documents/0/sentences/4/assessments/0"
                                        }
                                    ]
                                }
                            ],
                            "assessments": [
                                {
                                    "sentiment": "negative",
                                    "confidenceScores": {
                                        "positive": 0.01,
                                        "negative": 0.99
                                    },
                                    "offset": 429,
                                    "length": 22,
                                    "text": "deixou muito a desejar",
                                    "isNegated": false
                                }
                            ]
                        },
                        {
                            "sentiment": "negative",
                            "confidenceScores": {
                                "positive": 0,
                                "neutral": 0,
                                "negative": 1
                            },
                            "offset": 452,
                            "length": 130,
                            "text": "Houve momentos em que o barulho de outros hóspedes perturbou minha tranquilidade, o que tornou minha estadia um pouco conturbada.\n",
                            "targets": [],
                            "assessments": []
                        },
                        {
                            "sentiment": "neutral",
                            "confidenceScores": {
                                "positive": 0.43,
                                "neutral": 0.57,
                                "negative": 0
                            },
                            "offset": 582,
                            "length": 86,
                            "text": "Além disso, gostaria de mencionar que o serviço de quarto poderia ser mais eficiente. ",
                            "targets": [],
                            "assessments": []
                        },
                        {
                            "sentiment": "negative",
                            "confidenceScores": {
                                "positive": 0,
                                "neutral": 0,
                                "negative": 1
                            },
                            "offset": 668,
                            "length": 94,
                            "text": "Houve alguns atrasos e pequenas confusões com os pedidos, o que impactou a experiência geral.\n",
                            "targets": [
                                {
                                    "sentiment": "negative",
                                    "confidenceScores": {
                                        "positive": 0,
                                        "negative": 1
                                    },
                                    "offset": 719,
                                    "length": 7,
                                    "text": "pedidos",
                                    "relations": [
                                        {
                                            "relationType": "assessment",
                                            "ref": "#/documents/0/sentences/7/assessments/0"
                                        },
                                        {
                                            "relationType": "assessment",
                                            "ref": "#/documents/0/sentences/7/assessments/1"
                                        }
                                    ]
                                }
                            ],
                            "assessments": [
                                {
                                    "sentiment": "negative",
                                    "confidenceScores": {
                                        "positive": 0.01,
                                        "negative": 0.99
                                    },
                                    "offset": 683,
                                    "length": 7,
                                    "text": "atrasos",
                                    "isNegated": false
                                },
                                {
                                    "sentiment": "negative",
                                    "confidenceScores": {
                                        "positive": 0,
                                        "negative": 1
                                    },
                                    "offset": 702,
                                    "length": 9,
                                    "text": "confusões",
                                    "isNegated": false
                                }
                            ]
                        },
                        {
                            "sentiment": "negative",
                            "confidenceScores": {
                                "positive": 0.38,
                                "neutral": 0,
                                "negative": 0.62
                            },
                            "offset": 762,
                            "length": 140,
                            "text": "No entanto, apesar desses contratempos, a beleza natural do local e a hospitalidade da equipe foram pontos positivos que não posso ignorar. ",
                            "targets": [
                                {
                                    "sentiment": "positive",
                                    "confidenceScores": {
                                        "positive": 1,
                                        "negative": 0
                                    },
                                    "offset": 835,
                                    "length": 13,
                                    "text": "hospitalidade",
                                    "relations": [
                                        {
                                            "relationType": "assessment",
                                            "ref": "#/documents/0/sentences/8/assessments/0"
                                        }
                                    ]
                                }
                            ],
                            "assessments": [
                                {
                                    "sentiment": "positive",
                                    "confidenceScores": {
                                        "positive": 1,
                                        "negative": 0
                                    },
                                    "offset": 872,
                                    "length": 9,
                                    "text": "positivos",
                                    "isNegated": false
                                }
                            ]
                        },
                        {
                            "sentiment": "positive",
                            "confidenceScores": {
                                "positive": 0.52,
                                "neutral": 0.48,
                                "negative": 0.01
                            },
                            "offset": 902,
                            "length": 136,
                            "text": "Com algumas melhorias, tenho certeza de que o seu resort poderia oferecer uma experiência verdadeiramente excepcional aos seus hóspedes.",
                            "targets": [],
                            "assessments": []
                        }
                    ],
                    "warnings": []
                }
            ],
            "errors": [],
            "modelVersion": "2022-11-01"
        }
        ```
    </details>
