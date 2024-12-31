# Microsoft Fabric Real-Time Intelligence in a Day Laboratório 2

## Sumário

- Estrutura do documento

- Introdução

- Hub em Tempo Real do Fabric

  - Tarefa 1: Criar uma fonte de fluxo do evento

  - Tarefa 2: Destino do Eventstream de configuração

- KQL (Linguagem de Consulta Kusto)

  - Tarefa 3: Criação de consultas de banco de dados Kusto

  - Tarefa 4: Usando consultas T-SQL em um Banco de Dados KQL

- Conjunto de Consultas KQL

  - Tarefa 5: Trabalhando com um Conjunto de Consultas KQL

- Resumo

- Referências

# Estrutura do documento

O laboratório inclui etapas a serem seguidas pelo usuário juntamente com as capturas de tela associadas que fornecem auxílio visual. Em cada captura de tela, as seções estão destacadas com caixas laranjas para indicar as áreas nas quais o usuário deve se concentrar.

# Introdução

Neste laboratório, você experimentará uma maneira de lidar com um fluxo contínuo de dados em tempo real. Você usará um objeto do Fabric Real-Time Intelligence chamado Eventstream para ingerir esses dados no Eventhouse criado no último laboratório e
escrever algumas consultas KQL básicas.

Ao final deste laboratório, você terá aprendido a:

- Como criar um Eventstream

- Carregar dados em tempo real em um Banco de Dados KQL

- Escrever consultas Linguagem de Consulta Kusto básicas

# Hub em Tempo Real do Fabric

## Tarefa 1: Criar uma fonte de fluxo do evento

1. Abra o **workspace do Fabric** criado no último laboratório. Daqui, podemos ver o Eventhouse que criamos.

2. Navegue até o Hub em Tempo Real selecionando o botão **Em tempo real** no lado esquerdo, mesmo que não vejamos nenhum fluxo de dados que mudará em breve.

3. Selecione o botão verde **+ Conectar a fonte de dados**, que deve estar no canto superior direito.

4. Será aberta uma janela que permitirá a você selecionar uma fonte para nossos dados de fluxo. Como discutimos antes, há muitas opções fantásticas para escolher, mas para esta classe selecionaremos a opção "Hubs de Eventos do Azure". Se você não vir "Hubs de Eventos do Azure" facilmente, selecione as **Fontes da Microsoft** na parte superior para filtrar as opções exibidas.

5. Agora é necessário criar uma conexão com o Hub de Eventos do Azure. Clique no texto **Nova conexão**, pois você não tem uma conexão no momento.

6. Na página de detalhes do ambiente, copie e cole todas as configurações de conexão necessárias nos campos apropriados. Para estes laboratórios, estamos nos conectando a um Hub de Eventos com dados de fluxo sendo enviados de um notebook Python. Esse notebook está criando transações de vendas falsas a uma taxa de cerca de 3.100 transações por hora.

Namespace do Hub de Eventos: **rtiadhub{userid} -- fornecido por cloudlabs**

Hub de Eventos: **rti-iad-fabrikam**

Nome da Chave de Acesso Compartilhada: **rti-reader**

Chave de Acesso Compartilhada: **Disponível na guia Detalhes do Ambiente**

7. Depois que todas as propriedades forem preenchidas, clique em **Conectar**.

8. Na configuração da fonte de dados do Hub de Eventos do Azure, você talvez precise modificar o **Grupo de consumidores** do Hub de Eventos para garantir acesso a um ponto de acesso exclusivo ao fluxo de dados. Para este workshop, você pode deixar o valor "$Default" como mostrado abaixo.

9. Antes de finalizarmos essa fonte de dados e o Eventstream, vamos seguir em frente e renomear nosso Eventstream para algo mais útil. Na seção "Detalhes do fluxo" à direita, selecione o ícone de lápis ao lado do "Nome do Eventstream" e vamos chamar nosso Eventstream de "**es_Fabrikam_InternetSales".**

10. Agora, podemos clicar em **Próximo**, o que nos levará a uma página de visão geral final.

