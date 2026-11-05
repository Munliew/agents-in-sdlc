---
title: "Lição 9 - Comandos de barra no GitHub Copilot CLI"
description: "Explore comandos de barra para gerenciar contexto, selecionar modelos, compartilhar sessões e delegar tarefas à nuvem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como outras ferramentas de linha de comando, o GitHub Copilot CLI oferece comandos de barra para acessar recursos avançados e opções de configuração. Você já usou `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Agora explore outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar o gerenciamento de contexto.
- usar `/model` para conhecer os modelos disponíveis.
- aprender como `/share` exporta ou compartilha uma sessão.
- explorar comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

## Cenário

Depois do fluxo principal da CLI, explore recursos adicionais para gerenciar contexto, alternar modelos, compartilhar sessões e, se quiser, delegar trabalho ao [agente de nuvem do Copilot][about-cloud-agent].

## Explorar o contexto do Copilot CLI

Em tarefas maiores, você pode atingir o limite de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário; os comandos de barra também permitem inspecionar ou compactar o contexto manualmente.

1. Volte ao Codespace e, se necessário, inicie o Copilot CLI na raiz do repositório.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare os resultados. A diferença pode ser pequena se a conversa já for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente quando a janela fica cheia. Use `/compact` para escolher o momento; use `/clear` ou `/new` para iniciar uma conversa limpa ao trocar de tarefa.

## Escolher um modelo

Os modelos têm pontos fortes diferentes. O Copilot CLI permite listar e selecionar o modelo desejado.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhar uma sessão

O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar um GitHub gist.

1. Digite `/help` e consulte as opções de `/share` disponíveis na versão instalada.
2. Se quiser compartilhar a sessão, escolha o destino adequado, como `/share file` para exportar um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo ou publicá-lo. As exportações podem incluir prompts, respostas e detalhes do projeto.

A publicação de um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não pretenda compartilhar.

## Opcional: ampliar o trabalho ou delegar

O fluxo principal do workshop terminou. A CLI também oferece comandos para tarefas maiores:

- `/fleet` pode distribuir subtarefas independentes entre subagentes e executá-las em paralelo.
- `/worktree` pode criar uma worktree isolada para outra tarefa.
- `/delegate` pode enviar uma tarefa ao agente de nuvem do Copilot, que trabalha de forma assíncrona e pode abrir um pull request.

Esses comandos são opcionais porque podem criar worktrees adicionais ou trabalho remoto. Comece uma tarefa nova e bem delimitada antes de experimentá-los, e revise o resultado pelo fluxo habitual. Para saber mais sobre trabalho assíncrono, continue com o [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximos passos

Você:

- usou `/context` e `/compact` para explorar o gerenciamento de contexto.
- usou `/model` para conhecer os modelos disponíveis.
- aprendeu como `/share` exporta ou compartilha sessões.
- explorou comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

Em seguida, [revise o que aprendeu][next-lesson] e veja os próximos passos.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
