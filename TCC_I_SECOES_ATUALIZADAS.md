# TCC I — Seções atualizadas (conforme o sistema entregue)

> Documento de apoio: reúne as seções do TCC I reescritas para refletir o
> estado atual do sistema **Econom-IA**. Substitua no seu trabalho apenas os
> trechos abaixo. As seções **1 (Introdução)**, **2 (Revisão da Literatura)**,
> **4 (Cronograma)** e as **Referências** permanecem **sem alteração**.

---

## 3.5 Alertas integrados

A interface conta com um sistema de alertas visuais. Quando o modelo identifica
um sinal de compra ou venda em uma ação, o usuário visualiza um banner destacado
no Painel Avançado, contendo o nome do ativo, o tipo de sinal, o preço atual e a
estimativa de variação. A consolidação desses sinais também é apresentada na tela
inicial, em um resumo dos quatro ativos monitorados.

Uma seção dedicada ao **histórico de alertas**, que permitirá ao usuário
acompanhar todos os sinais emitidos nos últimos períodos e realizar a análise
retrospectiva do desempenho do modelo, encontra-se **em desenvolvimento, com
entrega prevista para o TC II**.

---

## 5 RESULTADOS PARCIAIS

Esta seção apresenta os resultados parciais obtidos durante a execução do TC I,
abrangendo a implementação da interface web, a estruturação do pipeline de dados,
o treinamento inicial do modelo de classificação Random Forest e a análise das
métricas de desempenho geradas. Os resultados aqui descritos validam o
funcionamento integrado de toda a cadeia técnica concebida e fornecem a base para
os ajustes e comparações previstos para a etapa subsequente do trabalho.

### 5.1 Implementação da interface

A interface web, denominada **Econom-IA**, encontra-se funcional no estado atual
do projeto. Adota tema escuro como padrão, decisão tomada com base na redução do
cansaço visual em sessões prolongadas de análise e no alinhamento estético com
plataformas consolidadas do setor financeiro, como Binance, TradingView e o
terminal Bloomberg. Nesta etapa (TC I), o visual foi mantido propositalmente
**simples e limpo**, reservando o refinamento estético — identidade visual,
logotipo e aprimoramento do front-end — para o TC II.

A navegação é organizada em uma barra lateral fixa com três seções: **Início,
Análise e Ferramentas**. A seção Início concentra a tela de boas-vindas, que
combina uma saudação contextualizada (variando conforme o turno do dia e o humor
do mercado) e quatro cards com indicadores macroeconômicos **atualizados em tempo
real**: o IBOVESPA e o dólar (USD/BRL), obtidos por meio da biblioteca yfinance
(Yahoo Finance), e a Taxa Selic e o IPCA acumulado em doze meses, obtidos pela API
pública do Banco Central do Brasil. A tela apresenta ainda um resumo consolidado
dos sinais gerados pelo modelo para os quatro ativos monitorados. Uma área
destinada às manchetes do mercado financeiro está **prevista para o TC II**.

Para atender ao público-alvo heterogêneo do sistema, foram desenvolvidas duas
telas distintas de análise: o **Painel Iniciante**, voltado a usuários com pouca
familiaridade no domínio, e o **Painel Avançado**, destinado a investidores
experientes. O Painel Iniciante apresenta a análise de cada ação com linguagem
cotidiana, perguntas diretas e respostas em texto narrativo, exibindo apenas três
cartões principais (preço atual, variação do último mês e estimativa para os
próximos pregões). O Painel Avançado exibe todos os indicadores técnicos com seus
valores numéricos brutos, o gráfico interativo com os sinais sobrepostos e os
cenários estatísticos de predição. Em ambas as telas o usuário pode selecionar o
ativo e o período exibido no gráfico (último mês, seis meses, um ano ou todo o
histórico desde 2019), sendo os **últimos seis meses** o período exibido por
padrão. Essa abordagem segue o conceito de *progressive disclosure* no design de
interfaces, no qual a complexidade é revelada gradualmente conforme a necessidade
do usuário.

A seção Ferramentas reúne recursos complementares — o assistente conversacional
EconomIA, o painel Previsões & Macro e o Centro de Alertas —, cujas
funcionalidades estão **em desenvolvimento, com entrega prevista para o TC II**
(ver seções 3.5 e 5.5).

Para garantir desempenho fluido na navegação, o framework Streamlit utiliza
mecanismos nativos de cache: os dados são armazenados em memória após a primeira
leitura por meio do decorador `@st.cache_data`, e o modelo treinado é mantido
carregado durante toda a sessão com `@st.cache_resource`.

