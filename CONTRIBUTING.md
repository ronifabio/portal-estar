# Guia de Contribuição e Método de Trabalho (SDD)

Bem-vindo ao repositório! Este documento explica como trabalhamos aqui. Leia antes de iniciar sua primeira entrega e volte a ele sempre que tiver dúvidas sobre "como eu deveria estar fazendo isso".

## 🥇 A Regra de Ouro

Pedir código para uma Inteligência Artificial é fácil. O problema aparece semanas depois, quando ninguém lembra o porquê de uma regra existir e alterar o sistema vira um caos. Para resolver isso, adotamos o **Spec-Driven Development (SDD)**.

A regra número um deste repositório é:

> **A IA escreve o código. Você continua responsável por cada decisão.**

Você é o Engenheiro e o Arquiteto; a IA é a sua equipe de execução.

---

## 🌳 Fluxo Git e Proteção da Produção (GitHub Flow)

- **A branch `main` é sagrada:** Ela reflete a produção e possui bloqueio de commits diretos.
- **Trabalho:** Crie uma branch curta **a partir da `main`** para cada Issue (feature branch).
- **Integração:** Ao finalizar, abra um Pull Request contra a `main` com `Closes #<n>`. O CI (Jest + lint, contra um PostgreSQL em container) precisa passar antes do merge.

---

## 🗺️ Onde as coisas vivem (Artefatos)

Nada é duplicado neste projeto. Informação repetida diverge.

| Artefato          | Onde fica               | O que responde                                                         |
| :---------------- | :---------------------- | :--------------------------------------------------------------------- |
| **Vitrine**       | `README.md` na raiz     | O que é o projeto e como rodar.                                        |
| **Produto**       | `docs/prd.md`           | O que o sistema faz (Glossário, Atores, Histórias).                    |
| **Arquitetura**   | `docs/architecture.md`  | Onde as coisas estão (estrutura, entidades, contratos).                |
| **Jornadas**      | `docs/user-flows.md`    | O caminho do usuário e onde ele desiste.                               |
| **Especificação** | `specs/<issue>-<slug>/` | O `spec.md` (o que fazer), o `plan.md` (tarefas técnicas) e `reviews/` (pareceres e triagem). |
| **Leis da IA**    | `.agents/`              | `rules/utf-rules.md` (constituição, carregada via `CLAUDE.md`), `workflows/` (ciclos) e `agents/` (prompts dos subagentes). |

---

## 🔄 O Ciclo de Trabalho

Todo trabalho que altera o comportamento do sistema segue o ciclo SDD, orquestrado pelos agentes do repositório: `/utf-issue <n>` inicia (spec → plano), `/utf-task <n>` executa cada tarefa com tutor, implementador de contexto limpo e dois revisores distintos, e o auditor final confere o diff inteiro antes do PR.

- **O passo a passo operacional** (comandos e o que fazer em cada pausa) está no [Tutorial do Método](./docs/tutorial-sdd.md).
- **O porquê de cada regra** está no [Guia da Disciplina](./docs/guia-sdd.md).

O que é **norma inegociável** deste repositório são os três portões humanos — nenhum agente passa por eles em seu lugar:

1. **🚪 Spec:** só você aprova, trocando `status: rascunho` → `status: aprovada` num commit seu.
2. **🚪 Triagem:** só você aceita ou recusa apontamentos de revisão (recusa exige justificativa, registrada em `specs/<issue>-<slug>/reviews/tarefa-NN-decisoes-rN.md`).
3. **🚪 Pull Request:** só você escreve a explicação, com as suas palavras, listando os apontamentos aceitos e recusados.

---

## 🛑 O Portão de Entendimento (Regras de Pull Request)

Se o Pull Request for a primeira vez que você olha o código, o método falhou. Todo PR que altera código em `apps/` passa por uma verificação rígida antes de ser mesclado.

**O PR será REPROVADO se:**

1. Não atualizar nenhum arquivo em `docs/` ou `specs/`.
2. A descrição não contiver a seção _"O que este PR faz e por quê"_ preenchida por você com pelo menos 200 caracteres (Não cole o _diff_ nem a saída da IA; explique com suas palavras).

**Exceção (Manutenção puramente técnica):**
Se a mudança não afeta o produto (ex: atualizar versão, refatorar código, arrumar formatação), você não precisa criar um `spec.md`. Abra o PR direto e aplique a etiqueta `manutencao`.

---

## ⚠️ Erros Comuns e Como Evitá-los

- **Inchar a Spec no meio do caminho:** Se descobrir um problema novo durante a implementação, não remende o plano em andamento. Pause, registre o problema e abra uma nova Issue focada apenas naquilo.
- **Fatiar na Horizontal:** "Criar tabela de usuários" não é uma história demonstrável. Fatie por valor: "Fazer login de usuário".
- **A Janela de Contexto Suja:** Se o agente travar, errar repetidamente ou defender ideias descartadas, descarte a sessão do chat. Comece uma nova entregando apenas o `spec.md` e o `plan.md` aprovados. O contexto limpo resolve 90% dos problemas.
- **Aceitar todos os apontamentos do revisor:** Revela que você não leu. Recusar com justificativa registrada vale mais do que aceitar tudo — e é o que a defesa presencial cobra.
