# Checksums SHA-256

Esta pasta contém os checksums SHA-256 utilizados para verificar a integridade dos arquivos experimentais publicados neste repositório.

## Arquivo

O arquivo principal é:

`SHA256SUMS.txt`

Ele contém o hash SHA-256 de cada um dos 30 arquivos brutos da coleta padronizada realizada na QPU.

Os arquivos verificados correspondem a:

`qpu_bruta_lote_01.txt`

até:

`qpu_bruta_lote_30.txt`

Cada arquivo contém 10.000 bits, totalizando 300.000 bits brutos provenientes da coleta utilizada no artigo.

## Finalidade

Os checksums permitem verificar se os arquivos permanecem idênticos aos dados publicados originalmente.

Qualquer alteração, mesmo pequena, no conteúdo de um arquivo produz um hash SHA-256 diferente.

## Verificação de integridade

Para verificar a integridade dos arquivos, basta calcular novamente o hash SHA-256 de cada arquivo e compará-lo com o valor correspondente registrado em `SHA256SUMS.txt`.

Se os valores forem idênticos, o conteúdo do arquivo permanece inalterado em relação à versão publicada.

## Observação

Os checksums são utilizados apenas para verificação de integridade dos arquivos e não consti
