---
title: "Exercício 5: Fazer iterações sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas voltadas a melhorar a experiência do usuário e adicionar funcionalidades. Pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design testar e implementasse recursos de acessibilidade, incluindo controles para alternar entre os modos de alto contraste e claro. Vamos explorar as mudanças no código e, se necessário, enviar comentários ao Copilot para aprimorar o trabalho.

### Cenário

Como destacamos várias vezes, a IA generativa não muda os fundamentos do design de software nem do DevOps. É sempre importante revisar o código gerado e seguir o processo normal de DevOps. Com isso em mente, vamos revisar as sugestões do GitHub Copilot para criar a documentação, implementar os jogos relacionados e adicionar recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, existem restrições de segurança para manter o ambiente protegido. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para desenvolver o código.
- O agente de nuvem executa dentro do GitHub Actions, que cria um ambiente separado e temporário para o trabalho.
- É necessária a aprovação de uma pessoa antes de executar qualquer fluxo de trabalho do GitHub Actions.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive o acesso a servidores MCP.

## Revise a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: o PR que adiciona documentação ao código. Você fará a revisão usando a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, talvez você veja um aviso de que o Copilot está bloqueado por um firewall. Isso **é esperado**, pois o acesso do Copilot a recursos externos — inclusive chamadas a servidores MCP externos — é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request terá a indicação **[WIP]**. Aguarde a conclusão do trabalho, o que pode levar alguns minutos. Enquanto isso, faça uma pausa ou reflita sobre o que aprendeu.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as mudanças.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os novos comentários TSDoc e outra documentação. As mudanças exatas variam.

   Ao revisar o código, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Eles seguem os arquivos de instruções personalizadas que você examinou no Exercício 2.

6. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
7. Você verá um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila da seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para a aplicação Astro deverão ser aprovadas. Isso pode levar alguns minutos.

## Peça mudanças ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode marcar o Copilot em comentários no PR, como faria com outras pessoas da equipe, ou em comentários associados a linhas específicas do código. O Copilot verá os comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não podemos indicar um texto exato para solicitar mudanças. Você pode pedir, por exemplo:

- Adicione um cabeçalho de comentário no início de cada arquivo de código com uma breve descrição de sua finalidade.
- Adicione comentários TSDoc a arquivos TypeScript e Astro.
- Crie um README que descreva a estrutura da aplicação Astro.

1. Adicione um comentário solicitando uma mudança na documentação gerada e marque **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou sugira outra melhoria de documentação para a base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Você pode selecionar **Back to pull request** para voltar ao pull request.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot terminar as mudanças, um novo commit deverá aparecer no pull request.
5. Selecione a guia **Files changed** para revisar as mudanças.

Continue fazendo iterações até ficar satisfeito. Depois, você pode marcar o PR como pronto para revisão e mesclá-lo na branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Vamos voltar ao PR criado pelo Copilot para resolver a issue que pede jogos relacionados na página de detalhes de cada jogo.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
6. Você verá um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Os fluxos de trabalho deverão entrar na fila da seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para a aplicação Astro deverão ser aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você também pode alternar para essa branch no Codespace e testar manualmente os jogos relacionados. Acesse o Codespace, abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode pedir atualizações ao agente de nuvem de forma iterativa. Por exemplo, solicite que ele ajuste a quantidade de jogos relacionados exibidos ou adicione cabeçalhos de comentário e comentários TSDoc — lembre-se de que essa tarefa foi atribuída **antes** de você atualizar as instruções personalizadas. Para fazer essas solicitações, adicione um novo comentário na guia **Conversation**. O Copilot verá o comentário e iniciará outra sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Este PR deve incluir o modo de alto contraste, atribuído no Exercício 3, e o modo claro, solicitado no painel de controle do Exercício 4.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o pull request terá a indicação **[WIP]**. Aguarde a conclusão do trabalho, o que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - os componentes de interface para alternar entre os modos.
   - o uso do armazenamento local para persistir as preferências das pessoas usuárias.
   - as mudanças de CSS ou estilo dos modos de alto contraste e claro.
   - os atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - qualquer código JavaScript/TypeScript que gerencie a alternância de modos.

6. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
7. Você verá um indicador de que alguns fluxos de trabalho estão aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila da seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para a aplicação Astro deverão ser aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** alterne para essa branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie a aplicação e teste os controles dos modos de alto contraste e claro no navegador. Confirme que eles funcionam como esperado e que as preferências permanecem após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot na implementação desses recursos de acordo com as práticas recomendadas. Se encontrar problemas de acessibilidade ou oportunidades de melhoria, marque **@copilot** em um comentário e peça uma atualização, como fez nos PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem é especialmente útil quando você pode delegar tarefas reais do backlog e se dedicar a outros assuntos. Para desenvolver esse hábito, registre mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse para apoiadores na página de detalhes do jogo.
- implementar paginação na página de lista de jogos.
- adicionar validação de entrada e tratamento de erros às funções auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizadas incluídos no repositório** para observar seu efeito no trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, com controles de alto contraste e modo claro.
- **Usou a página de agentes do Copilot como painel de controle** para monitorar e orientar a sessão de acessibilidade em andamento.
- **Revisou e aprimorou os pull requests do agente de nuvem**, marcando `@copilot` para solicitar mudanças e aprovando fluxos de trabalho.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar explorando, os outros percursos complementam o que você praticou aqui:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas mudanças revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` para o agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outra área.
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
