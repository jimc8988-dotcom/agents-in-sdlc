---
title: "Exercício 3 - Teste seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a filtragem com o modo agente. Antes de abrir um pull request, confirme se ela funciona no navegador. Configure o **servidor MCP do Playwright** para que o Copilot controle um navegador real, teste o recurso, publique seu branch e abra o PR.

Neste exercício, você vai conhecer o Model Context Protocol (MCP), adicionar o servidor do Playwright ao workspace, pedir ao Copilot para testar a filtragem no navegador e publicar o branch.

## O que é o Model Context Protocol (MCP)?

O Model Context Protocol (MCP) é um padrão para agentes de IA se comunicarem com ferramentas e serviços externos. Por meio do MCP, os agentes podem acessar informações atualizadas usando recursos e executar ações em seu nome usando ferramentas. Um servidor MCP faz a ponte entre o agente e as ferramentas e serviços externos, como APIs existentes ou ferramentas locais, incluindo pacotes npm.

![Diagrama da interação do modo agente com o contexto, o LLM e as ferramentas, incluindo ferramentas de servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

Servidores MCP conhecidos incluem o [GitHub MCP Server](https://github.com/github/github-mcp-server), que fornece APIs para criar e atualizar repositórios e gerenciar issues e pull requests, e o [Playwright MCP Server](https://github.com/microsoft/playwright-mcp), que permite navegar por páginas, preencher formulários e selecionar botões. O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer dependência. Revise o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use apenas servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no Codespace. Ele já deve conter o servidor `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

O modelo do projeto inclui essa entrada, que permitiu ao Copilot ler o backlog. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server]; o VS Code se conecta por HTTP e você autentica na primeira utilização.

## Adicione o servidor MCP do Playwright

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador controlável.

1. Em `.vscode/mcp.json`, adicione `playwright` junto com `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

O servidor `playwright` é local: o VS Code inicia `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, necessária no Codespace. O projeto já usa Playwright nos testes de ponta a ponta; se o navegador estiver ausente, peça ao Copilot para executar `npx playwright install chromium`.

## Inicie e confie no servidor

O VS Code inicia os servidores MCP sob demanda e solicita confirmação de confiança na primeira utilização.

1. Em `.vscode/mcp.json`, selecione **Start** acima da entrada `playwright` para iniciar o servidor e verificar a configuração.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e confirme para iniciá-lo. Essa etapa é opcional; o VS Code também pode iniciá-lo quando o Copilot precisar das ferramentas.

## Teste o recurso de filtragem

Com o servidor conectado e o aplicativo em execução, envie este prompt ao Copilot:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador, aplicará os filtros e relatará os resultados. Compare o relatório com os critérios de aceitação da issue. Se algo estiver errado, faça perguntas ou peça uma correção antes do PR.

O servidor usa a árvore de acessibilidade da página em vez de capturas de tela. Assim, o agente examina elementos estruturados e rotulados, como botões, links e itens de lista, de forma semelhante a tecnologias assistivas; uma verificação funcional também oferece uma avaliação básica de acessibilidade.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se o servidor de desenvolvimento estiver parado, inicie-o antes do prompt. Na primeira utilização, o Playwright pode precisar baixar um navegador; se isso acontecer, execute `npx playwright install chromium`.

## Publique o branch e crie um pull request

1. No Codespace, abra **Source Control** e revise as alterações do Copilot.
2. Prepare as alterações com o ícone **+**.
3. Gere uma mensagem de commit com o botão **Sparkle**.

   ![Painel Source Control com as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar o branch.

## Crie o pull request

1. No Copilot Chat, selecione **New Chat** e confirme que **Agent** está selecionado para permitir o uso das ferramentas do GitHub.
2. Envie este prompt:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

3. Selecione **Continue** quando necessário para permitir que o Copilot pesquise e execute operações. Na primeira utilização de uma ferramenta do GitHub, talvez seja necessário autenticar-se.
4. Quando o PR for criado, abra o link para revisá-lo, mas **não faça merge ainda**.

## Resumo e próximas etapas

Você aprendeu sobre MCP, adicionou o servidor do Playwright, testou a filtragem em um navegador e publicou o branch para abrir um PR. O MCP é uma das maneiras de ampliar o Copilot. Em seguida, [crie um agente personalizado][next-lesson] para tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
