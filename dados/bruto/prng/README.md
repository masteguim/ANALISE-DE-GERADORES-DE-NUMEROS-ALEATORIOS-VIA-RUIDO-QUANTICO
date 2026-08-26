# Dados brutos — PRNG

Esta pasta contém os dados brutos gerados pelo PRNG utilizado na comparação principal do artigo.

A geração foi realizada com o módulo `random` do Python, que utiliza o algoritmo Mersenne Twister.

## Configuração utilizada

- **Semente:** 7
- **Número de lotes:** 30
- **Bits por lote:** 10.000
- **Total de bits:** 300.000

A semente 7 corresponde à configuração utilizada na comparação principal apresentada no artigo.

## Organização dos arquivos

Os arquivos seguem o padrão:

`prng_lote_01.txt`

até:

`prng_lote_30.txt`

Cada arquivo contém exatamente 10.000 bits representados pelos caracteres `0` e `1`.

## Finalidade

Os arquivos brutos foram preservados para permitir a auditoria e a reprodução das métricas estatísticas apresentadas no trabalho.

As análises quantitativas devem ser realizadas diretamente a partir destes arquivos, garantindo correspondência entre os dados publicados e os resultados apresentados no artigo.

## Observação

Embora o PRNG seja determinístico quando utilizada a mesma semente e a mesma implementação, os dados brutos são mantidos no repositório para facilitar a verificação independente dos resultados e uniformizar o tratamento das quatro fontes avaliadas.
