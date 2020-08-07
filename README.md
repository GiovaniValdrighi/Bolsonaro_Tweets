# Bolsonaro Tweets
Projeto final da disciplina de fundamentos de ciência dos dados ministrada pelo professor Jorge Poco. Em uma era de alta conectividade e um crescimento da polarização política em diversos países da América, a grande quantidade de informação disseminada é capaz de transformar a sociedade, principalmente quando é apresentada por autoridades do país. Por esse motivo, o projeto tem como objetivo analisar o impacto socioeconômico dos tweets do presidente da República Jair Bolsonaro e seus familiares tem no Brasil, a análise é feita através de um estudo de diferentes indicadores econômicos e do acervo de tweets do presidente.

## Desenvolvimento
### Data scrapping
Os dados foram obtidos através da API distribuída pelo Twitter e com o auxílio da biblioteca Tweepy. De acordo com as limitações de coleta de dados, foram coletados os últimos 3200 tweets das contas _@jairbolsonaro_ (Jair Bolsonaro), _@bolsonarosp_ (Eduardo Bolsonaro), _@carlosbolsonaro_ (Carlos Bolsonaro) e _@flaviobolsonaro_ (Flávio Bolsonaro), entre os dados apresentados pela ferramenta, foram selecionados para compor o conjunto de dados: nome do usuário, data do tweet, texto do tweet, tipo de mídia do tweet, lista de hashtags, lista de menções a usuários, quantidade de retweets, quantidade de favoritos.

Os dados de indicadores econômicos foram obtidos através da API do Banco Central do Brasil que disponibiliza diversas séries temporais, as escolhidas para compor a base de dados do projeto são:
	* IPCA : Índice nacional de preços ao consumidor-amplo, Var. % mensal, (IBGE). Este índice mede a variação de preços de mercado para o consumidor final e é utilizado para medir a inflação.
	* INPC: Índice nacional de preços ao consumidor, Var. % mensal, (IBGE). Este índice mede a variação de preços da cesta de consumo da população assalariada com mais baixo rendimento.
	* IGP-M : Índice geral de preços do mercado, Var. % mensal, (FGV). Este índice mede a variação de preços através de outros índices, do IPCA, INCC, e IPC.
	* Meta SELIC : Taxa básica de juros definida pelo Copom, % a.a., (Copom).
	* CDI : Taxa de juros, % a.d., (Cetip). Taxa de juros para empréstimo entre bancos.
	* PIB : Produto Interno Bruto mensal, R$ (milhões), (BCB-Depec). 
	* Dólar: Taxa de câmbio - Dólar americano (venda), u.m.c/US$, (Sisbacen PTAX800).
	* Dívida pública: Dívida líquida do Setor Público (%PIB) - Total, %, (BSB - DSTAT).
	* Reserva Internacional: Reserva internacional total diária, US$ (milhões), (BCB - DSTAT). 
	* Índice de emprego formal, (Mtb).
	* PNADC: Taxa de desocupação, %, (IBGE). 
	* Índice de confiança do consumidor, (Fecomercio).
	