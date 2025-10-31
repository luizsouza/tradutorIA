# 🌐 Projeto: Tradutor Automático de Artigos Técnicos com Azure AI

## 🧠 Visão Geral

Este projeto propõe uma **arquitetura inteligente** baseada nos serviços da **Microsoft Azure**, voltada à **tradução automática de artigos técnicos publicados na web**.  
O objetivo é permitir que organizações, pesquisadores e profissionais de tecnologia tenham **acesso rápido a conteúdo técnico em múltiplos idiomas**, preservando **a precisão semântica e o contexto técnico**.

O sistema faz uso das capacidades de **Azure AI**, integrando **linguagem natural, tradução neural e análise de conteúdo**, tudo orquestrado em uma arquitetura escalável e automatizada.  

---

## 🎯 Objetivos do Projeto

- **Capturar** artigos técnicos publicados na web em diferentes idiomas.
- **Traduzir automaticamente** o conteúdo para o idioma alvo (ex: Português).
- **Preservar termos técnicos** e contexto semântico durante a tradução.
- **Armazenar e indexar** o conteúdo traduzido para consulta posterior.
- **Expor uma API e painel web** para busca e leitura dos artigos traduzidos.

---

## 🏗️ Arquitetura Conceitual

```
[Azure Logic Apps] ─► [Azure Cognitive Services - Translator] ─► [Azure OpenAI]
          │                                    │
          ▼                                    ▼
   [Azure Blob Storage] ◄────────────── [Azure Functions]
          │
          ▼
     [Azure AI Search]
          │
          ▼
      [Web App + API]
```

---

## ☁️ Componentes Provisionados no Azure

### 1. **Azure Cognitive Services - Translator**
- **Função:** Serviço principal de tradução neural.
- **Configuração:**
  - Provisionar no portal Azure: *Create Resource → AI + Machine Learning → Translator*.
  - Definir a **região** próxima ao público alvo (ex: `East US`).
  - Habilitar **tradução personalizada** (Custom Translator) para ajustar termos técnicos.
  - Criar um **glossário técnico** com pares de termos (ex: “load balancer → balanceador de carga”).

---

### 2. **Azure OpenAI Service**
- **Função:** Refinar traduções e ajustar nuances de linguagem técnica.
- **Configuração:**
  - Criar o recurso em *AI + Machine Learning → Azure OpenAI Service*.
  - Solicitar acesso ao modelo **GPT-4 Turbo**.
  - Configurar um **deployment** com nome identificável, ex: `gpt4-tech-refine`.
  - Utilizar *prompts engineering* para pós-processar a tradução, assegurando coerência e consistência técnica.

---

### 3. **Azure Logic Apps**
- **Função:** Orquestrar o fluxo de captura e tradução.
- **Configuração:**
  - Criar um **workflow recorrente** que:
    1. Consulta feeds RSS ou APIs de sites técnicos (via HTTP Connector).
    2. Extrai o conteúdo HTML e converte para texto.
    3. Envia o texto para a função do **Azure Translator**.
    4. Passa o resultado para uma **Azure Function** que aciona o OpenAI.
    5. Salva o texto final no **Blob Storage**.

---

### 4. **Azure Functions**
- **Função:** Camada de lógica customizada e integração.
- **Configuração:**
  - Criar uma função em Python ou C#.
  - Integrar via SDK com o Translator e o OpenAI.
  - Responsável por:
    - Limpar HTML e remover tags desnecessárias.
    - Executar o *prompt refinement*.
    - Armazenar a tradução finalizada.
  - Habilitar consumo sob demanda (planos **Consumption** ou **Premium**).

---

### 5. **Azure Blob Storage**
- **Função:** Armazenamento dos artigos originais e traduzidos.
- **Configuração:**
  - Criar dois containers:
    - `raw-articles` → artigos capturados da web.
    - `translated-articles` → traduções finais.
  - Habilitar **lifecycle management** para arquivamento automático após 90 dias.
  - Ativar **Azure Defender for Storage** para proteger contra malwares em arquivos de entrada.

---

### 6. **Azure AI Search**
- **Função:** Indexar e permitir busca semântica nos artigos traduzidos.
- **Configuração:**
  - Criar o serviço via *AI + Machine Learning → Azure AI Search*.
  - Definir um **indexador automático** para o container `translated-articles`.
  - Ativar a funcionalidade **semantic search** e **vector search**.
  - Integrar com **Azure OpenAI Embeddings** (para buscas contextuais mais inteligentes).

---

### 7. **Azure Web App + API Management**
- **Função:** Exposição de interface web e API pública.
- **Configuração:**
  - Criar um **App Service** com runtime Node.js, Python ou .NET.
  - Hospedar:
    - Um painel simples para pesquisa e leitura dos artigos.
    - Uma API REST (`/translate`, `/articles`, `/search`).
  - Utilizar **Azure API Management** para autenticação e controle de acesso.
  - Habilitar **CORS** e **Rate Limiting**.

---

### 8. **Azure Monitor + Application Insights**
- **Função:** Observabilidade e rastreamento do pipeline de tradução.
- **Configuração:**
  - Integrar com todos os serviços (Logic Apps, Functions e Web App).
  - Criar alertas para falhas no fluxo de tradução.
  - Habilitar logs de uso e métricas de custo por tradução.

---

## 🔐 Segurança e Compliance

- Ativar **Managed Identities** para comunicação segura entre serviços.
- Armazenar chaves e segredos no **Azure Key Vault**.
- Utilizar **Private Endpoints** para evitar exposição pública de APIs.
- Aplicar **RBAC (Role-Based Access Control)** com privilégios mínimos.

---

## 📈 Escalabilidade e Custos

- Todos os componentes utilizam planos **serverless ou sob demanda**, garantindo:
  - Escalabilidade automática conforme o volume de artigos.
  - Custo proporcional ao uso real.
- O custo médio estimado para 1000 artigos/mês é inferior a **US$50** (com otimização de requisições e cache).

---

## 🚀 Próximos Passos

1. Provisionar recursos principais no Azure.
2. Configurar os pipelines no Logic Apps e Functions.
3. Criar o modelo de glossário técnico personalizado.
4. Testar traduções e ajustar prompts.
5. Publicar o painel web e disponibilizar a API pública.

---

## 🧩 Tecnologias Envolvidas

| Categoria | Serviço Azure | Função |
|------------|----------------|--------|
| Tradução | Translator | Tradução neural multilíngue |
| IA Avançada | OpenAI Service | Refinamento de linguagem |
| Orquestração | Logic Apps | Automação de pipeline |
| Backend | Azure Functions | Processamento customizado |
| Armazenamento | Blob Storage | Armazenamento de conteúdo |
| Busca | AI Search | Indexação e busca semântica |
| Frontend | Web App | Exibição e interação com usuários |
| Segurança | Key Vault, RBAC | Proteção de credenciais |
| Monitoramento | Application Insights | Observabilidade completa |

---
