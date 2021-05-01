# CASE ADMISSAO COGNITIVO - Predicao de Precos de Estadia no AirBnB

Autor Bruno Theozzo

O Case consiste na predicao do preco de estadia em hospedagens do AirBnB de acordo com um compilado de informações publicas realizado pela Inside AirBnB e disponibilizado no link abaixo <br>

http://insideairbnb.com/get-the-data.html <br>

Os dados foram consultados no dia 27-abr-2021 as 14h20 (BRT). O dataset consultado foi o *listings.csv* referente a cidade de *Rio de Janeiro* consolidado no periodo de *22-fev-2021*.

O codigo aqui disponibilizado representou um *esforco inicial* em direcao a construcao de uma solucao de predicao de precos. Esse primeiro esforco tem o objetivo de elucidar as variaveis disponiveis, os desafios para a tarefa de predicao e fornecer insumos para estrategias mais robustas de modelagem.

Por exemplo, sabe-se que precos de hospedagens sao muito sensiveis a fatores temporais/sazonais. Para capturar essa dinamica, seria necessario a consolidacao das informacao no tempo, envolvendo a gestao de diferentes datasets, consolidacao de suas informacao e construcao de modelos e estrategias de validacao especificas para essa dinamica. Isso implica em um aumento significativo da complexidade da solucao, nao estando alinhado a um objetivo de exploracao inicial. *Portanto, a tratativa da dincamica temporal dos precos nao sera contemplada neste trabalho*. Vale notar ainda que o periodo de fevereiro-21 esta associado a um dos piores meses da crise do coronavirus no Brasil e, portanto, deve apresentar distribuicao bastante distinta da massa de dados de periodos pre-pandemicos.

Para esse esforco inicial, foram exploradas todas as variaveis disponiveis no dataset. Todas as variaveis foram analisadas com relacao a seu formato, distribuicao e relacao direta com a funcao objetivo. A partir desta analise inicial, decisoes foram tomadas sobre mante-las ou nao no dataset de treino. Nos casos que foram mantidas, estrategias de encoding e input tambem foram definidas.

As variaveis associadas a campos aberto de texto foram desconsideradas nessa primeira analise, embora possam dar originem a features importantes para o modelo atraves de seu processamento. Por exemplo, analise de sentimento e identificacao da presenca de algumas palavras-chave poderiam resultar em informacoes uteis para o modelo. Novamente, se tratando de um esforco inicial, optou-se por nao empregar esses recursos nesse momento.

A variavel objetivo *price* apresenta uma distribuicao que se assemelha a *log-normal* com precos variando entre poucos dolares ate milhares de dolares. Essa discrepancia na variacao pode gerar discrepancia ou vies na minimizacao de residuos dos precos maiores, o que nao e interessante. Assim, como uma primeira estrategia de modelagem, opta-se por adotar como objetivo de predicao o logaritmo dos precos, que apresenta distribuicao proxima a normal, com valores variando dentro de uma mesma ordem de grandeza.

Escolheu-se como funcao de custo uma funcao de norma L1 que, embora possa ser mais custosa computacionalmente, e capaz de fornecer maior robustez a presenca de outliers. Como o modelo preditivo escolhido apresenta baixo custo computacional, a escolha dessa metrica pareceu ser mais apropriada. Contudo, em esforcos com modelos mais complexos sua adocao pode inviabilizar o treino do modelo.

Como se trata de um primeiro esforco preditivo, escolheceu-se um unico algoritmo de machine learning, Gradient Bossting Regressor, baseado em arvores. Essa classe de modelos e bastante conhecida pelo seu excelente custo benefico entre performance preditiva e custo computacional, alem de apresentar metodos embutidos de importancia de variaveis. Tudo isso a torna especialmente atrativa para esse primeiro esforco de modelagem.

Como foram geradas inumeras features a partir do dataset original, o trabalho de selecao dessas features e especialmente relevante. Para isso, foi integrado ao gradiente Boosting Regressor uma etapa de RFE (Random Feature Extractor) que seleciona apenas uma fracao das features originais para uso no modelo. 

Esse pipeline RFE + GBR foi tunado usando busca ranadomizada e validacao cruzada. Os parametros tunados foram a fracao de features a ser mantida no RFE  e a profundidade de arvore, numero de estimadores e fracao de amostra por folha no GBR.

Para selecao final, foi priorizado modelos com menor tendencia a overfitting. Dessa forma, separou-se os modelos com menores diferencas de performance media de validacao cruzada entre treino e teste e, destes, escolheu-se o com melhor performance media de validacao cruzada no teste.

Os resultados mostram que o modelo conseguiu entregar uma performance preditiva razoavel, com r2 medio superior 0.6 nos dados de teste de validacao cruzada. E importante notar que o periodo utilizado apresenta uma distribuicao atipica dada pelo cenario de pandemia e tambem que informacoes temporais nao foram utilizadas no modelo.

