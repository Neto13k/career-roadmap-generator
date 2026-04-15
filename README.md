# career-roadmap-generator

Sistema de dois agentes com IA para orientar profissionais em início ou transição de carreira na área de tecnologia Fullstack — do autoconhecimento até o roadmap personalizado de 90 dias.

---

## 🤖 Career Roadmap Generator — Agentes de Planejamento de Carreira em Tecnologia

> Projeto desenvolvido como atividade prática de um bootcamp da DIO, utilizando a API da Anthropic (Claude), com foco em carreiras Fullstack e integração com IA generativa.

---

## 📌 Visão Geral

Este projeto implementa um fluxo de dois agentes conversacionais, cada um com uma responsabilidade clara dentro do processo de orientação de carreira:

| Agente | Papel | Responsabilidade |
|--------|-------|-----------------|
| **Agente 1 — Entrevistador** | Interface conversacional | Conduz 7 perguntas para descobrir o perfil do usuário e sugerir 3 carreiras ranqueadas |
| **Agente 2 — Planejador** | Gerador de roadmap | Recebe os dados e produz o plano completo personalizado de 90 dias |

O fluxo foi projetado para ser acessível e acionável: O usuário responde 7 perguntas e recebe um plano de carreira estruturado com base nas respostas fornecidas.

---

## 🧩 Arquitetura dos Agentes

### Agente 1 — Entrevistador

Agente conversacional que conduz uma entrevista estruturada de 7 perguntas, **uma por vez**, para entender o perfil profissional do usuário:

1. **Motivação** — o que atrai o usuário em tecnologia
2. **Experiência prévia** — zero, iniciante ou alguma experiência
3. **Disponibilidade** — horas por semana para estudo
4. **Preferência** — trabalhar com pessoas, dados ou código
5. **Objetivo** — primeiro emprego, transição de carreira ou crescimento
6. **Interesses técnicos** — áreas e tecnologias de interesse
7. **Bagagem anterior** — experiências fora de tech aproveitáveis

Após as 7 respostas, o Agente 1 aplica uma **matriz de decisão** interna (pontuação 0–20) avaliando:
- Afinidade com interesses
- Demanda de mercado
- Tempo até a primeira vaga (ramp-up)
- Aproveitamento de experiência prévia

E apresenta as **3 melhores carreiras ranqueadas**. Quando o usuário escolhe uma, o agente realiza o handoff para o Agente 2.

**Dados transmitidos ao Agente 2:**
```
- CARREIRA_ESCOLHIDA
- HORAS_SEMANA
- EXPERIENCIA (zero/iniciante/alguma)
- OBJETIVO (primeiro emprego/transição/crescimento)
- PREFERENCIA (pessoas/dados/código)
- INTERESSES (tecnologias mencionadas)
```

---

### Agente 2 — Planejador de Carreira

Agente de linguagem (Claude) que recebe os dados do Agente 1 e gera um plano completo com 6 seções:

#### 🧩 Visão do Dia a Dia
5 atividades típicas da carreira escolhida — rotina realista de quem já está na área.

#### 🧠 Mapa de Skills
- **Core Skills** — competências essenciais para a vaga
- **Nice-to-Have** — diferenciais que aumentam a empregabilidade
- **Ferramentas e Tecnologias** — stack recomendado alinhado aos interesses

#### 📅 Roadmap de 90 Dias
Plano quinzenal adaptado à disponibilidade do usuário, em três fases:
- **Mês 1:** Fundamentos
- **Mês 2:** Prática
- **Mês 3:** Portfólio e preparação para entrevistas

#### 🚀 Projeto de Portfólio
Projeto concreto com escopo, entregáveis e critérios de aceitação — pronto para publicar no GitHub.

#### 💬 Roteiro de Entrevistas
5 perguntas comuns em processos seletivos júnior com exemplos estruturados de como responder.

#### 🎓 Trilha DIO Recomendada
Bootcamp ou trilha gratuita na [DIO](https://dio.me) alinhada com a carreira escolhida, com instruções de acesso.

---

## ⚙️ Regras de Personalização

O Agente 2 adapta o plano com base nos dados recebidos:

**Por disponibilidade:**
- Menos de 5h/semana → prazos estendidos, foco no essencial
- 5–10h/semana → roadmap padrão de 90 dias
- Mais de 15h/semana → conteúdo extra e projetos avançados

**Por nível de experiência:**
- Zero → linguagem didática, fundamentos reforçados
- Iniciante → equilíbrio entre teoria e prática
- Alguma experiência → foco em gaps específicos e portfólio

**Por objetivo:**
- Primeiro emprego → ênfase em portfólio e roteiro de entrevistas
- Transição de carreira → destaque para transferência de habilidades
- Crescimento profissional → foco em skills avançadas

---

## 🚀 Como Usar

1. Acesse o chat com o agente no Claude.ai (ou interface configurada)
2. Interaja com o **Agente 1** respondendo às 7 perguntas, uma por vez
3. Escolha uma das 3 carreiras sugeridas
4. O **Agente 2** recebe o handoff e gera seu plano completo automaticamente
5. Tire dúvidas, peça detalhes ou ajuste qualquer seção conversando com o agente

---

## 📂 Estrutura do Repositório

```
career-roadmap-generator/
├── README.md                        # Este arquivo
├── AGENTS.md                        # Documentação técnica dos agentes
└── prompts/
    ├── agent1_interviewer.md        # System prompt do Agente 1
    └── agent2_planner.md            # System prompt do Agente 2
```

---

## 🛠️ Tecnologias Utilizadas

- **Claude (Anthropic)** — modelo de linguagem para entrevista e geração do plano
- **Claude.ai** — ambiente de execução dos agentes
- **Markdown** — formato dos prompts e documentação

---

## 🔗 Referências

- Repositório base: [digitalinnovationone/copilot-prompts](https://github.com/digitalinnovationone/copilot-prompts)
- Plataforma de cursos: [dio.me](https://dio.me)
- API utilizada: [Anthropic Claude](https://anthropic.com)

---

## 📄 Licença

Este projeto está licenciado sob a [MIT License](LICENSE).

---

## 🤝 Contribuindo

Contribuições são bem-vindas! Sinta-se à vontade para abrir issues com sugestões de novas carreiras, melhorias nas perguntas do Agente 1, ajustes nas regras de personalização ou novos exemplos de planos gerados.

---

Projeto educacional focado na construção de agentes de IA, aplicando arquitetura multi-agente, prompt engineering e tomada de decisão baseada em dados do usuário.