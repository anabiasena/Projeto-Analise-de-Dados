<p align="center">
  <img src="outputs/figures/assets/logo_ufpb.png" alt="Universidade Federal da Paraíba" width="180">
</p>

# Análise Comparativa do Perfil Discente e da Demanda Potencial por Assistência Estudantil — UFPB Campus I

Projeto que caracteriza o perfil discente e estima a proxy de demanda potencial por assistência estudantil no Campus I da Universidade Federal da Paraíba (UFPB). O repositório traz notebooks, scripts de transformação, artefatos processados e um dashboard para exploração dos resultados.

[![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-Interactive-orange?logo=streamlit)](https://streamlit.io/)
[![Pandas](https://img.shields.io/badge/Pandas-data-blue?logo=pandas)](https://pandas.pydata.org/)
[![NumPy](https://img.shields.io/badge/NumPy-numeric-blue?logo=numpy)](https://numpy.org/)
[![Plotly](https://img.shields.io/badge/Plotly-visualization-blue?logo=plotly)](https://plotly.com/)
[![SciPy](https://img.shields.io/badge/SciPy-statistics-blue?logo=scipy)](https://scipy.org/)
[![SQL](https://img.shields.io/badge/SQL-cells-lightgrey?logo=postgresql)](https://www.w3schools.com/sql/)
[![Jupyter](https://img.shields.io/badge/Jupyter-notebooks-orange?logo=jupyter)](https://jupyter.org/)

---

## Sobre o projeto

Este repositório contém o pipeline analítico, notebooks e o dashboard utilizados no estudo "Análise comparativa do perfil discente e da demanda potencial por assistência estudantil: um estudo sobre o Campus I da Universidade Federal da Paraíba". O objetivo é caracterizar o corpo discente de graduação do Campus I e identificar grupos com potencial demanda por assistência institucional.

## Telas do Dashboard

<p align="center">
  <img src="./outputs/figures/assets/visao-geral.png" alt="Visão Geral do Dashboard" width="85%">
</p>

### Priorização de Editais e Matriz de Cobertura
<p align="center">
  <img src="./outputs/figures/assets/priorizacao-editais.png" alt="Priorização de Editais" width="85%">
</p>

### Detalhamento Tático por Curso
<p align="center">
  <img src="./outputs/figures/assets/detalhamento-tatico.png" alt="Detalhamento Tático" width="85%">
</p>

### Análise de Turno e Estatística
<p align="center">
  <img src="./outputs/figures/assets/analise-turno.png" alt="Análise de Turno" width="85%">
</p>

### Equidade Demográfica
<p align="center">
  <img src="./outputs/figures/assets/equidade-demografica.png" alt="Equidade Demográfica" width="85%">
</p>

## Objetivo geral

Caracterizar perfis e disparidades no acesso às políticas de assistência estudantil no Campus I da UFPB e estimar uma proxy de demanda potencial não atendida para apoiar decisões institucionais.

## Recorte da pesquisa

- Instituição: Universidade Federal da Paraíba (UFPB)
- Unidade: Campus I (João Pessoa)
- População: Estudantes de graduação
- Observação: "comparativa" refere-se a comparações entre grupos dentro do Campus I (curso, centro, turno, raça, sexo), não entre universidades.

## Fontes de dados

- SEDAP+ (extrações usadas para gerar a tabela fato): arquivos em data/raw/SEDAP/ (ex.: fato_assistencia_raw.csv, dim_curso.csv).
- Censo da Educação Superior (INEP) — usado para validação/mapeamento de cursos (data/raw/INEP/ quando disponível localmente).

Nota: microdados brutos e informações pessoais não devem ser versionadas em repositórios públicos. Ver seção sobre proteção de dados abaixo.

## Indicador de Demanda Potencial Não Atendida (IDPNA)

Definição operacional usada neste projeto:

- IDPNA = 1 quando: IN_RESERVA_VAGAS == 1 e IN_APOIO_SOCIAL == 0
- IDPNA = 0 nos demais casos

Como implementado:
- O cálculo aparece em src/facts/fato_assistencia.py e é utilizado pelo dashboard em app/app.py.
- Internamente, RECEBE_AUXILIO é derivado de IN_APOIO_SOCIAL; IDPNA identifica cotistas (reservistas) que não recebem apoio.

Ressalvas metodológicas (obrigatórias):
- O IDPNA é uma PROXY de demanda potencial — uma medida agregada e indicativa construída a partir de campos disponíveis nas bases.
- Não deve ser interpretado como diagnóstico individual, comprovação de necessidade socioeconômica, medida de demanda observada nem prova de causalidade.
- Para decisões individuais, recomenda-se verificação e avaliação socioeconômica complementar.

## Principais resultados

- Caracterização do perfil discente do Campus I da UFPB;
- Identificação de grupos com maior demanda potencial por assistência;
- Construção do indicador de Demanda Potencial Não Atendida (IDPNA);
- Análises por curso, centro, turno, raça e sexo;
- Dashboard interativo para exploração dos resultados.

## Metodologia (resumo)

- Extração: células SQL e consultas presentes em notebooks (01_extracao_sedap.ipynb).
- Construção de dimensões: notebooks e módulos em src/dimensions/ geram as dimensões (curso, centro, raça, sexo, turno, etc.).
- Construção da tabela fato: src/facts/fato_assistencia.py aplica filtros, normalizações e calcula RECEBE_AUXILIO, IDPNA e TOTAL_IDPNA.
- Análises exploratórias e validações estatísticas: notebooks 04 e 05.
- Visualização: dashboard Streamlit em app/app.py consome os CSVs processados.

## Estrutura do repositório
```
Projeto-Analise-de-Dados-main/
├── .gitignore
├── README.md
├──.streamlit/
│   └──config.toml
├── app/
│   ├── assets/
│   ├── css.py
│   └── app.py                         (dashboard Streamlit — ponto de entrada)
├── data/
│   ├── raw/
│   │   ├── INEP/                      (Censo — quando disponível localmente)
│   │   └── SEDAP/
│   │       ├── Dicionário
│   │       ├── dim_curso.csv
│   │       └── fato_assistencia_raw.csv
│   └── processed/
│       ├── Dicionário.md
│       ├── Dimensões/                 (arquivos CSV das dimensões)
│       │   ├── dim_auxilio.csv
│       │   ├── dim_centro.csv
│       │   ├── dim_curso.csv
│       │   ├── dim_curso_centro.csv
│       │   ├── dim_curso_ufpb_campus_1.csv
│       │   ├── dim_grau.csv
│       │   ├── dim_modalidade.csv
│       │   ├── dim_raca.csv
│       │   ├── dim_sexo.csv
│       │   └── dim_turno.csv
│       └── Fato/
│           └── fato_assistencia.csv   (fato final consumido pelo dashboard)
├── docs/
│   └── RelatorioFinal_1.0.pdf
├── notebooks/
│   ├── 01_extracao_sedap.ipynb
│   ├── 02_construcao_dimensoes.ipynb
│   ├── 03_construcao_fato.ipynb
│   ├── 04_analise_exploratoria.ipynb
│   ├── 05_analise_idpna.ipynb
│   └── 06_dashboard.ipynb
├── outputs/
│   └── figures/                       (figuras geradas pelo pipeline)
├── requirements.txt
└── src/
    ├── dimensions/                    (módulos de construção de dimensões)
    ├── facts/                         (construção da tabela fato: fato_assistencia.py)
    └── utils/                         (utils / dicionários)
```


## Principais arquivos e funções

- app/app.py: arquivo que inicializa o dashboard (Streamlit) e contém os filtros, KPIs e visualizações.
- src/facts/fato_assistencia.py: regras de transformação do fato, cálculo de IDPNA e TOTAL_IDPNA.
- src/dimensions/: módulos que geram as dimensões usadas no merge com o fato.
- notebooks/: passo a passo reprodutível (extração, construção das dimensões, construção do fato, análises e protótipo do dashboard).

## Tecnologias encontradas

- Python (bibliotecas em requirements.txt)
- Pandas, NumPy (manipulação de dados)
- Plotly (visualização)
- SciPy (testes estatísticos)
- Streamlit (dashboard)
- Jupyter / notebooks
- SQL (células SQL presentes em notebooks de extração)

## Como executar (resumo)

1. Criar e ativar um ambiente virtual Python
2. Instalar dependências:

```Bash
pip install -r requirements.txt
```

3. (Opcional) Reproduzir processamento: executar notebooks 01 → 02 → 03 para gerar `data/processed/`.

4. Executar o dashboard:

```Bash
streamlit run app/app.py
```

## Análises realizadas (resumo)

- KPIs de cobertura e vulnerabilidade
- Top 10 cursos por volume de IDPNA
- Matriz cobertura vs vulnerabilidade por curso/centro
- Comparação Diurno vs Noturno (teste qui-quadrado)
- Análise de representatividade por raça e sexo

## Proteção dos dados

- O arquivo .gitignore contém a entrada: `data/raw/INEP/` e padrões de cache do Python (`__pycache__/`, `*.py[cod]`) — ver `.gitignore` no repositório.
- Microdados brutos não devem ser publicados em repositórios públicos; mantenha-os localmente e compartilhe apenas scripts de extração e amostras anonimizadas.

## Reprodutibilidade

Seguir os notebooks na ordem indicada garante a reprodução dos CSVs processados e dos gráficos. As funções em src/ suportam a automação das transformações.

## Limitações

- O IDPNA é uma proxy e não substitui avaliações individuais de necessidade socioeconômica.
- Dependência de mapeamento INEP ↔ SEDAP+ para identificar corretamente os cursos do Campus I.

## Referências

- INEP — Censo da Educação Superior
- SEDAP+ — extrações utilizadas (microdados)

## Status

- Notebooks e pipeline: presentes
- Dashboard: implementado em app/app.py

---
