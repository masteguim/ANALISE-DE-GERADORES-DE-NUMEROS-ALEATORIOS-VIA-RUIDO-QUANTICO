# Dados brutos da coleta em QPU

Esta pasta contém os dados brutos utilizados na análise experimental da coleta padronizada realizada em hardware quântico da IBM.

A coleta corresponde ao Job ID:

`da137vu3kjvs73868gp0`

Foi utilizado o backend:

`ibm_marrakesh`

com o qubit físico 0.

A execução foi organizada em 30 PUBs, cada um contendo 10.000 shots, totalizando 300.000 bits.

## Arquivos

Os dados estão organizados em 30 arquivos:

- `qpu_bruta_lote_01.txt`
- `qpu_bruta_lote_02.txt`
- ...
- `qpu_bruta_lote_30.txt`

Cada arquivo contém 10.000 bits binários, correspondentes aos resultados individuais de medição daquele lote.

## Origem dos dados

Os arquivos foram recuperados diretamente do Job original armazenado no IBM Quantum, utilizando os resultados individuais preservados em cada PUB.

Não foi realizada uma nova execução da QPU para gerar estes arquivos.

Dessa forma, os dados publicados nesta pasta correspondem à mesma coleta utilizada nas análises e resultados apresentados no artigo.

## Pós-processamento

Os arquivos desta pasta representam a saída bruta da QPU, antes da aplicação de técnicas de condicionamento, como:

- método de von Neumann;
- SHA-256.

A preservação da saída bruta permite avaliar propriedades como proporção de bits, viés, entropia, autocorrelação e Runs Test antes do condicionamento.

## Integridade

Os checksums SHA-256 dos arquivos desta pasta são disponibilizados separadamente no repositório, permitindo verificar a integridade dos dados publicados.

## Segurança e privacidade

Nenhuma credencial, token, IBM Cloud CRN ou identificador privado de conta está incluído nestes arquivos.

