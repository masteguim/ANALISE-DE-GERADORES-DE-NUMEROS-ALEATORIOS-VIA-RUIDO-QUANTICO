# ANÁLISE COMPARATIVA DE GERADORES PSEUDORRANDÔMICOS E DE SEQUÊNCIAS OBTIDAS POR MEDIÇÃO QUÂNTICA: IMPLICAÇÕES PARA A SEGURANÇA DA INFORMAÇÃO

Este repositório reúne os códigos, notebooks, dados experimentais, métricas e metadados utilizados em uma Iniciação Científica voltada à análise comparativa de diferentes mecanismos de geração de números aleatórios.

Foram analisados geradores pseudorrandômicos, um gerador criptograficamente seguro, um simulador de circuito quântico e uma execução em hardware quântico real da IBM.

O objetivo é comparar as propriedades estatísticas das sequências produzidas pelas diferentes fontes e avaliar o efeito de técnicas de condicionamento aplicadas aos dados obtidos na QPU.

## Protocolo experimental

Foram consideradas quatro fontes principais:

- PRNG didático baseado no Mersenne Twister;
- CSPRNG utilizando fonte segura do sistema operacional;
- simulador de# Análise comparativa de geradores pseudorrandômicos e de sequências obtidas por medição quântica

### Implicações para a segurança da informação

Projeto de Iniciação Científica desenvolvido na **Universidade Presbiteriana Mackenzie**, com foco na comparação entre diferentes mecanismos de geração de números aleatórios e na análise das propriedades estatísticas das sequências produzidas.

O estudo compara:

- geradores pseudorrandômicos;
- geradores criptograficamente seguros;
- simulação de circuitos quânticos;
- execução em hardware quântico real da IBM.

Também são analisadas técnicas de condicionamento aplicadas à saída da QPU.

---

## Objetivo

Comparar as propriedades estatísticas de sequências binárias produzidas por quatro fontes distintas e avaliar as diferenças entre mecanismos determinísticos, criptograficamente seguros, simulados e fisicamente quânticos.

A comparação padronizada utiliza:

| Parâmetro | Valor |
|---|---:|
| Fontes analisadas | 4 |
| Lotes por fonte | 30 |
| Bits por lote | 10.000 |
| Bits por fonte | 300.000 |
| Total da comparação principal | 1.200.000 bits |

---

## Fontes analisadas

### PRNG

Gerador pseudorrandômico didático baseado no **Mersenne Twister**, utilizando o módulo `random` do Python.

- semente: `7`;
- inicialização única antes dos 30 lotes;
- utilizado como referência determinística;
- não destinado ao uso criptográfico.

### CSPRNG

Gerador pseudorrandômico criptograficamente seguro utilizando:

```python
secrets.randbits(1)
```

A aleatoriedade é fornecida por uma fonte segura disponibilizada pelo sistema operacional.

Nenhuma semente controlada pelo usuário é utilizada.

### Simulador quântico

Circuito executado com **Qiskit Aer**, formado por:

1. preparação do qubit em `|0>`;
2. aplicação da porta Hadamard;
3. medição.

Idealmente:

```text
H|0> = (|0> + |1>) / sqrt(2)

P(0) = P(1) = 0,5
```

Foram utilizadas sementes de simulação entre `1001` e `1030`.

A simulação reproduz matematicamente o comportamento esperado do circuito, mas não constitui uma fonte física de aleatoriedade quântica.

### QPU IBM

A coleta padronizada foi executada em hardware quântico real da IBM.

| Informação | Valor |
|---|---|
| Backend | `ibm_marrakesh` |
| Qubit físico | `0` |
| Job ID | `da137vu3kjvs73868gp0` |
| PUBs | `30` |
| Shots por PUB | `10.000` |
| Total | `300.000 bits` |

O experimento deve ser interpretado como uma demonstração utilizando um dispositivo quântico confiado, e não como um QRNG certificado ou independente de dispositivo.

---

## Estrutura do repositório

