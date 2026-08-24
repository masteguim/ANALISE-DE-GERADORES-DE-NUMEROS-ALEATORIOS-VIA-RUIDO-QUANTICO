# ANÁLISE COMPARATIVA DE GERADORES PSEUDORRANDÔMICOS E DE SEQUÊNCIAS OBTIDAS POR MEDIÇÃO QUÂNTICA: IMPLICAÇÕES PARA A SEGURANÇA DA INFORMAÇÃO 

Este repositório reúne os notebooks desenvolvidos para uma Iniciação Científica voltada à análise e comparação de diferentes mecanismos de geração de números aleatórios, incluindo geradores pseudorrandômicos, geradores criptograficamente seguros, simulação de circuitos quânticos e execução em hardware quântico real.

O objetivo é comparar o comportamento estatístico das sequências produzidas por essas diferentes fontes e avaliar o efeito de técnicas de condicionamento aplicadas às sequências obtidas em uma QPU da IBM.

## Fontes analisadas

Foram consideradas quatro fontes principais:

- PRNG didático baseado no Mersenne Twister;
- CSPRNG utilizando fonte segura do sistema operacional;
- simulador ideal de circuitos quânticos utilizando Qiskit Aer;
- QPU real disponibilizada pela IBM Quantum.

Para a comparação padronizada, cada fonte produziu:

- 30 lotes;
- 10.000 bits por lote;
- 300.000 bits por fonte.

## Notebooks

### `IC_PRNG_padronizado_estatistica.ipynb`

Contém os experimentos com geradores pseudorrandômicos.

O experimento padronizado utiliza o módulo `random` do Python, baseado no algoritmo Mersenne Twister, com semente fixa igual a 7. O gerador é inicializado uma única vez antes da produção dos 30 lotes.

O PRNG é utilizado apenas como referência determinística e didática, não sendo considerado adequado para aplicações criptográficas.

### `CSPRNG_padronizado_estatistica.ipynb`

Contém os experimentos com geradores pseudorrandômicos criptograficamente seguros.

A implementação principal utiliza:

```python
secrets.randbits(1)
```

Os valores são obtidos utilizando uma fonte segura disponibilizada pelo sistema operacional. Diferentemente do PRNG didático, nenhuma semente controlada pelo usuário é definida.

### `sim_QRNGs_padronizado_estatistica.ipynb`

Contém os experimentos realizados por meio da simulação de circuitos quânticos.

O circuito principal utiliza:

1. um qubit inicialmente preparado no estado `|0>`;
2. aplicação da porta Hadamard;
3. medição do qubit.

Idealmente:

```text
H|0> = (|0> + |1>) / sqrt(2)
```

e, portanto:

```text
P(0) = P(1) = 0,5
```

A execução é realizada utilizando o Qiskit Aer.

Foram utilizadas sementes de simulação entre 1001 e 1030 para permitir reprodutibilidade sem produzir lotes idênticos.

Embora o circuito reproduza matematicamente o comportamento esperado de um sistema quântico, a simulação ocorre por mecanismos computacionais clássicos e, portanto, não constitui uma fonte física de aleatoriedade quântica.

### `IC_QRNG_IBM_padronizado_estatistica.ipynb`

Contém os experimentos executados em hardware quântico real por meio da IBM Quantum.

O circuito principal utiliza:

- 1 qubit;
- porta Hadamard;
- medição.

Na coleta comparativa principal foram produzidos 30 lotes de 10.000 bits, totalizando 300.000 bits.

O notebook também registra informações relacionadas à rastreabilidade da execução, incluindo:

- backend utilizado;
- qubit físico;
- identificador do trabalho;
- parâmetros de compilação do circuito;
- versões das bibliotecas;
- informações de calibração disponíveis.

Na coleta padronizada principal, o backend utilizado foi o `ibm_marrakesh`.

Os experimentos anteriores presentes nos notebooks também incluem execuções exploratórias nas quais o processador quântico podia ser selecionado automaticamente de acordo com sua disponibilidade.

O experimento em QPU deve ser interpretado como uma demonstração utilizando um dispositivo quântico confiado, e não como um QRNG certificado ou independente de dispositivo.

## Métricas estatísticas

As sequências produzidas são avaliadas utilizando as mesmas métricas para os quatro grupos principais:

- proporção de bits 0 e 1;
- viés absoluto em relação a 0,5;
- entropia de Shannon;
- min-entropia empírica;
- autocorrelação com defasagem 1;
- número de corridas;
- Runs Test;
- intervalo de confiança de 95% para a proporção de bits pelo método de Wilson.

As métricas são calculadas individualmente para cada lote.

## Runs Test

O Runs Test é aplicado às sequências após sua geração, sendo um procedimento estatístico clássico e não uma operação executada na QPU.

A análise utiliza `statsmodels` e considera:

```text
H0: a ordem dos bits é compatível com o comportamento esperado em relação ao número de corridas.

H1: o número de corridas difere do comportamento esperado.
```

Foi adotado nível de significância:

```text
α = 0,05
```

A regra de decisão utilizada é:

```text
p >= 0,05 -> não rejeitar H0
p < 0,05  -> rejeitar H0
```

Para cada lote são armazenados:

- número de corridas;
- estatística z;
- valor-p;
- resultado da decisão estatística.

## Comparação estatística entre as fontes

Além da análise descritiva, os 30 lotes de PRNG, CSPRNG, simulador e QPU podem ser comparados por testes estatísticos inferenciais.

São utilizados:

### Kruskal-Wallis

Utilizado para verificar se existem diferenças globais entre as quatro fontes para métricas como:

- P(1);
- viés;
- entropia de Shannon;
- min-entropia empírica;
- autocorrelação com defasagem 1.

O teste é não paramétrico e não exige assumir distribuição normal das métricas analisadas.

### Tamanho de efeito

Para o teste de Kruskal-Wallis é calculado o epsilon-quadrado (`ε²`) como medida do tamanho de efeito.

### Mann-Whitney U

Quando o Kruskal-Wallis identifica diferença estatisticamente significativa, são realizadas comparações par a par entre as fontes utilizando o teste de Mann-Whitney U.

### Correção de Holm

Como são realizadas múltiplas comparações, os valores-p são corrigidos pelo método de Holm, reduzindo o risco de resultados significativos decorrentes apenas da realização de vários testes.

Também é calculada a correlação bisserial de postos como medida do tamanho de efeito nas comparações par a par.

## Condicionamento da saída da QPU

As sequências brutas produzidas pela QPU são preservadas antes de qualquer transformação e posteriormente submetidas a dois procedimentos de condicionamento.

### Método de von Neumann

Os bits são analisados em pares.

```text
00 -> descartado
11 -> descartado
01 -> utilizado para produzir um bit de saída
10 -> utilizado para produzir um bit de saída
```

O método pode reduzir determinados tipos de viés sob hipóteses adequadas, porém também reduz significativamente a quantidade de bits disponíveis.

### SHA-256

A sequência bruta é dividida em blocos completos de 1.024 bits.

Cada bloco é utilizado como entrada para SHA-256, produzindo uma saída de 256 bits.

Para cada lote original de 10.000 bits são utilizados nove blocos completos:

```text
9 x 1.024 bits = 9.216 bits processados
```

resultando em:

```text
9 x 256 bits = 2.304 bits de saída
```

Os bits restantes que não completam um bloco são desconsiderados.

Neste trabalho, SHA-256 é utilizado como mecanismo experimental de condicionamento. O processo não cria nova entropia e não constitui, isoladamente, prova de uniformidade ou segurança criptográfica da sequência resultante.

## Resultados principais

Na comparação padronizada foram obtidos os seguintes resultados globais:

| Fonte | Bits analisados | Bits 0 | Bits 1 | P(1) |
|---|---:|---:|---:|---:|
| PRNG | 300.000 | 150.107 | 149.893 | 0,49964 |
| CSPRNG | 300.000 | 149.528 | 150.472 | 0,50157 |
| Simulador | 300.000 | 149.820 | 150.180 | 0,50060 |
| QPU IBM | 300.000 | 149.102 | 150.898 | 0,50299 |

As quatro fontes apresentaram proporções próximas do equilíbrio entre 0 e 1.

Apesar de utilizarem mecanismos de geração distintos, as sequências apresentaram propriedades estatísticas semelhantes.

Esse resultado reforça que bom desempenho estatístico não implica, isoladamente:

- verdadeira aleatoriedade;
- origem quântica;
- imprevisibilidade;
- segurança criptográfica.

## Intervalos de confiança

Para a proporção global de bits 1, foram utilizados intervalos de confiança de 95% pelo método de Wilson.

