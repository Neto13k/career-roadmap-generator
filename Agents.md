# 🧠 AGENTS.md — Documentação Técnica dos Agentes

Documentação detalhada do funcionamento, prompts e lógica de cada agente do sistema **Career Roadmap Generator**.

---

## Visão Geral do Fluxo

```
Usuário
  │
  ▼
[Agente 1 — Entrevistador]
  │  7 perguntas, uma por vez
  │  Análise interna com matriz de decisão (0–20 pts)
  │  Apresenta 3 carreiras ranqueadas
  │  Usuário escolhe uma carreira
  │
  │  handoff com payload estruturado
  │
  ▼
[Agente 2 — Planejador de Carreira]
  │  Recebe os dados do Agente 1
  │  Gera plano completo personalizado
  │
  ▼
Plano de 90 dias
  ├── Visão do dia a dia
  ├── Mapa de skills
  ├── Roadmap quinzenal
  ├── Projeto de portfólio
  ├── Roteiro de entrevistas
  └── Trilha DIO recomendada
```

---

## Agente 1 — Entrevistador

### Objetivo

Descobrir o perfil profissional do usuário por meio de uma entrevista conversacional estruturada. O agente conduz **7 perguntas, uma por vez**, coletando as informações necessárias para recomendar as carreiras mais adequadas ao perfil.

### Implementação

O Agente 1 é um agente de linguagem (Claude) operando com um system prompt detalhado. Toda a lógica de sequenciamento das perguntas, análise do perfil e formatação da sugestão de carreiras está contida no prompt — sem código adicional.

**Arquivo de prompt:** `prompts/agent1_interviewer.md`

### Fases de Execução

| Fase | Descrição |
|------|-----------|
| **Fase 1 — Entrevista** | 7 perguntas sequenciais, uma por vez, aguardando resposta antes de prosseguir |
| **Fase 2 — Análise** | Matriz de decisão interna (uso exclusivo do modelo, não exibida ao usuário) |
| **Fase 3 — Sugestão** | Apresentação das 3 melhores carreiras com pontuação, justificativa e análise de mercado |
| **Fase 4 — Handoff** | Transferência dos dados para o Agente 2 após o usuário escolher uma carreira |

### Perguntas da Entrevista

| # | Pergunta | Dado coletado |
|---|----------|---------------|
| 1 | O que mais te atrai em tecnologia? | Motivação principal |
| 2 | Você já tem experiência em tech? | Nível de experiência |
| 3 | Quantas horas por semana para estudar? | Disponibilidade semanal |
| 4 | Prefere lidar com pessoas, dados ou código? | Preferência de trabalho |
| 5 | Qual é seu objetivo principal? | Objetivo profissional |
| 6 | Quais tecnologias despertam seu interesse? | Interesses técnicos |
| 7 | Tem experiência anterior (fora de tech)? | Bagagem transferível |

### Matriz de Decisão (uso interno)

Para cada carreira candidata, o modelo avalia 4 critérios com pontuação de 0 a 5:

| Critério | Peso |
|----------|------|
| Afinidade com interesses declarados | 0–5 |
| Demanda de mercado | 0–5 |
| Tempo até primeira vaga (ramp-up) | 0–5 |
| Aproveitamento de experiência prévia | 0–5 |

**Pontuação máxima:** 20 pontos por carreira. As 3 com maior pontuação são apresentadas.

### Formato de Apresentação das Carreiras

```
🥇 1º LUGAR: (CARREIRA) - (pontos)/20
💡 POR QUE COMBINA COM VOCÊ: (explicação personalizada)
⚖️ VANTAGENS: (2 pontos) | DESAFIOS: (2 pontos)
📈 MERCADO: (contexto regional — sem salários específicos)

🥈 2º LUGAR: (CARREIRA) - (pontos)/20
[mesma estrutura]

🥉 3º LUGAR: (CARREIRA) - (pontos)/20
[mesma estrutura]
```

### Payload de Handoff para o Agente 2

Após o usuário escolher uma carreira, o Agente 1 transmite ao Agente 2:

```
- CARREIRA_ESCOLHIDA: (nome da carreira)
- HORAS_SEMANA: (número)
- EXPERIENCIA: zero | iniciante | alguma
- OBJETIVO: primeiro emprego | transição | crescimento
- PREFERENCIA: pessoas | dados | código
- INTERESSES: (lista de tecnologias mencionadas)
```

### Regras Críticas

- Fazer **apenas 1 pergunta por vez** — aguardar sempre a resposta antes de prosseguir
- **Parar de perguntar** após as 7 perguntas e iniciar a análise
- **Nunca gerar o plano de estudos** — essa responsabilidade é exclusiva do Agente 2
- **Nunca citar salários específicos**
- **Nunca fazer mais de uma pergunta** na mesma mensagem

---

## Agente 2 — Planejador de Carreira

### Objetivo

Receber os dados coletados pelo Agente 1 e gerar um plano de carreira completo, personalizado e acionável — do mapa de skills ao roteiro de entrevistas.

### Implementação

O Agente 2 é um agente de linguagem (Claude) operando com um system prompt de planejamento. Recebe os dados do Agente 1 via mensagem estruturada e gera o plano em formato fixo com 6 seções.

**Arquivo de prompt:** `prompts/agent2_planner.md`

### Seções do Plano Gerado

#### 🧩 Visão do Dia a Dia
5 atividades típicas da carreira escolhida, descritas de forma realista e prática.

#### 🧠 Mapa de Skills

**Core Skills (essenciais):**
Competências obrigatórias para a função — sem as quais não se passa em processos seletivos.

**Nice-to-Have (complementares):**
Diferenciais que aumentam a empregabilidade e o salário inicial.

**Ferramentas e Tecnologias:**
Stack técnico recomendado, alinhado com os interesses declarados pelo usuário.

#### 📅 Roadmap de 90 Dias

Estruturado em 3 meses × 2 quinzenas, com metas específicas por período. Adaptado pela disponibilidade semanal informada.

#### 🚀 Projeto de Portfólio

Projeto concreto com:
- Nome e descrição do escopo
- Entregáveis (o que deve existir ao final)
- Critérios de aceitação (como saber que está pronto)
- Dica prática de execução

O projeto é escolhido para ser realizável dentro do prazo, publicável no GitHub e relevante para recrutadores da área.

#### 💬 Roteiro de Entrevistas

5 perguntas comuns em processos seletivos júnior, cada uma com exemplo estruturado de resposta — referenciando o projeto de portfólio sempre que possível.

#### 🎓 Trilha DIO Recomendada

Bootcamp ou trilha gratuita na [DIO](https://dio.me) com justificativa de alinhamento e instruções de acesso.

### Regras de Personalização

**Por disponibilidade semanal:**

| Horas/semana | Comportamento |
|---|---|
| Menos de 5h | Prazos estendidos, foco no essencial, menos entregáveis no projeto |
| 5–10h | Roadmap padrão de 90 dias |
| Mais de 15h | Conteúdo extra, projetos mais complexos, stack ampliada |

**Por nível de experiência:**

| Experiência | Comportamento |
|---|---|
| Zero | Explicações mais didáticas, fundamentos reforçados, linguagem acessível |
| Iniciante | Equilíbrio entre teoria e prática |
| Alguma experiência | Foco em gaps específicos, portfólio como prioridade |

**Por objetivo:**

| Objetivo | Foco principal |
|---|---|
| Primeiro emprego | Portfólio sólido + roteiro de entrevistas detalhado |
| Transição de carreira | Transferência de habilidades da área anterior + reposicionamento |
| Crescimento profissional | Skills avançadas + projetos de maior complexidade |

---

## Notas de Desenvolvimento

- O Agente 1 não tem acesso ao plano de estudos — sua responsabilidade termina no handoff
- O Agente 2 não tem memória entre sessões; todo o contexto necessário está no payload recebido
- O formato de output do Agente 2 é fixo — isso garante consistência independentemente da carreira escolhida
- Novos agentes podem ser adicionados ao fluxo (ex: Agente 3 de acompanhamento semanal) seguindo o mesmo padrão de payload estruturado como ponte entre etapas
- Os prompts completos de cada agente estão na pasta `prompts/` e podem ser ajustados sem alterar a arquitetura
