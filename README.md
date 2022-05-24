# `Geração de cenários em sistemas de distribuição usando modelos generativos`
# `Scenario generation in distribution systems using generative models`

## Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação *IA376L - Deep Learning aplicado a Síntese de Sinais*, 
oferecida no primeiro semestre de 2022, na Unicamp, sob supervisão da Profa. Dra. Paula Dornhofer Paro Costa, do Departamento de Engenharia de Computação e Automação (DCA) da Faculdade de Engenharia Elétrica e de Computação (FEEC).


|Nome  | RA | Especialização|
|--|--|--|
| Karen Rosero    | 264373  | Eng. Telecomunicações e Telemática |
| Luís H. Tenorio | 156449  | Eng. Eletricisita |
| Hernan Ullon    | 262729  | Eng. de Computação |

## Link do vídeo de apresentação da proposta do projeto

https://www.youtube.com/watch?v=CWisubeqaLg
https://docs.google.com/presentation/d/1ZwE6jB5jJDbURiT_O3IdAIyou7ioP8jT-ngvxeFNXTg/edit?usp=drivesdk

## Resumo (Abstract)



## Descrição do Problema/Motivação
As análises para operação e planejamento em sistemas de distribuição de energia elétrica são tipicamente realizadas a partir de curvas de carga típicas que dependem da natureza das cargas de cada unidade consumidora (UC). A Figura 1 apresenta curvas de carga típicas para quatro diferentes tipos de UCs. Vale ressaltar que essas curvas de carga se referem ao consumo total de energia que uma dada UC demanda da concessionária, valor que é de fato utilizado para a operação e planejamento.

<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/13198_2019_891_Fig5_HTML.png" align="middle" width="700">
	  <figcaption>
  	Figura 1: Curvas de carga típicas para consumidores (a) residenciais, (b) industriais (um turno de trabalho), (c) industriais (dois turnos de trabalho) e (d) 	comerciais (JAIN; MANI; SIDDIQUI, 2019).
  	</figcaption>
</p>


Nos últimos anos tem surgido uma nova tendência de geração de energia que, diferente da tradicional geração centralizada, ocorre de forma distribuída através do uso de fontes de energia renováveis como, por exemplo, células fotovoltaicas. Interconectadas à subestação, ao longo do sistema de distribuição ou diretamente ao consumidor, com tamanho limitado, tipicamente inferior a 10MW, essa geração de energia recebe o nome de Geração Distribuída (GD) (BARKER; DE MELLO, 2000). Este tipo de geração tem representado um grande desafio para a operação e planejamento de sistemas de distribuição de energia por apresentarem uma natureza intermitente e estocástica, como pode ser observado na Figura 2, que apresenta a potência de saída de um painel fotovoltaico (PV) para diferentes condições climáticas. 

<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/Solar-power-output-for-different-weather-conditions-a-sunny-day-20-April-2013-cloudy.png" align="middle" width="400">
	<figcaption>
  	Figura 2: Potência de saída de um PV para diferentes condições climáticas: um dia ensolarado (20 de abril de 2013), um dia nublado (15 de abril de 2013) e um dia chuvoso (13 de abril de 2013) (RANA; KOPRINSKA; AGELIDIS, 2016)
  	</figcaption>
</p>

Como parte da demanda de potência de uma UC que possui GDs provém destes sistemas, e como a potência de saída do PV é influenciada pelas condições climáticas, a demanda de potência desta UC é alterada de acordo com as condições climáticas, sendo impossível atribuir uma única curva de consumo típica a esta UC, inviabilizando as análises de operação e planejamento.

Para contornar esse problema e contabilizar essas incertezas, os principais métodos disponíveis na literatura usam uma abordagem baseada na geração de conjuntos de possíveis cenários de geração/consumo na forma de séries temporais. Esses cenários são, então, utilizados para levantar informações relevantes, como os possíveis estados de operação do sistema e as possíveis demandas de energia. Dessa forma, modelar com precisão as incertezas associadas às GDs é fundamental para possibilitar que os operadores de sistemas de distribuição sejam capazes de tomar decisões mais assertivas.

