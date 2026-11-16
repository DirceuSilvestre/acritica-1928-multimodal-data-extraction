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
    <img src="https://img.shields.io/badge/Model-Gemini_3.1_Pro-10b981?style=flat-square&logo=google" alt="Gemini 3.1 Pro" />
    <img src="https://img.shields.io/badge/Methodology-SDD_%7C_RAG-38bdf8?style=flat-square" alt="Metodologia" />
    <img src="https://img.shields.io/badge/Commits-Conventional-ec4899?style=flat-square" alt="Conventional Commits" />
    <img src="https://img.shields.io/badge/Institution-UFRRJ_Pós-blue?style=flat-square" alt="UFRRJ" />
  </p>
  <p>
    <img src="https://img.shields.io/badge/Status-Conclu%C3%ADdo-brightgreen" alt="Status" />
    <img src="https://img.shields.io/badge/Data%20Modeling-JSON%20Schema-orange" />
    <img src="https://img.shields.io/badge/Dom%C3%ADnio-Humanidades%20Digitais%20%7C%20UFRRJ-lightgrey" />
  </p>
</div> 

> **Digitalizar não é estruturar.** Ler centenas de PDFs de jornais de 1928 com tipografia antiga e layout complexo desafia OCRs tradicionais. Este projeto demonstra como a orquestração avançada de Modelos de Visão e Linguagem (VLMs), guiada por forte Engenharia de Prompt e arquitetura de dados, transforma imagens de jornais centenários em dados estruturados e organizados prontos para Data Storytelling.

---

## 🎓 Origem e o Desafio Técnico

O desafio de processar e estruturar jornais da década de 1920 com layouts ruidosos foi inicialmente apresentado no **Bootcamp de IA do Departamento de Pós-graduação em Humanidades Digitais da UFRRJ**.

Identificando as severas limitações dos métodos tradicionais de OCR discutidos durante a imersão, decidi assumir a arquitetura integral deste desafio. Construí do zero uma solução independente baseada em VLMs (Vision-Language Models), elevando o escopo inicial a um pipeline robusto de Engenharia de Dados que garante rastreabilidade e governança.

---

## 🧠 Metodologia de IA e Boas Práticas

Este projeto foi desenhado para demonstrar maturidade no uso de LLMs/VLMs em ambientes de produção. O foco aqui não é o uso casual de chatbots, mas a construção de pipelines de software impulsionados por IA:

1. **Spec-Driven Development (SDD):** Utilização de metaprompts e RAG (anexando PDFs de referências de bancos de dados) para gerar um documento arquitetural robusto (`spec_driven_development...md`). A IA foi primeiro instruída a atuar como *Arquiteta de Dados* antes de realizar a extração.
2. **Engenharia de Prompt Avançada (Zero Hallucination):** Criação de um sistema cognitivo de extração (`01_extracao_visao_computacional.md`). O prompt obriga o **Gemini 3.1 Pro** a realizar Análise de Layout de Documentos (Document Layout Analysis), respeitando hierarquias espaciais, lidando com ruídos ("[ilegível]") e isolando blocos independentes.
3. **Extração Baseada em Esquema (JSON Strict):** Garantia de estruturação de dados utilizando um template rigoroso (`modelo_extracao_jornal_acritica.json`). A IA preenche metadados (edição, autores, colunas) gerando payloads prontos para ingestão direta em bancos SQL/NoSQL.
4. **Execução e Orquestração:** O processamento das imagens e geração dos esquemas JSON foi conduzido através do Gemini 3.1 Pro *[Nota: Ajuste aqui se usou a API em Python ou o Google AI Studio]*, garantindo a janela de contexto necessária para lidar com o alto volume de tokens visuais e textuais.
5. **Human-in-the-Loop (HITL) & Rastreabilidade:** Cada página extraída foi submetida a revisão humana para garantir qualidade e aderência histórica. Todo o versionamento seguiu o padrão **Conventional Commits** (ex: `docs(newspaper): add edition 10 of newspaper 'a critica'`), mantendo o histórico de evolução de dados limpo e auditável.

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

## ⚖️ A Abordagem VLM vs. OCR Tradicional

Para um problema complexo de layout de múltiplos recortes, ferramentas como Tesseract falham na extração de contexto. O modelo de Visão e Linguagem resolve lacunas críticas da arquitetura clássica:

| Desafio Histórico | OCR Tradicional (Tesseract) | **Abordagem VLM (Gemini 3.1 Pro)** |
| --- | --- | --- |
| **Layouts Complexos** | ❌ Lê colunas atravessadas, misturando textos. | ✅ **Compreende blocos visuais e hierarquia.** |
| **Grafia Antiga (ex: Pharmacia)** | ❌ Tenta corrigir para pt-BR moderno (Alucina). | ✅ **Transcreve com fidelidade histórica exata.** |
| **Metadados e Classificação** | ❌ Entrega apenas texto puro (TXT) sem contexto. | ✅ **Entrega JSON hierárquico (Página > Seção > Texto).** |
| **Isolamento Comercial** | ❌ Agrupa anúncios e notícias no mesmo bloco. | ✅ **Segmenta por tipo e intenção do conteúdo.** |

---

## 📊 Resultados e Data Storytelling

Além da extração de dados brutos, o projeto implementa uma camada analítica onde a IA atua como um *Analista de Dados C-Level* (`02_analise_dados_extraidos.md`).

O resultado final é a consolidação dos dados JSON em um **Dossiê Executivo (PDF)** de alto nível. O documento revela, através de práticas rigorosas de *Data Storytelling*, dinâmicas socioeconômicas da época — como retenção de anunciantes, gamificação social e tendências comerciais —, transformando registros históricos em conhecimento.

---

## 🚀 Como Explorar este Repositório

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