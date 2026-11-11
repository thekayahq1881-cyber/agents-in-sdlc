---
title: "Lição 9 - Comandos de barra no GitHub Copilot CLI"
description: "Explore comandos de barra para gerenciar o contexto, selecionar modelos, compartilhar sessões e delegar tarefas opcionalmente ao agente de nuvem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como qualquer boa ferramenta de CLI, o GitHub Copilot CLI inclui vários comandos de barra para interagir com ele. Esses comandos expõem recursos avançados, informações internas e opções adicionais de configuração. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Vamos explorar outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa;
- usar `/model` para conhecer os modelos disponíveis;
- aprender como `/share` pode exportar ou compartilhar uma sessão;
- explorar comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

## Cenário

Você concluiu o fluxo principal da CLI. Agora, vamos conhecer alguns recursos adicionais: gerenciar o contexto, trocar de modelo, compartilhar sessões e, opcionalmente, delegar trabalho ao [agente de nuvem do Copilot][about-cloud-agent].

## Explorar o contexto do Copilot CLI

Ao trabalhar em tarefas maiores ou mais complexas, você pode atingir o limite da janela de contexto do modelo. Quando necessário, o Copilot CLI compacta a conversa automaticamente. Você também pode inspecionar ou compactar o contexto com comandos de barra.

1. Volte ao Codespace e inicie o Copilot CLI na raiz do repositório, se ele ainda não estiver aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e como o contexto é dividido entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare o resultado. Talvez não haja uma grande diferença se a conversa ainda for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente quando a janela fica cheia. Use `/compact` se quiser escolher o momento. Use `/clear` ou `/new` ao mudar para uma tarefa sem relação e quiser iniciar uma conversa limpa.

## Escolher um modelo

Os modelos têm pontos fortes diferentes, e cada pessoa desenvolvedora tem suas preferências. O Copilot CLI permite listar e selecionar o modelo que você quer usar.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhar uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todo mundo a usar melhor as ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar uma GitHub gist.

1. Digite `/help` e consulte as opções de `/share` disponíveis na versão instalada.
2. Se quiser compartilhar esta sessão, escolha o destino que atende às suas necessidades, por exemplo, `/share file` para exportar um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações de sessões podem incluir prompts, respostas e detalhes do projeto.

Publicar um link ou uma gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não pretende compartilhar.

## Opcional: ampliar o trabalho ou delegar

O fluxo principal do workshop foi concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` pode dividir subtarefas independentes entre subagentes e executá-las em paralelo.
- `/worktree` pode criar um worktree isolado do Git para uma tarefa separada.
- `/delegate` pode enviar uma tarefa ao agente de nuvem do Copilot, que trabalha de forma assíncrona e pode abrir um pull request.

Esses comandos são opcionais, pois podem criar worktrees adicionais ou trabalho remoto. Antes de experimentá-los, inicie uma tarefa nova e bem delimitada e revise o resultado pelo fluxo normal. Se quiser aprender mais sobre o trabalho assíncrono com agentes, continue com o [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximos passos

Os comandos de barra do Copilot CLI permitem configurá-lo, compartilhar sessões e ver o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa;
- usou `/model` para conhecer os modelos disponíveis;
- aprendeu como `/share` pode exportar ou compartilhar uma sessão;
- explorou comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

Há muitos outros comandos de barra e recursos do Copilot CLI para explorar. Vamos encerrar o percurso [revisando o que aprendemos][next-lesson] e conhecendo os próximos passos.

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
