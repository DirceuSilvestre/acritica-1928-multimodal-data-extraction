 <div align="center">

  <!-- Sugestão: adicione uma imagem ou logo do projeto aqui -->

  <!-- <img src="images/logo-acritica.png" alt="A Crítica IA Logo" width="120" /> -->

  <h1>📰 Projeto A Crítica: Arqueologia de Dados com IA</h1>

  <h3>Estruturação, Extração e Análise de Periódicos Históricos (1928-1938) usando Gemini 3.1 Pro.</h3>

  <p>De imagens não estruturadas a conhecimento histórico.<br/><b>RAG · Spec-Driven Development · Prompt Engineering · Human-in-the-loop.</b></p>

  <p>
    <a href="#origem-e-o-desafio-tecnico">Origem</a> ·
    <a href="#metodologia-de-ia-e-boas-praticas">Metodologia (IA)</a> ·
    <a href="#arquitetura-do-pipeline">Arquitetura</a> ·
    <a href="#a-abordagem-vlm-vs-ocr-tradicional">Por que IA?</a> ·
    <a href="#resultados-e-data-storytelling">Resultados</a>
  </p>

  <!-- Badges -->

  <p>
    <img src="https://img.shields.io/badge/Model-Gemini_3.1_Pro-10b981?logo=google" alt="Gemini 3.1 Pro" />
    <img src="https://img.shields.io/badge/Methodology-SDD_%7C_RAG-38bdf8?" alt="Metodologia" />
    <img src="https://img.shields.io/badge/Commits-Conventional-ec4899?" alt="Conventional Commits" />
    <img src="https://img.shields.io/badge/Institution-UFRRJ_Pós-blue?" alt="UFRRJ" />
  </p>
  <p>
    <img src="https://img.shields.io/badge/Status-Conclu%C3%ADdo-brightgreen" alt="Status" />
    <img src="https://img.shields.io/badge/Data%20Modeling-JSON%20Schema-orange" />
    <img src="https://img.shields.io/badge/Dom%C3%ADnio-Humanidades%20Digitais%20%7C%20UFRRJ-lightgrey" />
  </p>
</div> 

> **Digitalizar não é estruturar.** Ler diversos PDFs de jornais de 1928 com tipografia antiga e layout complexo desafia OCRs tradicionais. Este projeto demonstra como a capacidade de orquestração de modelos de LLM para uso como Modelo de Visão e Linguagem (VLM), guiada por engenharia de prompt e arquitetura de dados, transforma imagens de jornais centenários em dados estruturados e organizados prontos para Data Storytelling.

---

## 🎓 Origem e o Desafio Técnico

O desafio de processar e estruturar jornais da década de 1920 com layouts ruidosos foi inicialmente apresentado no **Bootcamp de IA do Departamento de Pós-graduação em Humanidades Digitais da UFRRJ**.

Identificando as limitações dos métodos tradicionais de OCR discutidos durante a imersão, decidi assumir a arquitetura integral deste desafio. Construí do zero uma solução independente baseada em VLMs (Vision-Language Models), elevando o escopo inicial a um pipeline robusto de Engenharia de Dados que garante rastreabilidade e governança.

---

## 🧠 Metodologia de IA e Boas Práticas

Este projeto foi desenhado para demonstrar maturidade no uso de LLMs/VLMs, reconhecendo que modelos generativos não são "balas de prata". Para contornar as limitações inerentes aos VLMs puros — como alucinação textual, dificuldade com coordenadas espaciais finas em layouts fragmentados e custo computacional —, construímos um fluxo de trabalho fundamentado em restrições lógicas e validação humana:

