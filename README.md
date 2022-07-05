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
As análises para operação e planejamento em sistemas de distribuição de energia elétrica são tipicamente realizadas a partir de curvas de carga típicas que dependem da natureza das cargas de cada unidade consumidora (UC). A Figura 1 apresenta curvas de carga típicas para quatro diferentes tipos de UCs. Vale ressaltar que essas curvas de carga se referem demanda medida de cada UC [1], valor que é de fato utilizado para a faturamento, operação e planejamento.

<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/13198_2019_891_Fig5_HTML.png" align="middle" width="700">
	  <figcaption>
  	Figura 1: Curvas de carga típicas para consumidores (a) residenciais, (b) industriais (um turno de trabalho), (c) industriais (dois turnos de trabalho) e (d) 	comerciais [2].
  	</figcaption>
</p>


Nos últimos anos tem surgido uma nova tendência de geração de energia que, diferente da tradicional geração centralizada, ocorre de forma distribuída através do uso de fontes de energia renováveis como, por exemplo, células fotovoltaicas. Interconectadas à subestação, ao longo do sistema de distribuição ou diretamente ao consumidor, com tamanho limitado, tipicamente inferior a 10MW, essa geração de energia recebe o nome de Geração Distribuída (GD) [3]. Este tipo de geração tem representado um grande desafio para a operação e planejamento de sistemas de distribuição de energia por apresentarem uma natureza intermitente e estocástica, como pode ser observado na Figura 2, que apresenta a potência de saída de um painel fotovoltaico (PV) para diferentes condições climáticas. 

<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/Solar-power-output-for-different-weather-conditions-a-sunny-day-20-April-2013-cloudy.png" align="middle" width="400">
	<figcaption>
  	Figura 2: Potência de saída de um PV para diferentes condições climáticas: um dia ensolarado (20 de abril de 2013), um dia nublado (15 de abril de 2013) e um dia chuvoso (13 de abril de 2013) [4].
  	</figcaption>
</p>

Como parte da demanda de potência de uma UC que possui GDs provém destes sistemas, e como a potência de saída do PV é influenciada pelas condições climáticas, a demanda de potência desta UC é alterada de acordo com as condições climáticas, sendo impossível atribuir uma única curva de consumo típica a esta UC, inviabilizando as análises de operação e planejamento. Para contornar esse problema e contabilizar essas incertezas, os principais métodos disponíveis na literatura usam uma abordagem baseada na geração de conjuntos de possíveis cenários de geração/consumo na forma de séries temporais através da caracterização e da quantificação dos perfis de consumo de energia elétrica. Essas informações de consumo podem ser extraídas a partir das curvas de carga diárias de cada unidade consumidora, curvas estas que descrevem as trajetórias da demanda por energia elétrica ao longo do dia por valores de demanda coletados a cada intervalo de 15 minutos. A partir da análise das curvas de carga e possível identificar um conjunto de diversas informações sobre o desempenho energético destes consumidores, possibilitando a realização de diferentes estudos, como a geração de cenários.

## Objetivos

O objetivo deste projeto é aplicar Modelos Generativos Profundos para gerar diferentes cenários de séries temporais a partir de dados coletados nos sistema de distribuição do campus da UNICAMP em Barão Geraldo. Este sistema conta com a presença de diferentes tipos de cargas, painéis fotovoltaicos e uma estação de recarga para veículos elétricos.

<!-- A saída do modelo será uma curva de consumo/geração. Essa curva consiste em um dado tabular com uma coluna e  96 linhas, sendo que numa primeira abordagem a ideia é transformar cada uma dessas linhas numa feature e obter a partir do modelo um valor para cada ponto dessa curva.  -->

## Metodologia Proposta

### Base de dados
Atualmente, existem 324 medidores inteligentes instalados no lado de baixa tensão dos transformadores de distribuição na Universidade Estadual de Campinas (UNICAMP) como parte do Projeto Campus Sutentável. O subprojeto intitulado Mini Centro de Operações tem o objetivo de implantar um minicentro inteligente de dados de consumo e operação de redes elétricas para o Campus Cidade Universitária Zeferino Vaz da Unicamp, através da instalação de medidores inteligentes em todas as unidades consumidoras (faculdades, institutos, laboratórios, núcleos interdisciplinares, administração, etc.) de forma a monitorar o consumo real e diário de cada unidade consumidora [5].

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
	<caption>
  	Tabela 1: Características elétricas coletadas pelos medidores inteligentes.
  	</caption>
	
