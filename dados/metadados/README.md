# Metadados da coleta quântica

Esta pasta contém os metadados associados à coleta padronizada realizada em hardware quântico da IBM e utilizada nos resultados apresentados no artigo.

O arquivo principal é:

- `qpu_metadados_coleta.csv`

Nele estão registradas informações necessárias para a rastreabilidade experimental da coleta realizada na QPU, incluindo, quando disponíveis:

- backend utilizado;
- Job ID da execução;
- qubit físico empregado;
- quantidade de lotes;
- quantidade de bits ou shots por lote;
- horários de submissão e recebimento dos resultados em UTC;
- versões de Python, Qiskit e Qiskit IBM Runtime;
- nível de otimização;
- semente utilizada na preparação do circuito para o hardware;
- valores de T1 e T2;
- erro de leitura;
- data de calibração do dispositivo.

A coleta comparativa padronizada foi realizada no backend `ibm_marrakesh`, utilizando o qubit físico 0, em 30 lotes de 10.000 bits.

O Job ID associado à coleta é:

`da137vu3kjvs73868gp0`

Os metadados disponibilizados correspondem às informações preservadas durante a execução experimental. Campos que não puderam ser recuperados ou que não estavam presentes nos registros originais não foram preenchidos com valores estimados.

Por motivos de segurança e privacidade, informações como credenciais, tokens, IBM Cloud CRN e identificadores de conta não são disponibilizadas publicamente.

Os checksums SHA-256 dos arquivos publicados no repositório podem ser consultados na pasta `checksums/`.