```text
dados/
├── bruto/
│   ├── qpu_bruta_lote_01.txt
│   ├── qpu_bruta_lote_02.txt
│   ├── ...
│   └── qpu_bruta_lote_30.txt
│
├── metricas/
│   ├── prng_metricas_30x10000.csv
│   ├── csprng_metricas_30x10000.csv
│   ├── simulador_metricas_30x10000.csv
│   ├── qpu_bruta_metricas_30x10000.csv
│   └── qpu_condicionamento_metricas.csv
│
└── metadados/
    └── qpu_metadados_coleta.csv

notebooks/
├── IC_PRNG_padronizado_estatistica.ipynb
├── CSPRNG_padronizado_estatistica.ipynb
├── sim_QRNGs_padronizado_estatistica.ipynb
└── IC_QRNG_IBM_padronizado_estatistica.ipynb

checksums/
└── SHA256SUMS.txt
```

---

## Notebooks

### `IC_PRNG_padronizado_estatistica.ipynb`

Contém os experimentos realizados com o PRNG didático baseado no Mersenne Twister.

### `CSPRNG_padronizado_estatistica.ipynb`

Contém os experimentos realizados com o CSPRNG baseado no módulo `secrets`.

### `sim_QRNGs_padronizado_estatistica.ipynb`

Contém a simulação do circuito quântico utilizando Qiskit Aer.

### `IC_QRNG_IBM_padronizado_estatistica.ipynb`

Contém a coleta e a análise dos dados obtidos em hardware quântico real da IBM.

---

## Dados brutos da QPU

Os dados brutos utilizados no artigo estão disponíveis em:

```text
dados/bruto/
```

A pasta contém 30 arquivos:

```text
qpu_bruta_lote_01.txt
qpu_bruta_lote_02.txt
...
qpu_bruta_lote_30.txt
```

Cada arquivo contém **10.000 bits**, totalizando **300.000 bits**.

Os dados foram recuperados diretamente do Job original:

```text
da137vu3kjvs73868gp0
```

Não foi realizada uma nova execução da QPU para gerar esses arquivos.

As sequências correspondem à saída bruta anterior à aplicação de qualquer procedimento de condicionamento.

---

## Metadados experimentais

Os metadados da coleta estão disponíveis em:

```text
dados/metadados/qpu_metadados_coleta.csv
```

Entre as informações preservadas estão, quando disponíveis:

- backend;
- Job ID;
- qubit físico;
- quantidade de PUBs;
- quantidade de shots;
- horários em UTC;
- versões de Python, Qiskit e Qiskit IBM Runtime;
- nível de otimização;
- semente utilizada na preparação do circuito para o hardware;
- T1;
- T2;
- erro de leitura;
- data de calibração.

Campos que não estavam disponíveis nos registros preservados não foram preenchidos com valores estimados.

Credenciais, tokens, IBM Cloud CRN e identificadores privados de conta não são publicados.

---

## Métricas estatísticas

As sequências são avaliadas utilizando:

- proporção de bits 0 e 1;
- viés absoluto em relação a `0,5`;
- entropia de Shannon;
- min-entropia empírica;
- autocorrelação com defasagem 1;
- número de corridas;
- Runs Test;
- intervalo de confiança de 95% para a proporção de bits pelo método de Wilson.

As métricas são calculadas individualmente para cada lote.

---

## Runs Test

O Runs Test é aplicado após a geração das sequências e não corresponde a uma operação executada na QPU.

A análise utiliza a biblioteca `statsmodels`.

Hipóteses:

```text
H0: o número de corridas observado é compatível com o comportamento esperado para a sequência analisada.

H1: o número de corridas observado difere do comportamento esperado.
```

Nível de significância:

```text
α = 0,05
```

Regra de decisão:

```text
p >= 0,05 -> não rejeitar H0
p < 0,05  -> rejeitar H0
```

Para cada lote são armazenados, quando aplicável:

- número de corridas;
- estatística `z`;
- valor-p;
- decisão estatística.

---

## 📐 Comparação estatística entre as fontes

Além da análise descritiva, são utilizados procedimentos inferenciais para comparação dos lotes.

### Kruskal-Wallis

Utilizado para verificar diferenças globais entre as quatro fontes em métricas como:

- P(1);
- viés;
- entropia de Shannon;
- min-entropia;
- autocorrelação com defasagem 1.

### Tamanho de efeito

O epsilon-quadrado (`ε²`) é utilizado como medida de tamanho de efeito associada ao teste de Kruskal-Wallis.

### Mann-Whitney U

Quando o teste global identifica diferença estatisticamente significativa, podem ser realizadas comparações par a par utilizando o teste de Mann-Whitney U.