## Objetivos

O objetivo deste projeto é aplicar Modelos Generativos Profundos para gerar diferentes cenários de séries temporais a partir de dados coletados nos sistema de distribuição do campus da UNICAMP em Barão Geraldo. Este sistema conta com a presença de diferentes tipos de cargas, painéis fotovoltaicos e uma estação de recarga para veículos elétricos.

<!-- A saída do modelo será uma curva de consumo/geração. Essa curva consiste em um dado tabular com uma coluna e  96 linhas, sendo que numa primeira abordagem a ideia é transformar cada uma dessas linhas numa feature e obter a partir do modelo um valor para cada ponto dessa curva.  -->

## Metodologia Proposta

### Base de dados
Atualmente, existem 324 medidores inteligentes instalados no lado de baixa tensão dos transformadores de distribuição na Universidade Estadual de Campinas (UNICAMP) como parte do Projeto Campus Sutentável. O subprojeto intitulado Mini Centro de Operações tem o objetivo de implantar um minicentro inteligente de dados de consumo e operação de redes elétricas para o Campus Cidade Universitária Zeferino Vaz da Unicamp, através da instalação de medidores inteligentes em todas as unidades consumidoras (faculdades, institutos, laboratórios, núcleos interdisciplinares, administração, etc.) de forma a monitorar o consumo real e diário de cada unidade consumidora (CAMPUS SUSTENTÁVEL).

No total, a UNICAMP possui cinco alimentadores (BGE02, BGE03, BGE04, BGE05 e BGE06), sendo que o sistema fotovoltaico e a estação de recarga para ônibus elétrico estão conectadas ao alimentador BGE06. Além disso, o modelo elétrico deste alimentador é bem conhecido e confiável para realização de análises elétricas. Neste sentido, este projeto irá utilizar apenas os dados relacionados ao alimentador BGE06, que possui 40 dos 324 medidores inteligentes instalados no campus e cuja topologia é ilustrada na Figura 3.

<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/BaseDados.jpeg" align="middle" width="600">
	<figcaption>
  	Figura 3: Topologia do alimentador BGE06 da UNICAMP (Desenvolvimento próprio).
  	</figcaption>
</p>

Os medidores inteligentes instalados realizam medições a cada 30 segundos, coletando um total de 12 características elétricas (_features_). Contudo, algumas dessas _features_ são medidas em cada fase do sistema, produzindo assim um conjunto de dados com 27 _features_ para cada registro. Um resumo das _features_ coletadas pelos medidores é mostrado na Tabela 1. Dentre todas as _features_ disponíveis, apenas a potência ativa trifásica (threephaseActivePower) será analisada por estar diretamente relacionada ao consumo de cada unidade. Grandezas como potência reativa trifásica e fator de potência podem ser incluídos em análises futuras visando a obtenção de cenários mais completos.


<div align="center">
<table>
    <tr>
        <td>activePowerA</td> <td>currentA</td> <td>reactivePowerA</td> 
    </tr>
    <tr>
        <td>activePowerB</td> <td>currentB</td> <td>reactivePowerB</td> 
    </tr>
    <tr>
        <td>activePowerC</td> <td>currentC</td> <td>reactivePowerC</td> 
    </tr>
    <tr>
        <td>angleA</td> <td>frequencyA</td> <td>threephaseActivePower</td> 
    </tr>
    <tr>
        <td>angleB</td> <td>frequencyB</td> <td>threephaseApparentPower</td> 
    </tr>
    <tr>
        <td>angleC</td> <td>frequencyC</td> <td>threephaseReactivePower</td> 
    </tr>
    <tr>
        <td>apparentPowerA</td> <td>powerFactorA</td> <td>voltageA</td> 
    </tr>
    <tr>
        <td>apparentPowerB</td> <td>powerFactorB</td> <td>voltageB</td> 
    </tr>
    <tr>
        <td>apparentPowerC</td> <td>powerFactorC</td> <td>voltageC</td> 
    </tr>
