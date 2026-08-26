# Dados brutos — Simulador quântico

Esta pasta contém os dados brutos produzidos pelo simulador quântico utilizado na comparação principal do artigo.

A geração foi realizada com o `AerSimulator`, utilizando um circuito quântico de um qubit inicialmente no estado `|0⟩`, seguido pela aplicação da porta Hadamard e medição.

## Configuração utilizada

- **Simulador:** Qiskit AerSimulator
- **Circuito:** `|0⟩ → H → medição`
- **Número de lotes:** 30
- **Bits por lote:** 10.000
- **Total de bits:** 300.000
- **Nível de otimização:** 0
- **Semente de compilação:** 7
- **Sementes do simulador:** 1001 a 1030, sendo uma semente distinta para cada lote

As sementes distintas foram utilizadas para tornar a geração reprodutível sem repetir a mesma amostra em todos os lotes.

## Organização dos arquivos

Os arquivos seguem o padrão:

`simulador_lote_01.txt`

até:

`simulador_lote_30.txt`

Cada arquivo contém exatamente 10.000 bits representados pelos caracteres `0` e `1`.

## Métricas

As métricas estatísticas associadas aos lotes incluem:

- proporção de zeros e uns;
- viés absoluto;
- intervalo de confiança de Wilson para `P(1)`;
- entropia de Shannon;
- min-entropia empírica;
- autocorrelação com defasagem 1;
- número de corridas;
- estatística `z` do Runs Test;
- valor-p do Runs Test;
- indicação de não rejeição de `H0` para nível de significância de 5%.

Um valor-p maior ou igual a 0,05 não é interpretado como aprovação da aleatoriedade, mas apenas como ausência de evidência suficiente para rejeitar a hipótese nula nas condições do teste.

## Finalidade

Os dados brutos são preservados para permitir a reprodução das métricas e a auditoria independente dos resultados apresentados no artigo.

As análises devem ser realizadas diretamente a partir destes arquivos, garantindo correspondência entre os dados publicados e os resultados quantitativos utilizados no trabalho.

## Observação

Os dados desta pasta correspondem exclusivamente ao simulador quântico ideal utilizado na comparação principal. Execuções exploratórias ou configurações alternativas não fazem parte deste conjunto de dados.