### Correção de Holm

Os valores-p das comparações múltiplas são ajustados pelo método de Holm.

Também pode ser utilizada a correlação bisserial de postos como medida do tamanho de efeito nas comparações par a par.

---

## Condicionamento da saída da QPU

A saída bruta é preservada antes da aplicação dos procedimentos de condicionamento.

Foram avaliados dois métodos.

### Método de von Neumann

```text
00 -> descartado
11 -> descartado
01 -> utilizado para produzir um bit
10 -> utilizado para produzir um bit
```

O procedimento pode reduzir determinados tipos de viés sob hipóteses adequadas, porém reduz a quantidade de bits disponível.

### SHA-256

A sequência bruta é dividida em blocos completos de **1.024 bits**.

Cada bloco produz **256 bits**.

Para cada lote original de 10.000 bits:

```text
9 x 1.024 = 9.216 bits processados
9 x 256   = 2.304 bits de saída
```

Os bits restantes que não completam um bloco são desconsiderados.

Neste projeto, SHA-256 é utilizado experimentalmente como componente de condicionamento.

Sua aplicação não cria nova entropia e não constitui, isoladamente, prova de uniformidade, extração formal ou segurança criptográfica.

---

## Resultados principais

| Fonte | Bits analisados | Bits 0 | Bits 1 | P(1) |
|---|---:|---:|---:|---:|
| PRNG | 300.000 | 150.107 | 149.893 | 0,49964 |
| CSPRNG | 300.000 | 149.528 | 150.472 | 0,50157 |
| Simulador | 300.000 | 149.820 | 150.180 | 0,50060 |
| QPU IBM | 300.000 | 149.102 | 150.898 | 0,50299 |

As quatro fontes apresentaram distribuições próximas do equilíbrio entre 0 e 1.

Mesmo utilizando mecanismos distintos, foram observadas propriedades estatísticas semelhantes.

Esse resultado reforça que bom desempenho estatístico não comprova, isoladamente:

- verdadeira aleatoriedade;
- origem quântica;
- imprevisibilidade;
- segurança criptográfica.

---

## Intervalos de confiança

Foram calculados intervalos de confiança de 95% para a proporção global de bits 1 pelo método de Wilson.

| Fonte | P(1) | IC95% |
|---|---:|---:|
| PRNG | 0,49964 | [0,49785; 0,50143] |
| CSPRNG | 0,50157 | [0,49978; 0,50336] |
| Simulador | 0,50060 | [0,49881; 0,50239] |
| QPU IBM | 0,50299 | [0,50120; 0,50478] |

Os intervalos utilizam aproximação binomial e assumem independência entre as observações.

Caso exista dependência entre os bits, a incerteza real pode ser maior.

---

## Resultados do condicionamento

| Condição | Bits resultantes | P(1) | Taxa útil |
|---|---:|---:|---:|
| QPU bruta | 300.000 | 0,50299 | 100% |
| Von Neumann | 75.188 | 0,50039 | 25,06% |
| SHA-256 | 69.120 | 0,49860 | 23,04% |

Os métodos de condicionamento aproximaram a proporção global de bits de `0,5`, porém reduziram significativamente a quantidade de bits disponíveis.

As métricas pós-condicionamento devem ser interpretadas considerando que os tamanhos das sequências são diferentes daqueles da saída bruta.

---

## Arquivos de métricas

Os arquivos utilizados nas análises estão disponíveis em:

```text
dados/metricas/
```

Principais arquivos:

```text
prng_metricas_30x10000.csv
csprng_metricas_30x10000.csv
simulador_metricas_30x10000.csv
qpu_bruta_metricas_30x10000.csv
qpu_condicionamento_metricas.csv
```

Esses arquivos contêm as métricas calculadas por lote e servem de base para as tabelas e análises estatísticas apresentadas no artigo.

---

## Integridade dos dados

Os hashes SHA-256 dos arquivos experimentais estão disponíveis em:

```text
checksums/SHA256SUMS.txt
```

Os hashes permitem verificar a integridade dos arquivos e identificar eventuais alterações em seu conteúdo.

Em sistemas Linux, a verificação pode ser realizada com:

```bash
sha256sum -c SHA256SUMS.txt
```

---

## Tecnologias utilizadas