11. Nessa tela de visão geral, verifique se o conteúdo parece correto e clique em **Criar fonte**.

**Observação:** Seus detalhes serão diferentes do que você vê na captura de tela.

12. Depois que o Eventstream e a fonte do Eventstream forem criados, selecione a opção "**Abrir Eventstream**"

13. Isso levará você à interface do usuário do Eventstream. É aqui que você verá seu fluxo de dados de origem fluindo para nosso eventstream e podemos adicionar eventos de transformação também.

14. Pode levar alguns instantes para que sua fonte esteja **Ativa**, mas depois de esperar alguns momentos, clique no ícone do meio com o nome do seu Eventstream e você deverá ver uma visualização dos dados.

**Observação:** se você receber um status de "Aviso" e uma política de auditoria, não há problemas. O fluxo ainda funcionará.

15. Agora você deve ver uma amostra dos dados na janela inferior.

16. Isso mostrará uma visualização dos dados que estão sendo recebidos do Hub de Eventos do Azure. Se deslizar a barra de rolagem horizontal inferior até o lado direito da visualização, você poderá ver a hora em que os dados foram recebidos no Hub de Eventos em duas colunas chamadas **EventProcessedUtcTime** e **EventEnqueuedUtcTime**. Isso deve refletir a data/hora atual no formato UTC.

## Tarefa 2: Destino do Eventstream de configuração

1. Clique no bloco dentro da área da tela chamada "Alternar para o modo de edição para Transformar o evento ou adicionar o destino"

2. Na interface do usuário do Eventstream, clique na opção **Transformar eventos ou adicionar destino** para abrir o menu suspenso.

3. Exiba a lista de operações disponíveis que podem ser feitas no fluxo.

4. Veja abaixo a seção **Operações** e você encontrará **Destinos**. Selecione a opção que diz **Eventhouse**.

5. Um novo menu será aberto no lado direito da tela. O primeiro item que você precisa modificar para o destino é o **modo de ingestão de dados**. As duas opções são **Ingestão direta** e **Processamento de eventos antes da ingestão.** Como não vamos transformar nada em nosso Eventstream e carregar diretamente essas informações em uma tabela de Banco de Dados KQL, certifique-se de ter selecionado a opção **Ingestão direta**.

6. Modifique o restante das configurações com os seguintes detalhes abaixo.

 - Nome do destino -- **eh-kql-db-fabrikam**

 - Workspace -- **RTI_username**

 - Eventhouse -- **eh_Fabrikam**

 - Banco de Dados KQL -- **eh_Fabrikam**

7. Clique em Salvar.

8. Com o Eventstream configurado, clique no botão **Publicar** para salvar este Eventstream e iniciar sua ingestão.

9. Se você notar que a fonte **AzureEventHub** ficou inativa, defina o botão de alternância para o estado "Ativo" e escolha a opção "Agora" quando a caixa de diálogo for aberta

10. Escolha a opção **Configurar** em **Destino** para mapear corretamente o fluxo para uma tabela no Banco de Dados KQL.

11. Clique na opção **+ Nova tabela** abaixo do banco de dados **eh_Fabrikam**.

12. Dê à nova tabela o nome **InternetSales** e, em seguida, clique na marca de seleção.

13. Talvez seja necessário atualizar o **"Nome da conexão de dados"** para atender aos requisitos. Vamos renomeá-lo para **"eh_Fabrikam_es_InternetSales"**. Em seguida, poderemos clicar em **Próximo.**

14. Após alguns momentos de pesquisa de eventos, a interface do usuário deverá permitir que você veja que os dados de exemplo foram encontrados. Clique em **Concluir** na parte inferior da tela.

15. Depois disso, você verá um resumo. Depois que todas as marcas de verificação ficarem verde, clique em **Fechar** para avançar.

16. Depois de ver a interface do usuário mostrando os mapeamentos da origem para o Eventstream para o destino, você configurou e iniciou corretamente um fluxo de dados no Banco de Dados KQL.

# KQL (Linguagem de Consulta Kusto)

