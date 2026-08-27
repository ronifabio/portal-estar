# 📄 Product Requirements Document (PRD)

**Projeto:** Portal EstaR
**Versão:** 1.0.0
**Última atualização:** 2026-08-27

> 🤖 **Este documento é a fonte da verdade sobre o QUE o produto faz.** Regra de
> negócio que não estiver aqui não existe — nem para a equipe, nem para a IA.
> Tecnologia **não** se discute aqui: isso é assunto do `architecture.md`.

---

## 🎯 1. Visão Geral e Objetivo

**O problema:** quem viaja de carro entre cidades precisa descobrir, a cada
cidade, se existe app de estacionamento rotativo (EstaR) ali e qual é — e ainda
gerenciar saldo espalhado em vários apps diferentes.

**A solução:** um portal que, pela localização da pessoa, lista os apps de
estacionamento que atendem aquela cidade, destaca os que ela já tem instalados
(como uma carteira de apps) e permite colocar saldo no portal para usar nos
apps de qualquer cidade.

**Como saberemos que deu certo:** ao chegar numa cidade, a pessoa descobre
facilmente pelo portal qual é o app de estacionamento dali e consegue usar o
saldo que carregou no portal para pagar naquele app.

---

## 📖 2. Glossário Ubíquo

> Os termos do negócio, como o cliente fala. É daqui que o `architecture.md`
> deriva os nomes das entidades.

| Termo                     | Significa                                                                             | Não confundir com                                                             |
| :------------------------ | :------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------- |
| **Portal**                | O produto: agregador de apps de estacionamento + carteira de saldo.                   | Os apps de estacionamento das cidades — o Portal não vende EstaR diretamente. |
| **EstaR**                 | O estacionamento rotativo regulamentado das cidades (zona azul).                      | O Portal ou os apps — EstaR é o serviço público, não o software.              |
| **App de estacionamento** | Aplicativo credenciado que vende EstaR em uma ou mais cidades.                        | O Portal — o app é de terceiros, o Portal só o lista e conecta.               |
| **Cidade**                | Município atendido por um ou mais apps de estacionamento.                             | —                                                                             |
| **Carteira de apps**      | Os apps que a pessoa já tem instalados no celular, exibidos em destaque no Portal.    | Saldo — a carteira de apps não guarda dinheiro.                               |
| **Saldo**                 | Crédito em dinheiro que a pessoa carrega no Portal e usa nos apps de qualquer cidade. | Crédito de EstaR dentro de um app específico.                                 |
| **Recarga**               | A compra que adiciona saldo ao Portal (a venda avulsa, com pedido e pagamento).       | Uso do saldo — recarga é entrada de dinheiro, não gasto.                      |

---

## 👤 3. Atores e Permissões

> ⚠️ A coluna **"Não pode"** vira Guard e controle de role na API.

| Ator              | Quem é                      | Pode                                                                                                               | Não pode                                                                  |
| :---------------- | :-------------------------- | :----------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------ |
| **Visitante**     | Pessoa sem login            | Ver os apps de estacionamento da cidade em que está (pela localização ou buscando a cidade pelo nome); criar conta | Mexer com crédito: recarregar, ver ou usar saldo; ter carteira de apps    |
| **Usuário**       | Viajante com conta e logado | Tudo do visitante; recarregar saldo; ver saldo e extrato; usar saldo nos apps; manter sua carteira de apps         | Gerenciar o catálogo de cidades e apps; mexer no saldo de outros usuários |
| **Administrador** | Quem mantém o portal        | Cadastrar e manter cidades e apps de estacionamento (o catálogo)                                                   | Alterar ou usar o saldo dos usuários                                      |

---

## 📝 4. Escopo Funcional (User Stories)

> Toda story nasce `Draft` — **só o aluno promove a `Ready`**, quando as regras
> estiverem definidas.

### US01 — Descobrir os apps da cidade · `M` · Status: `Ready`

**Como** visitante, **eu quero** ver os apps de estacionamento da cidade em que
estou, pela minha localização, **para que** eu descubra facilmente como pagar
EstaR ali sem pesquisar fora do portal.

**Critérios de aceite:**