</table>
</div>

Apesar de os medidores coletarem dados a cada 30 segundos, segundo a Resolução Normativa ANEEL Nº 414 DE 09/09/2010, demanda medida é a maior demanda de potência ativa, verificada por medição, integralizada em intervalos de 15 (quinze) minutos durante o período de faturamento [1]. Assim, para obter curvas com 96 pontos a partir dos dados coletados pelos medidores, a média de todas as medições coletadas a cada 15 minutos é utilizada como referência. Nesta etapa da análise, medidores que possuem dados incompletos serão desconsiderados.

No banco de dados utilizado no trabalho, os medidores são agrupados em classes definidas com base na análise das curvas de carga obtidas a partir dos dados coletados. Para tornar possível o agrupamento de curvas de carga de medidores associados a transformadores de diferentes potências nominais num mesmo conjunto, uma normalização a partir do valor máximo de potência ativa trifásica observada para cada dia foi utilizado. Dessa forma garante-se que todas as curvas excursionem num intervalo entre 0 e 1 e que os comportamentos semelhantes para cada classe de carga sejam evidenciados de forma uniforme. As clases obtidas são mostradas na Tabela 2.

<div align="center">
<table>
	<tr>
		<td>Classe 0</td> <td>Laboratório</td> <td>Classe 6</td> <td>CECOM</td>
	</tr>
   	 <tr>
		<td>Classe 1</td> <td>Administrativo</td> <td>Classe 7</td> <td>Poço</td>
	</tr>
   	 <tr>
		<td>Classe 2</td> <td>Ilu. Pública</td> <td>Classe 8</td> <td>Sala de aula</td>
	</tr>
   	 <tr>
		<td>Classe 3</td> <td>Restaurante</td> <td>Classe 9</td> <td>Eletroposto</td>
	</tr>
    	<tr>
		<td>Classe 4</td> <td>PV</td> <td>Classe 10</td> <td>Biblioteca</td>
	</tr>
    	<tr>
		<td>Classe 5</td> <td>Comercial</td>
	</tr>
	<caption>
  	Tabela 2: Classes representativas das cargas da UNICAMP.
  	</caption>	
</table>
</div>


Apesar de existirem um total de 10 classes representativas, nem todas elas possuem uma quantidade de dados suficiente para se treinar os modelos generativos e, portanto, apenas as classes que possuem as maiores quantidades de dados serão utilizadas: classes 0, 1, 2, 3, 4 e 5. As Figuras 3-7 apresentam as curvas medidas em preto, com a curva média em vermelho, a curva média mais três desvios-padrão em vermelho tracejado e a curva média menos um desvio-padrão em azul tracejado. As curvas medidas que ficam de fora deste intervalo de desvios-padrão são destacadas em verde e não são levadas em consideração nas análises.


<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/0.png" width="330" >
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/1.png" width="330" >
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/2.png" width="330" >
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/5.png" width="330" >
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/WhatsApp Image 2022-07-04 at 4.40.19 PM.jpeg" width="330" >	
	<figcaption>
  	Figura 3: Curvas de carga para cada classe avaliada (Desenvolvimento próprio).
  	</figcaption>
</p>


Na primeira parte do trabalho, a rede será treinada com cada uma das classes individualmente. Na segunda parte, todas as classes de cargas não intermitentes, ou seja, exceto o PV, serão fornecidas ao modelo de forma misturada. Por fim, na terceira parte, as curvas do PV serão adicionados juntamente ao restante das cargas. Por conta da intermitência da geração, a inclusão do PV no grupo de classes pode levar o modelo generativo a apresentar dificuldades em aprender a distribuição destas curvas.

Outra consideração a ser feita sobre os dados utilizados é a diferenciação entre o perfil de consumo em dias úteis e o perfil de consumo em dias não úteis. Uma vez que nem todas as unidades da UNICAMP funcionam nos mesmos horários nos dias de semana e aos finais de semana, o perfil de consumo obtido para uma dada UC pode variar muito entre os dois cenários. Por exemplo, um prédio comercial que tem um perfil de consumo típico ao longo da semana e que não abre aos finais de semana pode eventualmente ser classificado como uma sala de aula, que também não funciona aos finais de semana. Para simplificar as análises e evitar que registros de classes diferentes sejam confundidos, apenas as medições coletadas em dias úteis são utilizadas por caracterizarem melhor o comportamento esperado das UCs.