- Python
- Google Colab
- Qiskit
- Qiskit Aer
- Qiskit IBM Runtime
- NumPy
- Pandas
- SciPy
- Statsmodels

---

## IBM Quantum e segurança das credenciais

O acesso ao hardware quântico exige uma conta válida na IBM Quantum.

Durante os experimentos, as credenciais foram armazenadas utilizando o sistema de Secrets do Google Colab.

Não são publicados:

- tokens de acesso;
- IBM Cloud CRN;
- identificadores privados de conta;
- outras credenciais de autenticação.

---

## Reprodutibilidade e rastreabilidade

Foram preservados, quando disponíveis:

- dados brutos da QPU;
- métricas calculadas por lote;
- arquivos CSV utilizados nas análises;
- metadados experimentais;
- backend;
- qubit físico;
- Job ID;
- quantidade de PUBs e shots;
- horários em UTC;
- parâmetros de execução;
- versões das bibliotecas;
- informações de calibração;
- checksums SHA-256.

O PRNG utiliza semente fixa e pode reproduzir a mesma sequência.

O simulador utiliza sementes controladas.

O CSPRNG e a QPU real não são destinados à reprodução das mesmas sequências em novas execuções.

Os dados brutos da QPU publicados neste repositório foram recuperados diretamente do Job original utilizado no artigo, evitando sua substituição por uma nova coleta.

---

## Limitações

A QPU utilizada neste projeto não constitui um QRNG dedicado ou certificado.

A coleta padronizada principal foi realizada em uma única sessão e condição de calibração.

Os 30 lotes devem ser interpretados como blocos operacionais da mesma coleta e não como réplicas temporais independentes.

Os experimentos atuais não permitem avaliar de forma conclusiva a estabilidade temporal da QPU.

A min-entropia corresponde a uma estimativa empírica e não representa uma caracterização formal de uma fonte de entropia segundo a NIST SP 800-90B.

Os testes estatísticos também não constituem certificação de segurança criptográfica.

## Finalidade acadêmica

Os experimentos possuem finalidade exclusivamente acadêmica e científica.

As sequências produzidas não foram utilizadas para:

- criação de chaves criptográficas reais;
- armazenamento de senhas;
- geração de credenciais;
- proteção de dados reais;
- sistemas criptográficos em produção.

A aprovação em testes estatísticos não constitui, isoladamente, garantia de imprevisibilidade ou segurança criptográfica.

---

##  Autor

**Lucas Diniz Ferreira Masteguim**

Universidade Presbiteriana Mackenzie  
Iniciação Científica — PIVIC Mackenzie

**Orientador:** Antonio Newton Licciardi Junior

---

##  Título do projeto

**Análise de geradores de números aleatórios via ruído quântico e como contribuem para segurança da informação em computação** circuito quântico utilizando Qiskit Aer;
- QPU real disponibilizada pela IBM Quantum.

Para a comparação padronizada, cada fonte foi analisada em:

- 30 lotes;
- 10.000 bits por lote;
- 300.000 bits por fonte.

A comparação principal corresponde, portanto, a 1.200.000 bits distribuídos entre as quatro fontes.

## Estrutura do repositório

``text
dados/
├── bruto/
│   ├── qpu_bruta_lote_01.txt
│   ├── qpu_bruta_lote_02.txt
│   ├── ...
│   └── qpu_bruta_lote_30.txt
│
├── metricas/
│   ├── prng_metricas_30x10000.csv
│   ├── csprng_metricas_30x10000.csv
│   ├── simulador_metricas_30x10000.csv
│   ├── qpu_bruta_metricas_30x10000.csv
│   └── qpu_condicionamento_metricas.csv
│
└── metadados/
    └── qpu_metadados_coleta.csv

notebooks/
├── IC_PRNG_padronizado_estatistica.ipynb
├── CSPRNG_padronizado_estatistica.ipynb
├── sim_QRNGs_padronizado_estatistica.ipynb
└── IC_QRNG_IBM_padronizado_estatistica.ipynb

checksums/
└── SHA256SUMS.txt

Os arquivos publicados correspondem aos dados e procedimentos utilizados na elaboração das análises apresentadas no artigo.

Notebooks
IC_PRNG_padronizado_estatistica.ipynb

Contém os experimentos realizados com o gerador pseudorrandômico didático.

