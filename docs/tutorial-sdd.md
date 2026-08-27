# 🎮 Tutorial: operando o método SDD

Este é o passo a passo **operacional**: o que digitar e o que fazer em cada pausa.
O *porquê* de cada regra está no [guia da disciplina](./guia-sdd.md); as normas de
Git e de PR estão no [CONTRIBUTING](../CONTRIBUTING.md).

> A regra que resume tudo: **a IA escreve o código; você decide nos portões.**
> São três portões por história — aprovar a spec, aceitar a explicação do tutor
> antes de cada tarefa e fazer a triagem dos apontamentos — mais o PR no fim.

---

## Antes de começar (uma vez por história)

1. A história está no [prd.md](./prd.md) com status `🟡 Ready` (regras definidas).
2. Existe a Issue no GitHub Projects apontando para ela — a descrição só linka o
   PRD, nunca copia regra de negócio.

---

## Passo 1 — Iniciar a Issue

```
/utf-issue 12
```

O agente vira orquestrador: lê a Issue e o PRD e **faz perguntas** sobre casos de
borda e caminhos tristes (brainstorming). Da conversa sai
`specs/012-<slug>/spec.md` com `status: rascunho` — e ele **para**.

## Passo 2 — 🚪 Aprovar a spec (fora do chat)

Leia o arquivo **inteiro**. Em dúvida sobre alguma decisão técnica, rode
`/utf-tutor spec` antes.

A aprovação é **você** trocar `status: rascunho` por `status: aprovada` no
frontmatter e **commitar essa linha** — ela fica no `git log`, com o seu nome.
Nenhum agente altera esse campo.

## Passo 3 — Aprovar o plano

Avise que aprovou; o agente gera o `plan.md` (tarefas de 2–5 minutos — mais de
10, a história é grande demais e ele propõe dividir). Você lê, dá o OK na
conversa, e ele cria a branch a partir da `main`.

## Passo 4 — Implementar, uma tarefa por vez

```
/utf-task 1
```

Dentro do comando acontece o ciclo completo, com as suas paradas:

| Etapa | Quem age | Você faz o quê |
| --- | --- | --- |
| Tutor explica a tarefa, bem mastigado | tutor (contexto limpo) | **🚪 aceita** ("pode implementar") ou pergunta |
| Implementação com TDD | implementador novo | acompanha |
| Revisão em paralelo | revisor-conformidade + revisor-codigo | nada — quem despacha é o fluxo |
| Pareceres gravados em `reviews/` | orquestrador | nada |
| **Triagem** (se houve apontamentos) | orquestrador apresenta a lista | **🚪 aceita ou recusa cada um** — recusa exige justificativa, registrada em `reviews/tarefa-NN-decisoes-rN.md` |
| Commit `tarefa 1: ...` | orquestrador | confere o diff na IDE; `/utf-tutor 1` se quiser a aula |

Repita para cada tarefa: `/utf-task 2`, `/utf-task 3`… — ou apenas
`/utf-task`, que pega a próxima pendente do `plan.md` e avisa quando não
houver mais nenhuma. O ciclo devolve
o controle a você ao fim de **cada** tarefa — nunca emenda duas.

## Passo 5 — Fechar a Issue

Com todas as tarefas prontas, o orquestrador despacha o **auditor-final** (diff
inteiro contra a spec, ignorando o plano) e atualiza os docs no mesmo commit.

Antes de escrever o PR:

```
/utf-tutor prova
```

O simulado da defesa: uma pergunta por vez sobre o diff, com correção das suas
respostas e a lista de arquivos para reler.

Então **você** escreve a seção *"O que este PR faz e por quê"* com as suas
palavras, lista os apontamentos aceitos e recusados (saem dos arquivos
`decisoes` em `reviews/`) e abre o PR com `Closes #12`.

---

## Resumo dos comandos

| Comando | Quando usar |
| --- | --- |
| `/utf-prd` | Uma vez por projeto — a entrevista que gera o `docs/prd.md` |
| `/utf-architecture` | Uma vez, depois do PRD — a entrevista que gera o `docs/architecture.md` |
| `/utf-setup` | Uma vez, depois do architecture — gera o scaffold do monorepo |
| `/utf-issue <n>` | Uma vez, para iniciar o ciclo da Issue (spec → plano) |
| `/utf-task [n]` | Uma vez **por tarefa** do plano — sem número, executa a próxima pendente |
| `/utf-tutor spec` | Antes de aprovar a spec |
| `/utf-tutor <n>` | Depois de uma tarefa, para a aula sobre aquele diff |
| `/utf-tutor antes <n>` | Para reouvir a explicação pré-implementação de uma tarefa |
| `/utf-tutor prova` | Antes de escrever o PR — o ensaio da defesa |

Dizer "vamos trabalhar na Issue 12" em linguagem natural também dispara o fluxo
(`utf-rules.md` §1) — os comandos são só o caminho mais curto.

---

## Quando algo dá errado

- **Estourou as 2 rodadas de revisão:** o ciclo para sozinho e te chama, com os
  pareceres no disco. Quase sempre a causa é spec ambígua, tarefa grande demais
  ou dependência não declarada. Corrija a spec e **abra uma sessão nova**
  entregando só a spec e o plano — o contexto da conversa velha está sujo.
- **Descobriu um problema novo no meio:** não inche a spec. Registre como
  comentário na Issue e abra uma Issue nova. O escopo do PR é o escopo da spec.
- **Bug ou tarefa técnica (sem história):** não passa por aqui — Issue direto no
  GitHub, PR com a etiqueta `manutencao`, sem `spec.md`.

Os detalhes desses desvios estão no [guia](./guia-sdd.md), seção *"Quando o
ciclo não é linear"*.