### Abordagens usando modelos generativos

A utilização de modelos generativos permite utilizar grandes volumes de dados de treinamento não rotulados, permitindo gerar diretamente novos cenários baseados em dados históricos, sem especificar explicitamente um modelo ou as distribuições de probabilidade. Com o uso deste tipo de aprendizado não supervisionado, evita-se o processo de etiquetamento manual dos dados. Três modelos generativos têm sido usados para gerar curvas de carga:

	
* Redes generativas baseadas em fluxo [6]
	
* Redes adversárias generativas (GANs) [7,8]
	
* Autoencoders Variacionais [9]

Os autores em [6] comparam as três abordagens, obtendo melhores resultados com a implementação de redes generativas baseadas em fluxo. Por tal motivo, este projeto adotou a implementação do modelo generativo baseado em fluxo NICE (do inglês _Non-linear Independent Component Estimation_).


### NICE: Estimativa não-linear de componentes independentes

<!-- Descrever a metodologia geral do NICE. Podemos tirar a figura do artigo que tínhamos colocado aqui e deixar só essa aqui. -->

O modelo NICE utiliza fluxos normalizados e funções reversíveis para mapear a distribuição de probabilidade de amostras reais em uma distribuição a priori. Como mostrado na Figura 3, uma série de funções reversíveis $f(.)$ mapeiam as amostras reais $x$ para um espaço latente $z$ que mantém a dimensão dos dados de entrada. Quando o treinamento termina, as curvas de carga sintéticas são geradas pela função inversa $f^-1(z).$ 

O modelo NICE implementado está formado por 4 camadas aditivas acopladas que mapeiam as curvas de carga diária em variáveis latentes que seguem uma distribuição Gaussiana. Após o treinamento, vetores de ruído Gaussiano são processados pelas funções inversas que são blocos do tipo Multi-Layer Perceptron (MLP). Cada MLP contem 5 camadas totalmente conectadas com 500 neurônios, e uma camada densa final com 96 neurônios que é a dimensão temporal das amostras a serem geradas. 

<!-- 
<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/NICE.png" align="middle" width="600">
</p> -->

<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/Flow.png" align="middle">
	<figcaption style="text-align:center">
  	Figura 3: Arquitetura do modelo NICE adotado neste trabalho (Desenvolvimento próprio).
  	</figcaption>
</p>

### Proposta de avaliação

Para avaliar a qualidade das curvas de carga geradas pelo modelo NICE em relação as curvas reais, métricas de avaliação tipicamente empregadas no contexto de geração de cenários em sistemas de distribuição são adotadas [6, 8, 9, 10]:

* **Correlação temporal:** Nesta abordagem, a função de autocorrelação, dada pela equação a seguir, é utilizada para avaliar a correlação temporal entre as curvas de carga geradas e reais. Na equação, $E $ é a esperança, $k $ é o deslocamento temporal e $\sigma ^{2}$ é a variância da variável $X_{t}$.

$$R(k) = \frac{E[(X_t - \mu)(X_{t+k} - \mu)]}{\sigma ^2}$$

* **Divergência KL (Kullback–Leibler):** A divergência KL é uma medida de como uma distribuição de probabilidade é diferente de outra (referência). Uma divergência KL de 0 indica que as duas distribuições são idênticas. A divergência KL é matematicamente definida como mostrado na equação abaixo, onde, $P$ e $Q$ são distribuições de probabilidade definidas no mesmo espaço $X$. Normalmente, $P$ representa os dados ou uma distribuição de probabilidade medida, equanto $Q$ representa uma teoria, um modelo, uma descrição ou uma aproximação de $P$. Uma divergência KL inferior a 0,3 é assumida para indicar que as distribuições de parâmetros dos perfis de carga gerados são muito semelhantes aos dos dados originais.

$$D_{KL}(P||Q) = \sum_{x \in X} P(x) log \left( \frac{P(X)}{Q(X)} \right)$$

