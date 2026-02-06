# Engenharia_Software_2025-2_langextract_atividade3_parte2

# Projeto: langextract

**Repositório base:**  
https://github.com/google/langextract  

**Fork utilizado:**  
https://github.com/Fernand0-jf/langextract  

---

## 1. Implementação Prática (Hands-on)

### 1.1 Estratégia Adotada

Para a implementação do pipeline de Integração Contínua (CI), a equipe optou pela **Estratégia A – Pipeline de Construção (Build & Test)**.

Essa escolha foi motivada pelo fato de o projeto **langextract** ser desenvolvido em Python e possuir dependências que podem ser instaladas e executadas normalmente no ambiente gratuito do **GitHub Actions**, sem exigir recursos especiais de hardware ou tempos de execução elevados.

Dessa forma, foi possível configurar um workflow que realiza a instalação do projeto, a verificação de sintaxe (compilação) e a execução de testes básicos, garantindo a validação funcional mínima do código a cada alteração.

---

### 1.2 Descrição do Workflow Implementado

O workflow foi configurado utilizando **GitHub Actions** e é acionado automaticamente nos seguintes eventos:

- push para a branch **main**
- pull_request direcionado para a branch **main**

O pipeline executa as seguintes etapas:

**Checkout do repositório**  
O código-fonte do projeto é obtido a partir do fork do repositório original.

**Configuração do ambiente Python**  
O ambiente de execução é configurado com a versão **Python 3.10**, garantindo compatibilidade com o projeto.

**Atualização do gerenciador de pacotes (pip)**  
O pip é atualizado para evitar problemas de compatibilidade durante a instalação das dependências.

**Instalação do projeto (Build)**  
O projeto é instalado em modo editável (pip install -e .), simulando o processo de build típico de projetos Python e validando a configuração do pacote.

**Verificação de sintaxe (Compilação)**  
É executado o comando `python -m compileall langextract`, que percorre todos os arquivos do módulo principal e verifica se o código pode ser compilado para bytecode, detectando erros de sintaxe ou indentação antes da execução.

**Execução de teste básico (Smoke Test)**  
Por fim, é realizado um teste simples que importa o módulo principal do projeto (**langextract**). Esse teste valida se o pacote foi corretamente instalado e se suas dependências principais estão resolvidas. Esse tipo de verificação é conhecido como **smoke test**, garantindo que o sistema está funcional em um ambiente limpo.

---

### 1.3 Trecho do Workflow (.yml)

Workflow disponível em:  
https://github.com/Fernand0-jf/langextract/blob/main/.github/workflows/etapa2-build-test.yml

name: Etapa 2 - Build and Test  

on:  
&nbsp;&nbsp;push:  
&nbsp;&nbsp;&nbsp;&nbsp;branches: [ "main" ]  
&nbsp;&nbsp;pull_request:  
&nbsp;&nbsp;&nbsp;&nbsp;branches: [ "main" ]  

jobs:  
&nbsp;&nbsp;build-test:  
&nbsp;&nbsp;&nbsp;&nbsp;name: Build & Smoke Test  
&nbsp;&nbsp;&nbsp;&nbsp;runs-on: ubuntu-latest  

&nbsp;&nbsp;&nbsp;&nbsp;steps:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- name: Checkout repository  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;uses: actions/checkout@v4  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- name: Set up Python  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;uses: actions/setup-python@v5  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;with:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;python-version: '3.10'  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- name: Upgrade pip  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;run: python -m pip install --upgrade pip  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- name: Install project (editable)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;run: pip install -e .  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- name: Compile code (syntax check)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;run: python -m compileall langextract  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- name: Smoke test - import langextract  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;run:  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;import langextract  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;print("LangExtract import OK")  

---

### 1.4 Diagrama do Pipeline Implementado

Push / Pull Request (main)  
↓  
Checkout do Repositório  
↓  
Setup do Python 3.10  
↓  
Upgrade do pip  
↓  
Instalação do Projeto (Build)  
↓  
Compilação / Verificação de Sintaxe (compileall)  
↓  
Smoke Test (import do módulo principal)  
↓  
Pipeline finalizado com sucesso  

---

### 1.5 Critério de Sucesso

Actions:  
https://github.com/Fernand0-jf/langextract/actions/runs/21712596731/job/62619814748  

O pipeline foi executado com sucesso no fork do repositório da equipe, apresentando o sinal verde (**Success**) na aba **Actions** do GitHub, atendendo integralmente ao critério de sucesso definido para a Etapa 2.

---

## 2. Justificativa do Não Uso das Outras Estratégias

### 2.1 Estratégia B – Quality Gate (Análise Estática)

A Estratégia B é indicada para projetos muito grandes ou legados, nos quais a execução do build é inviável dentro das limitações do ambiente de CI. No entanto, o projeto **langextract** permite a instalação, compilação e execução de testes básicos no GitHub Actions sem restrições técnicas.

Dessa forma, limitar o pipeline apenas a análises estáticas resultaria em uma validação menos completa do software.

---

### 2.2 Estratégia C – Pipeline de Contingência

A Estratégia C é destinada a cenários em que o projeto possui dependências impossíveis de serem satisfeitas no ambiente de Integração Contínua, exigindo a criação de scripts artificiais apenas para demonstrar o funcionamento do pipeline.

Como o projeto **langextract** pôde ser validado diretamente no GitHub Actions, não houve necessidade de adotar essa abordagem.

---

## 3. Etapa 3 – Análise de Impacto na Evolução do Software

A automação implementada por meio do pipeline de Integração Contínua contribui diretamente para a evolução saudável do software. A validação automática do build, da compilação e da execução mínima do projeto permite identificar erros de integração de forma precoce, reduzindo o risco de degradação gradual do código (**software decay**).

Além disso, o pipeline facilita o onboarding de novos desenvolvedores, pois estabelece um processo padronizado e automatizado de verificação. Assim, qualquer novo integrante da equipe pode confiar que, se o pipeline estiver verde, o projeto encontra-se em um estado funcional e consistente.

---

## 4. Conclusão

A implementação do pipeline de **Build & Test** utilizando **GitHub Actions** atendeu plenamente aos objetivos propostos, demonstrando na prática a aplicação de Integração Contínua e evidenciando seus benefícios para a qualidade, manutenção e evolução do software.
