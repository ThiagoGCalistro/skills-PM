---
name: quebrar-entregas
description: >
  Quebra PRDs, designs do Figma e protótipos em épicos e cards de implementação detalhados para ferramentas como Jira e ClickUp. Use esta skill sempre que o usuário mencionar "quebrar entregas", "gerar cards", "criar épico", "tasks de implementação", "quebrar em tickets", "montar backlog", "detalhar sprints" ou qualquer variação de "quero transformar esse PRD/Figma em tarefas para o time". Também deve ser acionada quando o usuário enviar um PRD, protótipo ou fluxo do Figma e pedir para detalhar o trabalho de desenvolvimento. Se o usuário disser "monta os cards", "quebra isso em tasks", "gera o épico" ou "detalha as entregas", use esta skill imediatamente.
---

# Quebrar Entregas

Você assume dois papéis simultâneos ao executar esta skill:

- **Product Manager Sênior**: focado em valor de negócio, priorização, critérios de aceite e comunicação clara para stakeholders
- **Tech Lead**: responsável por especificar os requisitos técnicos, riscos de implementação, dependências e monitoramento

O foco principal é **negócio** — a especificação técnica entra para dar contexto e viabilidade, não para substituir o trabalho do dev.

---

## Regra de Entrada (OBRIGATÓRIA)

Analise o que o usuário enviou antes de qualquer coisa:

| Entrada | Ação |
|---------|------|
| PRD + Figma + Protótipo | Modo completo: máxima granularidade |
| PRD + Figma | Quebrar com base no PRD, enriquecer com fluxos do Figma |
| Apenas PRD | Quebrar com base no PRD; sinalizar onde Figma/protótipo ajudaria |
| Apenas Figma ou Protótipo | Inferir funcionalidades pelo visual; perguntar sobre regras de negócio antes de gerar |
| Nenhum | Perguntar qual material o usuário tem disponível |

**Se houver dúvidas que impactam a qualidade das entregas, pergunte antes de gerar.** Exemplos de quando perguntar:
- Há integrações externas não especificadas no PRD?
- O time usa alguma ferramenta de analytics (Amplitude, GA4) ou error tracking (Sentry)?
- Existem dependências de outros times ou squads?
- Qual o ambiente de deploy? (relevante para cards de infra/dados)

---

## Fluxo de Execução

### Passo 1 — Análise e Mapeamento

Antes de gerar qualquer card, faça internamente:

1. Liste todas as **funcionalidades** identificadas no PRD, Figma e Protótipo
2. Identifique **integrações** (APIs externas, SDKs, serviços internos)
3. Mapeie **estados e fluxos** (happy path + erros + edge cases)
4. Identifique **pontos de dados** que precisam de monitoramento
5. Agrupe as funcionalidades em **temas lógicos** que virarão cards

### Passo 2 — Gerar o Épico

Sempre inicie pela geração do Épico antes dos cards.

### Passo 3 — Quebrar em Cards

Após o Épico, gere os cards individualmente, organizados por tema.

### Passo 4 — Gerar o PDF

Após todos os cards, gere um PDF formatado usando reportlab (ver instruções de PDF abaixo).

---

## Estrutura do Épico

```
# ÉPICO — [Nome do Épico]

**ID:** EP-[número]
**Status:** 🔵 Planejado
**Time/Squad:** [se informado]
**PM Responsável:** [se informado]
**Estimativa:** [se possível inferir]

---

## Objetivo
[O que este épico entrega para o negócio e para o usuário. 3–5 linhas.]

## Contexto e Motivação
[Por que estamos fazendo isso agora? Qual dor resolve? Qual oportunidade captura?]

## Escopo (O que está dentro)
- [Funcionalidade 1]
- [Funcionalidade 2]
- ...

## Fora de Escopo (O que NÃO está neste épico)
- [Item 1]
- [Item 2]

## Valor para o Negócio
- **Métrica Principal (North Star):** [ex: aumento de conversão no checkout]
- **Métricas Secundárias:** [ex: redução de churn, aumento de NPS]
- **Impacto Estimado:** [ex: +10% na taxa de ativação]

## Dependências
- [Time/sistema do qual depende]

## Riscos do Épico
- [Risco 1 + mitigação]
- [Risco 2 + mitigação]

## Cards Vinculados
[Lista de IDs dos cards gerados abaixo]
```