</table>
</div>

No banco de dados, os medidores serão agrupados em classes definidas com base na análise das curvas de carga geradas: salas de aula, laboratórios, GMU, RU, Iluminação pública, estação de recarga do ônibus elétrico e PV .... (EDITAR COM DADOS DA WALQUIRIA). Na primeira parte do trabalho a rede será treinada com cada uma das classes individualmente. Sa segunda parte do trabalho, todas as classes de cargas não intermitentes, ou seja, exceto o PV, serão fornecidas ao modelo de forma misturada. Na terceira parte do trabalho, as curvas do PV serão adicionados juntamente ao restante das cargas. Por conta da intermitência da geração, a inclusão do PV no grupo de classes pode aumentar a dificuldade do modelo em aprender como gerar tais curvas.

Apesar de os medidores coletarem dados a cada 30 segundos, é usual que curvas de consumo típicas sejam conpostas por dados coletados a cada 15 minutos, resultando em um conjunto de 96 pontos. Para obter curvas com 96 a partir dos dados coletados pelos medidores, a média de todas as medições coletadas a cada 15 minutos é utilizada como referência. Essa é uma abordagem justa uma vez que, devido a natureza do consumo de energia, espera-se que as cargas não apresentem variações abruptas em seu perfil (REFERENCIA). Medidores que possuem dados incompletos serão desconsiderados na análise.

Outra consideração a ser feita sobre os dados utilizados é a diferenciação entre o perfil de consumo em dias úteis e o perfil de consumo em dias não úteis. Uma vez que nem todas as unidades da UNICAMP funcionam nos mesmos horários nos dias de semana e aos finais de semana, o perfil de consumo obtido para uma vez UC pode variar muito entre os dois cenários. Por exemplo, um prédio comercial que tem um perfil de consumo típico ao longo da semana e que não abre aos finais de semana pode eventualmente ser classificado como uma sala de aula, que também não funciona aos finais de semana. Para simplificar as análises e evitar que registros de classes diferentes sejam confundidos, apenas as medições coletadas em dias úteis são utilizadas por caracterizarem melhor o comportamento esperado das UCs.



### Abordagens usando modelos generativos

A utilização de modelos generativos permite utilizar grandes volumes de dados de treinamento não rotulados, permitindo gerar diretamente novos cenários baseados em dados históricos, sem especificar explicitamente um modelo ou as distribuições de probabilidade. Com o uso deste tipo de aprendizado não supervisionado, evita-se o processo de etiquetado manual dos dados. Três modelos generativos têm sido usados para gerar curvas de carga:

	
* Redes generativas baseadas em fluxo [1]
	
* Redes adversárias generativas (GANs) [2,3]
	
* Autoencoders Variacionais [4]

Os autores em [1] comparam as três abordagens, obtendo melhores resultados com a implementação de redes generativas baseadas em fluxo. Por tal motivo, este projeto vai estar focado na implementação do modelo generativo baseado em fluxo NICE (do inglês Non-linear independent component estimation).

O modelo NICE utiliza fluxos normalizados e funções reversíveis para mapear a distribuição de probabilidade de amostras reais em uma distribuição a priori. Como mostrado na Figura ?, uma série de funções reversíveis f(.) mapeiam as amostras reais x para um espaço latente z que mantém a dimensão dos dados de entrada. Quando o treinamento termina, as curvas de carga sintéticas são geradas pela função inversa f1(z).

<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/NICE.png" align="middle" width="600">
</p>

### Ferramentas utilizadas

A arquitetura do modelo base vai ser desenvolvida usando PyTorch. 

### Proposta de avaliação

Cinco indicadores podem ser usados para avaliar a semelhança entre as amostras geradas e as reais:
* Função de autocorrelação
* Coeficiente de correlação de Pearson
* Curva de duração da curva de consumo 
* A volatilidade dos perfis de carga diária
* Estudos de fluxo de carga e perdas no OpenDSS

