---
description: Gera a estrutura inicial do monorepo a partir do docs/architecture.md — apps via geradores oficiais, scripts da raiz, template de PR, Portão de Entendimento e índice de specs. Roda UMA vez, antes da primeira Issue. É uma Task de manutenção — sem spec.
---

# Setup do monorepo

Você gera a fundação sobre a qual o ciclo SDD vai rodar. Isso é uma **Task técnica**,
não uma história: não tem `spec.md`, e o PR dela recebe a etiqueta `manutencao`.

A fonte da verdade é o `docs/architecture.md`. Você **não decide stack** — você lê a
que foi decidida. O que não estiver escrito lá, você pergunta; não escolhe.

---

## Passo 0 — Pré-condições (PARE se qualquer uma falhar)

1. `docs/prd.md` e `docs/architecture.md` existem e declaram: o framework do backend,
   o framework do frontend, a estrutura de pastas do monorepo e como rodar os testes.
   Se algum desses quatro estiver ausente ou ambíguo, **PARE** e diga o que falta —
   setup com stack adivinhada é retrabalho garantido.
2. As pastas de app previstas no `architecture.md` (ex.: `apps/web`, `apps/api`)
   **não existem** ou estão vazias. Se já existirem com conteúdo, **PARE**: o setup
   roda uma vez, e rodá-lo de novo por cima é destrutivo.
3. Você está na `main`, limpa e atualizada.

## Passo 1 — Branch

```
git switch -c setup-monorepo
```

Nenhum arquivo é criado antes da branch existir. A `main` é bloqueada.

## Passo 2 — Os apps, pelos geradores oficiais

Gere cada app com o gerador oficial da stack declarada no `architecture.md`
(ex.: `@nestjs/cli` para NestJS, `ng new` para Angular, `create-vite` para React/Vue),
dentro da estrutura de pastas que o documento descreve.

- Desative o `git init` interno do gerador — o repositório é um só, na raiz.
- Aceite os padrões do gerador. Não adicione biblioteca que o `architecture.md`
  não menciona.
- Se o `architecture.md` declara ferramenta de teste diferente do padrão do gerador,
  siga o documento; se não declara, fique com o padrão do gerador e **relate isso no
  fim** como decisão que o usuário precisa ratificar no `architecture.md`.

## Passo 3 — A raiz

1. `package.json` da raiz com os scripts de orquestração descritos no
   `architecture.md` (ex.: `start`, `api`, `test`). Se o documento traz os scripts
   prontos, copie-os literalmente.
2. `.gitattributes` com:

   ```
   * text=auto eol=lf
   ```

   Sem isso, um repositório tocado em Windows e Linux reescreve todos os arquivos a
   cada troca de máquina, e o diff de qualquer PR vira ruído.

3. `.gitignore` da raiz cobrindo `node_modules`, artefatos de build e `.env`.

## Passo 4 — As ferramentas do método

1. `.github/pull_request_template.md` — o modelo com a seção
   **"O que este PR faz e por quê"** e a seção de apontamentos aceitos/recusados,
   conforme o Apêndice B do guia.
2. `.github/workflows/portao-de-entendimento.yml` — o Portão de Entendimento,
   copiado do Apêndice B do guia, sem alterações.
3. `specs/README.md` — o índice de specs, com a tabela vazia:

   ```markdown
   # Índice de specs

   | Issue | Spec | Estado | Observação |
   | --- | --- | --- | --- |
   ```

## Passo 5 — Prova de vida

Rode a suíte de testes de **cada** app e o lint, com os comandos da raiz.

O scaffold precisa nascer **verde**. É esse verde que dá sentido ao RED do TDD a
partir da primeira tarefa: um teste que falha só é informação num repositório onde
os testes comprovadamente rodam.

**Regra das 2 rodadas:** se um gerador ou a suíte falhar duas vezes pelo mesmo
motivo, **PARE** e relate. Não tente uma terceira abordagem.

## Passo 6 — Entrega

1. Commits pequenos e nomeados por passo (apps, raiz, ferramentas do método).
2. Relate ao usuário: o que foi gerado, a saída dos testes, e as decisões que o
   `architecture.md` não cobria (Passo 2) para ele ratificar no documento.
3. Instrua o usuário a abrir o PR com a etiqueta **`manutencao`** — setup é Task,
   não história.

---

## Proibições

- **Nenhuma entidade, endpoint, tela, modelo ou migration de negócio.** Se um nome
  do glossário do PRD aparecer em código gerado por você, você passou do ponto — o
  negócio começa na primeira Issue, pelo `/utf-issue`, com spec aprovada.
- Nada de CI além do Portão de Entendimento — esteira de testes é Issue própria.
- Não editar `docs/prd.md` nem `docs/architecture.md`. Se encontrar contradição
  entre eles, relate; não resolva por conta própria.