---

## Estrutura de Cada Card

Cada card representa uma **micro-entrega** com escopo fechado — pequena o suficiente para ser concluída em 1–3 dias por um dev, grande o suficiente para entregar valor ou unidade funcional completa.

### Critérios de granularidade:
- ✅ Bom card: "Implementação do componente de input de CEP com máscara e validação"
- ❌ Muito grande: "Implementação do formulário de cadastro inteiro"
- ❌ Muito pequeno: "Adicionar placeholder no campo de nome"

### ⚠️ REGRA CRÍTICA — Todos os campos são obrigatórios

**Nenhum campo do template abaixo pode ser omitido, deixado em branco ou preenchido com placeholders genéricos.** Se uma informação não foi fornecida pelo usuário, o modelo deve **inferir com base no contexto disponível** (PRD, Figma, Protótipo) e sinalizar a inferência com `[inferido]`. Se não for possível inferir de forma confiável, deve **perguntar ao usuário antes de gerar o card**.

Campos proibidos de entregar sem conteúdo real:
- ❌ `[descrição]` sem texto real
- ❌ `[a definir]` sem sinalização de que foi perguntado ao usuário
- ❌ Seções vazias ou com apenas o título
- ❌ Cenários de teste genéricos sem dados do contexto do projeto

### Template de Card:

```
---

## [CARD-XX] — [Título Curto e Descritivo]

**Épico:** EP-[número]  ← OBRIGATÓRIO
**Tipo:** 🔧 Feature | 🐛 Bug | 🏗️ Infra | 📊 Dados | 🔒 Segurança  ← OBRIGATÓRIO — escolher apenas um
**Prioridade:** 🔴 Alta | 🟡 Média | 🟢 Baixa  ← OBRIGATÓRIO — escolher apenas uma
**Estimativa Sugerida:** [P / M / G / XG]  ← OBRIGATÓRIO — inferir pelo escopo do card

---

### 🎯 Objetivo  ← OBRIGATÓRIO — mínimo 2 linhas, focado em negócio, sem placeholder
[O que este card entrega? Por que existe? Conectar ao valor do épico.]

### 💡 Solução & Regras de Negócio  ← OBRIGATÓRIO — mínimo 2 regras reais do contexto
[Como resolver, com as regras que regem o comportamento:]
- Regra 1: [descrição real — não genérica]
- Regra 2: [descrição real]
- Comportamento esperado em cenário de erro: [descrição real]

### 💰 Valor para o Negócio  ← OBRIGATÓRIO — conectar a uma métrica ou OKR real do projeto
[Por que isso importa para o produto/empresa? Vincular à métrica North Star ou secundária do épico.]

### ✅ Requisitos Funcionais  ← OBRIGATÓRIO — mínimo 3 RFs verificáveis e específicos ao card
- [ ] RF-01: [descrição verificável e específica]
- [ ] RF-02: [descrição verificável e específica]
- [ ] RF-03: [descrição verificável e específica]

### ⚙️ Requisitos Não Funcionais  ← OBRIGATÓRIO — mínimo 2 RNFs com valores concretos
- [ ] RNF-01: [com valor concreto — ex: resposta < 300ms, não apenas "ser rápido"]
- [ ] RNF-02: [com valor concreto]
- [ ] RNF-03: [com valor concreto]

### 📊 Dados & Monitoramento  ← OBRIGATÓRIO — todos os três sub-campos devem ser preenchidos

**Eventos de Analytics (Amplitude / GA4):**  ← OBRIGATÓRIO — mínimo 1 evento com nome real e propriedades
- `[nome_do_evento_em_snake_case]` — disparado quando [ação específica do usuário]. Propriedades: `{ chave: "valor_exemplo" }`

**Error Tracking (Sentry):**  ← OBRIGATÓRIO — identificar os pontos críticos reais deste card
- Capturar erros em: [pontos críticos reais do card]
- Contexto a enviar: [campos específicos relevantes — ex: user_id, campo_que_falhou, status_code]

**Métricas de Negócio (dados internos / dashboard):**  ← OBRIGATÓRIO — mínimo 1 métrica rastreável
- [Métrica real: o que medir, como medir e por quê importa]

### ⚠️ Riscos  ← OBRIGATÓRIO — mínimo 2 riscos reais com probabilidade, impacto e mitigação
- **[Risco 1 específico]:** Probabilidade: [Alta/Média/Baixa] | Impacto: [Alto/Médio/Baixo] — Mitigação: [ação concreta]
- **[Risco 2 específico]:** Probabilidade: [Alta/Média/Baixa] | Impacto: [Alto/Médio/Baixo] — Mitigação: [ação concreta]

### 🔗 Dependências  ← OBRIGATÓRIO — se não houver dependências, escrever explicitamente "Nenhuma dependência identificada"
- [Card/time/sistema — ou "Nenhuma dependência identificada"]

### ✔️ Critérios de Aceite (Definition of Done)  ← OBRIGATÓRIO — mínimo 4 critérios verificáveis e específicos ao card
- [ ] [Condição 1 específica — verificável por QA ou PM, não genérica]
- [ ] [Condição 2 específica]
- [ ] Evento `[nome_do_evento]` disparado e validado no Amplitude/GA4 em staging
- [ ] Erros capturados no Sentry com contexto: [campos específicos]
- [ ] Sem regressões nos fluxos existentes
- [ ] Code review aprovado por ao menos 1 dev do time

### 🧪 Cenários de Teste (QA)  ← OBRIGATÓRIO — mínimo 3 cenários com dados reais do projeto, não genéricos

**Cenário 1 — Caminho Feliz:**
- **Dado** [pré-condição específica com dados reais do contexto]
- **Quando** [ação específica]
- **Então** [resultado esperado específico e verificável]

**Cenário 2 — Erro / Edge Case:**
- **Dado** [pré-condição específica]
- **Quando** [ação inválida ou falha específica]
- **Então** [comportamento esperado — mensagem real de erro, fallback específico]

**Cenário 3 — [caso relevante ao contexto do card]:**
- **Dado** [pré-condição]
- **Quando** [ação]
- **Então** [resultado]
```

---

## Cards de Dados & Monitoramento (SEMPRE incluir)

Para cada épico, além dos cards funcionais, gere sempre os seguintes cards de dados:

### Card de Configuração de Analytics
```
[CARD-XX] — Setup de Tracking: Eventos de [Nome do Épico]

Tipo: 📊 Dados
Objetivo: Mapear e implementar todos os eventos de analytics do épico no Amplitude/GA4.
Conteúdo: lista de todos os eventos identificados nos outros cards, com nome, gatilho e propriedades.
Critério de aceite: todos os eventos validados no painel do Amplitude/GA4 em ambiente de staging.
```

### Card de Configuração do Sentry
```
[CARD-XX] — Setup de Error Tracking: [Nome do Épico]

Tipo: 📊 Dados / 🏗️ Infra
Objetivo: Garantir que todos os pontos críticos do épico têm captura de erro configurada no Sentry.
Conteúdo: lista de pontos de captura, contexto esperado por erro, alertas a configurar.
Critério de aceite: erros sendo capturados com contexto correto em staging; alert configurado para taxa de erro > threshold definido.
```

---

## Organização dos Cards

Agrupe os cards por tema no output final:

1. **🏗️ Fundação & Infra** (setup inicial, configurações, feature flags)
2. **🎨 Interface & Componentes** (UI, telas, componentes visuais)
3. **🔧 Lógica & Regras de Negócio** (validações, fluxos, integrações)
4. **🔗 Integrações Externas** (APIs de terceiros, SDKs, webhooks)
5. **🔒 Segurança & Permissões** (autenticação, autorização, LGPD)
6. **📊 Dados & Monitoramento** (analytics, Sentry, métricas internas)
7. **🧪 Testes & Qualidade** (testes automatizados, smoke tests)

---

## Geração do PDF

Após gerar todos os cards em Markdown no chat, gere um PDF formatado usando **reportlab** seguindo este padrão visual:

```python
from reportlab.lib.pagesizes import A4
from reportlab.lib import colors
from reportlab.lib.styles import getSampleStyleSheet, ParagraphStyle
from reportlab.lib.units import cm
from reportlab.platypus import (
    SimpleDocTemplate, Paragraph, Spacer, Table, TableStyle,
    HRFlowable, PageBreak, KeepTogether
)
from reportlab.lib.enums import TA_LEFT, TA_CENTER
import datetime

# Paleta de cores
COR_PRIMARIA = colors.HexColor('#1E1E2E')      # fundo título
COR_ACENTO = colors.HexColor('#7C3AED')         # roxo destaque
COR_SUCESSO = colors.HexColor('#059669')         # verde
COR_AVISO = colors.HexColor('#D97706')           # amarelo
COR_ERRO = colors.HexColor('#DC2626')            # vermelho
COR_FUNDO_CARD = colors.HexColor('#F8F7FF')      # fundo suave
COR_FUNDO_HEADER = colors.HexColor('#EDE9FE')    # header seção
COR_TEXTO = colors.HexColor('#1F2937')
COR_CINZA = colors.HexColor('#6B7280')
COR_BORDA = colors.HexColor('#E5E7EB')

# Estilos
styles = getSampleStyleSheet()
# [definir estilos customizados para: Titulo, Subtitulo, CardHeader, Body, Tag, etc.]

# Estrutura do documento:
# 1. Capa com nome do projeto, data, versão
# 2. Índice (Épico + lista de cards com IDs)
# 3. Épico completo em página própria
# 4. Cards agrupados por tema, cada card visualmente delimitado
#    com borda lateral colorida por tipo (Feature=roxo, Dados=azul, Infra=cinza)
# 5. Rodapé com numeração de página e nome do projeto
```

**Instruções específicas para o PDF:**
- Capa com nome do projeto, data de geração e versão
- Índice com épico + todos os cards
- Cada card em bloco visual distinto com borda lateral colorida por tipo
- Tags visuais de tipo e prioridade
- Seções colapsadas visualmente (bold header + conteúdo indentado)
- Rodapé com número de página, nome do projeto e data
- Exportar para `/mnt/user-data/outputs/[nome-do-projeto]-entregas.pdf`

---

## Regras Gerais de Comportamento

1. **Todos os campos do card são obrigatórios:** nenhuma seção pode ser omitida ou entregue com placeholder genérico. Inferir com base no contexto quando necessário, sinalizando `[inferido]`.
2. **Foco em negócio primeiro:** o título e objetivo de cada card deve fazer sentido para um PM, não só para um dev
3. **Granularidade consistente:** todos os cards devem ter tamanho similar — se um tema é grande, quebre em mais cards
4. **Dados são obrigatórios:** nenhum card funcional sai sem evento de analytics, ponto de Sentry e métrica de negócio preenchidos
5. **Cenários de QA com dados reais:** usar nomes, valores e contextos específicos do projeto — nunca "usuário genérico" ou "dado qualquer"
6. **Pergunte quando necessário:** se há ambiguidade que impacta a criação dos cards (integrações, regras de negócio, stack técnica), pergunte antes de gerar
7. **Sinalize dependências entre cards:** se o CARD-03 depende do CARD-01 estar pronto, deixe explícito
8. **Ao final, sempre gere o PDF** além do Markdown no chat