* **Estudos de fluxo de carga e análise de perdas no OpenDSS:**
	No total, a UNICAMP possui cinco alimentadores (BGE02, BGE03, BGE04, BGE05 e BGE06), sendo que o maior sistema fotovoltaico e a estação de recarga para ônibus elétrico estão conectadas ao alimentador BGE06, cuja topologia é ilustrada na Figura 4. Na figura, cada ponto corresponde a uma barra (um poste) e o traçado representa os cabos, sendo que nem todas as barras possuem transformadores conectados e os cabos possuem diferentes bitolas não evidenciadas na figura. Além disso, o modelo elétrico deste alimentador é bem conhecido e confiável para realização de análises elétricas, como cálculo de fluxo de carga e análise de perdas. Neste sentido, este projeto irá utilizar apenas o alimentador BGE06 para verificação da qualidade das curvas geradas. Para essa análise, espera-se que ao substituir uma curva real por uma curva gerada para um tranformador de uma dada classe, as perdas elétricas e os níveis de tensão observados em cada barramento sejam mantidos aproximadamente constantes.

<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/BaseDados.jpeg" align="middle" width="600">
	<figcaption>
  	Figura 4: Topologia do alimentador BGE06 da UNICAMP (Desenvolvimento próprio).
  	</figcaption>
</p>




## Análise de Resultados e Discussão 

### Ferramentas utilizadas
A arquitetura do modelo foi desenvolvida utilizando TensorFlow 2.6.0, e está baseada no repositório disponível em: https://github.com/bojone/flow/blob/master/nice.py. Com o objetivo de acelerar o processo de treinamento da rede, foi utilizada uma unidade de processamento gráfico (GPU) NVIDIA Titan V. 

### Geração de curvas para cada classe de transformador

Neste experimento, um modelo NICE foi treinado para cada subconjunto de dados dos diferentes transformadores. O procedimento que será descrito se mantem para as 6 classes de curvas de carga. 

A rede recebe vetores pré-processados de curvas de carga unidimensionais com 96 amostras correspondentes a medidas coletadas a cada 15 minutos ao longo do dia, quer dizer, no domínio do tempo. Os vetores passam pelas 4 camadas de acoplamento aditivo que contêm 5 camadas densas com 500 neurônios, e a camada final que mapeia um vetor da mesma dimensão da entrada (96 amostras). Os hiperparâmeros selecionados para cada modelo são apresentados na Tabela 3. O conjunto de dados de cada transformador foi separado aletoriamente em amostras de treinamento e validação segundo a relação 8:2. Foram implementadas técnicas de regularização como Early Stopping com diferentes valores de paciência para cada modelo. Durante o treinamento, a perda no conjunto de validação é monitorada para salvar o melhor modelo, dado que se durante o número de épocas definido pela paciência, a perda de validação não melhorou, o treinamento é encerrado.

<div align="center">
<table>
<thead>
  <tr>
    <th class="tg-7btt">Medidor</th>
    <th class="tg-7btt">Amostras</th>
    <th class="tg-7btt">Batch</th>
    <th class="tg-7btt">Épocas</th>
    <th class="tg-7btt">Paciência</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td class="tg-c3ow">0</td>
    <td class="tg-c3ow">9737</td>
    <td class="tg-c3ow">128</td>
    <td class="tg-c3ow">65</td>
    <td class="tg-c3ow">10</td>
  </tr>
  <tr>
    <td class="tg-c3ow">1</td>
    <td class="tg-c3ow">9731</td>
    <td class="tg-c3ow">128</td>
    <td class="tg-c3ow">81</td>
    <td class="tg-c3ow">10</td>
  </tr>
  <tr>
    <td class="tg-c3ow">2</td>
    <td class="tg-c3ow">8646</td>
    <td class="tg-c3ow">128</td>
    <td class="tg-c3ow">95</td>
    <td class="tg-c3ow">10</td>
  </tr>
  <tr>
    <td class="tg-c3ow">3</td>
    <td class="tg-c3ow">1593</td>
    <td class="tg-c3ow">100</td>
    <td class="tg-c3ow">286</td>
    <td class="tg-c3ow">25</td>
  </tr>
  <tr>
    <td class="tg-c3ow">4</td>
    <td class="tg-c3ow">2883</td>
    <td class="tg-c3ow">128</td>
    <td class="tg-c3ow">230</td>
    <td class="tg-c3ow">10</td>
  </tr>
  <tr>
    <td class="tg-c3ow">5</td>
    <td class="tg-c3ow">929</td>
    <td class="tg-c3ow">64</td>
    <td class="tg-c3ow">327</td>
    <td class="tg-c3ow">35</td>
  </tr>
