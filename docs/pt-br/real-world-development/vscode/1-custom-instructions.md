---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica ou o Copilot precisar conhecer informações adicionais, disponibilize esse contexto. Os [arquivos de instruções][instruction-files] orientam a geração de código.

Neste exercício, você vai explorar como as instruções do repositório e os arquivos associados a caminhos fornecem contexto e padrões ao Copilot; gerar, com as instruções atuais, a primeira parte dos dados para filtragem; acrescentar um padrão global; repetir o prompt e observar as mudanças no código; e integrar a atualização e o auxiliar de filtragem ao branch `main`.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos, pois o Copilot é não determinístico. O objetivo é observar a tendência após atualizar as instruções, não obter resultados idênticos.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes de desenvolvimento: a camada de dados precisa de testes unitários; a interface deve ter modo escuro e aparência moderna; o código deve ter comentários TSDoc; e cada arquivo deve começar com um bloco de comentários sobre sua finalidade.

### Instruções personalizadas

As instruções personalizadas fornecem contexto, preferências, convenções e bibliotecas ao Copilot. Você pode criar instruções para todo o repositório ou para arquivos e tarefas específicos.

- `.github/copilot-instructions.md` é enviado ao Copilot em todas as solicitações do repositório. Inclua contexto global, como a pilha de tecnologias, uma visão geral do projeto e práticas recomendadas.
- `.github/instructions/*.instructions.md` fornece orientações para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para conclusões de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem usam instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot também aceita [outros padrões de arquivos de instruções][custom-instructions-support], como AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Mantenha `copilot-instructions.md` focado nas orientações globais do projeto. Use arquivos `*.instructions.md` para tipos de arquivo ou tarefas específicas. Escreva instruções claras em linguagem natural e inclua exemplos do que fazer e evitar. Experimente para descobrir o que funciona melhor.

> [!TIP]
> Use uma coleção sólida de instruções, inclusive para tarefas como [atualizações de interface][ui-instructions] e [Astro][astro-instructions]. O Copilot pode gerar instruções; no VS Code, por exemplo, use **Configure Chat → Generate Agent Instructions**; no Copilot CLI, use `/init`. Para obter modelos, explore o [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções do projeto

1. Abra `.github/copilot-instructions.md` e examine **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**, incluindo a seção aninhada **GitHub Actions Workflows**.
2. Examine `.github/instructions`, que contém orientações para Astro, Drizzle, testes e outros tópicos.
3. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`: o glob relativo à raiz corresponde, por exemplo, a `**/*.test.ts`.
4. Examine as instruções de testes unitários e abra `.github/instructions/drizzle.instructions.md`. No fim, observe os links para instruções e exemplos; os caminhos são relativos ao arquivo de instruções.

> [!NOTE]
> **Code formatting requirements** ainda não exige documentação no código. Você adicionará comentários TSDoc e cabeçalhos de arquivo.

## Crie um branch para as alterações

1. Volte ao Codespace e abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Crie e selecione um branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat antes de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> No Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa informa quando alterar essas opções.

1. Feche as guias do editor abertas para limitar o contexto.
2. Abra `src/lib/publishers.ts` para indicar onde o auxiliar deve ficar.
3. No seletor de agentes da exibição Chat, selecione **Agent**.

   ![Seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explora o projeto e atualiza o auxiliar e, muitas vezes, os testes.
6. Observe que a função proposta recebe um cliente `db` e retorna uma matriz tipada de editoras, conforme as convenções de `.github/instructions/drizzle.instructions.md`.
7. Observe se faltam comentários TSDoc e um cabeçalho de arquivo.

> [!CAUTION]
> Como o Copilot é não determinístico, talvez ele inclua comentários mesmo sem a instrução. Tudo bem: o objetivo é observar a consistência após a atualização.

## Adicione um padrão para o repositório

1. Abra `.github/copilot-instructions.md` e localize **Code formatting requirements**, perto da linha 27.
2. Depois dos padrões existentes, adicione:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

3. Salve o arquivo.

> [!TIP]
> Instruções globais pertencem a `.github/copilot-instructions.md`; use arquivos `*.instructions.md` para linguagens, arquivos e tarefas específicas.

## Repita o prompt e observe a diferença

1. No Copilot Chat, selecione **New Chat** e volte a `src/lib/publishers.ts`.
2. Envie novamente o mesmo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

3. Observe se o arquivo começa com um comentário semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

4. Observe se a função inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo ao código usado no próximo exercício.

## Confirme as instruções e envie o branch

1. Abra um terminal e verifique as alterações:

   ```bash
   git status
   ```

2. Prepare e confirme as instruções e todos os arquivos alterados pelo Copilot para o auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações relacionadas (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

3. Envie o branch:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e integre um pull request

O modelo já configura as ferramentas do GitHub para o projeto. Na primeira utilização, o VS Code pode pedir que você entre na sua conta.

1. Abra o Copilot Chat no Codespace, selecione **New Chat** e confirme que **Agent** está selecionado.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux).
3. Peça ao Copilot para localizar a issue das instruções e criar um PR do branch atual:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Revise cada solicitação de permissão do GitHub e selecione **Allow** quando apropriado.
5. Depois da criação do PR, peça ao Copilot para integrá-lo e atualizar seu branch local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Revise as solicitações de permissão antes de autorizá-las.

## Resumo e próximas etapas

Você observou o código gerado com as instruções atuais, adicionou um padrão global, repetiu o prompt e observou as mudanças. Em seguida, confirmou as instruções e o auxiliar em `main`. Agora, use essas instruções no [modo agente][next-lesson] enquanto o Copilot adiciona um recurso.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores][copilot-instructions-five-tips]
- [Instruções pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
