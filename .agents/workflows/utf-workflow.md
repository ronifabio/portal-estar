---
description: Define o fluxo operacional obrigatório da IA. Exige a criação e aprovação do spec.md e plan.md antes de qualquer código. Garante a execução em micro-tarefas com TDD, revisão por agentes distintos, trava erros em 2 rodadas e prepara o Pull Request.
---

# Fluxo de Trabalho de Implementação (SOP)

Sempre que o usuário pedir para trabalhar em uma Issue (Feature), você atuará como o orquestrador do método SDD seguindo ESTA ORDEM rigorosa:

**Passo 1: Entendimento e Brainstorming**
- Leia a Issue apontada e busque no `docs/prd.md` os critérios e o Glossário Ubíquo.
- Faça perguntas ao usuário de forma proativa. Questione sobre casos de borda, caminhos tristes (ex: falhas de rede, dados inválidos) e como validar os critérios de aceite.
- Após sanar as dúvidas, redija o documento e salve no caminho `specs/<numero-da-issue>-<slug>/spec.md`, com este frontmatter:

```yaml
---
issue: 27
status: rascunho   # rascunho | aprovada
---
```

- **PAUSA OBRIGATÓRIA:** Pare de gerar respostas e exija que o usuário leia e aprove o `spec.md`. Ofereça `/utf-tutor spec` para ele entender as consequências técnicas de cada decisão antes de aprovar. A aprovação é o **próprio usuário** trocar `status: rascunho` por `status: aprovada` e commitar essa linha — assim a aprovação fica no `git log`, com o nome dele. Você não altera esse campo em hipótese nenhuma.

**Passo 2: Planejamento**
- Com o `spec.md` aprovado, quebre o trabalho em tarefas curtas e encadeadas (2 a 5 minutos cada).
- Cada tarefa deve prever a criação de testes primeiro (TDD).
- Se o plano passar de **10 tarefas**, pare: a história é grande demais. Proponha dividi-la em duas Issues antes de continuar.
- Salve o resultado no caminho `specs/<numero-da-issue>-<slug>/plan.md`.
- **PAUSA OBRIGATÓRIA:** Peça a aprovação do usuário para o plano.

**Passo 3: Execução (uma tarefa por vez)**
- Crie a branch da Issue a partir da `main`.
- Execute **uma tarefa por vez** através do fluxo `ciclo-tarefa` (`.agents/workflows/ciclo-tarefa.md`), que despacha o subagente **implementador** com contexto limpo e, depois dele, dois revisores distintos e somente-leitura: **revisor-conformidade** (diff × critérios de aceite da `spec.md`) e **revisor-codigo** (diff × `docs/architecture.md`).
- **Você nunca revisa o código que você mesmo despachou.** Revisor é sempre outro agente, sem permissão de escrita. Auto-auditoria não conta como revisão: quem escreveu carrega os mesmos pontos cegos.
- Ao fim de cada tarefa, pare e devolva o controle ao usuário. Ele pede a próxima.

**Passo 4: Auditoria final e Pull Request**
- Terminadas todas as tarefas, despache o subagente **auditor-final**, que compara o diff **inteiro** contra o `spec.md` original — nunca contra o `plan.md`.
- Atualize, no mesmo commit do comportamento: o status da história no `docs/prd.md`, os diagramas do `docs/architecture.md` que mudaram, e o `specs/README.md`.
- Antes de o usuário escrever o PR, sugira `/utf-tutor prova` — o simulado interativo sobre o diff inteiro, que é o ensaio da defesa presencial.
- Prepare as alterações (commit) e lembre o usuário de abrir o Pull Request com `Closes #<n>`.
- A seção **"O que este PR faz e por quê"** é escrita **pelo usuário, com as palavras dele**. Ofereça os fatos do diff; não ofereça o texto pronto.
- No corpo do PR devem constar os apontamentos **aceitos e recusados**, com o motivo de cada recusa. A fonte é `specs/<issue>-<slug>/reviews/`: os pareceres (`tarefa-NN-<tipo>-r<N>.md`) e as decisões de triagem (`tarefa-NN-decisoes-r<N>.md`).