- [ ] **Dado** que estou numa cidade atendida, **quando** permito o uso da minha localização, **então** vejo a lista de apps de estacionamento daquela cidade.
- [ ] **Dado** que estou numa cidade **sem** app cadastrado, **quando** consulto pela localização, **então** vejo um aviso claro de que a cidade não tem app conhecido.
- [ ] **Dado** que nego ou falha o acesso à localização, **quando** abro o portal, **então** posso buscar a cidade pelo nome e ver a mesma lista.

**Regras relacionadas:** RN06, RN07

### US02 — Criar conta e entrar · `M` · Status: `Ready`

**Como** visitante, **eu quero** criar uma conta com nome, e-mail e senha e
entrar no portal **para que** eu possa ter saldo e carteira de apps.

**Critérios de aceite:**

- [ ] **Dado** que informo nome, e-mail e senha válidos, **quando** confirmo o cadastro, **então** minha conta é criada e consigo entrar.
- [ ] **Dado** que informo um e-mail já cadastrado, **quando** tento me cadastrar, **então** vejo um erro claro e nada é criado.
- [ ] **Dado** que erro e-mail ou senha, **quando** tento entrar, **então** vejo um erro claro e não entro.

**Regras relacionadas:** RN02

### US03 — Manter minha carteira de apps · `S` · Status: `Ready`

**Como** usuário, **eu quero** marcar quais apps de estacionamento eu já tenho
instalados **para que** eles apareçam em destaque quando eu consultar uma
cidade.

**Critérios de aceite:**

- [ ] **Dado** que marquei um app como instalado, **quando** vejo a lista de uma cidade atendida por ele, **então** ele aparece em destaque no topo.
- [ ] **Dado** que não marquei nenhum app, **quando** vejo a lista de uma cidade, **então** vejo a lista normal, sem destaques.
- [ ] **Dado** que desmarco um app, **quando** consulto a cidade novamente, **então** ele volta a aparecer sem destaque.

**Regras relacionadas:** RN01, RN07

### US04 — Recarregar saldo · `L` · Status: `Ready`

**Como** usuário, **eu quero** escolher um valor e pagar uma recarga **para
que** meu saldo no portal aumente.

**Critérios de aceite:**

- [ ] **Dado** que escolho um valor válido (bloco pré-definido ou valor livre dentro da faixa), **quando** confirmo a recarga, **então** um pedido é criado e sou levado ao pagamento no ambiente de testes do gateway.
- [ ] **Dado** um pedido pago, **quando** o gateway confirma o pagamento, **então** o pedido muda para pago e o valor entra no meu saldo.
- [ ] **Dado** que o pagamento é recusado, **quando** o gateway responde, **então** o pedido fica como recusado e meu saldo não muda.
- [ ] **Dado** que abandono o pagamento no meio, **quando** volto ao portal, **então** o pedido consta como pendente e meu saldo não muda.
- [ ] **Dado** que informo um valor livre fora da faixa permitida, **quando** tento recarregar, **então** vejo um erro claro e nenhum pedido é criado.

**Regras relacionadas:** RN01, RN03, RN05, RN08

### US05 — Ver saldo e extrato · `S` · Status: `Ready`

**Como** usuário, **eu quero** ver meu saldo atual e o histórico de recargas e
usos **para que** eu saiba quanto tenho e para onde meu dinheiro foi.

**Critérios de aceite:**

- [ ] **Dado** que tenho movimentações, **quando** abro meu extrato, **então** vejo saldo atual e a lista de recargas e usos, do mais recente ao mais antigo.
- [ ] **Dado** que nunca movimentei, **quando** abro meu extrato, **então** vejo saldo zero e um aviso de extrato vazio.

**Regras relacionadas:** RN01, RN03, RN04, RN05

### US06 — Usar saldo em um app · `M` · Status: `Draft`

**Como** usuário, **eu quero** converter parte do meu saldo em crédito num app
de estacionamento que eu escolher **para que** eu pague EstaR naquela cidade
sem recarregar app por app.

**Critérios de aceite:**

- [ ] **Dado** que tenho saldo suficiente, **quando** uso um valor num app da cidade, **então** meu saldo diminui desse valor e o uso aparece no extrato vinculado àquele app.
- [ ] **Dado** que meu saldo é insuficiente, **quando** tento usar, **então** vejo um erro claro e nada é debitado.