1. **Spec-Driven Development (SDD):** Utilização de metaprompts e RAG (anexando PDFs de referências de bancos de dados) para gerar um documento arquitetural robusto (`spec_driven_development...md`). A IA foi primeiro instruída a atuar como *Arquiteta de Dados* antes de realizar a extração estruturada.
2. **Engenharia de Prompt como Guardrail (Mitigação de Alucinações):** Sabendo que VLMs tendem a inventar dados ao lidar com ruídos ou tipografias degradadas, o prompt de extração (`01_extracao_visao_computacional.md`) foi desenhado com restrições severas de Análise de Layout. O modelo é forçado a processar blocos visuais de forma independente e instruído a inserir a string `[ilegível]` estritamente, sendo proibido de tentar adivinhar caracteres danificados.
3. **Extração Baseada em Esquema (JSON Strict):** A dependência da precisão espacial do VLM foi ancorada em um template lógico rigoroso (`modelo_extracao_jornal_acritica.json`). Ao invés de pedir texto livre (onde o modelo se perde espacialmente), a IA preenche nós de metadados específicos, gerando payloads previsíveis e prontos para ingestão em bancos SQL/NoSQL.
4. **Execução e Orquestração de Contexto:** O processamento visual e textual foi orquestrado de forma iterativa via interface de chat do Gemini 3.1 Pro. Essa abordagem estratégica otimizou o uso da massiva janela de contexto do modelo, garantindo o processamento pesado dos tokens da imagem original, contornando provisoriamente os gargalos de latência e altos custos computacionais de uma esteira assíncrona via API para esta prova de conceito.
5. **Human-in-the-Loop (HITL) & Rastreabilidade:** Como VLMs apresentam fragilidades em recortes espaciais densos, a automação cega foi descartada por design. As etapas de saída foram submetidas a uma curadoria humana rigorosa para assegurar a fidelidade histórica. Todo o repositório é versionado seguindo o padrão **Conventional Commits** (ex: `docs(newspaper): add edition 10 of newspaper 'a critica'`), mantendo o histórico da curadoria de dados auditável.

---

## 🏗️ Arquitetura do Pipeline

```text
┌────────────────────────────────────────────────────────────────────┐
│  Pipeline de Extração e Análise (A Crítica 1928-1938)              │
├────────────────────────────────────────────────────────────────────┤
│  1. Ingestão    ➔ PDFs de Época (Scans com ruído visual e físico)  │
│  2. Orquestração➔ Prompt OCR Visual (Gemini 3.1 Pro + SDD)         │
│  3. Estruturação➔ Validação em JSON Schema (Hierarquia de Dados)   │
│  4. Revisão     ➔ Human-in-the-loop + Conventional Commits         │
│  5. Analytics   ➔ Dossiê Executivo C-Level (Data Storytelling)     │
└────────────────────────────────────────────────────────────────────┘

```

---

## ⚖️ A Abordagem VLM vs. OCR Tradicional: Trade-offs Arquiteturais

Afirmar que Modelos de Visão e Linguagem (VLMs) resolvem de forma automática todas as lacunas de layouts complexos é uma visão reducionista. Motores clássicos como o Tesseract possuem méritos indiscutíveis: operam com baixíssimo custo computacional e mapeiam coordenadas espaciais finas (*bounding boxes*) com extrema precisão (pixel a pixel).

Em contrapartida, VLMs puros operam com compressão de tokens visuais (*downsampling*), o que prejudica tarefas de mascaramento ou recorte posicional rigoroso, além de apresentarem riscos severos de alucinação (invenção ou deturpação de dados críticos) e exigirem hardware massivo (GPUs/VRAM).

No entanto, para o escopo deste projeto — que prioriza a **compreensão semântica e a estruturação de dados** sobre o mapeamento de pixels —, as fragilidades do VLM foram controladas via *Prompt Engineering* estrito e revisão *Human-in-the-Loop*, justificando sua escolha:

| Desafio do Periódico Histórico | OCR Clássico (ex: Tesseract) | **Orquestração VLM (Gemini 3.1 Pro + Prompting)** |
| --- | --- | --- |
| **Compreensão de Layouts Complexos** | ❌ Processa "linha a linha", frequentemente fundindo textos de colunas e anúncios paralelos. | ✅/⚠️ Compreende a hierarquia de blocos, **compensando a perda de precisão de coordenadas finas** através da inteligência semântica de segmentação. |
| **Grafia Antiga e Ruído Físico** | ❌ Falha estrutural ao ler manchas/rasgos e tenta forçar correções gramaticais incorretas. | ✅/⚠️ **Risco inerente de alucinação mitigado por restrições lógicas no prompt**, forçando a transcrição fiel e o uso obrigatório da tag `[ilegível]`. |
| **Estruturação de Metadados** | ❌ Entrega saídas textuais contínuas (TXT), sem distinção espacial de cabeçalhos ou seções. | ✅ Entrega dados tipados e aninhados (**JSON hierárquico**), preenchendo esquemas prontos para bancos de dados. |
| **Isolamento de Intenção (Contexto)** | ❌ Trata editoriais, poemas e propagandas comerciais como o mesmo bloco contínuo de texto. | ✅ Segmenta logicamente os conteúdos interpretando sua intenção comercial, editorial ou literária. |
| **Custo e Performance (Trade-off)** | ✅ **Alta eficiência:** Baixíssima latência e baixo custo computacional (executável em CPUs locais). | ❌ **Alta latência:** Exigência de infraestrutura robusta, justificável no projeto pela entrega do dado estruturado e curado para análises C-Level. |

---

## 📊 Resultados e Data Storytelling

Além da extração de dados brutos, o projeto implementa uma camada analítica onde a IA atua como um *Analista de Dados C-Level* (`02_analise_dados_extraidos.md`).

O resultado final é a consolidação dos dados JSON em um **Dossiê Executivo (PDF)** de alto nível. O documento revela, através de práticas rigorosas de *Data Storytelling*, dinâmicas socioeconômicas da época — como retenção de anunciantes, gamificação social e tendências comerciais —, transformando registros históricos em conhecimento.

---

## 🚀 Como Explorar este Repositório

### ⚠️ Nota sobre Reprodutibilidade (Proof of Concept)

Este repositório é um arquivo documental focado em **Engenharia de Prompt e Arquitetura de Dados**. Você deve copiar o prompt do arquivo `01_extracao_visao_computacional.md` e o layout estrutural `modelo_extracao_jornal_acritica.json`, e aplicá-los diretamente na sua própria interface de LLM (como o Google AI Studio), anexando as imagens contidas na pasta `data/raw/`.

### 1. Entendendo a Arquitetura (SDD)

Leia o arquivo `spec_driven_development_arquitetura_de_dados_para_gemini_3_1_pro.md` para compreender as decisões de modelagem e estruturação de dados exigidas do modelo antes da execução.

### 2. O Motor de Extração

Analise o arquivo `01_extracao_visao_computacional.md`. Este é o *core* do pipeline de visão computacional, contendo as restrições estritas e lógicas de bounding-box mental impostas à IA.

### 3. Visualizando os Dados

Navegue pela pasta de arquivos `.json` para visualizar o resultado prático da extração das edições, notando a consistência das chaves e o tratamento seguro de dados faltantes ou rasgados (`null` / `[ilegível]`).

---

## 🤝 Vamos Conversar?

Este projeto demonstra minha capacidade de orquestrar Modelos de IA Generativa para resolver problemas reais de dados não estruturados, unindo Engenharia de Software, visão analítica e governança.

Ficou interessado na abordagem ou possui desafios de orquestração de dados e IA no seu time? 
**Conecte-se comigo:**

* ![LinkedIn](https://img.shields.io/badge/Linkedin-@dirceu--silvestre-0A66C2?style=flat-square&labelColor=075A9B)
* [![Outlook](https://shields.io/badge/Email-dirceu__silvestre-0A66C2?style=flat-square&labelColor=075A9B)](mailto:dirceu_silvestre@hotmail.com)

---

## 📄 Licença e Reconhecimentos

Este projeto é de uso acadêmico e profissional. Desenvolvido para demonstrar capacidades avançadas de orquestração de IA e engenharia de dados. Sinta-se livre para explorar os prompts e metodologias (Licença MIT).

*Agradecimento especial aos facilitadores do bootcamp, professores **Marcelo Zamith** e **Marcel William Rocha da Silva**, pela apresentação inicial do desafio na UFRRJ.*