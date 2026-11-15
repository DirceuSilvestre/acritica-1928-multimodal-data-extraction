# Extração Multimodal e Inteligência de Dados em Acervos Históricos (*A Crítica*, 1928)

> **Transformando fotografias de jornais centenários em bases JSON auditadas e inteligência executiva com Visão Computacional (Gemini) e Governança de Dados.**

![Status](https://img.shields.io/badge/Status-Conclu%C3%ADdo-brightgreen)
![AI](https://img.shields.io/badge/AI-Gemini%20Multimodal-blue)
![Data](https://img.shields.io/badge/Data%20Modeling-JSON%20Schema-orange)
![Domain](https://img.shields.io/badge/Dom%C3%ADnio-Humanidades%20Digitais%20%7C%20UFRRJ-lightgrey)

---

## 1. O Problema de Negócio e Pesquisa
Organizações e centros de pesquisa acumulam volumes massivos de documentos não estruturados (PDFs digitalizados, imagens, contratos e acervos físicos) que permanecem inacessíveis para busca, cruzamento analítico ou algoritmos de IA. A transcrição manual é lenta, cara e inviável em escala; por outro lado, o uso indiscriminado de IA sem modelagem prévia gera saídas inconsistentes, alucinações e perda de rastreabilidade.

Desenvolvido no âmbito do **Bootcamp de Humanidades Digitais da UFRRJ**, este projeto resolve esse gargalo aplicando uma abordagem **Schema-First** e **Human-in-the-Loop** sobre as 10 primeiras edições do periódico *A Crítica* (Nova Iguaçu, RJ — junho a agosto de 1928).

---

## 2. Arquitetura da Solução (Pipeline sem Código Imperativo)

Em vez de depender de *parsers* rígidos ou de solicitar resumos genéricos à IA, o fluxo foi estruturado em quatro etapas auditáveis:

1. **Modelagem de Dados (*Schema-First*):** Mapeamento prévio da estrutura física do jornal e definição de uma hierarquia relacional em JSON (`periodico` → `edicao` → `paginas` → `conteudos`), preservando metadados de autoria, seção, tipologia e notas de pesquisa.
2. **Extração Multimodal (*Prompt-as-Code*):** Uso do modelo **Gemini** com capacidades de Visão Computacional para interpretar as páginas digitalizadas, segmentar colunas e popular o contrato JSON padronizado (`prompts/02_extracao_visao_computacional.md`).
3. **Critério de Domínio (*Human-in-the-Loop*):** Incorporação da metodologia de historiadores para sinalizar trechos comprometidos pela degradação física do papel ou tinta com a tag padronizada `[inelegivel]`, evitando que a IA "adivinhasse" palavras ausentes.
4. **Auditoria de Qualidade e EDA Executiva:** Validação cruzada entre arquivos para detecção de anomalias de digitalização, deduplicação de registros e síntese de indicadores de negócio (`reports/dossie_executivo_clevel.pdf`).

![Modelo de Grafo — Estrutura de Dados JSON](assets/modelo_grafo_json.png)

---

## 3. Governança e Auditoria de Dados (Resultados Técnicos)

A aplicação de rigor analítico sobre a base extraída permitiu identificar e corrigir problemas críticos antes da análise final[cite: 66, 72]:

| Métrica de Engenharia & Qualidade | Resultado Auditado | Ação de Governança Aplicada |
| :--- | :--- | :--- |
| **Registros Brutos vs. Corpus Canônico** | **413 brutos → 324 únicos** | Remoção de **89 registros excedentes** causados por vazamento de fronteira (*boundary bleed*) onde a página 4 de um PDF correspondia à capa da edição seguinte. |
| **Integridade Sintática (EOF)** | **2 arquivos truncados (Ed. 08 e 09)** | Recuperação íntegra dos dados via reconciliação cruzada com as edições subsequentes. |
| **Rastreabilidade Física (OCR/Papel)** | **388 marcações `[inelegivel]`** | Isolamento de falhas físicas (30,9% concentradas na Edição nº 1) sem preenchimento especulativo pela IA. |
| **Volume Lexical Consolidado** | **25.610 palavras válidas** | Média de 2.561 palavras/edição organizadas em 11 categorias editoriais e comerciais normalizadas. |

---

## 4. Principais Insights Extraídos (Visão Executiva)

Com a base limpa e estruturada, a Análise Exploratória de Dados (EDA) revelou dinâmicas econômicas e sociais em poucos instantes:

* **Pivotagem de Monetização:** A partir da 5ª edição, o jornal migrou 13 anunciantes âncora da contracapa direto para o cabeçalho da Página 1, sustentando uma taxa de retenção de **59%** entre as 22 empresas anunciantes.
* **Equilíbrio de Produto:** Anúncios curtos ocupavam **45,4%** dos blocos da página, mas preservavam **81,0%** do volume textual para crônicas, sátira e cultura.
* **Estratégias de Aquisição (1928):** Identificação de modelos semelhantes ao *freemium* (consultas médicas gratuitas matinais em farmácias) e vantagens logísticas (ternos sob medida em 24h com 40% de economia).
* **Inteligência Geoeconômica:** Mais de **85%** do comércio com endereço publicado concentrava-se em um triângulo de apenas três vias adjacentes à estação ferroviária.
* **Protagonismo Feminino:** A escritora **Sara J. B. do Correia** assinou o artigo de abertura da capa em **9 das 10 edições** analisadas.

---

## 5. Lições sobre Uso Adequado de IA no Trabalho
Este projeto demonstra que o uso profissional de Inteligência Artificial generativa exige **pensamento sistêmico**:
1. **IA não substitui modelagem:** Sem um *schema* claro, a extração escala a desorganização.
2. **Auditabilidade é inegociável:** Todo *pipeline* baseado em LLMs precisa de métricas de controle (taxa de duplicidade, falhas de leitura e validação de *schema*).
3. **Interdisciplinaridade gera vantagem:** A combinação de regras de domínio (Humanidades) com estruturação computacional entrega resultados que nenhuma das áreas alcançaria isoladamente.

---

## Créditos e Reconhecimentos
Projeto desenvolvido durante o bootcamp do Programa de Pós-Graduação em Humanidades Digitais da **UFRRJ**, sob orientação dos professores **Marcelo Zamith** e **Marcel William Rocha da Silva**.