## Tarefa 3: Criação de consultas de banco de dados Kusto

1. Volte para o workspace **RTI_username**. Você deve ver o novo Eventstream que acabou de criar ao lado de todos os itens Evenhouse.

2. Abra o item do Banco de Dados KQL **eh_Fabrikam**.

3. Dentro dessa experiência, você pode obter uma visão geral da estrutura atual, tamanho e uso do Banco de Dados KQL. Como o Eventstream está enviando dados para esse Banco de Dados KQL de forma consistente, você notará que a quantidade de armazenamento aumentará com o tempo.

4. Clique no **ícone de atualização** no canto superior direito da tela.

5. O tamanho do banco de dados deve ter aumentado. O valor que você vê pode não ser exato em comparação com as capturas de tela no restante do laboratório. Dependendo de quanto tempo você levar para concluir o conteúdo, terá recebido menos ou mais registros do que outros membros da classe. Isso não representa problema nenhum e não afetará sua capacidade de acompanhar de forma alguma.

6. Na área de navegação do banco de dados, no lado esquerdo da tela, clique na tabela dentro do seu Banco de Dados KQL, denominada **InternetSales**, e você verá uma visão geral da tabela.

7. Essa visão geral fornecerá detalhes de metadados sobre a tabela que você criou e quaisquer dados de streaming ativo com seu Eventstream. Novamente, o tamanho da tabela e o número de linhas dentro da tabela variam de aluno para aluno e não afetarão seus resultados finais deste ou de qualquer laboratório. Alguns itens adicionais para chamar a atenção nesse menu incluem:

- **Rastreador de Atividade de Dados** -- mostra o número de linhas ingeridas, a hora em que foi gerado pela última vez e o intervalo de exibição.

- **Versão Preliminar de Dados** -- mostra uma versão preliminar dos resultados da ingestão da tabela.

- **Insights do Esquema**-- inclui detalhes sobre o nome da coluna e os tipos de dados da coluna que podem ser consultados com o KQL. Também mostra a contagem dos 10 principais para os valores na coluna selecionada.

- **Detalhes da Tabela** -- mostra o tamanho de tabela Compactado e Original, a disponibilidade do OneLake, o número de linhas nas tabelas e vários outros detalhes.

8. Retorne à exibição do banco de dados e clique em **Explorar seus dados** no canto superior direito.

9. Isso abrirá o Conjunto de Consultas KQL padrão que foi criado junto com o Eventhouse. Há algumas consultas pré-escritas que já foram criadas, mas precisam de algumas pequenas personalizações. Há também há dois links para a documentação da Microsoft que podem ser úteis ao aprender KQL ou também examinar as conversões de SQL em KQL
que serão discutidas posteriormente ao longo desta aula.

10. Clique na **Linha 8** e onde a consulta disser **YOUR_TABLE_HERE**, substitua o texto pelo nome da tabela, **InternetSales**.

11. Realce as **Linhas 8 e 9** e clique no botão **Run** no canto superior esquerdo da janela.

12. A consulta usa o operador **take** para trazer de volta um número especificado de linhas. Quando a consulta for executada, ela extrairá dados da tabela InternetSales e trará de volta qualquer número de linhas que você conectou à consulta. Para este exemplo, 100 linhas retornarão somente, como uma cláusula WHERE em SQL. As linhas específicas retornadas não podem ser determinadas com esse operador e os resultados de sua consulta
variarão do resultado de outros.

13. Clique na **Linha 12** e onde a consulta disser **YOUR_TABLE_HERE**, substitua o texto pelo nome da tabela, **InternetSales**.

14. Realce as **Linhas 12 e 13** e clique no botão **Run** no canto superior esquerdo da janela.

15. Essa consulta usa o operador count. Essa consulta retornará um número agregado de registros existentes no momento da execução da consulta na tabela Banco de Dados KQL. Sinta-se à vontade para executar essa consulta mais algumas vezes e você deverá notar que o número de registros aumenta a cada poucos segundos.

16. Repita as etapas anteriores para a consulta final criada automaticamente para você na **Linha 16/17** e execute a consulta novamente.

