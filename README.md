# Projeto: Classificação de Sentimentos com MLOps

![CI/CD](https://github.com/mosmendes/mlops-classificador-sentimentos-cd/actions/workflows/mlops.yml/badge.svg)

## Alunos

- Márcio Leandro
- Mônica Mendes
- Rudi Modena

## Visão Geral

Este projeto implementa um fluxo de MLOps para classificação de sentimentos em tweets utilizando Python, Scikit-Learn, Streamlit, Docker e GitHub Actions.

A solução contempla desde a exploração e preparação dos dados até a automação de integração contínua (CI) e deploy contínuo (CD) utilizando containers Docker.

O projeto inclui:

- Exploração e limpeza de dados
- Construção e validação de pipeline de Machine Learning
- Aplicação de inferência com Streamlit
- Testes automatizados
- Monitoramento de fairness
- Containerização com Docker
- Pipeline CI/CD automatizado com GitHub Actions
- Publicação automática da imagem Docker no Docker Hub

---

# Estrutura do Projeto

```text
.
├── .github/
│   └── workflows/
│       └── mlops.yml
├── data/
│   ├── tweets.csv
│   └── tweets_limpo.csv
├── notebooks/
│   ├── 01_exploracao.ipynb
│   ├── 02_pipeline_validacao.ipynb
│   ├── 03_deploy_streamlit.ipynb
│   └── 04_monitorar_fairness.ipynb
├── app.py
├── Dockerfile
├── model.joblib
├── vectorizer.joblib
├── requirements.txt
├── test_pipeline.py
└── README.md
```

---

# Descrição dos Arquivos

- `app.py`  
  Aplicação Streamlit responsável pela interface de previsão de sentimentos.

- `Dockerfile`  
  Arquivo de definição da imagem Docker da aplicação.

- `data/tweets.csv`  
  Dataset bruto contendo tweets para treinamento.

- `data/tweets_limpo.csv`  
  Dataset processado após limpeza e tratamento dos dados.

- `notebooks/01_exploracao.ipynb`  
  Notebook de exploração, análise e limpeza dos dados.

- `notebooks/02_pipeline_validacao.ipynb`  
  Notebook responsável pelo treinamento, validação e exportação do modelo.

- `notebooks/03_deploy_streamlit.ipynb`  
  Notebook com exemplos de deploy e execução da aplicação Streamlit.

- `notebooks/04_monitorar_fairness.ipynb`  
  Notebook de monitoramento de fairness e desempenho do modelo.

- `model.joblib`  
  Modelo treinado para classificação de sentimentos.

- `vectorizer.joblib`  
  Vetorizador utilizado para transformar os textos em features numéricas.

- `test_pipeline.py`  
  Arquivo contendo testes automatizados do pipeline.

- `.github/workflows/mlops.yml`  
  Pipeline CI/CD executado automaticamente no GitHub Actions.

---

# Fluxo do Projeto

## 1. Exploração e Limpeza de Dados

Etapa responsável pela análise exploratória e preparação do dataset.

Principais atividades:

- Carregamento dos dados
- Limpeza de texto
- Tratamento de inconsistências
- Geração do dataset processado

Notebook utilizado:

```text
notebooks/01_exploracao.ipynb
```

Arquivo gerado:

```text
data/tweets_limpo.csv
```

---

## 2. Construção e Validação do Modelo

Etapa de treinamento e validação do modelo de Machine Learning.

Principais atividades:

- Vetorização dos textos
- Treinamento do modelo
- Avaliação de métricas
- Persistência dos artefatos

Notebook utilizado:

```text
notebooks/02_pipeline_validacao.ipynb
```

Artefatos gerados:

```text
model.joblib
vectorizer.joblib
```

---

## 3. Aplicação Streamlit

A aplicação Streamlit permite realizar previsões de sentimentos em tempo real.

Arquivo principal:

```text
app.py
```

A aplicação carrega automaticamente:

- `model.joblib`
- `vectorizer.joblib`

para executar inferências de sentimento.

---

## 4. Testes Automatizados

Os testes automatizados garantem a integridade da pipeline e do modelo.

Arquivo:

```text
test_pipeline.py
```

Os testes validam:

- Existência dos artefatos
- Transformações do vectorizer
- Classificação de sentimentos
- Integridade dos dados limpos
- Fairness por tamanho de texto

---

## 5. Monitoramento de Fairness

O notebook de monitoramento avalia possíveis vieses e diferenças de desempenho do modelo.

Notebook utilizado:

```text
notebooks/04_monitorar_fairness.ipynb
```

O monitoramento considera:

- Acurácia por tamanho de texto
- Estabilidade do modelo
- Possíveis desvios de comportamento

---

# Docker

O projeto foi containerizado utilizando Docker para padronizar o ambiente de execução e automatizar o processo de deploy.

## Build local da imagem

```bash
docker build -t classificador-sentimentos .
```

## Execução local

```bash
docker run -p 5000:5000 classificador-sentimentos
```

A aplicação ficará disponível em:

```text
http://localhost:5000
```

---

# CI/CD Pipeline

O projeto utiliza uma estratégia de Continuous Integration (CI) e Continuous Deployment (CD) utilizando GitHub Actions e Docker Hub.

---

## Continuous Integration (CI)

O pipeline CI foi configurado no arquivo:

```text
.github/workflows/mlops.yml
```

O processo é executado automaticamente a cada push realizado na branch `main`.

### Etapas executadas

### 1. Exploração e Limpeza de Dados

Execução automática do notebook:

```text
notebooks/01_exploracao.ipynb
```

---

### 2. Treinamento e Validação do Modelo

Execução automática do notebook:

```text
notebooks/02_pipeline_validacao.ipynb
```

Geração dos artefatos:

- `model.joblib`
- `vectorizer.joblib`

---

### 3. Monitoramento de Fairness

Execução automática do notebook:

```text
notebooks/04_monitorar_fairness.ipynb
```

---

### 4. Testes Automatizados

Execução automática dos testes:

```bash
pytest test_pipeline.py
```

---

# Continuous Deployment (CD)

O processo de Continuous Deployment foi implementado utilizando Docker Hub.

Após a conclusão bem-sucedida do pipeline de integração contínua, o workflow executa automaticamente:

1. Build da imagem Docker
2. Publicação da imagem no Docker Hub
3. Disponibilização da aplicação para execução em qualquer ambiente Docker

---

## Publicação da Imagem Docker

A imagem Docker é construída automaticamente utilizando o `Dockerfile` do projeto.

Após o build, a imagem é publicada automaticamente no Docker Hub utilizando GitHub Actions.

---

## Secrets Configurados

Para publicação segura da imagem Docker foram configurados os seguintes secrets no GitHub:

- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`

---

## Runner Utilizado

Foi utilizado o runner hospedado do GitHub Actions:

```text
ubuntu-latest
```

Não foi necessária configuração de runner local.

---

# Execução da Aplicação via Docker Hub

Após o deploy automático, a aplicação pode ser executada diretamente a partir do Docker Hub.

## Download da imagem

```bash
docker pull mosmendes/classificador-sentimentos:latest
```

## Execução da aplicação

```bash
docker run -p 5000:5000 mosmendes/classificador-sentimentos:latest
```

A aplicação ficará disponível em:

```text
http://localhost:5000
```

---

# Benefícios da Estratégia CI/CD

- ✅ Automação completa do pipeline
- ✅ Execução automática de notebooks
- ✅ Execução automatizada de testes
- ✅ Validação contínua do modelo
- ✅ Publicação automática da imagem Docker
- ✅ Reprodutibilidade do ambiente com containers
- ✅ Deploy contínuo automatizado
- ✅ Monitoramento contínuo de fairness
- ✅ Rastreabilidade de alterações e builds

---

# Dependências Principais

- pandas
- scikit-learn
- great_expectations
- mlflow
- streamlit
- joblib
- matplotlib
- seaborn
- pytest
- jupyter
- nbconvert
- docker

---

# Observação sobre CI/CD

O processo de CI/CD foi implementado utilizando GitHub Actions devido à integração nativa com o repositório hospedado no GitHub, mantendo os mesmos conceitos de automação contínua solicitados na atividade.

---

# Repositórios

## GitHub

https://github.com/mosmendes/mlops-classificador-sentimentos-cd

## Docker Hub

https://hub.docker.com/

```