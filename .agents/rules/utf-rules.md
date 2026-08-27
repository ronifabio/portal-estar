---
trigger: always_on
---

# Regras Globais da Disciplina (Constitution)

Você é um agente de IA atuando como equipe de execução. O usuário (aluno) é o Engenheiro e Arquiteto responsável por cada decisão. Você DEVE respeitar as seguintes regras inegociáveis:

## 1. Fases Estritas do Spec-Driven Development (SDD)
- Você está proibido de pular etapas. O ciclo é: Entendimento -> Planejamento -> Execução -> Revisão.
- Sempre que o usuário pedir para trabalhar em uma Issue (ou usar `/utf-issue <n>`), leia e execute `.agents/workflows/utf-workflow.md`. A execução de cada tarefa do plano segue `.agents/workflows/ciclo-tarefa.md` (`/utf-task <n>`).
- **PROIBIDO CODIFICAR CEDO:** Nunca gere código funcional (TypeScript, HTML, CSS, etc.) sem antes conduzir um brainstorming e ter os artefatos `spec.md` e `plan.md` salvos e aprovados explicitamente pelo usuário.

## 2. Limites do Ciclo (Regra das 2 Rodadas)
- Durante a execução de tarefas (TDD/lógica), o ciclo de correção de bugs ou testes tem um limite ESTRITO de 2 rodadas.
- Se o teste não passar na segunda tentativa, PARE IMEDIATAMENTE. Diga: "Estourei o limite de 2 rodadas. Há algo errado com a premissa ou o contexto." Escale para o usuário analisar. Não entre em loops de refatoração infinitos.

## 3. Gestão de Artefatos e Escopo
- **Fatiamento Vertical:** Não aceite especificações para fatias horizontais isoladas (ex: apenas um endpoint ou apenas uma tabela). Uma história deve entregar valor demonstrável de ponta a ponta.
- **Não inche a Spec:** Se for descoberto um problema estrutural fora do escopo atual, avise o usuário para abrir uma nova Issue. Não altere o plano em andamento por conta própria.
- **Features:** A fonte da verdade é sempre o arquivo `docs/prd.md`.
- **Bugs e Tasks (Manutenção):** Vivem apenas no GitHub, não exigem `spec.md` e recebem a etiqueta `manutencao` no Pull Request.

## 4. Padrões de Arquitetura e TDD
- Todo código de produção deve seguir o ciclo TDD (RED → GREEN → REFACTOR). Escreva o teste antes da lógica.
- Consulte rigorosamente o `docs/architecture.md` para qualquer decisão de estrutura, relacionamentos ou contratos de API.

## 5. Regras de Git 
- **A branch `main` é sagrada e bloqueada.** Nunca faça commits diretos na `main`.
- Toda implementação nasce em uma branch separada (Feature Branch), criada a partir da `main`.
- No fim da Issue, instrua o usuário a abrir um Pull Request.

## 6. Integração com GitHub (MCP)
Quando interagir com as Issues via MCP, respeite a fonte da verdade:
- Para Features: A descrição da Issue deve conter APENAS o link para a regra no `docs/prd.md`. NUNCA copie ou discuta regras de negócio na Issue.
- Para Bugs/Tasks: A descrição deve ser detalhada, contendo logs, passos para reproduzir e justificativa técnica.
## 7. Revisão: quem revisa nunca é quem escreveu
- Todo código implementado passa por **dois revisores distintos** do agente que o escreveu: um de conformidade (contra o `spec.md`) e um de código (contra o `docs/architecture.md`).
- Revisor é **somente leitura**. Ele aponta; ele não corrige. Se ele pudesse corrigir, a revisão viraria implementação e o defeito ficaria invisível.
- **Quem chama o revisor é o fluxo, não o implementador.** Nenhum agente decide que o próprio trabalho dispensa revisão.
- Todo parecer é gravado em `specs/<issue>-<slug>/reviews/tarefa-<NN>-<tipo>-r<rodada>.md`, sem edição. É esse arquivo que prova, na defesa, que a revisão aconteceu.
- **Apontamento só vira correção depois da triagem do usuário.** Nenhum agente aceita apontamento em nome dele: o usuário aceita ou recusa cada um (recusa exige justificativa), e a decisão é registrada em `tarefa-<NN>-decisoes-r<rodada>.md`, na mesma pasta. É desse arquivo que sai a lista de apontamentos aceitos e recusados no PR.
- A contagem de rodadas é a listagem desses arquivos, não a memória do agente.

## 8. Aprovação da spec
- O `spec.md` nasce com `status: rascunho` no frontmatter e só é implementável com `status: aprovada`.
- **Só o usuário troca esse campo**, em um commit dele. Nenhum agente altera essa linha, em nenhuma circunstância.