</tbody>
 	<caption>
  	Tabela 3: Hiper-parâmetros dos modelos de cada transformador.
  	</caption>
</table>
</div>

As curvas de aprendizado de cada modelo NICE são apresentadas na Figura 5, onde se mostra a evolução da perda de treinamento e de validação ao longo das épocas. Pode-se ver que os medidores 3, 4 e 5 foram mais difíceis de modelar, posto que um maior número de épocas de treinamento foram precisas para atingir baixa perda. Também é possível analisar que o uso de técnicas de regularização evitou sobre ajustar os modelos, já que as curvas de validação mostram um comportamento homogêneo nas últimas épocas.

<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/transf0.png" width="330" >
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/transf1.png" width="330" >
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/transf2.png" width="330" >
 	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/transf3.png" width="330" >
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/transf4.png" width="330" >
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/transf5.png" width="330" >
	<figcaption>
  	Figura 5: Erro de treinamento e validação para cada classe avaliada (Desenvolvimento próprio).
  	</figcaption>
</p>


O bloco de transformação inversa tem camadas com as mesmas características que o bloco codificador. Com o objetivo de amostrar novas curvas de carga da função de distribuição estimada, alimentamos o transformador invertido (ou decodificador) com amostras de uma distribuição Gaussiana aleatória e aplicamos o modelo aprendido. A Figura 7 apresenta exemplos de curvas sintéticas geradas pelo modelo NICE para cada uma das classes estudadas (0, 1, 2, 3, 4 e 5), cujas curvas reais são mostradas na Figura 6. É possível observar a partir de uma comparação entre as curvas da Figura 6 e da Figura 7 que as curvas sintéticas apresentam perfis visualmente semelhantes ao das curvas reais medidas, indicando que o modelo NICE adotado foi capaz de identificar corretamente as features mais importantes cada classe. Por exemplo, um consumo ascendente de energia ao longo do dia, seguido por uma queda suava a partir de por volta das 18h para a classe 0, os "degraus" de desligamento/ligamento dos sensores de iluminação pública observados na classe 2, o perfil de consumo em plateau das classes 1 e 5, sendo estes dois perfis corretamente marcados segundo os valores de consumo nas regiões fora do plateau, mais altos para a classe 1 e mais baixos para a classe 5. O perfil de geração de potência do PV também foi corretamente identificado.

<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/curvas_reais.jpeg" align="middle" width="700">
	<figcaption>
  	Figura 6: Curvas de carga reais para cada transformador. 
  	</figcaption>
</p>


<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/curvas_geradas.jpeg" align="middle" width="700">
	<figcaption>
  	Figura 7: Curvas de carga geradas para cada transformador. 
  	</figcaption>
</p>

<!--<p align="center">-->
<!--	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/sample_transf1_15k.png" width="330" >-->
<!--	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/sample_transf0_15k.png" width="330" >-->
<!--	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/sample_transf2_15k.png" width="330" >-->
<!-- 	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/transf3.png" width="400" > -->
<!--	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/sample_transf4_15k.png" width="330" >-->
<!--	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/sample_transf5_15k.png" width="330" >-->
<!-- 	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/sample_transf6_15k.png" width="330" > -->
<!--	<figcaption>-->
 <!-- 	Figura 6: Curvas sintéticas geradas para as classes 0, 1, 2, 4 e 5, respectivamente (Desenvolvimento próprio).-->
  <!--	</figcaption>-->
</p>

Para a classe 2, que corresponde a transformadores de iluminação pública, o modelo NICE produziu curvas com valores de potência negativos, o que é fisicamente incoerente pois durante o dia essas cargas estão desligadas e, portanto, possuem um consumo nulo. Uma possível justificativa para o modelo ter aprendido e reproduzido esse comportamento pode estar associado a dados ruidosos e/ou medidores conectados a transformadores cujos sensores de atuação para ligar e desligar os equipamentos de iluminação estejam com defeitos. Uma alternativa para atuar nesse problema seria uma etapa de pós processamento das curvas para assegurar que esses eventuais valores negativos fiquem em zero. 

### Geração de curvas de carga condicionais