<!-- ### Cronograma
A nossa proposta inclui o pré-processamento e análise da base de dados, assim como a adaptação do modelo NICE para o cenário estudado. A implementação da arquitetura estará sujeita a modificações nos parâmetros da rede: o número de camadas MLP, o número de neurônios em cada camada, e as épocas de treinamento da rede, posto que isso dependerá fortemente dos nossos dados. 

<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/cronograma.png" align="middle" width="900">
</p> -->

## Resultados e Discussão dos Resultados

Espera-se obter um conjunto de curvas de cargas que representem adequadamente o comportamento real das cargas, tanto daquelas que possuem GDs conectadas quanto aquelas que não possuem esse tipo de geração.

<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/results.png" align="middle" width="900">
</p>



## Conclusões

A proposta inclui o pré-processamento e análise da base de dados, assim como a adaptação do modelo NICE para o cenário estudado. A implementação da arquitetura estará sujeita a modificações nos parâmetros da rede: o número de camadas MLP, o número de neurônios em cada camada, e as épocas de treinamento da rede, posto que isso dependerá fortemente dos nossos dados. 

<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/cronograma.png" align="middle" width="900">
</p>

De acordo com o cronograma, até a data de entrega desta versão E2 esperava-se ter uma base de dados completamente tratada e pronta para a realização dos estudos com o modelo generativo NICE. Além disso, o cronograma inicial previa uma breve familiarização com o modelo generativo NICE a ser usado no trabalho. Ambos os objetivos foram alcançados, uma vez que tem-se uma base de dados devidamente tratada e pornta para ser usada nos estudos, além de ter sido iniciado os primeiros contatos com o modelo NICE.


## Referências Bibliográficas
[1] L. Ge, W. Liao, S. Wang, B. Bak-Jensen and J. R. Pillai, "Modeling Daily Load Profiles of Distribution Network for Scenario Generation Using Flow-Based Generative Network," in IEEE Access, vol. 8, pp. 77587-77597, 2020, doi: 10.1109/ACCESS.2020.2989350.

[2] Tajer, A., Perlaza, S., & Poor, H. (Eds.). (2021). Advanced Data Analytics for Power Systems. Cambridge: Cambridge University Press. doi:10.1017/9781108859806

[3] C. Jiang, Y. Mao, Y. Chai, M. Yu and S. Tao, "Scenario Generation for Wind Power Using Improved Generative Adversarial Networks," in IEEE Access, vol. 6, pp. 62193-62203, 2018, doi: 10.1109/ACCESS.2018.2875936.

[4] Pan, Zhixin, Jianming Wang, Wenlong Liao, Haiwen Chen, Dong Yuan, Weiping Zhu, Xin Fang, and Zhen Zhu. 2019. "Data-Driven EV Load Profiles Generation Using a Variational Auto-Encoder" Energies 12, no. 5: 849. https://doi.org/10.3390/en12050849 


[1] BARKER, P.P.; DE MELLO, R.W. Determining the impact of distributed generation on power systems. I. Radial distribution systems. In: 2000 Power Engineering
Society Summer Meeting (Cat. No. 00CH37134). [S.l.]: IEEE, 2000. p. 1645–1656.

[2] JAIN, Anjali; MANI, Ashish; SIDDIQUI, Anwar Shahzad. Network architecture for demand response implementation in smart grid. International Journal of System Assurance Engineering and Management, v. 10, n. 6, p. 1389-1402, 2019.

[3] RANA, Mashud; KOPRINSKA, Irena; AGELIDIS, Vassilios G. Solar power forecasting using weather type clustering and ensembles of neural networks. In: 2016 International Joint Conference on Neural Networks (IJCNN). IEEE, 2016. p. 4962-4969.

[4] "Mini Centro de Operações", Campus Sustentável, 2019, https://www.campus-sustentavel.unicamp.br/cos/ Acessado 23 Maio 2022.
