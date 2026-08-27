# 🛠️ Architecture / Software Design Document

**Projeto:** [nome]
**Versão:** 0.0.0 · esqueleto — preencha via `/utf-architecture`
**Última atualização:** [data]

> 🤖 **O `prd.md` responde _o quê_ o produto faz. Este responde _onde as coisas
> moram e como se chamam_.** Detalhe de tela — rota, componente, contrato —
> **não** se decide aqui: isso é trabalho da spec de cada história.
>
> ✍️ **Não preencha na mão:** rode `/utf-architecture` (depois do `/utf-prd`).
> A entrevista decide com você cada seção e garante as quatro declarações que o
> `/utf-setup` exige: **framework do backend, framework do frontend, estrutura
> do monorepo e como rodar os testes.**

---

## 🤖 1. Fontes de Contexto para a IA

> Onde a IDE agêntica busca a verdade. **Isto é o índice; a configuração mora
> nos arquivos** — documento não configura ferramenta.

| Fonte | Onde configurar | Serve para |
| :---- | :-------------- | :--------- |
| Constituição da IA | `.agents/rules/utf-rules.md` (via `CLAUDE.md`) | Regras inegociáveis: fases do SDD, 2 rodadas, revisores distintos, Git |
| Fluxos da IA | `.agents/workflows/` | PRD, architecture, setup, ciclo por Issue, ciclo por tarefa, tutor |
| Agentes (subagentes) | `.agents/agents/` (cascas em `.claude/agents/`) | Implementador, revisores, auditor final e tutor |
| Ficha da disciplina | `docs/checklist.md` | Regras do projeto, IDs e entregas |
| Design (Figma/Stitch) | [link] | Cores, tipografia, hierarquia visual |

---

## 📦 2. Stack Tecnológica

> Definição **estrita**: nenhuma dependência entra sem aparecer aqui. Esta
> seção e o `package.json` contam a mesma história, ou o projeto já se perdeu.
> O que a ficha da disciplina fixa entra como está; o que ela deixa livre é
> decidido na entrevista.

- **Backend:** [conforme a ficha — framework + ORM + banco, com versões]
- **Frontend:** [a escolha entre as opções da ficha, com versão]
- **Padrões de código do frontend:** [o bloco da stack escolhida, gravado pelo `/utf-architecture`]
- **Estilo:** [CSS/framework de estilo]
- **Testes:** [ferramenta em cada app + comandos exatos de suíte e lint]

### 🧱 2.1. Backend — regras estruturais

> Declaradas uma a uma na entrevista, percorrendo os IDs da ficha (camadas,
> validação de entrada, autenticação, formato de resposta e erro, segredos,
> integrações). **É esta declaração que os revisores usam como critério fixo.**

[preenchido via `/utf-architecture`]

### 🌐 2.2. O contrato da API

> A documentação viva (ex.: OpenAPI/Swagger) é gerada do código e servida pela
> própria API; o frontend deriva os contratos dela. Este documento **não mantém
> tabela de endpoints à mão**, e o arquivo gerado **não é commitado** — cópia
> no repositório desatualiza; a fonte é o endpoint vivo.

[onde a documentação é servida, ex.: `/docs`]

---

## 🗂️ 3. Estrutura do Repositório (Monorepo)

> Uma pasta por aplicação, cada uma com o seu `package.json`. Sem npm
> workspaces, Nx ou Turborepo enquanto não houver código compartilhado de
> verdade — ferramenta sem problema para resolver é só custo.

```text
.
├── .agents/               # constituição, workflows e prompts dos agentes (§1)
├── .claude/               # cascas dos agentes e comandos para o harness
├── CLAUDE.md              # carrega a constituição em toda sessão
├── README.md              # a vitrine: o que é e como rodar
├── docs/                  # prd.md, este arquivo, checklist.md e guias
├── specs/                 # uma pasta por história implementada
└── apps/
    ├── [app]/             # [preencher na entrevista]
    └── [app]/
```

---

## 🏗️ 4. Arquitetura Frontend

> 📏 **A regra que vale para qualquer stack: componente não fala com o
> servidor.** Todo acesso à API passa por uma camada de repositório/serviço —
> mudança de contrato mexe só nessa camada, nunca nas telas.

[organização por feature/domínio, regras de dependência entre pastas —
preenchido via `/utf-architecture`]

---

## 🗄️ 5. Arquitetura de Dados

### 📖 5.1. Glossário Técnico (Mapeamento)

> A ponte entre o português do negócio (PRD §2) e o inglês do código.
> **Dados e código em inglês, interface em português.**

| Termo PRD (PT-BR) | Entidade técnica (EN) | Atributos principais |
| :---------------- | :-------------------- | :------------------- |
| | | |

### 📊 5.2. Diagrama ER (Mermaid)

> Inclui as entidades que o escopo mínimo da ficha exige.

```mermaid
erDiagram
```

### 🌍 5.3. O banco por ambiente

| Ambiente | Onde roda | Como conecta |
| :--- | :--- | :--- |
| **Local** | | |
| **CI** | | |
| **Produção** | | |

> 🔒 Credenciais **nunca** aparecem no repositório — nem em código, nem em
> YAML, nem em doc. Só nos *secrets* da plataforma.

---

## 🗺️ 6. Mapa de Domínios e Rotas

> **Este índice cresce.** Não é para preencher agora: **uma linha por história
> implementada** — a spec é que define rota e contrato. Aqui fica só o mapa de
> quem já existe.

| Domínio | Rota | Guard | Dados (repository) | US |
| :------ | :--- | :---- | :----------------- | :-- |
| | | | | |

---

## 📅 7. Histórico

| Data | Versão | O que mudou |
| :--- | :----- | :---------- |
| | 1.0.0 | Versão inicial via `/utf-architecture` |

---

## 🛑 O que ainda **não** está neste documento

Detalhes de funcionalidade — DTOs de endpoints específicos, máquinas de estado
de uma história — **não entram aqui**: nascem sob demanda no `spec.md` de cada
história. Este documento guarda só o que vale para o sistema inteiro.
