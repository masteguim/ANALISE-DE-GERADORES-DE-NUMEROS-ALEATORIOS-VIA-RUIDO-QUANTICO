# Métricas estatísticas

Esta pasta contém os arquivos CSV com as métricas estatísticas calculadas a partir das sequências utilizadas no artigo.

Os resultados foram obtidos a partir de 30 lotes de 10.000 bits para cada uma das fontes analisadas.

## Arquivos

- `prng_metricas_30x10000.csv`  
  Métricas referentes ao gerador pseudorrandômico didático.

- `csprng_metricas_30x10000.csv`  
  Métricas referentes ao gerador pseudorrandômico criptograficamente seguro.

- `simulador_metricas_30x10000.csv`  
  Métricas referentes ao circuito quântico executado em simulador.

- `qpu_bruta_metricas_30x10000.csv`  
  Métricas referentes à saída bruta da QPU utilizada na coleta padronizada.

- `qpu_condicionamento_metricas.csv`  
  Métricas referentes às sequências obtidas após a aplicação dos métodos de condicionamento sobre a saída da QPU.

## Métricas avaliadas

Entre as métricas utilizadas estão:

- proporção de bits 0 e 1;
- viés absoluto em relação à probabilidade ideal de 0,5;
- entropia de Shannon;
- min-entropia empírica;
- autocorrelação com atraso 1;
- Runs Test, incluindo estatística de teste e valor-p;
- quantidade de bits por lote.

Os arquivos desta pasta correspondem aos dados utilizados na elaboração das tabelas e análises estatísticas apresentadas no artigo.

## Observação sobre interpretação

As métricas estatísticas permitem avaliar propriedades observáveis das sequências, mas não demonstram isoladamente imprevisibilidade, segurança criptográfica ou origem quântica.

No caso das sequências submetidas ao condicionamento, o número de bits disponível após o processamento é menor que o da sequência bruta. Portanto, métricas dependentes do tamanho da amostra devem ser interpretadas considerando essa diferença.

## Integridade dos arquivos

Os checksums SHA-256 dos arquivos disponibilizados no repositório podem ser consultados na pasta `checksums/`.
