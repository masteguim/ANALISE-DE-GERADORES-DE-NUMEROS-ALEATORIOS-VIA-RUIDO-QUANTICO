# Notebooks exploratórios

Esta pasta contém notebooks e experimentos realizados durante o desenvolvimento da pesquisa que não fazem parte da análise quantitativa principal apresentada no artigo.

Os arquivos foram preservados para manter o histórico do desenvolvimento experimental e permitir a rastreabilidade das etapas exploratórias realizadas ao longo do projeto.

Os notebooks aqui armazenados podem conter, por exemplo:

- execuções preliminares em hardware quântico;
- diferentes Job IDs;
- testes com configurações alternativas;
- experimentos com sementes diferentes da utilizada na comparação principal;
- análises exploratórias não incorporadas aos resultados finais do artigo;
- versões anteriores dos códigos de aquisição ou avaliação estatística.

Esses arquivos **não devem ser utilizados como referência principal para reproduzir os resultados publicados**.

## Experimento utilizado no artigo

A coleta em hardware quântico utilizada na análise principal está associada ao:

- **Job ID:** `da137vu3kjvs73868gp0`
- **Backend:** `ibm_marrakesh`
- **Qubit físico:** `0`
- **Quantidade de lotes:** 30
- **Bits por lote:** 10.000
- **Total de bits brutos:** 300.000

A reanálise dessa coleta é realizada pelo notebook:

`../../notebooks/IC_QRNG_IBM_reanalise_job_original.ipynb`

Esse notebook utiliza os dados brutos preservados no repositório e não realiza uma nova submissão à QPU.

## Observação

Os materiais desta pasta possuem caráter histórico e exploratório. Para reprodução das tabelas, métricas e resultados apresentados no artigo, devem ser utilizados os notebooks definitivos disponíveis no diretório `notebooks/`.