### 5.2 Pipeline de dados e estrutura de armazenamento

Foi realizada a coleta dos dados para os quatro ativos selecionados (ITUB4,
BBDC4, BBAS3 e SANB11), abrangendo o período compreendido entre janeiro de 2019 e
setembro de 2025, totalizando 6.968 registros diários de OHLCV (abertura, máxima,
mínima, fechamento e volume). A engenharia de atributos foi aplicada sobre esse
conjunto, produzindo as variáveis derivadas necessárias ao treinamento: RSI de 14
períodos, MACD com configuração padrão (12, 26, 9), médias móveis simples e
exponenciais de 9, 21 e 50 períodos, e Bandas de Bollinger de 20 períodos. Após o
warm-up dos indicadores e o tratamento de valores ausentes, o dataset final
consolidou-se em 6.732 registros válidos com a variável-alvo de classificação
atribuída.

A arquitetura de armazenamento adotada baseia-se em arquivos persistidos em
disco, sem necessidade de um sistema gerenciador de banco de dados relacional. Os
datasets são salvos em formato Parquet, que oferece compressão colunar nativa,
leitura rápida em pandas e portabilidade entre máquinas (McKinney, 2010). Após o
treinamento realizado nesta primeira etapa, o modelo Random Forest é serializado
por meio da biblioteca joblib, gerando o arquivo `random_forest.pkl`, enquanto as
métricas obtidas são salvas em arquivo JSON estruturado. A interface consome esses
arquivos diretamente, de modo que qualquer novo treinamento do modelo se reflete
automaticamente nas informações apresentadas, sem necessidade de alterações no
código.

> *Observação técnica:* na interface, os dados históricos exibidos são
> complementados em tempo de execução por cotações atualizadas obtidas via
> yfinance, mantendo os gráficos sempre próximos do pregão mais recente. O
> conjunto utilizado para o **treinamento** do modelo, contudo, é o descrito
> acima (janeiro de 2019 a setembro de 2025).

### 5.3 Treinamento do modelo e métricas obtidas

Conforme descrito na seção 3.3, os algoritmos previstos para o trabalho são
Random Forest, SVM e LSTM. Nesta primeira etapa, foi realizado o treinamento do
**Random Forest** como ponto de partida para validar a cadeia técnica completa. O
modelo foi configurado com 150 árvores, profundidade máxima de 10 níveis,
`min_samples_leaf` igual a 10 e balanceamento automático das classes para mitigar
o desbalanceamento natural do problema. A ação SANB11 foi reservada integralmente
como conjunto de validação out-of-sample, totalizando 1.683 registros nunca vistos
pelo modelo durante o aprendizado. Os 5.049 registros restantes, referentes às
ações ITUB4, BBDC4 e BBAS3, foram divididos em 70% para treino (3.534 registros) e
30% para teste (1.515 registros), respeitando rigorosamente a ordem cronológica.

Os sinais de compra, venda e neutralidade exibidos na interface são gerados por
esse modelo treinado, a partir dos indicadores técnicos calculados para cada
ativo, em conformidade com a metodologia descrita na seção 3.

A validação cruzada temporal com cinco folds (TimeSeriesSplit) resultou em
F1-Score macro médio de **0,3307**, com desvio-padrão de **0,0299** entre os
folds. No conjunto de teste, formado pelas mesmas ações do treinamento, mas em
período cronologicamente posterior, o modelo obteve F1-Score macro de **0,3238**,
Precision macro de **0,3263**, Recall macro de **0,3234** e acurácia de
**0,3254**. No conjunto out-of-sample referente à SANB11, o desempenho foi
ligeiramente superior: F1-Score macro de **0,3596**, Precision macro de
**0,3601**, Recall macro de **0,3600** e acurácia de **0,3678**.

| Métrica (macro) | Conjunto de teste | Out-of-sample (SANB11) |
|---|---|---|
| F1-Score | 0,3238 | 0,3596 |
| Precision | 0,3263 | 0,3601 |
| Recall | 0,3234 | 0,3600 |
| Acurácia | 0,3254 | 0,3678 |

A análise da importância das features indicou que os três indicadores técnicos
com maior contribuição nas decisões do modelo foram o z-score do histograma do
MACD (**15,99%**), a razão entre o preço de fechamento e a média móvel simples de
9 períodos (**13,35%**) e a razão entre o preço de fechamento e a média móvel
simples de 50 períodos (**12,87%**); o RSI de 14 períodos aparece logo em seguida
(12,14%). Em conjunto, os três principais indicadores respondem por
aproximadamente **42%** da capacidade discriminativa do modelo, validando
empiricamente a relevância da combinação entre indicadores de momento (MACD) e de
tendência (médias móveis), já estabelecida na literatura de análise técnica
(Achelis, 2013).

