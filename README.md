# Organizar Fluxo

Skill para organizar visualmente workflows do n8n sem mudar a lógica da automação.

Disponível em dois formatos: **Codex** e **Claude Code**.

## Exemplo visual

![Workflow n8n organizado em seções](assets/workflow-organizado.png)

## O que ela faz

- Reorganiza nós, espaçamento e ramificações no canvas.
- Substitui stick notes genéricas por notas que explicam cada sessão.
- Mantém as notas alinhadas pelo topo e dimensionadas para cobrir suas áreas.
- Valida as alterações antes e depois de aplicá-las.

Ela não altera conexões, credenciais, prompts, regras de negócio, execução ou ativação do workflow.

## Quando usar

Peça ao Codex ou ao Claude Code para **organizar o fluxo**, **ajustar a estrutura visual**, **melhorar o espaçamento**, **organizar o canvas** ou **revisar as stick notes** de um workflow n8n.

## Instalação

### Codex

Clone este repositório e deixe a pasta `organizar-fluxo` dentro do diretório de skills do Codex:

```text
~/.codex/skills/organizar-fluxo/
```

Depois reinicie ou abra uma nova sessão do Codex. A skill é detectada automaticamente quando a solicitação envolve organização visual de workflows n8n.

### Claude Code

Copie a pasta `claude-code/organizar-fluxo` para o diretório de skills do Claude Code:

```text
~/.claude/skills/organizar-fluxo/
```

Para instalar apenas no projeto atual, use `.claude/skills/organizar-fluxo/` dentro do repositório.

```bash
mkdir -p ~/.claude/skills
cp -r claude-code/organizar-fluxo ~/.claude/skills/
```

Abra uma nova sessão do Claude Code. A skill aparece como `organizar-fluxo` e é acionada automaticamente quando o pedido envolve organização visual de workflows n8n; também pode ser chamada com `/organizar-fluxo`.

A versão do Claude Code é idêntica à do Codex: mesmo processo, mesmas convenções, mesmo escopo.

## Estrutura

```text
organizar-fluxo/
├── SKILL.md                        # skill do Codex
├── agents/
│   └── openai.yaml                 # metadados do Codex
└── claude-code/
    └── organizar-fluxo/
        └── SKILL.md                # skill do Claude Code
```

## Escopo

Esta skill cuida somente da apresentação visual. Para mudanças de comportamento, integrações ou regras da automação, faça uma solicitação separada e explícita.