Nesta seção do projeto o modelo NICE foi treinado utilizando as amostras de todos os transformadores, simulando um cenário no qual as curvas de carga não tinham sido etiquetadas. Um total de 33519 curvas foram separadas em conjuntos de treinamento e teste com a relação 7:3. O modelo foi treinado durante 48 épocas, com uma paciência de 10 épocas e batch de 128 amostras. A curva de apredizado é apresentada na Figura 8. 

<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/transf6.png" align="middle" width="350">
	<figcaption>
  	Figura 8: Curvas de treinamento e validação da perda do modelo condicional.
  	</figcaption>
</p>

O processo de geração se mantém igual do que na geração de curvas para cada transformador. Porém, considerando que o modelo estará gerando curvas de todos os tipos de transformadores, é preciso implementar um classificador das curvas geradas. O classificador contem dois blocos de camadas convolucionais unidimensionais com 60 e 30 filtros respectivamente. Foram usadas camadas de max pooling e dropout visando reduzir a dimensionalidade dos dados de entrada e evitar sobre ajustar o modelo. Duas camadas densas processam a saída dos blocos convolucionais e entregam a predição da classe de curva. O modelo foi treinado com curvas de carga reais etiquetadas, mas a predição do modelo pode ser aplicada nos dados sintéticos com o objetivo de filtrar as curvas de carga de um tipo determinado. Consideramos o modelo relativamente confiável posto que consegue classificar as curvas com 89% de acurácia. Este enfoque do projeto é especialmente útil para a geração de diferentes cenários de consumo, do qual as curvas reais não têm etiquetas.  O notebook deste classificador também está disponível no repositório.


### Avaliação das curvas pelo OpenDSS

A Tabela 4 apresenta as perdas ativas e reativas para o alimentador BGE06 considerando o caso base (nenhuma curva sintética é usada), e os casos em que uma curva de cada classe foi subtituída por uma curva sintética da mesma classe. É importante salientar que apenas uma classe com curvas sintéticas foi usada em cada caso para avaliar os impactos individuais. É também apresentado um caso em que uma curva de cada classe é substituída por curvas sintéticas, totalizando cinco curvas sintéticas. É possível observar a partir da Tabela 4 que as curvas sintéticas geradas mantém as perdas de potência ativa em valores próximos ao caso base, sendo que no pior caso observado (Classe 1), a diferença é de 0.00002 MW, uma difenreça de menos de 1%. É possível ainda notar que, apesar de as potências reativas terem sido mantidas constantes em todos os casos (apenas as potências ativas foram geradas pelo modelo) pequenas diferenças são observadas. Essas diferenças se devem possivelmente a questões associadas ao equilíbrio de potências do sistema frente aos novos perfis de consumo. Por fim, é possível verificar que as curvas geradas pelo modelo NICE a partir dos dados coletados pelo sistema de monitoramento do campus são suficientemente representativas do perfil de consumo das diferentes classes do campus do ponto de vista das perdas elétricas no sistema.

<div align="center">
<table>
    <tr>
        <td>Caso</td> <td>Perdas ativas [MW]</td> <td>Perdas reativas [MVAr]</td> 
    </tr>
    <tr>
        <td>Base    </td> <td>0.02139</td> <td>0.15400</td> 
    </tr>
    <tr>
        <td>Classe 0</td> <td>0.02140</td> <td>0.15402</td> 
    </tr>
    <tr>
        <td>Classe 1</td> <td>0.02141</td> <td>r0.15405</td> 
    </tr>
    <tr>
        <td>Classe 2</td> <td>0.02139</td> <td>t0.15400</td> 
    </tr>
    <tr>
        <td>Classe 4</td> <td>0.02138</td> <td>0.15399</td> 
    </tr>
    <tr>
        <td>Classe 5</td> <td>0.02138</td> <td>0.15399</td> 
    </tr>
    <tr>
        <td>Todas   </td> <td>0.02139</td> <td>0.15404</td> 
    </tr>
 	<caption>
  	Tabela 4: Perdas de potência ativa e reativa considerando diferentes cenários.
  	</caption>
	
</table>
</div>

## Conclusões

A proposta inclui o pré-processamento e análise da base de dados, assim como a adaptação do modelo NICE para o cenário estudado. A implementação da arquitetura estará sujeita a modificações nos parâmetros da rede: o número de camadas MLP, o número de neurônios em cada camada, e as épocas de treinamento da rede, posto que isso dependerá fortemente dos nossos dados. 