### 5.4 Análise dos resultados parciais

Os valores obtidos nesta primeira execução do treinamento encontram-se próximos
do desempenho esperado de um classificador aleatório, que seria de
aproximadamente 0,333 para um problema balanceado de três classes. Esse
resultado, embora modesto em termos absolutos, é considerado coerente e
satisfatório para esta fase do projeto, pois confirma o funcionamento integrado de
toda a cadeia técnica concebida: a coleta dos dados via API, o cálculo correto dos
indicadores técnicos, a criação adequada da variável-alvo, a divisão temporal
respeitada, o treinamento estável do modelo, a persistência dos artefatos e o
consumo automatizado das métricas pela interface.

Observou-se ainda que o desempenho no conjunto out-of-sample (SANB11) foi
ligeiramente superior ao desempenho no conjunto de teste com as mesmas ações do
treinamento, o que sugere ausência de overfitting significativo aos ativos
específicos utilizados durante o aprendizado. Esse comportamento corrobora a
estratégia de treinamento generalista descrita por Najem et al. (2024), segundo a
qual modelos treinados com dados de múltiplos ativos tendem a aprender padrões
mais universais dos indicadores técnicos.

Considerando o desempenho ainda próximo do aleatório, fica evidente a necessidade
de ajustes adicionais nas próximas etapas do projeto. As principais direções
identificadas para a continuidade do trabalho incluem o ajuste sistemático de
hiperparâmetros por meio de busca em grade ou validação bayesiana, a inclusão de
novas variáveis explicativas (volume normalizado, volatilidade e defasagens do
retorno), a comparação experimental com os modelos SVM e LSTM previstos no escopo
original, e a reavaliação do limiar de classificação atualmente fixado em ±1,5%.

### 5.5 Painel macroeconômico e integração com mercado de previsão

Foi planejada uma tela complementar denominada Previsões & Macro, voltada à
contextualização macroeconômica dos sinais gerados pelo modelo. Essa tela reunirá
cards informativos com as principais taxas e indicadores brasileiros relevantes
para o setor bancário (Taxa Selic, IPCA acumulado em doze meses, Taxa do Fed Funds
americano e cotação do dólar comercial); cards com os rendimentos dos títulos do
Tesouro americano (Treasury Yields) em quatro prazos distintos, acompanhados de um
gráfico interativo da curva de juros americana; um gráfico histórico comparativo
entre a Taxa Selic e o IPCA; e um painel dedicado ao mercado de previsão
Polymarket, exibindo as probabilidades agregadas para a próxima decisão do Comitê
de Política Monetária (Copom) sobre a Taxa Selic.

A incorporação do Polymarket representa uma das contribuições originais deste
trabalho. Mercados de previsão (*prediction markets*) são instrumentos em que
participantes apostam valores reais sobre o resultado de eventos futuros, e cujos
preços agregados refletem com notável precisão a expectativa coletiva dos agentes
(Wolfers; Zitzewitz, 2004).

Cabe destacar que esses indicadores macroeconômicos têm caráter exclusivamente
**contextual** e **não** são utilizados como entrada do modelo de classificação —
os sinais de compra e venda derivam unicamente dos indicadores técnicos
processados pelo Random Forest. Esta tela encontra-se **em desenvolvimento**: a
automatização da leitura dos indicadores por meio das APIs do Banco Central do
Brasil, do sistema FRED (Federal Reserve Economic Data) e do Polymarket está
**prevista para o TC II**.

---

## Resumo das mudanças em relação à versão anterior do texto

1. Nome do sistema padronizado como **Econom-IA**; interface com visual
   simplificado e tema escuro (sem ícones decorativos).
2. Barra lateral com **três seções** (Início, Análise, Ferramentas); a antiga
   seção *Sistema* (Performance, Dados & Modelos, Sobre) foi removida.
3. Os **sinais de compra/venda** passaram a ser gerados pelo **Random Forest**
   treinado (e não mais por regras heurísticas).
4. **Métricas atualizadas** (treinamento mais recente): F1 teste 0,3238 / OOS
   0,3596 / CV 0,3307; nova ordem de importância das features (MACD-z, SMA-9,
   SMA-50).
5. Cards macroeconômicos da tela Início **atualizados em tempo real** (Yahoo
   Finance + Banco Central).
6. **Notícias**, **Histórico de Alertas** e **Previsões & Macro** marcados como
   **em desenvolvimento (TC II)**.