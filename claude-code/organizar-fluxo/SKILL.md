---
name: organizar-fluxo
description: "Organiza visualmente workflows do n8n: ajusta espaçamento, alinhamento e notas de seção sem alterar a lógica. Use quando o usuário pedir para organizar o fluxo, ajustar sua estrutura visual, canvas, layout ou stick notes."
---

# Organizar Fluxo

Use esta skill para tornar o canvas de um workflow n8n claro e navegável. Ela é exclusivamente visual: não altera nós funcionais, parâmetros, credenciais, conexões, expressões, execução ou ativação do workflow.

## Processo

1. Leia a estrutura completa do workflow e identifique os caminhos principais, ramificações, nós de IA auxiliares e stick notes existentes.
2. Reposicione apenas os nós para tornar a sequência de leitura evidente, preservando todas as conexões. Mantenha espaçamento regular, deixe ramificações abaixo ou acima do caminho principal e evite cruzamentos desnecessários.
3. Remova notas genéricas, desatualizadas ou redundantes. Crie stick notes por sessão funcional, com título explícito e uma explicação curta do que aquela região faz.
4. As stick notes devem ficar niveladas pelo topo (normalmente `y = 0`), com largura e altura ajustadas para cobrir visualmente os nós de sua sessão. Use uma nota mais alta apenas quando a sessão incluir ramificações inferiores, como revisão ou tratamento de falhas.
5. Antes de salvar, valide as operações. Depois de aplicar, valide o workflow em perfil de runtime. Não execute o workflow como parte de uma organização visual.

## Convenções

- Organize o fluxo em sessões legíveis, por exemplo: disparo e travas; coleta de briefing; geração de conteúdo/imagem; aprovação; publicação, revisão e falhas.
- Mantenha as notas atrás dos nós que documentam e evite que a área total do canvas cresça sem necessidade.
- Não reescreva textos de prompt, regras de negócio ou mensagens enviadas ao Edu, exceto se o usuário pedir explicitamente.
- Se uma alteração de layout exigir mexer na lógica, pare e peça autorização antes de fazê-la.

## Ferramentas (n8n MCP)

Mapeamento do processo acima para as ferramentas do servidor `n8n-mcp`:

- `n8n_get_workflow` — leia a estrutura completa antes de qualquer alteração (passo 1).
- `n8n_update_partial_workflow` — aplique as mudanças com operações cirúrgicas (passos 2 a 4). Use apenas operações visuais:
  - `moveNode` para reposicionar nós.
  - `addNode` / `updateNode` / `removeNode` restritos a nós do tipo `n8n-nodes-base.stickyNote`.
  - Nunca emita `addConnection`, `removeConnection`, `rewireConnection` nem altere `parameters` de nós funcionais.
- Informe sempre `intent` descrevendo a organização visual feita (ex.: "Reorganizar canvas em sessões e atualizar stick notes").
- `n8n_update_partial_workflow` com `validateOnly: true` — valide as operações antes de salvar (passo 5).
- `n8n_validate_workflow` com `profile: "runtime"` — valide o workflow depois de aplicar (passo 5).
- Não chame `n8n_test_workflow`, `n8n_executions` nem ative/desative o workflow.

Se o workflow não estiver em uma instância acessível por MCP e sim em um JSON local, aplique as mesmas regras editando apenas `position` dos nós e os nós `stickyNote` do arquivo.
