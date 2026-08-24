# Instruções de execução

Este documento descreve o procedimento necessário para reproduzir as análises realizadas no projeto a partir dos notebooks, dados e arquivos disponibilizados no repositório.

## 1. Ambiente

Recomenda-se Python 3.12.

As dependências utilizadas no projeto estão listadas no arquivo:

`requirements.txt`

A instalação pode ser realizada com:

```bash
pip install -r requirements.txt
```

Os experimentos foram desenvolvidos principalmente no Google Colab.

---

## 2. PRNG

Notebook:

`notebooks/IC_PRNG_padronizado_estatistica.ipynb`

O experimento utiliza:

- 30 lotes;
- 10.000 bits por lote;
- 300.000 bits no total;
- Mersenne Twister;
- semente de 7 ou 42.

Para reproduzir o experimento:

1. abrir o notebook;
2. reiniciar o ambiente de execução;
3. executar todas as células em ordem;
4. verificar se todas as células foram executadas sem erros;
5. conferir os resultados e métricas produzidos.

No Google Colab, utilize a opção equivalente a:

`Ambiente de execução → Reiniciar sessão e executar tudo`

---

## 3. CSPRNG

Notebook:

`notebooks/CSPRNG_padronizado_estatistica.ipynb`

O experimento utiliza:

- 30 lotes;
- 10.000 bits por lote;
- 300.000 bits no total;
- módulo `secrets` do Python;
- fonte segura de aleatoriedade disponibilizada pelo sistema operacional.

Não é utilizada uma semente controlada pelo usuário.

Para executar:

1. abrir o notebook;
2. reiniciar o ambiente;
3. executar todas as células;
4. conferir se as métricas foram calculadas corretamente;
5. preservar apenas as saídas pertinentes à análise.

Uma nova execução não produzirá necessariamente os mesmos bits, pois o CSPRNG não foi desenvolvido para reproduzir uma sequência anteriormente gerada.

---

## 4. Simulador quântico

Notebook:

`notebooks/sim_QRNGs_padronizado_estatistica.ipynb`

O circuito utilizado contém:

- um qubit inicialmente no estado `|0>`;
- uma porta Hadamard;
- medição do qubit.

Idealmente:

```text
H|0> = (|0> + |1>) / sqrt(2)

P(0) = P(1) = 0,5
```

A execução utiliza Qiskit Aer.

Foram utilizadas sementes de simulação controladas entre 1001 e 1030.

Para reproduzir:

1. abrir o notebook;
2. reiniciar o ambiente;
3. executar todas as células em ordem;
4. verificar a geração dos 30 lotes;
5. conferir as métricas estatísticas calculadas.

A utilização de sementes controladas permite reproduzir os resultados do simulador.

---

## 5. QPU IBM

Notebook:

`notebooks/IC_QRNG_IBM_padronizado_estatistica.ipynb`

A coleta utilizada no artigo foi realizada em hardware quântico real da IBM.

Informações principais:

| Parâmetro | Valor |
|---|---|
| Backend | `ibm_marrakesh` |
| Qubit físico | `0` |
| Job ID | `da137vu3kjvs73868gp0` |
| PUBs | `30` |
| Shots por PUB | `10.000` |
| Total | `300.000 bits` |
| Nível de otimização | `0` |
| Semente de compilação | `7` |

A submissão da coleta ocorreu em:

`2026-08-16 22:04:46 UTC`

Versões registradas:

- Python 3.12.13;
- Qiskit 2.5.2;
- Qiskit IBM Runtime 0.49.0.

Para o qubit utilizado foram preservados:

- T1 aproximado de 214 µs;
- T2 aproximado de 34 µs;
- erro de leitura agregado de 0,354%.

### Reanálise da coleta existente

Não é necessário realizar uma nova execução da QPU para reproduzir as análises estatísticas do artigo.

Os dados brutos associados ao Job original já estão armazenados no repositório.

A recomendação é utilizar esses arquivos para as análises posteriores, preservando exatamente a coleta utilizada no artigo.

### Nova execução em hardware IBM

Uma nova coleta exige:

- conta válida na IBM Quantum;
- credenciais válidas;
- acesso a um backend disponível.

As credenciais devem ser armazenadas de forma segura, por exemplo utilizando o sistema Secrets do Google Colab.

Tokens, IBM Cloud CRN e identificadores privados de conta não devem ser inseridos diretamente no notebook ou publicados no GitHub.

Uma nova execução poderá produzir resultados diferentes e não substitui a coleta utilizada no artigo.

---

## 6. Dados brutos

Os dados brutos da coleta padronizada realizada na QPU estão disponíveis em:

`dados/bruto/`

A pasta contém:

```text
qpu_bruta_lote_01.txt
qpu_bruta_lote_02.txt
...
qpu_bruta_lote_30.txt
```

Cada arquivo contém 10.000 bits.