<p align="center">
	<img src="https://github.com/hernanullon/SynteticLoadCurves/blob/main/reports/figures/cronograma.png" align="middle" width="900">
</p>

De acordo com o cronograma, até a data de entrega desta versão E2 esperava-se ter uma base de dados completamente tratada e pronta para a realização dos estudos com o modelo generativo NICE. Além disso, o cronograma inicial previa uma breve familiarização com o modelo generativo NICE a ser usado no trabalho, sendo que ambos os objetivos foram alcançados.

Os medidores inteligentes instalados nos transformadores do alimentador BGE06 foram selecionados para serem analisados, uma vez que este alimentador possui tanto o PV quanto a estação de recarga de ônibus elétrico instalados, além de ser o alimentador que possui o modelo elétrico mais confiável para análises de fluxo de carga, um dos critérios de análise da qualidade dos resultados adotada neste trabalho.

Foram geradas curvas de carga com 96 pontos tomados como a média das medições de potência ativa trifásica a cada 15 minutos, sendo que as amostras (curvas de carga diárias) que apresentam dados faltantes foram desconsideradas, e os medidores/transformadores foram agrupados em classes segundo seu perfil de consumo. Por questões associadas a variações no perfil de consumo das UCs da universidade em dias úteis e dias não úteis, optou-se por trabalhar apenas com dias úteis uma vez qeu o consumo nesses dias representa o comportamento típico das UCs. Por fim, medidores com dados

Os primeiros testes com o modelo NICE se mostraram promissores, sendo que foi possível obter uma curva de carga sintética visulamente muito próxima da curva de consumo típica para um dado medidor com características comerciais.



## Referências Bibliográficas
[1] ANEEL. Resolução Normativa nº 414, de 09 de setembro de 2010. Agência Nacional de Energia Elétrica, Rio de Janeiro.

[2] JAIN, Anjali; MANI, Ashish; SIDDIQUI, Anwar Shahzad. Network architecture for demand response implementation in smart grid. International Journal of System Assurance Engineering and Management, v. 10, n. 6, p. 1389-1402, 2019.

[3] BARKER, P.P.; DE MELLO, R.W. Determining the impact of distributed generation on power systems. I. Radial distribution systems. In: 2000 Power Engineering
Society Summer Meeting (Cat. No. 00CH37134). [S.l.]: IEEE, 2000. p. 1645–1656.

[4] RANA, Mashud; KOPRINSKA, Irena; AGELIDIS, Vassilios G. Solar power forecasting using weather type clustering and ensembles of neural networks. In: 2016 International Joint Conference on Neural Networks (IJCNN). IEEE, 2016. p. 4962-4969.

[5] "Mini Centro de Operações", Campus Sustentável, 2019, https://www.campus-sustentavel.unicamp.br/cos/ Acessado 23 Maio 2022.

[6] L. Ge, W. Liao, S. Wang, B. Bak-Jensen and J. R. Pillai, "Modeling Daily Load Profiles of Distribution Network for Scenario Generation Using Flow-Based Generative Network," in IEEE Access, vol. 8, pp. 77587-77597, 2020, doi: 10.1109/ACCESS.2020.2989350.

[7] Tajer, A., Perlaza, S., & Poor, H. (Eds.). (2021). Advanced Data Analytics for Power Systems. Cambridge: Cambridge University Press. doi:10.1017/9781108859806

[8] C. Jiang, Y. Mao, Y. Chai, M. Yu and S. Tao, "Scenario Generation for Wind Power Using Improved Generative Adversarial Networks," in IEEE Access, vol. 6, pp. 62193-62203, 2018, doi: 10.1109/ACCESS.2018.2875936.

[9] Pan, Zhixin, Jianming Wang, Wenlong Liao, Haiwen Chen, Dong Yuan, Weiping Zhu, Xin Fang, and Zhen Zhu. 2019. "Data-Driven EV Load Profiles Generation Using a Variational Auto-Encoder" Energies 12, no. 5: 849. https://doi.org/10.3390/en12050849

[10] Wang, Z., & Hong, T. (2020). Generating realistic building electrical load profiles through the Generative Adversarial Network (GAN). Energy and Buildings, 224, 110299.