A implementação utiliza o módulo random do Python, baseado no algoritmo Mersenne Twister, com semente fixa igual a 7.

O gerador é inicializado uma única vez antes da produção dos 30 lotes.

O PRNG é utilizado como referência determinística e didática e não como gerador destinado a aplicações criptográficas.

CSPRNG_padronizado_estatistica.ipynb

Contém os experimentos realizados com um gerador pseudorrandômico criptograficamente seguro.

A implementação utiliza:

secrets.randbits(1)

A fonte de aleatoriedade utilizada pelo módulo secrets é fornecida pelo sistema operacional.

Não é definida uma semente controlada pelo usuário.

sim_QRNGs_padronizado_estatistica.ipynb

Contém os experimentos realizados por meio da simulação de um circuito quântico.

O circuito utilizado possui:

um qubit inicialmente no estado |0>;
aplicação da porta Hadamard;
medição do qubit.

Idealmente:

H|0> = (|0> + |1>) / sqrt(2)

e:

P(0) = P(1) = 0,5

A execução é realizada utilizando Qiskit Aer.

Foram utilizadas sementes de simulação entre 1001 e 1030, permitindo reprodutibilidade sem produzir lotes idênticos.

Embora o circuito reproduza matematicamente as probabilidades previstas para o sistema quântico, a simulação é executada por mecanismos computacionais clássicos e não constitui uma fonte física de aleatoriedade quântica.

IC_QRNG_IBM_padronizado_estatistica.ipynb

Contém a coleta e a análise dos dados produzidos em hardware quântico real da IBM.

O circuito principal utiliza:

1 qubit;
porta Hadamard;
medição.

A coleta comparativa padronizada foi executada no backend:

ibm_marrakesh

utilizando o qubit físico 0.

A execução está associada ao Job ID:

da137vu3kjvs73868gp0

O Job contém:

30 PUBs × 10.000 shots

totalizando 300.000 resultados de medição.

Os experimentos exploratórios realizados durante etapas anteriores do estudo também incluíram seleção automática de QPUs de acordo com a disponibilidade dos dispositivos.

A execução em hardware real deve ser interpretada como uma demonstração experimental utilizando um dispositivo quântico confiado, e não como um QRNG certificado ou independente de dispositivo.

Dados brutos da QPU

Os dados brutos da coleta padronizada estão disponíveis em:

dados/bruto/

A pasta contém 30 arquivos:

qpu_bruta_lote_01.txt
qpu_bruta_lote_02.txt
...
qpu_bruta_lote_30.txt

Cada arquivo contém 10.000 resultados binários individuais.

Os arquivos foram recuperados diretamente do Job original:

da137vu3kjvs73868gp0

Não foi realizada uma nova execução da QPU para gerar os dados disponibilizados nesta pasta.

Essas sequências correspondem à saída anterior à aplicação de qualquer procedimento de condicionamento.

Metadados da coleta

Os metadados disponíveis para rastreabilidade experimental estão armazenados em:

dados/metadados/qpu_metadados_coleta.csv

Entre as informações preservadas estão, quando disponíveis:

backend utilizado;
Job ID;
qubit físico;
quantidade de PUBs;
quantidade de shots;
horários em UTC;
versões de Python, Qiskit e Qiskit IBM Runtime;
nível de otimização;
semente utilizada na preparação do circuito para o hardware;
T1;
T2;
erro de leitura;
data de calibração.

Campos que não estavam disponíveis nos registros preservados não são preenchidos com valores estimados.

Nenhum token, IBM Cloud CRN ou identificador privado de conta é disponibilizado publicamente.

Métricas estatísticas

As quatro fontes são avaliadas utilizando métricas comuns:

proporção de bits 0 e 1;
viés absoluto em relação a 0,5;
entropia de Shannon;
min-entropia empírica;
autocorrelação com defasagem 1;
número de corridas;
Runs Test;
intervalo de confiança de 95% para a proporção de bits pelo método de Wilson.

As métricas são calculadas individualmente para cada lote.

Runs Test

O Runs Test é aplicado após a geração das sequências e não corresponde a uma operação realizada na QPU.

A análise utiliza a biblioteca statsmodels.

São consideradas as hipóteses:

H0: o número de corridas observado é compatível com o comportamento esperado para a sequência analisada.

