---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações, se houver alguma, não enfrentam problemas com dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidade que permanecem no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele gera um pull request (PR) em rascunho para revisão humana. Assim, você pode delegar tarefas tediosas, acelerar o desenvolvimento e se concentrar em questões mais importantes.

Neste exercício, você explorará:

- a personalização do ambiente usado para gerar código.
- como garantir que as operações sejam realizadas com segurança.
- a importância de issues bem delimitadas.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívida técnica a resolver. As pessoas contratadas para criar a primeira versão do site não deixaram a documentação em boas condições — na verdade, ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar títulos relacionados — outros jogos da mesma categoria — para que as pessoas possam continuar explorando. Por enquanto, a equipe precisa de uma implementação básica para testes de aceitação da experiência do usuário. Essa tarefa está bloqueando o trabalho, mas há issues de prioridade mais alta.

Esses são exemplos de tarefas que podem ser adiadas e que são boas candidatas para o agente de nuvem. Ele pode trabalhar de forma assíncrona enquanto você se concentra em outras atividades; depois, você volta para revisar o resultado.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode trabalhar em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Uma forma de iniciar esse trabalho é [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request em rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções bem delimitadas

O GitHub Copilot não faz milagres: não é possível pular a etapa de pensar. Até tarefas aparentemente simples podem se tornar complexas. Por isso, [delimite cuidadosamente as tarefas do agente de nuvem][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e ajuste a abordagem. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu a atividade sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções incluídos no repositório para observar seu efeito mais adiante.

Abra estes arquivos na interface web do GitHub ou no Codespace:

- `.github/copilot-instructions.md` — examine a seção **Code standards**, especialmente as expectativas sobre convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Quando você atribuir a issue `Code lacks documentation` ao agente de nuvem, observe o pull request resultante em busca de comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções que serão examinados novamente ao revisar os PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem

O desenvolvimento de código normalmente exige um ambiente específico e scripts de configuração para instalar as ferramentas e dependências necessárias. O mesmo acontece ao atribuir uma tarefa ao Copilot, que trabalha de forma semelhante a um engenheiro de software.

O agente de nuvem usa [GitHub Actions][github-actions] para configurar o ambiente. Você pode personalizá-lo com um [workflow de configuração especial][setup-workflow] em `.github/workflows/copilot-setup-steps.yml`, executado antes de o agente começar. Isso disponibiliza as ferramentas e dependências necessárias. O workflow já está configurado para este laboratório e instala Node.js, dependências do projeto, binários de navegador para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para as etapas. O Copilot recebe seu próprio token para as operações.
    permissions:
      # Se as etapas de configuração precisarem clonar o repositório, por exemplo para instalar dependências, use `contents: read`. Caso contrário, o Copilot fará isso automaticamente depois da conclusão das etapas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (o aplicativo agora é Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com qualquer workflow do GitHub, mas há alguns detalhes importantes:

- Ele contém um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se o workflow funciona sem esperar que o Copilot o execute.

## Adicionar documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica pode reduzir a produtividade e dificultar a manutenção do código ou a integração de novas pessoas. O Copilot é eficaz na criação de documentação, por isso essa é uma boa issue para atribuir ao agente de nuvem. Ele trabalhará em segundo plano; em um exercício futuro, você revisará o resultado.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuir o Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o novo pull request, cujo título será semelhante a `[WIP]: Code lacks documentation`. Se ele não aparecer na lista, aguarde alguns instantes e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista pode levar alguns minutos para aparecer. O Copilot está criando o ambiente com o workflow descrito anteriormente, analisando o projeto e determinando como abordar a tarefa.

13. Revise a lista e as tarefas planejadas.
14. Role a linha do tempo do pull request e procure a atualização que indica que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

  ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe a sessão em tempo real e como o Copilot resolve o problema: explora o código e o estado do projeto, pausa para decidir a abordagem e cria código.

O processo pode levar vários minutos. Uma das principais vantagens do agente de nuvem do Copilot é trabalhar de forma assíncrona para que você possa se concentrar em outras tarefas. Vamos aproveitar esse recurso: enquanto o agente trabalha, atribua outra tarefa e depois desenvolva uma funcionalidade no aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma das vantagens do agente de nuvem é poder dividir o trabalho. Enquanto você se concentra em um conjunto de tarefas, ele cuida de outro. Vamos atribuir ao agente a solicitação da equipe de design.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, digite:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot**.

  ![Atribuir o Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá um conjunto de 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando na segunda solicitação. Como ele funciona de maneira semelhante a uma pessoa engenheira de software, você não precisa acompanhar o trabalho continuamente; basta revisar o resultado quando estiver concluído. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Este exercício explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Você pode atribuir issues ao agente para que ele trabalhe de forma assíncrona em dívida técnica, novas funcionalidades ou migrações de código entre frameworks.

Você explorou estes conceitos:

- personalização do ambiente usado para gerar código.
- como garantir que as operações sejam realizadas com segurança.
- a importância de issues bem delimitadas.
- como atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha, você pode criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores MCP][cloud-agent-mcp] e instruções personalizadas, apresentadas em exercícios anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
