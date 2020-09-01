# Bolsonaro Tweets

Projeto final da disciplina de Fundamentos de Ciência dos Dados do Mestrado em Modelagem Matemática da EMAp-FGV lecionada pelo professor Jorge Poco. Os integrantes do grupo são [Fredson Aguiar](https://github.com/FredsoNerd), [Giovani Valdrighi](https://github.com/GiovaniValdrighi) e [Tomás Ferranti](https://github.com/TomasFerranti).

O objetivo do projeto é analisar e verificar a relação entre os tweets do Presidente da República Jair Messias Bolsonaro e seus filhos, Eduardo Bolsonaro, Flávio Bolsonaro, Carlos Bolsonaro, e a variação observada em indíces econômicos, tais como a cotação do dólar, o índice IBOVESPA, o IPCA, entre outros. Os resultados obtidos, bem como uma descrição do desenvolvimento pode ser encontrados no seguinte [site](https://sites.google.com/view/bolsonaro-tweets/).

# Motivação

Em uma era de alta conectividade e um crescimento da polarização política em diversos países da América, a grande quantidade de informação disseminada é capaz de transformar a sociedade, principalmente quando é apresentada por autoridades do país. Por esse motivo, o projeto tem como objetivo analisar o impacto socioeconômico dos tweets do presidente da República Jair Bolsonaro e seus familiares tem no Brasil, a análise é feita através de um estudo de diferentes indicadores econômicos e do acervo de tweets do presidente.

# Repositório

- `notebooks/`: Notebooks Python contendo a descrição de cada uma das etapas do trabalho, os códigos para a realização, e as diferentes alternativas utilizadas para a obtenção do resultado final. Especificamente:
    
    - `data_scrapping`: Notebook contendo o processo de coleta dos dados tanto dos tweets quanto os índices econômicos. Para os tweets é apresentada a de coleta a partir de um conjunto disponível no Kraggle e a partir da API do Twitter com o auxílio da biblioteca [Tweepy](https://www.tweepy.org/). Para os índices econômicos é apresentada a coleta a partir da API do Banco Central do Brasil.
    
    - `preprocessing`: Notebook contendo o processo de pré-processamento dos dados utilizados, adequando as diferentes variáveis para os seguintes processos de análise exploratória de dados e desenvolvimento de modelos.
    
    - `eda` : Notebook contendo o processo de geração das visualizações utilizadas para a identificação de características e padrões presentes nos dados, tanto para a verificação de hipóteses quanto para o desenvolvimento de modelos que levam em consideração características importantes dos dados.
    
    - `premodelling`: Notebook contendo o processo de extração de variáveis dos textos do tweets, que são a análise de sentimentos e a modelagem de tópicos.
    
    - `effects_tweeter_economic_data`: Notebook contendo o processo de criação e validação de modelos, também contém a apresentação dos resultados obtidos e da conclusão obtida com o desenvolvimento do trabalho.
    
    - `logistic_classification`: Notebook contendo proposta de modelo alternativo; contém, ainda, apresentação do processo de modelagem, validação e discussões finais.   
    
    - `images/`: Pasta contendo as visualizações geradas na análise exploratória dos dados e também para a apresentação dos resultados de etapas individuais.

Ainda, durante a execussão do código, deverá ser criado o diretório `data` na raíz do diretório principal contendo o conjunto de dados coletados e manipulados.
    
# Reprodução

## Linguagem e pacotes

O projeto foi desenvolvimento com a linguagem Python, desenvolvido parcialmente com a distribuição [Anaconda](https://www.anaconda.com/) e com a distribuição do [Google Colab](http://colab.research.google.com/). Além das bibliotecas já presentes nas duas distribuições, foram utilizadas:

- [Tweepy](https://www.tweepy.org/): biblioteca que permite a conexão com a API do Twitter a partir de uma script Python, a biblioteca é utilizada no processo de coleta dos tweets.

- [WordCloud](https://github.com/amueller/word_cloud): biblioteca que permite a geração de nuvens de palavras, foi utilizada no processo de análise exploratória e na visualização dos resultados da análise de sentimentos.

- [gensim](https://radimrehurek.com/gensim/): biblioteca que contém métodos para a modelagem de tópicos.

- [textblob](https://textblob.readthedocs.io/en/dev/): biblioteca para o processamento de dados textuais, foi utilizada para a realização da análise de sentimentos.

- [googletrans](https://github.com/ssut/py-googletrans): biblioteca que permite a utilização do Google Tradutor a partir de um script Python, foi utilizada no processo de análise de sentimentos.

- [tqdm](https://github.com/tqdm/tqdm): biblioteca utilizada para a visualização de uma barra de progresso nas etapas de processamento.

## Dados

 - Dados do Twitter: Para a obtenção é necessário uma chave de acesso para a API do Twitter disponível para usuários cadastrados através do site [Twitter Developer](https://developer.twitter.com/en). Pelas restrições da API, só é possível a obtenção dos 3000 últimos tweets de um perfil, dessa forma, os dados disponíveis são diferentes quando o processo de coleta é feito em diferentes momentos (dependendo da publicação de novos tweets pelo usuário).
 
 - Dados econômicos: Os dados podem ser obtidos a qualquer momento atraés da API do Banco Central, no projeto foram utilizados os dados disponíveis até julho de 2020.
 
## Execução

### 1. Coleta de dados

Execute  `/notebooks/data_scrapping.ipynb`.

### 2. Pré-processamento

Execute `/notebooks/preprocessing.ipynb`. 

### 3. Análise exploratória

Execute `/notebooks/eda.ipynb`.

### 4. Análise de sentimentos e modelagem de tópicos

Execute `/notebooks/premodelling.ipynb`.

### 5. Construção de modelos

Execute `/notebooks/effects_tweeter_economic_data.ipynb`.

Execute `/notebooks/logistic_classification.ipynb`.
