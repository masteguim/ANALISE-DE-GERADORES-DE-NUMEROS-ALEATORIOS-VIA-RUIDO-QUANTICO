# Manifesto de proveniência dos dados

Este documento registra a origem dos principais dados e resultados utilizados no projeto.

## Coleta em hardware quântico

A coleta padronizada em hardware quântico foi realizada utilizando o backend:

`ibm_marrakesh`

Job ID:

`da137vu3kjvs73868gp0`

Qubit físico utilizado:

`0`

Configuração da coleta:

- 30 PUBs;
- 10.000 shots por PUB;
- 300.000 resultados de medição no total;
- nível de otimização: 0;
- semente de compilação: 7.

Data de submissão:

`2026-08-16 22:04:46 UTC`

## Ambiente computacional

Versões registradas para a coleta principal:

- Python 3.12.13;
- Qiskit 2.5.2;
- Qiskit IBM Runtime 0.49.0.

## Condições do hardware

Para o qubit utilizado foram preservados:

- T1 aproximado: 214 µs;
- T2 aproximado: 34 µs;
- erro de leitura agregado: 0,354%;
- data de calibração disponível nos metadados publicados.

As probabilidades direcionais P(0|1) e P(1|0) não estavam disponíveis separadamente nos registros preservados e não foram estimadas posteriormente.

## Dados brutos

Os 30 arquivos contendo as sequências brutas da QPU foram recuperados diretamente do Job original.

Cada arquivo contém 10.000 bits.

Os dados correspondem à saída anterior à aplicação de qualquer procedimento de condicionamento.

## Métricas

As métricas estatísticas foram calculadas por lote e armazenadas em arquivos CSV.

Entre as métricas utilizadas estão:

- proporção de zeros e uns;
- viés;
- entropia de Shannon;
- min-entropia empírica;
- autocorrelação com defasagem 1;
- Runs Test.

## Condicionamento

Foram avaliados dois procedimentos de condicionamento:

- método de von Neumann;
- SHA-256.

A aplicação desses procedimentos não cria nova entropia.

## Integridade

Os hashes SHA-256 dos dados brutos estão disponíveis em:

`checksums/SHA256SUMS.txt`

## Segurança das credenciais

Tokens, IBM Cloud CRN e identificadores privados de conta não são publicados neste repositório.