H1: o número de corridas observado difere do comportamento esperado.

Foi adotado nível de significância:

α = 0,05

Regra de decisão:

p >= 0,05 -> não rejeitar H0
p < 0,05  -> rejeitar H0

Para cada lote são armazenados, quando aplicável:

número de corridas;
estatística z;
valor-p;
decisão estatística.
Comparação estatística entre as fontes

Além da análise descritiva, os lotes podem ser comparados por procedimentos estatísticos inferenciais.

Kruskal-Wallis

O teste de Kruskal-Wallis é utilizado para verificar a existência de diferenças globais entre as quatro fontes em métricas como:

P(1);
viés;
entropia de Shannon;
min-entropia empírica;
autocorrelação com defasagem 1.
Tamanho de efeito

O epsilon-quadrado (ε²) é utilizado como medida de tamanho de efeito associada ao teste de Kruskal-Wallis.

Mann-Whitney U

Quando identificada diferença estatisticamente significativa no teste global, podem ser realizadas comparações par a par utilizando o teste de Mann-Whitney U.

Correção de Holm

Nas comparações múltiplas, os valores-p são ajustados pelo método de Holm.

Também pode ser utilizada a correlação bisserial de postos como medida do tamanho de efeito nas comparações par a par.

Condicionamento da saída da QPU

As sequências brutas são preservadas antes da aplicação dos procedimentos de condicionamento.

Foram avaliados dois métodos.

Método de von Neumann

A sequência é processada em pares:

00 -> descartado
11 -> descartado
01 -> utilizado para produzir um bit
10 -> utilizado para produzir um bit

Sob hipóteses adequadas, o procedimento pode reduzir determinados tipos de viés, mas provoca redução da quantidade de bits disponíveis.

SHA-256

A sequência bruta é dividida em blocos completos de 1.024 bits.

Cada bloco gera uma saída de 256 bits por meio de SHA-256.

Para cada lote de 10.000 bits:

9 × 1.024 = 9.216 bits processados

produzindo:

9 × 256 = 2.304 bits de saída

Os bits que não completam um bloco são desconsiderados.

Neste estudo, SHA-256 é utilizado experimentalmente como componente de condicionamento. Sua aplicação não cria nova entropia e não constitui, isoladamente, prova de uniformidade, extração formal ou segurança criptográfica.

Resultados principais

Na comparação padronizada foram obtidos:

Fonte	Bits analisados	Bits 0	Bits 1	P(1)
PRNG	300.000	150.107	149.893	0,49964
CSPRNG	300.000	149.528	150.472	0,50157
Simulador	300.000	149.820	150.180	0,50060
QPU IBM	300.000	149.102	150.898	0,50299

As quatro fontes apresentaram proporções próximas de 0,5.

Mesmo utilizando mecanismos de geração distintos, foram observadas propriedades estatísticas semelhantes.

Esse resultado reforça que desempenho estatístico satisfatório não comprova, isoladamente:

verdadeira aleatoriedade;
origem quântica;
imprevisibilidade;
segurança criptográfica.
Intervalos de confiança

Foram calculados intervalos de confiança de 95% para a proporção global de bits 1 pelo método de Wilson.

Fonte	P(1)	IC95%
PRNG	0,49964	[0,49785; 0,50143]
CSPRNG	0,50157	[0,49978; 0,50336]
Simulador	0,50060	[0,49881; 0,50239]
QPU IBM	0,50299	[0,50120; 0,50478]

Os intervalos utilizam uma aproximação binomial com hipótese de independência entre as observações. Caso exista dependência entre os bits, a incerteza real pode ser maior.

Resultados do condicionamento da QPU
Condição	Bits resultantes	P(1)	Taxa útil
QPU bruta	300.000	0,50299	100%
Von Neumann	75.188	0,50039	25,06%
SHA-256	69.120	0,49860	23,04%

Os procedimentos aproximaram a proporção global de bits de 0,5, mas reduziram significativamente a quantidade de bits disponíveis.

As métricas obtidas após o condicionamento devem ser interpretadas considerando que as sequências resultantes possuem tamanhos diferentes dos lotes brutos.

Arquivos de métricas

Os arquivos utilizados nas análises estão disponíveis em:

dados/metricas/

Entre os principais arquivos estão:

