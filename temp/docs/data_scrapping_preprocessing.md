# Dados 

Para a realização do projeto são utilizadas dois conjuntos diferentes de dados - tweets dos perfis estudados e séries temporais econômicas brasileiras - que devem ser organizadas e preparadas para a posterior utilização na geração de visualização, de análises e de modelos. Nessa etapa, serão muito úteis duas APIs, a do Twitter e a do Banco Central do Brasil.  

## Coleta de dados

### Twitter

Os dados do Twitter consiste no histórico de tweets dos perfis do presidente Jair Bolsonaro (@jairbolsonaro), do deputado federal Eduardo Bolsonaro (@bolsonarosp), do senador Flávio Bolsonaro (@flaviobolsonaro) e do vereador Carlos Bolsonaro (@carlosbolsonaro). Além do texto presentes nos tweets, também poderá ser considerados para o projeto outras informações presentes, como a data de postagem, quantidade de retweets, existência de mídias no tweet, entre outras.

Para realizar o acesso, utilizaremos da API do Twitter que permite, entre outras tarefas, a obtenção de tweets através de buscas por palavras chaves e também a obtenção de tweets através do perfil de um usuário, no entanto, uma limitação é o número de tweets que podem ser coletados, sendo possível apenas coletar os últimos 3000 tweets de um determinado perfil. Para realizar as solicitações para a API usaremos a biblioteca [_Tweepy_](https://www.tweepy.org/), uma biblioteca Python que permite a conexão. Com a utilização do _Tweepy_, criamos um objeto chamado _Cursor_ que facilita a obtenção dos dados em poucas linhas.

<details>
<summary>Código</summary>


```python
#requesting data from twitter
twitter_data = {}
for user in ['jairbolsonaro', 'bolsonarosp', 'flaviobolsonaro', 'carlosbolsonaro']:
    twitter_data[user] = [tweet for tweet in tw.Cursor(api.user_timeline,id=user, tweet_mode ="extended").items()]
```


```python
#saving original data
with open("..//data//tweets//brute_scrapping,pkl", "wb+") as f:
    pkl.dump(twitter_data, f)
```


</details>

### Séries econômicas

Para os dados de econômicos desejamos obter séries históricas de indicadores importantes para o mercado brasileiro, que sejam tanto diários, mensais, trimestrais. Inicialmente iremos coletar diversos indicadores que não necessariamente possam apresentar relação com os tweets da família do presidente, mas irermos realizar a análise de relação em etapas posteriores do projeto. Para essa segunda coleta, também usaremos uma a API, dessa vez oferecida pelo BACEN (Banco Central do Brasil), nela, podemos acessar diferentes séries através apenas de um número identificador. O código é baseado em um vídeo do Youtube: __Código Quant - Finanças Quantitativas__, [COMO ACESSAR A BASE DE DADOS DO BANCO CENTRAL DO BRASIL COM PYTHON | Python para Investimentos #10](https://www.youtube.com/watch?v=7rFsu48oBn8). 

As séries temporais escolhidas para compor o nosso conjunto de dados estão listadas abaixo, tabmém está listado sua unidade e o instituto que publica o indicador:

- IPCA : Índice nacional de preços ao consumidor-amplo, Var. % mensal, (IBGE). Este índice mede a variação de preços de mercado para o consumidor final e é utilizado para medir a inflação.
- INPC: Índice nacional de preços ao consumidor, Var. % mensal, (IBGE). Este índice mede a variação de preços da cesta de consumo da população assalariada com mais baixo rendimento.
- IGP-M : Índice geral de preços do mercado, Var. % mensal, (FGV). Este índice mede a variação de preços através de outros índices, do IPCA, INCC, e IPC.
- Meta SELIC : Taxa básica de juros definida pelo Copom, % a.a., (Copom).
- CDI : Taxa de juros, % a.d., (Cetip). Taxa de juros para empréstimo entre bancos.
- PIB : Produto Interno Bruto mensal, R\$ (milhões), (BCB-Depec). 
- Dólar: Taxa de câmbio - Dólar americano (venda), u.m.c/US\$, (Sisbacen PTAX800).
- Dívida pública: Dívida líquida do Setor Público (%PIB) - Total, %, (BSB - DSTAT).
- Reserva Internacional: Reserva internacional total diária, US\$ (milhões), (BCB - DSTAT). 
- Índice de emprego formal, (Mtb).
- PNADC: Taxa de desocupação, %, (IBGE). 
- Índice de confiança do consumidor, (Fecomercio).

<details>
<summary>Código</summary>

```python
def query_bc(code):
    url = 'http://api.bcb.gov.br/dados/serie/bcdata.sgs.{}/dados?formato=json'.format(code)
    df = pd.read_json(url)
    df['data'] = pd.to_datetime(df['data'], dayfirst=True)
    df.set_index('data', inplace=True)
    return df
```


```python
ipca = query_bc(433) 
ipca.to_csv("..\\data\\economic_index\\ipca.csv", sep = ";")
time.sleep(5)

igpm = query_bc(189) 
igpm.to_csv("..\\data\\economic_index\\igpm.csv", sep = ";")
time.sleep(5)

inpc = query_bc(188)
inpc.to_csv("..\\data\\economic_index\\inpc.csv", sep = ";")
time.sleep(5)

selic_meta = query_bc(432)
selic_meta.to_csv("..\\data\\economic_index\\selic_meta.csv", sep = ";")
time.sleep(5)

international_reserve = query_bc(13621)
international_reserve.to_csv("..\\data\\economic_index\\international_reserve.csv", sep = ";")
time.sleep(5)

pnad = query_bc(24369)
pnad.to_csv("..\\data\\economic_index\\pnad.csv", sep = ";")
time.sleep(5)

cdi = query_bc(12)
cdi.to_csv("..\\data\\economic_index\\cdi.csv", sep = ";")
time.sleep(5)

gdp = query_bc(4385)
gdp.to_csv("..\\data\\economic_index\\gdp.csv", sep = ";")
time.sleep(5)

dollar = query_bc(1)
dollar.to_csv("..\\data\\economic_index\\dollar.csv", sep = ";")
time.sleep(5)

employment = query_bc(25239)
employment.to_csv("..\\data\\economic_index\\employment.csv", sep = ";")
time.sleep(5)

gov_debt = query_bc(4503)
gov_debt.to_csv("..\\data\\economic_index\\gov_debt.csv", sep = ";")
time.sleep(5)

consumer_confidence = query_bc(4393)
consumer_confidence.to_csv("..\\data\\economic_index\\consumer_confidence.csv", sep = ";")
time.sleep(5)
```

</details>

Depois de obtida todas as séries, vamos gerar um _dataframe_ final incluindo todas as séries temporais econômicas, existirão células vazias pois os indicadores econômicos apresentam frequências diferentes e também os indicadores começaram a ser calculados em momentos distintos na história.

## Pré-processamento dos dados

Em sequência da coleta de dados, iremos realizar o pré-processamento dos dados para apresenta-los em um resultado adequado para a análise exploratória e para o desenvolvimento de modelos. Entre todas as variáveis retornadas pela API do Twitter, iremos selecionar aquelas que podem ser utilizadas no desenvolvimento do projeto. As informações selecionadas para compor o conjunto de dados processado serão as seguintes (e os nomes presente no _dataframe_):

- Nome do ususário que postou o tweet (name).
- Data de criação do tweet (created_at).
- Texto completo do tweet (full_text).
- Hashtags presentes no tweet (hashtags).
- Menções a usuários presentes no tweet (user_mentions).
- Tipo de mídia presente no tweet, se há mídia (media_type).
- Se o tweet é um reply (status_reply).
- Número de retweets que o tweet recebeu (retweet_count).
- Núméro de likes que o tweet recebeu (favorite_count).


<details>
<summary>Código</summary>

```python
#create dataframe from tweets data
tweets_dict = {}
columns_list = ['created_at', 'full_text', 'hashtags', 'user_mentions', 'media_type', 
                'status_reply','name', 'retweet_count', 'favorite_count'] 
for var in columns_list:
    tweets_dict[var] = []

for user in ['jairbolsonaro', 'bolsonarosp', 'flaviobolsonaro', 'carlosbolsonaro']:
    for tweet in tweets_data[user]:
        for var in columns_list:
            if var == 'hashtags':
                aux = '/'.join([u['text'] for u in tweet._json['entities'][var]])
                if aux == "":
                    tweets_dict[var].append("None")
                else:
                    tweets_dict[var].append(aux)
            elif var == "user_mentions":
                aux = '/'.join([u['screen_name'] for u in tweet._json['entities'][var]])
                if aux== "":
                    tweets_dict[var].append("None")
                else:
                    tweets_dict[var].append(aux)
            elif var == "status_reply":
                tweets_dict[var].append(1 if tweet._json['in_reply_to_status_id'] != None else 0)
            elif var == "name":
                tweets_dict[var].append(user)
            elif var == "media_type":
                if 'media' in list(tweet._json['entities'].keys()):
                    tweets_dict[var].append(tweet._json['extended_entities']['media'][0]['type'])
                else:
                    tweets_dict[var].append(None)
            else:
                tweets_dict[var].append(tweet._json[var])
tweets_df = pd.DataFrame(tweets_dict)
```
</details>

Em seguida, iremos criar variáveis extras a partir da variável da data de postagem do tweet, a apresentação das data dessa maneira permite expressões mais diversas para os modelos. Serão as seguintes:

- Ano de postagem (year).
- Mês de postagem (month).
- Dia de postagem (day).
- Hora de postagem (hour).
- Minuto de postagem (minute).
- Dia da semana de postagem (weekday).

Essas variáveis auxiliarão para o processo de modelagem. Além delas, iremos criar variáveis indicadoras (isto é, assume o valor 0 ou 1), que serão:

- Se o tweet possui hashtags (has_hashtags).
- Se o tweet possui menções (has_mentions).
- Se o tweet possui alguma mídia (has_midia).

<details>
<summary>Código</summary>
```python
tweets_df['date'] = pd.to_datetime(tweets_df.created_at)
tweets_df['year'] = tweets_df.date.apply(lambda x : x.year)
tweets_df['month'] = tweets_df.date.apply(lambda x : x.month)
tweets_df['day'] = tweets_df.date.apply(lambda x : x.day)
tweets_df['hour'] = tweets_df.date.apply(lambda x : x.hour)
tweets_df['minute'] = tweets_df.date.apply(lambda x : x.minute)
tweets_df['weekday'] = tweets_df.date.apply(lambda x : x.weekday)
tweets_df['has_hashtags'] = tweets_df.hashtags.apply(lambda x : 1 if x != "None" else 0)
tweets_df['has_mentions'] = tweets_df.user_mentions.apply(lambda x : 1 if x != "None" else 0)
tweets_df['has_media'] = tweets_df.media_type.apply(lambda x : 1 if x != "None" else 0)
tweets_df.drop(columns = ['created_at'], inplace = True)
```

```python
tweets_df.to_csv("..\\dataa\\tweets\\preprocessed_tweets.csv", sep = "~")
```
</details>
<div class = "scrolldiv">
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
	
	.scrolldiv {
    width: 800px;
    overflow-x: auto;
    white-space: nowrap;
}
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>full_text</th>
      <th>hashtags</th>
      <th>user_mentions</th>
      <th>media_type</th>
      <th>status_reply</th>
      <th>name</th>
      <th>retweet_count</th>
      <th>favorite_count</th>
      <th>date</th>
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>hour</th>
      <th>minute</th>
      <th>weekday</th>
      <th>has_hashtags</th>
      <th>has_mentions</th>
      <th>has_media</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-Edifício Joelma/SP, 1974.\n\n-Sgt CASSANIGA s...</td>
      <td>None</td>
      <td>None</td>
      <td>video</td>
      <td>0</td>
      <td>jairbolsonaro</td>
      <td>3154</td>
      <td>16202</td>
      <td>2020-07-27 20:51:13+00:00</td>
      <td>2020</td>
      <td>7</td>
      <td>27</td>
      <td>20</td>
      <td>51</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>- Água para quem tem sede.\n- Liberdade para u...</td>
      <td>None</td>
      <td>None</td>
      <td>video</td>
      <td>0</td>
      <td>jairbolsonaro</td>
      <td>8101</td>
      <td>37357</td>
      <td>2020-07-27 11:10:36+00:00</td>
      <td>2020</td>
      <td>7</td>
      <td>27</td>
      <td>11</td>
      <td>10</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>@tarcisiogdf @MInfraestrutura 🤝🇧🇷, Ministro!</td>
      <td>None</td>
      <td>tarcisiogdf/MInfraestrutura</td>
      <td>NaN</td>
      <td>1</td>
      <td>jairbolsonaro</td>
      <td>1074</td>
      <td>16840</td>
      <td>2020-07-26 20:18:19+00:00</td>
      <td>2020</td>
      <td>7</td>
      <td>26</td>
      <td>20</td>
      <td>18</td>
      <td>6</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2- @MinEconomia @MinCidadania @onyxlorenzoni @...</td>
      <td>None</td>
      <td>MinEconomia/MinCidadania/onyxlorenzoni/MEC_Com...</td>
      <td>photo</td>
      <td>1</td>
      <td>jairbolsonaro</td>
      <td>1337</td>
      <td>6383</td>
      <td>2020-07-26 15:40:39+00:00</td>
      <td>2020</td>
      <td>7</td>
      <td>26</td>
      <td>15</td>
      <td>40</td>
      <td>6</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1- Acompanhe as redes sociais! @secomvc @fabio...</td>
      <td>None</td>
      <td>secomvc/fabiofaria5555/tarcisiogdf/MInfraestru...</td>
      <td>photo</td>
      <td>0</td>
      <td>jairbolsonaro</td>
      <td>3287</td>
      <td>14836</td>
      <td>2020-07-26 15:39:47+00:00</td>
      <td>2020</td>
      <td>7</td>
      <td>26</td>
      <td>15</td>
      <td>39</td>
      <td>6</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>


Acesse os notebooks completos da [coleta de dados](https://github.com/GiovaniValdrighi/Bolsonaro_Tweets/blob/master/notebooks/data_scrapping.ipynb) e do [pré-processamento](https://github.com/GiovaniValdrighi/Bolsonaro_Tweets/blob/master/notebooks/preprocessing.ipynb) no Github.



