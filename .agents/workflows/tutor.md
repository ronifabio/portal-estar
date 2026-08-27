---
description: Despacha o agente tutor no modo certo — explicar uma tarefa já feita (número), as decisões da spec (spec) ou conduzir o simulado pré-PR (prova). O modo "antes" é despachado automaticamente pelo ciclo-tarefa.
---

# Fluxo do tutor

Você é o orquestrador. Você não explica com o seu próprio contexto — **quem explica é o subagente tutor**, que vê só a spec, o diff e o `architecture.md`. A sua conversa está cheia de tentativas e correções; a dele, não. É por isso que a explicação vem dele.

Argumento recebido: **$1**

## Decidir o modo

| `$1` | Modo do tutor |
| --- | --- |
| um número (ex.: `3`) | `depois` — explica a tarefa 3, já implementada e aprovada |
| `antes <n>` | `antes` — explica a tarefa antes da implementação (normalmente quem chama isso é o ciclo-tarefa) |
| `spec` | `spec` — explica as consequências técnicas da spec em rascunho |
| `prova` | `prova` — simulado interativo sobre o diff inteiro da branch |

Sem argumento, pergunte ao usuário qual modo ele quer.

## Preparar o despacho

1. Descubra a pasta `specs/<issue>-<slug>/` da branch atual.
2. Todo despacho leva os caminhos completos de `spec.md`, `plan.md` e `docs/architecture.md`, além do **modo**.
3. Conforme o modo, acrescente:
   - **`depois`**: número e texto literal da tarefa, e o comando de diff. Encontre o commit da tarefa com `git log --oneline --grep "tarefa <n>"` (convenção de commit do ciclo-tarefa) e monte `git diff <sha>^..<sha>`. Se a tarefa tiver mais de um commit ou o commit não for encontrado, monte o intervalo à mão e confirme com o usuário antes de despachar.
   - **`antes`**: número e texto literal da tarefa, e os critérios de aceite ligados a ela, transcritos.
   - **`prova`**: o comando do diff completo da branch: `git diff main..HEAD`.

## Entregar

- **Modos `antes`, `depois` e `spec`:** apresente a resposta do tutor ao usuário **na íntegra, sem resumir** — resumo seu é exatamente a contaminação que o tutor existe para evitar. Depois, coloque-se à disposição para dúvidas: repasse cada dúvida ao tutor em novo despacho se ela exigir olhar o código de novo.
- **Modo `prova`:** o tutor te devolve as questões **com gabarito**. O gabarito é seu, não do aluno:
  1. Faça **uma pergunta por vez** e espere a resposta do usuário.
  2. Compare com o gabarito e dê o retorno de forma didática: o que acertou, o que faltou, onde está a evidência (`arquivo:linha`).
  3. Ao final, resuma os pontos fracos e diga **quais arquivos reler** antes do PR.
  4. Não mostre o gabarito de perguntas ainda não respondidas.

## Proibições

- Explicar você mesmo, sem despachar o tutor ("adiantar" a explicação é pular o agente de contexto limpo).
- Deixar o tutor escrever ou alterar qualquer arquivo.
- Usar o tutor para gerar texto de PR ou respostas prontas para o aluno colar — ele ensina, não redige entregáveis.
