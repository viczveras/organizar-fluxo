---
name: organizar-fluxo
description: "Organiza visualmente workflows do n8n. Ajusta espaçamento, alinhamento e notas de seção sem alterar a lógica. Use quando o usuário pedir para organizar o fluxo, ajustar sua estrutura visual, canvas, layout ou stick notes."
---

# Organizar Fluxo

Use esta skill para tornar o canvas de um workflow n8n claro e navegável. Ela é exclusivamente visual. Não altera nós funcionais, parâmetros, credenciais, conexões, expressões, execução ou ativação do workflow.

## Pré requisitos

Esta skill depende de um servidor MCP do n8n conectado ao agente. Sem ele não há como ler nem gravar o workflow, e a skill não tem o que fazer.

O agente precisa de acesso a ferramentas equivalentes a estas.

- Leitura da estrutura do workflow, por exemplo `n8n_get_workflow`.
- Atualização parcial por operações de diff, por exemplo `n8n_update_partial_workflow`, com suporte a `moveNode`, `addNode` e `updateNode`.
- Validação do workflow, por exemplo `n8n_validate_workflow`.

Referência de instalação em https://github.com/czlonkowski/n8n-mcp

Confirme a conexão antes de começar. Se o MCP não estiver disponível, avise o usuário em vez de tentar editar o workflow por outro caminho.

## Processo

1. Leia a estrutura completa do workflow e identifique os caminhos principais, ramificações, nós de IA auxiliares e stick notes existentes.
2. Reposicione apenas os nós para tornar a sequência de leitura evidente, preservando todas as conexões. Mantenha espaçamento regular, deixe ramificações abaixo ou acima do caminho principal e evite cruzamentos desnecessários.
3. Remova notas genéricas, desatualizadas ou redundantes. Crie stick notes por sessão funcional, com título explícito e uma explicação curta do que aquela região faz.
4. As stick notes ficam niveladas pelo topo em `y = 0`, com largura e altura ajustadas para cobrir os nós de sua sessão.
5. Antes de salvar, valide as operações com `validateOnly`. Depois de aplicar, valide o workflow em perfil de runtime. Não execute o workflow como parte de uma organização visual.

## Grid de 16px

O canvas do n8n encaixa em grid de 16px. Toda coordenada `x` e `y`, de nó e de stick note, precisa ser múltipla de 16. Valor fora do grid faz o nó parecer desalinhado e obriga o usuário a rearrumar na mão.

Antes de enviar as operações, confira que cada coordenada satisfaz `posição % 16 === 0`.

## Métricas de espaçamento

- Passo horizontal entre colunas de nós, 256 ou 272. Use 208 quando dois nós pequenos e sequenciais formam um par de leitura óbvia, como webhook mais parser.
- Passo vertical entre ramos paralelos, 160.
- Primeira fileira de nós em `y = 240`. Isso reserva o cabeçalho da stick note.
- Altura da stick note, `240 + (número de fileiras × 160) + 160` de respiro no rodapé.

## Cabeçalho da stick note

O texto da nota fica por cima dos nós, então precisa de espaço reservado. Calcule a altura ocupada pelo texto antes de posicionar a primeira fileira.

- Título `##`, cerca de 60px.
- Cada linha de corpo, cerca de 24px.
- Texto que não cabe na largura da nota quebra em mais linhas do que o escrito, o que aumenta a altura real.

Com `y = 240` na primeira fileira, cabem título mais cinco linhas de corpo. Se o texto for maior, desça a primeira fileira em passos de 160, ou encurte o texto.

Nota estreita força quebra de linha. Nota de largura 240 comporta cerca de 26 caracteres por linha. Ou aumenta a largura, ou escreve frases curtas.

## Nós com múltiplas saídas

Switch e If de três saídas são mais altos que um nó comum. Alinhados pelo topo, a porta do meio cai abaixo da linha do ramo. Suba esses nós 16px em relação à fileira para recentralizar a saída do meio.

Exemplo. Fileira em `y = 400`, switch de três saídas em `y = 384`.

## Convenções

- Organize o fluxo em sessões legíveis. Exemplos de recorte, disparo e travas, coleta de dados, processamento e geração, aprovação, encaminhamento e falhas.
- Cada stick note recebe uma cor distinta para separar as sessões à vista.
- Mantenha as notas atrás dos nós que documentam e evite que a área total do canvas cresça sem necessidade.
- Não reescreva textos de prompt, regras de negócio ou mensagens enviadas ao Edu, exceto se o usuário pedir explicitamente.
- Se uma alteração de layout exigir mexer na lógica, pare e peça autorização antes de fazê-la.

## Erros recorrentes

- Coordenada fora do grid de 16.
- Primeira fileira acima de `y = 240`, o que joga os nós por cima do texto da nota.
- Texto de nota longo demais para a largura escolhida.
- Switch de três saídas alinhado pelo topo em vez de subido 16px.
- Altura da nota calculada só pelos nós, ignorando o cabeçalho de texto.
