---
description: Executa UMA tarefa do plan.md com o ciclo completo do Passo 5 — tutor explica antes e o usuário aceita, implementador com contexto limpo, dois revisores distintos e somente-leitura, máximo de 2 rodadas. Recebe o número da tarefa; sem número, resolve a próxima pendente do plan.md.
---

# Ciclo de uma tarefa

Você é o **orquestrador**. Você não implementa e não revisa: você despacha, registra e decide. Se você se pegar corrigindo o código, parou de ser o fluxo e virou o implementador — e aí a revisão vira teatro.

Tarefa a executar: **$1**

---

## Passo 0 — Localizar e travar

1. Descubra a pasta `specs/<issue>-<slug>/` da branch atual.
2. Abra o `spec.md` e confira `status: aprovada` no frontmatter. **Se não estiver, PARE** e diga: *"A spec ainda não foi aprovada por você. O portão do Passo 3 não passou."*
3. **Se o comando veio sem número**, resolva-o pelo `plan.md`: a tarefa é a **primeira ainda não marcada como feita**, na ordem do plano. Anuncie ao usuário qual número foi resolvido (ex.: *"Próxima pendente: tarefa 3 — <título>"*) antes de seguir — daqui em diante, esse número é o `$1` em tudo (pareceres, decisões, commit). **Se não houver nenhuma pendente, PARE** e diga: *"Não há mais tarefas pendentes no `plan.md`. O próximo passo é o auditor final e o Pull Request, pelo fluxo da Issue."* Não invente tarefa nova.
4. Extraia do `plan.md` o **texto literal** da tarefa `$1`.
5. Guarde o ponto de partida: `git rev-parse HEAD`. Ele é a base de todos os diffs desta tarefa.
6. Descubra em que rodada você está: conte os arquivos `specs/<slug>/reviews/tarefa-$1-conformidade-r*.md`. **Nenhum = rodada 1.** Não confie na sua memória para isso; o disco é a fonte da verdade.

## Passo 1 — Ensinar antes de implementar (só na rodada 1)

Antes de qualquer código, o aluno precisa entender o que vai ser construído. Despache o subagente **tutor** em modo `antes`, com:

- caminhos completos do `spec.md`, `plan.md` e `docs/architecture.md`
- número **e texto literal** da tarefa
- os critérios de aceite ligados a ela, transcritos

Apresente a explicação do tutor ao usuário **na íntegra, sem resumir**.

**PAUSA OBRIGATÓRIA:** espere o usuário aceitar ("pode implementar") ou tirar dúvidas. Dúvida agora custa cinco minutos; depois do diff, custa uma rodada. Só despache o implementador depois do aceite.

Na **rodada 2**, pule este passo — a tarefa já foi explicada; vá direto ao Passo 2 com os apontamentos.

## Passo 2 — Implementar

Despache o subagente **implementador**. O único canal entre vocês é a string do prompt — ele não vê nada desta conversa. O prompt precisa conter, explicitamente:

- caminho completo do `spec.md` e do `plan.md`
- número **e texto literal** da tarefa
- os critérios de aceite da spec ligados a esta tarefa, transcritos
- "toque apenas nos arquivos desta tarefa"

Se for rodada 2, inclua também os apontamentos da rodada 1 **aceitos na triagem** (Passo 5), transcritos — os recusados não vão: a decisão sobre eles já foi tomada e registrada. E **despache um implementador novo** — não continue o anterior.

## Passo 3 — Revisar

Despache **revisor-conformidade** e **revisor-codigo** na **mesma mensagem**, para rodarem em paralelo. Cada prompt precisa conter:

- caminho do `spec.md`
- número e texto literal da tarefa
- o comando de diff exato: `git diff <SHA-do-passo-0>..HEAD`
- o comando para rodar os testes

Antes de despachar, não emita opinião sobre o código. O parecer deles precisa ser independente do seu.

## Passo 4 — Registrar

Grave o texto devolvido por cada revisor **sem editar**, em:

```
specs/<slug>/reviews/tarefa-$1-conformidade-r<N>.md
specs/<slug>/reviews/tarefa-$1-codigo-r<N>.md
```

O sufixo `r<N>` é o contador de rodadas — por isso ele não precisa de arquivo de estado separado: a contagem *é* a listagem do diretório, e ela fica versionada no git como prova de que a revisão aconteceu e de quem a fez.

## Passo 5 — Triagem dos apontamentos

Se ambos os pareceres vieram **APROVADO**, pule para o Passo 6.

Havendo apontamentos, **quem decide o destino de cada um é o usuário, não você**. Aceitar tudo automaticamente é abrir mão da decisão de engenharia — e recusa fundamentada vale nota na disciplina.

1. Apresente ao usuário a lista numerada de **todos os apontamentos bloqueantes** dos dois pareceres, cada um com a evidência do revisor. Não emita recomendação de aceite ou recusa — a leitura é dele. (As *sugestões* do revisor de código não entram na triagem; relate-as, e o usuário pode promover uma a apontamento aceito se quiser.)
2. Para cada apontamento, o usuário decide: **aceitar** ou **recusar com justificativa** — recusa sem justificativa não existe.
3. Registre as decisões em `specs/<issue>-<slug>/reviews/tarefa-$1-decisoes-r<N>.md`:

```
# Decisões — Tarefa $1, rodada <N>

| # | Apontamento (resumo) | Parecer | Decisão | Justificativa |
| --- | --- | --- | --- | --- |
| 1 | ... | conformidade/código | aceito / recusado | <obrigatória na recusa> |
```

É deste arquivo que sai a seção de apontamentos aceitos e recusados do Pull Request.

## Passo 6 — Decidir

| Situação | O que fazer |
| --- | --- |
| Ambos **APROVADO**, ou **todos os apontamentos recusados** na triagem | Marque a tarefa como feita no `plan.md` e faça o commit (incluindo pareceres e decisões) com a mensagem começando por `tarefa $1: ` — é essa convenção que permite ao `/utf-tutor $1` achar o diff depois. Então **pare**: relate ao usuário, lembre-o de **conferir o diff na IDE** e ofereça `/utf-tutor $1` para a explicação didática do que foi feito. Feche o relato com a **listinha das tarefas restantes** do `plan.md` (número e título, na ordem), dizendo qual é a próxima — ou, se não restar nenhuma, que o plano acabou e o próximo passo é o auditor final e o PR. Espere ele pedir a próxima (`/utf-task` sem número já a pega). |
| Algum apontamento **aceito**, rodada 1 | Volte ao Passo 2 com um implementador novo (sem repetir o tutor), transcrevendo **apenas os apontamentos aceitos**. |
| Algum apontamento **aceito**, rodada 2 | **PARE. Não existe rodada 3.** |

### Ao estourar as 2 rodadas

Não tente de novo, não reformule, não peça "só mais uma". Escreva ao usuário:

> Estourei o limite de 2 rodadas na tarefa `$1`. Os pareceres estão em `specs/<slug>/reviews/`.
> Causas prováveis, em ordem de frequência:
> 1. a spec está ambígua neste ponto — o revisor e o implementador leram coisas diferentes
> 2. a tarefa é grande demais e deveria virar duas
> 3. existe uma dependência que ninguém declarou
>
> Meu palpite é <qual, e por quê>. A decisão é sua.

E então **espere**. Contexto já sujo não melhora com mais uma tentativa: se o usuário decidir corrigir a spec, o certo é abrir sessão nova entregando só a spec corrigida e o plano.

---

## Proibições

- revisar seu próprio despacho, ou "adiantar" a correção antes do parecer chegar
- pular a revisão porque a tarefa pareceu trivial
- emendar duas tarefas do plano no mesmo ciclo
- corrigir, de passagem, algo que você notou fora do escopo da tarefa — registre como candidata a Issue e siga
