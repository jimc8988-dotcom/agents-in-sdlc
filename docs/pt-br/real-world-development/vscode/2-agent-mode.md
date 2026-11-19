---
title: "Exercício 2 - Adicionar funcionalidade com o modo agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até a atualização mais simples de um aplicativo pode envolver vários arquivos e tarefas, como executar testes. O modo agente do Copilot pode explorar a estrutura do projeto, atualizar os arquivos necessários, executar testes e corrigir problemas. Use-o para adicionar recursos ao site.

> [!NOTE]
> O modo agente e o agente de nuvem oferecem experiências diferentes. O modo agente trabalha no IDE, com ciclos rápidos de interação; o agente de nuvem trabalha de forma assíncrona, por meio de issues e pull requests.

Neste exercício, você vai explorar como o modo agente identifica arquivos relevantes, coordena alterações na interface e na camada de dados e permite revisar o código e os testes antes do merge.

## Cenário

À medida que a lista de jogos cresce, permita filtrar por categoria e editora. Você já criou um auxiliar para editoras; agora conclua o trabalho na camada de dados, na interface e nos testes com o modo agente.

## Execute o site Tailspin Toys

O site é uma única aplicação Astro que gera HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local com Drizzle ORM; não há API de back-end separada nem framework de interface no cliente. Os auxiliares de acesso a dados ficam em `src/lib/`; a interatividade usa um `<script>` Astro pequeno e as APIs DOM padrão.

### Inicie o site

1. Volte ao Codespace no branch `main`.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie um branch para a filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, aparecerá um banner semelhante a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

5. Se uma caixa de diálogo perguntar se deseja abrir `http://localhost:4321`, feche-a selecionando **x**. Abra o endereço do servidor mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o link no terminal.

> [!NOTE]
> No Codespace, o link localhost redireciona para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o servidor.

### Explore o site

A página inicial lista jogos de tabuleiro com títulos, imagens e descrições. Ao selecionar um jogo, a página de detalhes mostra mais informações, como título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> No Codespace, abra o Copilot Chat e selecione **New Chat**. O modo e o modelo variam por exercício; cada etapa informa quando alterá-los.

1. No seletor de agentes da exibição Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos, executa comandos de terminal e usa ferramentas.

   ![Seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos.

> [!CAUTION]
> A menção a Claude Sonnet 4.5 não expressa preferência dos autores. Ele foi usado para manter a consistência durante a criação do workshop, mas as sugestões podem variar devido à natureza probabilística dos LLMs.

> [!NOTE]
> O Copilot pode usar um comando MCP diferente, mas ainda deve conseguir concluir a tarefa.

3. Envie este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista as issues e revise a lista priorizada. O Copilot também prioriza os itens de acordo com o possível benefício para os usuários.

## Revise os arquivos de instruções

Antes de gerar o código, examine `.github/instructions/ui.instructions.md`, que orienta o Copilot sobre como adicionar funcionalidades ao site:

1. No Codespace, abra o arquivo.
2. Observe a arquitetura, os princípios de design de componentes, testabilidade e acessibilidade, e os links para instruções de Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os caminhos nos arquivos de instruções são relativos ao próprio arquivo. Assim, você pode reutilizar instruções e exemplos em conjuntos menores.

## Implemente a filtragem

O recurso exige alterações na lógica de filtragem em `src/lib/`, testes de comportamento e interface de filtragem na página de listagem. O modo agente pode executar essas tarefas.

1. Continue a conversa atual ou selecione **New Chat**.
2. Selecione **Add Context** > **Instructions** > **ui**.

   ![Exemplo da seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agent** e **Claude Sonnet 4.5** continuam selecionados.

   ![Seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Peça a implementação com base na issue:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

5. Observe o Copilot explorar o projeto e localizar os arquivos relacionados ao recurso, incluindo a camada de dados, a interface e os testes; em seguida, ele modifica os arquivos e executa os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

6. Revise as alterações. Quando solicitado, selecione **Continue** para executar os testes. O Copilot pode pausar várias vezes, e alguns testes podem falhar durante o processo; ele alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   Enquanto o Copilot trabalha, os controles **Keep** e **Undo** podem aparecer no editor. Quando ele terminar, você poderá aceitar ou desfazer todas as alterações; não é necessário selecionar esses controles durante o trabalho.

   ![Solicitação no painel Copilot Chat para confirmar a execução dos testes](../../../_images/ex3-agent-mode-run-tests.png)

   ![Sessão concluída do modo agente do Copilot Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

7. Examine o código em busca de problemas. Depois, volte ao navegador e teste o recurso. Quando o Copilot terminar, haverá controles **Keep** e **Undo** para cada alteração; selecione **Keep** somente depois de confirmar que o recurso funciona e de revisar o código.

> [!CAUTION]
> Revise sempre o código gerado pelo Copilot ou por outras ferramentas de IA.

## Resumo e próximas etapas

Você usou o modo agente para explorar o projeto, implementar um recurso na interface e na camada de dados e revisar alterações e testes. Em seguida, [teste a filtragem com o servidor MCP do Playwright][next-lesson] e abra um pull request.

### Exercício extra — implemente paginação

Quando a lista de jogos crescer, poderá ser necessário adicionar paginação. Siga as práticas recomendadas e os arquivos de instruções existentes; decida se o usuário poderá escolher o tamanho da página ou se ele será fixo; e forneça orientações claras no prompt. Talvez seja necessário iterar e fornecer contexto adicional ao Copilot — esse é o fluxo normal de trabalho.

## Recursos

- [Copilot Ask, Edit e Agent: o que fazem e quando usar cada modo][choose-mode]
- [Modo agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