| Fonte | P(1) | IC95% |
|---|---:|---:|
| PRNG | 0,49964 | [0,49785; 0,50143] |
| CSPRNG | 0,50157 | [0,49978; 0,50336] |
| Simulador | 0,50060 | [0,49881; 0,50239] |
| QPU IBM | 0,50299 | [0,50120; 0,50478] |

Esses intervalos utilizam uma aproximação binomial e assumem independência entre as observações. Caso exista dependência entre os bits, a incerteza real pode ser maior.

## Resultados do condicionamento da QPU

| Condição | Bits resultantes | P(1) | Taxa útil |
|---|---:|---:|---:|
| QPU bruta | 300.000 | 0,50299 | 100% |
| Von Neumann | 75.188 | 0,50039 | 25,06% |
| SHA-256 | 69.120 | 0,49860 | 23,04% |

Os métodos de condicionamento aproximaram a proporção global de bits do equilíbrio de 0,5, porém reduziram significativamente a quantidade de bits disponíveis.

As métricas calculadas após condicionamento devem ser interpretadas com cautela, pois as sequências resultantes possuem tamanhos menores do que os lotes brutos.

## Arquivos gerados

Os notebooks podem gerar arquivos contendo sequências, métricas e metadados experimentais.

Entre eles:

```text
prng_metricas_30x10000.csv
csprng_metricas_30x10000.csv
simulador_metricas_30x10000.csv
qpu_bruta_metricas_30x10000.csv
qpu_condicionamento_metricas.csv
qpu_metadados_coleta.csv
kruskal_wallis_fontes.csv
mannwhitney_holm_fontes.csv
```

Os valores-p individuais do Runs Test também são armazenados nos arquivos de métricas.

## Tecnologias utilizadas

O projeto utiliza principalmente:

- Python;
- Google Colab;
- Qiskit;
- Qiskit Aer;
- Qiskit IBM Runtime;
- NumPy;
- Pandas;
- SciPy;
- Statsmodels.

## IBM Quantum

Para executar os experimentos em hardware quântico real é necessário possuir acesso à plataforma IBM Quantum.

As credenciais utilizadas para acesso não devem ser incluídas diretamente nos notebooks publicados neste repositório.

Durante os experimentos, as credenciais foram armazenadas utilizando o sistema de Secrets do Google Colab.

## Reprodutibilidade

Para aumentar a rastreabilidade dos experimentos, são preservados:

- sequências brutas;
- métricas calculadas por lote;
- arquivos CSV de resultados;
- backend utilizado;
- qubit físico;
- identificador da execução;
- parâmetros de compilação do circuito;
- versões das bibliotecas;
- informações de calibração disponíveis.

O PRNG utiliza semente fixa, permitindo reprodução da sequência.

O simulador utiliza sementes controladas.

O CSPRNG e a QPU real não são destinados a produzir novamente as mesmas sequências em execuções futuras.

## Limitações

A QPU utilizada neste projeto não constitui um QRNG dedicado ou certificado.

A coleta padronizada principal foi realizada em uma única sessão e condição de calibração. Dessa forma, os 30 lotes devem ser interpretados como subamostras da mesma coleta e não como réplicas temporais independentes.

Consequentemente, os experimentos atuais não permitem avaliar de forma conclusiva a estabilidade temporal da QPU.

A min-entropia utilizada corresponde a uma estimativa empírica e não representa uma caracterização formal de uma fonte de entropia segundo a NIST SP 800-90B.

Os resultados estatísticos também não constituem certificação de segurança criptográfica.

## Finalidade dos experimentos

Os experimentos deste repositório possuem finalidade exclusivamente acadêmica e científica.

As sequências produzidas não foram utilizadas para:

- criação de chaves criptográficas reais;
- armazenamento de senhas;
- geração de credenciais;
- proteção de dados reais;
- sistemas criptográficos em ambientes de produção.

A aprovação em testes estatísticos não constitui, isoladamente, garantia de imprevisibilidade ou segurança criptográfica.

## Autor

**Lucas Diniz Ferreira Masteguim**

Universidade Presbiteriana Mackenzie  
Iniciação Científica — PIVIC Mackenzie

Orientador: **Antonio Newton Licciardi Junior**

## Título do projeto

**Análise de geradores de números aleatórios via ruído quântico e como contribuem para segurança da informação em computação**