prng_metricas_30x10000.csv
csprng_metricas_30x10000.csv
simulador_metricas_30x10000.csv
qpu_bruta_metricas_30x10000.csv
qpu_condicionamento_metricas.csv

Esses arquivos contêm as métricas calculadas por lote e são utilizados na construção das tabelas e análises estatísticas apresentadas no artigo.

Integridade dos dados

Os checksums SHA-256 dos arquivos experimentais são disponibilizados em:

checksums/SHA256SUMS.txt

Os hashes permitem verificar a integridade dos arquivos e identificar alterações posteriores à publicação.

Em sistemas Linux, a verificação pode ser realizada com:

sha256sum -c SHA256SUMS.txt
Tecnologias utilizadas

O projeto utiliza principalmente:

Python;
Google Colab;
Qiskit;
Qiskit Aer;
Qiskit IBM Runtime;
NumPy;
Pandas;
SciPy;
Statsmodels.
IBM Quantum e segurança das credenciais

O acesso ao hardware quântico exige uma conta válida na IBM Quantum.

Durante os experimentos, as credenciais foram armazenadas utilizando o sistema de Secrets do Google Colab.

Não são publicados:

tokens de acesso;
IBM Cloud CRN;
identificadores privados de conta;
outras credenciais de autenticação.
Reprodutibilidade e rastreabilidade

Foram preservados, quando disponíveis:

dados brutos da QPU;
métricas calculadas por lote;
arquivos CSV utilizados nas análises;
metadados experimentais;
backend;
qubit físico;
Job ID;
quantidade de PUBs e shots;
horários em UTC;
parâmetros de execução;
versões das bibliotecas;
informações de calibração;
checksums SHA-256.

O PRNG utiliza semente fixa e pode reproduzir a mesma sequência.

O simulador utiliza sementes controladas.

O CSPRNG e a QPU real não são destinados à reprodução das mesmas sequências em novas execuções.

Os dados brutos da QPU disponibilizados neste repositório foram recuperados do Job original, evitando a substituição da coleta utilizada no artigo por uma nova execução.

Limitações

A QPU utilizada neste projeto não constitui um QRNG dedicado ou certificado.

A coleta padronizada principal foi realizada em uma única sessão e condição de calibração. Os 30 lotes devem, portanto, ser interpretados como blocos operacionais da mesma coleta e não como réplicas temporais independentes.

Os experimentos não permitem uma avaliação conclusiva da estabilidade temporal da QPU.

A min-entropia apresentada corresponde a uma estimativa empírica e não representa uma caracterização formal de uma fonte de entropia segundo a NIST SP 800-90B.

Os testes estatísticos utilizados também não constituem certificação de segurança criptográfica.

Uso ético de Inteligência Artificial

Durante o desenvolvimento desta pesquisa, foi utilizada a ferramenta ChatGPT, da OpenAI, como recurso auxiliar em atividades de revisão textual, organização e estruturação de conteúdo, esclarecimento de conceitos e apoio à elaboração e revisão de códigos e análises.

A ferramenta não substituiu a execução dos experimentos, a análise científica ou as decisões metodológicas. Os códigos foram executados pelo autor, e os resultados foram obtidos a partir das fontes e ambientes experimentais descritos no projeto.

As informações, sugestões, códigos e textos produzidos com auxílio de Inteligência Artificial foram submetidos à revisão e verificação antes de sua utilização.

O emprego da ferramenta ocorreu de maneira complementar, mantendo-se a autoria, o julgamento crítico e a responsabilidade acadêmica e científica sobre os resultados, interpretações e conteúdo publicados.

Finalidade dos experimentos

Os experimentos possuem finalidade exclusivamente acadêmica e científica.

As sequências produzidas não foram utilizadas para:

criação de chaves criptográficas reais;
armazenamento de senhas;
geração de credenciais;
proteção de dados reais;
sistemas criptográficos em produção.

A aprovação em testes estatísticos não constitui, isoladamente, garantia de imprevisibilidade ou segurança criptográfica.

Autor

Lucas Diniz Ferreira Masteguim

Universidade Presbiteriana Mackenzie
Iniciação Científica — PIVIC Mackenzie

Orientador: Antonio Newton Licciardi Junior

Título do projeto

Análise de geradores de números aleatórios via ruído quântico e como contribuem para segurança da informação em computação
