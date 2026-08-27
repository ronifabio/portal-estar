# 🎓 UTF SDD Template

Template da disciplina de **Tópicos Especiais** (UTFPR). Ele traz o
método completo — comandos, agentes, workflows e guias — para você desenvolver
seu projeto com **Spec-Driven Development**: a IA escreve o código; você decide
nos portões.

## 🚀 Como começar

1. Clique em **Use this template → Create a new repository** (não faça fork).
   O repositório criado é seu.
2. Clone o seu repositório e abra-o na sua IDE agêntica (Claude Code, Cursor…).
3. Leia `docs/checklist.md` — é a **ficha da disciplina**: as regras do projeto,
   os Indicadores de Desempenho (IDs) e as entregas.
4. Siga o fluxo, um comando por fase:

| Fase | Comando | Produz |
| --- | --- | --- |
| Requisitos | `/utf-prd` | `docs/prd.md` — o QUE o produto faz |
| Arquitetura | `/utf-architecture` | `docs/architecture.md` — onde as coisas moram |
| Scaffold | `/utf-setup` | `apps/` — o monorepo, nascendo verde |
| Cada história | `/utf-issue <n>` → `/utf-task` | spec, plano e código, tarefa a tarefa |
| Aprender | `/utf-tutor` | a explicação didática de cada passo |

O passo a passo detalhado está em [`docs/tutorial-sdd.md`](docs/tutorial-sdd.md);
o porquê de cada regra, em [`docs/guia-sdd.md`](docs/guia-sdd.md).

Depois do `/utf-prd`, crie o Kanban no **GitHub Projects** e mapeie as user
stories em Issues (ID3) — a descrição da Issue só linka o PRD, nunca copia
regra de negócio.

---

> ✂️ **Daqui para baixo é a vitrine do SEU projeto.** Apague tudo acima desta
> linha (incluindo ela) quando o projeto tiver nome, e preencha o que segue.

# [Nome do projeto]

[Uma frase: o problema que resolve e para quem.]

## Autores

- [Nome — GitHub]

## Stack

[Preenchida a partir do `docs/architecture.md` — backend, frontend, banco.]

## Em produção

- **Aplicação:** [URL]
- **API (Swagger):** [URL/docs]

## Quick Start

[Como rodar localmente: pré-requisitos, variáveis de ambiente, comandos —
gerado/refinado no `/utf-setup`.]