Total:

`30 × 10.000 = 300.000 bits`

Esses arquivos foram recuperados diretamente do Job original:

`da137vu3kjvs73868gp0`

Não foi realizada uma nova execução da QPU para gerar esses arquivos.

---

## 7. Métricas

Os arquivos de métricas estão disponíveis em:

`dados/metricas/`

Arquivos principais:

```text
prng_metricas_30x10000.csv
csprng_metricas_30x10000.csv
simulador_metricas_30x10000.csv
qpu_bruta_metricas_30x10000.csv
qpu_condicionamento_metricas.csv
```

As métricas calculadas incluem:

- proporção de bits 0;
- proporção de bits 1;
- viés;
- entropia de Shannon;
- min-entropia empírica;
- autocorrelação com defasagem 1;
- número de corridas;
- estatística do Runs Test;
- valor-p.

---

## 8. Metadados

Os metadados da execução em hardware quântico estão disponíveis em:

`dados/metadados/qpu_metadados_coleta.csv`

O arquivo contém, quando disponíveis:

- backend;
- Job ID;
- qubit físico;
- número de PUBs;
- shots por PUB;
- horários em UTC;
- versões das bibliotecas;
- nível de otimização;
- semente de compilação;
- T1;
- T2;
- erro de leitura;
- data de calibração.

As probabilidades direcionais P(0|1) e P(1|0) não estavam preservadas separadamente nos registros utilizados e não foram estimadas posteriormente.

---

## 9. Condicionamento da QPU

As análises incluem dois procedimentos de condicionamento.

### Von Neumann

Os bits são analisados em pares:

```text
00 -> descartado
11 -> descartado
01 -> produz um bit
10 -> produz um bit
```

A saída obtida no experimento contém:

`75.188 bits`

Taxa útil aproximada:

`25,06%`

### SHA-256

A sequência é dividida em blocos completos de 1.024 bits.

Cada bloco produz 256 bits.

Para cada lote de 10.000 bits:

```text
9 × 1.024 = 9.216 bits processados
9 × 256 = 2.304 bits de saída
```

A saída total obtida contém:

`69.120 bits`

Taxa útil:

`23,04%`

O uso de SHA-256 como condicionamento não cria nova entropia.

---

## 10. Testes estatísticos

As sequências são analisadas por métricas comuns às quatro fontes.

Entre os procedimentos estão:

- proporção de zeros e uns;
- viés;
- entropia de Shannon;
- min-entropia;
- autocorrelação com defasagem 1;
- Runs Test;
- intervalo de confiança de 95% pelo método de Wilson.

O Runs Test utiliza:

```text
α = 0,05
```

Regra de decisão:

```text
p >= 0,05 -> não rejeitar H0
p < 0,05  -> rejeitar H0
```

Os notebooks também incluem procedimentos para comparação estatística entre os lotes, quando aplicável.

---

## 11. Integridade dos arquivos

Os hashes SHA-256 estão disponíveis em:

`checksums/SHA256SUMS.txt`

O arquivo contém os hashes correspondentes aos dados brutos da QPU publicados no repositório.

Esses valores permitem verificar se os arquivos foram alterados após sua publicação.

---

## 12. Reexecução dos notebooks

Antes da publicação de uma versão final do repositório, os notebooks devem ser executados integralmente.

Para cada notebook:

1. reiniciar o ambiente de execução;
2. executar todas as células em ordem;
3. verificar a ausência de erros;
4. conferir se os resultados correspondem aos dados utilizados no artigo;
5. remover saídas desnecessárias;
6. verificar se nenhuma credencial ou informação privada aparece nas saídas;
7. salvar o notebook com as saídas relevantes;
8. atualizar a versão correspondente no GitHub.

No Google Colab, utilize a opção equivalente a:

`Ambiente de execução → Reiniciar sessão e executar tudo`

No notebook da IBM Quantum, deve-se evitar substituir a coleta original por uma nova execução. Para reproduzir as análises apresentadas no artigo, devem ser utilizados os dados brutos já preservados no repositório.

---

## 13. Segurança das credenciais

Não devem ser publicados:

- IBM Quantum Token;
- IBM Cloud CRN;
- identificadores privados de conta;
- arquivos contendo credenciais;
- variáveis de ambiente com informações privadas.

Antes de enviar um notebook para o GitHub, suas células e saídas devem ser verificadas para garantir que nenhuma dessas informações esteja presente.

---

## 14. Rastreabilidade

A rastreabilidade da coleta é baseada em:

- notebooks utilizados;
- dados brutos;
- CSVs de métricas;
- metadados da QPU;
- Job ID;
- backend;
- qubit físico;
- parâmetros de execução;
- informações de calibração disponíveis;
- versões das bibliotecas;
- checksums SHA-256.

O manifesto detalhado da origem desses dados está disponível em:

`PROVENANCE.md`

---
