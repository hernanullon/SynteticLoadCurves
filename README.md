# `<Modelando curvas de carga diárias de sistemas de distribução usando modelos generativos>`
# `<Modeling daily load profiles of distribution network using generative models>`

## Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação *IA376L - Deep Learning aplicado a Síntese de Sinais*, 
oferecida no primeiro semestre de 2022, na Unicamp, sob supervisão da Profa. Dra. Paula Dornhofer Paro Costa, do Departamento de Engenharia de Computação e Automação (DCA) da Faculdade de Engenharia Elétrica e de Computação (FEEC).


|Nome  | RA | Especialização|
|--|--|--|
| Rosero Karen  | 264373  | Eng. Telecomunicações e Telemática|
| Tenorio Luis  | 156449  | Eng. Eletricisita|
| Ullon Hernan  | 262729  | Eng. de Computação|


## Descrição Resumida do Projeto
As análises para operação e planejamento em sistemas de distribuição de energia são tipicamente realizadas a partir de curvas de consumo típicas que dependem da natureza das cargas de cada unidade consumidora (UC). A Figura 1 apresenta curvas típicas para quatro diferentes tipos de UCs. Vale ressaltar que esse consumo se refere ao total de energia que uma dada UC demanda da concessionária, valor que é de fato utilizado para a operação e planejamento.

<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/figuras/13198_2019_891_Fig5_HTML.png" align="middle" width="700">

Nos últimos anos tem surgido uma nova tendência de geração de energia que, diferente da tradicional geração centralizada, ocorre de forma distribuída através do uso de fontes de energia renováveis como, por exemplo, células fotovoltaicas. A esse tipo de geração dá-se o nome de Geração Distribuída (GD), geração esta que tem representado um grande desafio para a operação e planejamento de sistemas de distribuição de energia. Podendo estar conectadas diretamente aos consumidores, GDs possuem uma natureza intermitente e estocástica, como pode ser observado na Figura 2, que apresenta a potência de saída de um painel fotovoltaico ao longo de um dia ensolarado, de um dia nublado e de um dia chuvoso. 

<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/figuras/Solar-power-output-for-different-weather-conditions-a-sunny-day-20-April-2013-cloudy.png" align="middle" width="700">

Como parte da demanda de potência de uma UC que possui um painel fotovoltaico provém deste, e como a potência de saída do painel é influenciada pelas condições climáticas, a demanda de potência (consumo) desta UC é alterada de acordo com as condições climáticas, sendo impossível atribuir uma única curva de consumo típica a esta UC, inviabilizando as análises de operação e planejamento.

Para contornar esse problema e contabilizar essas incertezas, os principais métodos disponíveis na literatura usam uma abordagem baseada na geração de conjuntos de possíveis cenários de geração/consumo na forma de séries temporais. Esses cenários são, então, utilizados para levantar informações relevantes, como os possíveis estados de operação do sistema e as possíveis demandas de energia. Dessa forma, modelar com precisão as incertezas associadas às GDs é fundamental para possibilitar que os operadores de sistemas de distribuição sejam capazes de tomar decisões mais assertivas.

O objetivo deste projeto é aplicar Modelos Generativos Profundos para gerar diferentes cenários de séries temporais a partir de dados coletados nos sistema de distribuição do campus da UNICAMP em Barão Geraldo. Este sistema conta com a presença de diferentes tipos de cargas, painéis fotovoltaicos e uma estação de recarga para veículos elétricos.

	A saída do modelo será uma curva de consumo/geração. Essa curva consiste em um dado tabular com uma coluna e  96 linhas, sendo que numa primeira abordagem a ideia é transformar cada uma dessas linhas numa feature e obter a partir do modelo um valor para cada ponto dessa curva. 

> Incluir nessa seção link para vídeo de apresentação da proposta do projeto (máximo 5 minutos).

## Metodologia Proposta
> Para a primeira entrega, a metodologia proposta deve esclarecer:
> * Qual(is) base(s) de dado(s) o projeto pretende utilizar, justificando a(s) escolha(s) realizadas.
> * Quais abordagens de modelagem generativa o grupo já enxerga como interessantes de serem estudadas.
> * Artigos de referência já identificados e que serão estudados ou usados como parte do planejamento do projeto
> * Ferramentas a serem utilizadas (com base na visão atual do grupo sobre o projeto).
> * Resultados esperados
> * Proposta de avaliação

## Cronograma
> Proposta de cronograma. Procure estimar quantas semanas serão gastas para cada etapa do projeto.

## Referências Bibliográficas
> Apontar nesta seção as referências bibliográficas adotadas no projeto.