17. Essa consulta fornecerá o número de registros que foram ingeridos na tabela dentro de uma janela de uma hora. A distribuição geral desses registros para os dados que você está ingerindo no momento será de aproximadamente 4.100 por hora. Haverá pequenas variações dentro dos números de transações por hora, e esta consulta detalhará se menos ou mais registros foram ingeridos em cada janela.

## Tarefa 4: Usando consultas T-SQL em um Banco de Dados KQL

Talvez você esteja trabalhando com a Linguagem de Consulta Kusto pela primeira vez. Embora essa linguagem seja intuitiva e fácil de aprender para consultas simples, convém retornar os resultados de consultas mais complexas do que as que você é capaz de realizar atualmente. Várias ferramentas úteis foram incluídas nos recursos do Conjunto de Consultas KQL, incluindo a conversão de consultas SQL em consultas KQL e a simples criação de consultas T-SQL dentro do Conjunto de Consultas KQL. Vamos explorar!

1. Você precisa criar uma consulta que retorne cada produto com o número de quantas vezes ele foi vendido. Isso é algo que você pode fazer rapidamente com o T-SQL. Na janela de consulta, você pode traduzir suas consultas SQL em KQL para entender melhor como criar consultas KQL no futuro. Comece escrevendo o comando a seguir.

**(Observação: clique duas vezes no objeto abaixo para poder copiar o texto)**

 ```
 --
 explain
 ```