**Regras relacionadas:** RN01, RN04, RN05

### US07 — Manter o catálogo de cidades e apps · `M` · Status: `Ready`

**Como** administrador, **eu quero** cadastrar cidades e apps e definir quais
apps atendem cada cidade **para que** os visitantes encontrem informação
correta.

**Critérios de aceite:**

- [ ] **Dado** uma cidade nova, **quando** a cadastro e associo apps a ela, **então** ela passa a aparecer nas consultas com esses apps.
- [ ] **Dado** que tento cadastrar cidade ou app duplicado, **quando** salvo, **então** vejo um erro claro e nada é duplicado.
- [ ] **Dado** um usuário sem papel de administrador, **quando** tenta acessar o catálogo, **então** o acesso é negado.

**Regras relacionadas:** RN06

---

## 🛡️ 5. Regras de Negócio (Constraints)

| ID   | Regra                                                                                                                                                                                            |
| :--- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RN01 | Sem login não há crédito: recarga, saldo, extrato, uso e carteira de apps exigem usuário autenticado.                                                                                            |
| RN02 | E-mail é único: não existem duas contas com o mesmo e-mail.                                                                                                                                      |
| RN03 | Saldo só aumenta após a confirmação do pagamento pelo gateway; pedido pendente ou recusado não altera saldo.                                                                                     |
| RN04 | Saldo nunca fica negativo: uso maior que o saldo disponível é rejeitado.                                                                                                                         |
| RN05 | Toda movimentação (recarga confirmada ou uso) gera registro no extrato, com data, valor e, no caso de uso, o app de destino.                                                                     |
| RN06 | Cidades e apps são únicos no catálogo, e só o administrador o mantém.                                                                                                                            |
| RN07 | Na lista de uma cidade, os apps marcados na carteira do usuário aparecem em destaque, antes dos demais.                                                                                          |
| RN08 | A recarga oferece valores pré-definidos em blocos (R$ 10, R$ 20, R$ 50, R$ 100) e um campo de valor livre. Valor mínimo por recarga: R$ 5; máximo: R$ 500. Valores fora da faixa são rejeitados. |

---

## 🚫 6. Fora de Escopo (Non-goals)

> O que o produto deliberadamente **não** faz neste semestre.

- Integração real com os apps de terceiros: o uso do saldo é um registro no portal, o crédito não é transferido de fato para o app.
- Pagamento direto do EstaR pelo portal (ativar estacionamento, escolher vaga, emitir CAD): quem faz isso é o app da cidade.
- Detecção automática dos apps instalados no celular: o usuário marca manualmente sua carteira.
- Estorno, saque ou transferência de saldo entre usuários.
- Assinatura recorrente: o pagamento é só venda avulsa (recarga).

---

## ⚙️ 7. Requisitos Não Funcionais (Qualidade)

- **Segurança de credenciais e do crédito** — senhas nunca guardadas em texto puro; ações de crédito só do próprio dono da conta. _Justificativa: o portal guarda dinheiro do usuário._
- **Consistência do saldo** — o saldo reflete exatamente as movimentações confirmadas, mesmo com a confirmação do pagamento chegando de forma assíncrona; nenhuma recarga é creditada duas vezes. _Justificativa: erro de saldo destrói a confiança numa carteira._
- **Privacidade da localização** — a localização é usada só para descobrir a cidade da consulta, não é armazenada nem rastreada. _Justificativa: dado sensível que o produto não precisa reter._
- **Clareza nos erros** — toda falha (pagamento recusado, saldo insuficiente, cidade sem app) mostra mensagem compreensível, nunca erro técnico cru. _Justificativa: o público é viajante leigo, no celular, na rua._

---

## ❓ Dúvidas em aberto

- O projeto é individual ou em dupla? (Não decidido na entrevista; não bloqueia o PRD.)

---

## 🛠️ 8. Histórico

| Data       | Versão | O que mudou                   |
| :--------- | :----- | :---------------------------- |
| 2026-08-27 | 1.0.0  | Versão inicial via `/utf-prd` |