2. A linha de comentário "---" seguida da palavra-chave "explain" permitirá que você crie agora uma consulta SQL e retorne um resultado com a consulta KQL que poderia ser usado para obter uma consulta e resultado semelhantes. Abaixo, insira a seguinte consulta para explicar como seria a consulta KQL:

    ```
    --
    explain
    SELECT COUNT(OrderQuantity) AS CountOfProducts
    , ProductKey FROM InternetSales GROUP BY ProductKey
    ````

3. Essa é uma consulta SQL simples que recuperará resultados da tabela InternetSales para retornar duas colunas, a chave do produto (Product Key) e uma contagem do número de ordens. Como há uma coluna agregada e uma coluna não agregada, você deverá usar um GROUP BY para retornar resultados para cada produto individual. Execute toda a consulta começando com o "---" até o final da consulta T-SQL.

4. A saída da consulta explain deve ser um registro único com a consulta KQL traduzida como resultado. Clique no **ícone de circunflexo (>)** para expandir os
resultados e facilitar a tradução.

5. Clique no painel de consulta realçado abaixo em laranja. Isso permitirá que você selecione a consulta KQL traduzida e a copie. Cole essa consulta no Conjunto de Consultas KQL que temos usado

6. Com os resultados no seu painel de consulta, realce e execute a consulta para recuperar os resultados. O operador **summarize** produzirá uma tabela que agrega o conteúdo da tabela de entrada enquanto determina como agrupar cada registro **por Chave do Produto (Product Key)** e o operador **project** selecionará as colunas a serem incluídas, renomeadas ou descartadas na inserção de novas colunas computadas.

7. Sinta-se à vontade para explorar a lista de operações na folha de referências do SQL para KQL na parte superior do conjunto de consultas para obter capacidades e conversões adicionais.

8. Em vez de usar o KQL, outra alternativa para consultar os resultados do Banco de Dados KQL no Fabric é escrever e executar uma consulta T-SQL. Realce a instrução SQL original que foi usada para traduzir a consulta KQL e execute somente isso.

9. Isso também produzirá resultados perfeitamente válidos sem ter que converter para KQL de antemão.

# Conjunto de Consultas KQL

## Tarefa 5: Trabalhando com um Conjunto de Consultas KQL

1. Embora a maioria das consultas dentro desta janela tenha sido criada automaticamente a partir da interface do usuário, pode haver momentos no futuro em que você deseja criar suas próprias consultas KQL do zero. Isso pode ser gerenciado por meio do recurso de guias localizado na parte superior. Também deve ser notado que este Conjunto de Consultas é salvo automaticamente de tempos em tempos.

2. Observe na parte superior do Conjunto de Consultas que o nome padrão da nossa primeira página é o mesmo nome do nosso banco de dados.

3. Vamos seguir em frente e renomear esta guia clicando no ícone de lápis. Vamos chamá-la de "**My First KQL Query"**.

4. No futuro, se quisermos isolar nosso código, poderemos simplesmente criar guias adicionais clicando no ícone "+".

5. Retorne para o seu workspace **RTI_username**. Você deve ter os objetos a seguir presentes

# Resumo

Neste laboratório, você começou configurando uma conexão com um Hub de Eventos que tem um fluxo de dados em execução e usou um Eventstream para pegar esses dados e ingeri-los em um Banco de Dados KQL. Depois que os dados foram ingeridos, você pôde criar várias consultas KQL e examinar a funcionalidade de usar T-SQL para ajudar a aprender a sintaxe KQL ou simplesmente retornar resultados com instruções SQL.

# Referências

O Fabric Real-time Intelligence in a Day (RTIIAD) apresenta algumas das principais funções disponíveis no Microsoft Fabric.

No menu do serviço, a seção Ajuda (?) tem links para ótimos recursos.

Veja aqui mais alguns recursos que ajudarão você com as próximas etapas do Microsoft Fabric.

- Veja a postagem do blog para ler o anúncio completo da [GA do Microsof t Fabric](https://aka.ms/Fabric-Hero-Blog-Ignite23)

- Explore o Fabric por meio do [Tour Guiado](https://aka.ms/Fabric-GuidedTour)

- Inscreva-se na [avaliação gratuita do Microsoft Fabric](https://aka.ms/try-fabric)

- Visite o [site do Microsoft Fabric](https://aka.ms/microsoft-fabric)

- Aprenda novas habilidades explorando os [módulos de Aprendizagem do Fabric](https://aka.ms/learn-fabric)

- Explore a [documentação técnica do Fabric](https://aka.ms/fabric-docs)

- Leia o livro eletrônico [gratuito](https://aka.ms/fabric-get-started-ebook) [sobre como começar a usar o Fabric](https://aka.ms/fabric-get-started-ebook)

- Participe da [comunidade do Fabric](https://aka.ms/fabric-community) para postar suas perguntas, compartilhar seus comentários e aprender com outras pessoas

Leia os blogs de comunicados de experiências do Fabric em mais detalhes:

- [Experiência do Data Factory no blog do Fabric](https://aka.ms/Fabric-Data-Factory-Blog)

- [Experiência do Synapse Data Engineering no blog do Fabric](https://aka.ms/Fabric-DE-Blog)

- [Experiência do Synapse Data Science no blog do Fabric](https://aka.ms/Fabric-DS-Blog)

- [Experiência do Synapse Data Warehousing no blog do Fabric](https://aka.ms/Fabric-DW-Blog)

- [Experiência do Real-](https://aka.ms/Fabric-RTA-Blog)[Time Intelligence no blog do Fabric](https://blog.fabric.microsoft.com/en-us/blog/category/real-time-intelligence)

- [Blog de anúncio do Power BI](https://aka.ms/Fabric-PBI-Blog)

- [Experiência do Data Activator no blog do Fabric](https://aka.ms/Fabric-DA-Blog)

- [Administração e governança no blog do Fabric](https://aka.ms/Fabric-Admin-Gov-Blog)

- [OneLake](https://aka.ms/Fabric-OneLake-Blog) [no blog do Fabric](https://aka.ms/Fabric-OneLake-Blog)

- [Blog de integração do Dataverse e Microsoft Fabric](https://aka.ms/Dataverse-Fabric-Blog)

© 2024 Microsoft Corporation. Todos os direitos reservados.

Ao usar esta demonstração/este laboratório, você concorda com os seguintes termos:

A tecnologia/funcionalidade descrita nesta demonstração/neste laboratório é fornecida pela Microsoft Corporation para obter seus comentários e oferecer uma experiência de aprendizado. Você pode usar a demonstração/o laboratório somente para avaliar tais funcionalidades e recursos de tecnologia e fornecer comentários à Microsoft. Você não pode usá-los para nenhuma outra finalidade. Você não pode modificar, copiar, distribuir, transmitir, exibir, executar, reproduzir, publicar, licenciar, criar obras derivadas, transferir nem vender esta demonstração/este laboratório ou qualquer parte deles.

A CÓPIA OU A REPRODUÇÃO DA DEMONSTRAÇÃO/DO LABORATÓRIO (OU DE QUALQUER PARTE DELES) EM QUALQUER OUTRO SERVIDOR OU LOCAL PARA REPRODUÇÃO OU REDISTRIBUIÇÃO ADICIONAL É EXPRESSAMENTE PROIBIDA.

ESTA DEMONSTRAÇÃO/LABORATÓRIO FORNECE DETERMINADAS TECNOLOGIAS DE SOFTWARE/RECURSOS E FUNCIONALIDADE DO PRODUTO, INCLUINDO POTENCIAIS NOVOS RECURSOS E CONCEITOS, EM UM AMBIENTE SIMULADO SEM CONFIGURAÇÃO OU INSTALAÇÃO COMPLEXA PARA A FINALIDADE DESCRITA ACIMA. A TECNOLOGIA/CONCEITOS REPRESENTADOS NESTA DEMONSTRAÇÃO/LABORATÓRIO PODEM NÃO REPRESENTAR A FUNCIONALIDADE COMPLETA DO RECURSO E PODE NÃO FUNCIONAR DA MESMA FORMA DO QUE UMA VERSÃO FINAL. NÓS TAMBÉM PODEMOS NÃO LANÇAR UMA VERSÃO FINAL DE TAIS RECURSOS OU CONCEITOS. SUA EXPERIÊNCIA COM O USO DE TAIS RECURSOS E FUNCIONALIDADES EM UM AMBIENTE FÍSICO TAMBÉM PODE SER DIFERENTE.

**COMENTÁRIOS.** Caso você forneça comentários sobre os recursos de tecnologia, as funcionalidades e/ou os conceitos descritos nesta demonstração/neste laboratório à Microsoft, você concederá à Microsoft, sem encargos, o direito de usar, compartilhar e comercializar seus comentários de qualquer forma e para qualquer finalidade. Você também concede a terceiros, sem encargos, quaisquer direitos de patente necessários para que seus produtos, suas tecnologias e seus serviços usem ou interajam com partes específicas de um software ou um serviço da Microsoft que inclua os comentários. Você não fornecerá comentários que estejam sujeitos a uma licença que exija que a Microsoft licencie seu software ou sua documentação para terceiros em virtude da inclusão de seus comentários neles. Esses direitos continuarão em vigor após o término do contrato.

A MICROSOFT CORPORATION SE ISENTA DE TODAS AS GARANTIAS E CONDIÇÕES COM RELAÇÃO À DEMONSTRAÇÃO/LABORATÓRIO, INCLUINDO TODAS AS GARANTIAS E CONDIÇÕES DE COMERCIALIZAÇÃO, SEJAM EXPRESSAS, IMPLÍCITAS OU ESTATUTÁRIAS, ADEQUAÇÃO A UM DETERMINADO FIM, TÍTULO E NÃO VIOLAÇÃO. A MICROSOFT NÃO DECLARA NEM GARANTE A PRECISÃO DOS RESULTADOS DERIVADOS DO USO DA DEMONSTRAÇÃO/DO LABORATÓRIO NEM A ADEQUAÇÃO DAS INFORMAÇÕES CONTIDAS NA DEMONSTRAÇÃO/NO LABORATÓRIO A QUALQUER FINALIDADE.

## AVISO DE ISENÇÃO DE RESPONSABILIDADE

Esta demonstração/este laboratório contém apenas uma parte dos novos recursos e aprimoramentos do Microsoft Power BI. Alguns dos recursos podem ser alterados em versões futuras do produto. Nesta demonstração/neste laboratório, você aprenderá sobre alguns dos novos recursos, mas não todos.
