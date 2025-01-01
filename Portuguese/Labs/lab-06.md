# Sumário

- Estrutura do documento	
- Introdução	
- Criando um alerta com o Reflex	
    - Tarefa 1: Usar o Dashboard em Tempo Real para definir alertas	
    - Tarefa 2: Testar alerta de email de experiência do Reflex	
    - Tarefa 3: Criar um objeto do Reflex do fluxo de dados	
- Limpar recursos	
    - Tarefa 4: Limpar espaço de trabalho	
- Resumo	
- Referências	

# Estrutura do documento

O laboratório inclui etapas a serem seguidas pelo usuário juntamente com as capturas de tela associadas que fornecem auxílio visual. Em cada captura de tela, as seções estão destacadas com caixas laranjas para indicar as áreas nas quais o usuário deve se concentrar.

# Introdução

Neste laboratório, você aprenderá como aproveitar o Data Activator para criar um Reflex para enviar alertas do nosso recém-criado Dashboard em Tempo Real. Além disso, veremos como podemos estender o uso do Reflex para criar alertas personalizados adicionais sobre os dados que estamos
transmitindo em nosso Eventhouse

Ao final deste laboratório, você terá aprendido a:

- Criar um Reflex da Opção de Alerta em um Dashboard em Tempo Real

- Usar itens do Reflex do Data Activator para criar mais alertas personalizados

# Criando um alerta com o Reflex

## Tarefa 1: Usar o Dashboard em Tempo Real para definir alertas

1. Abra o **espaço de trabalho do Fabric** para o curso e selecione o Dashboard em Tempo Real que você criou no último laboratório.

2. No visual **Click Through Rate**, clique nas reticências (...) e selecione a opção **Set alert**.

3. Um novo submenu será aberto no lado direito da tela. Você pode ver o que está monitorando no dashboard, incluindo o visual específico ao qual o alerta será afiliado. A condição é algo sobre o qual você tem controle total. Modifique a **Condição** para **É menor que**.

4. Um novo campo será exibido para você inserir um **Valor**, modifique-o para **20**.
 
5. Você tem três ações possíveis atualmente disponíveis para o tipo de alerta que deseja receber assim que o item Reflex do Data Activator reconhecer que sua condição foi atendida. Escolha
a opção de **Envie-me uma mensagem no Teams**.
 
6. Por fim, você precisa decidir o local onde armazenará o **item do Reflex** que você está criando com esse alerta. Por padrão, isso deve selecionar seu espaço de trabalho atual, mas você precisa chamar especificamente **Criar um item** no menu suspenso Item.

7. Renomeie o item para **CTR Reflex** e clique em **Criar**. Isso levará alguns minutos para ser criado.
 
8. Você receberá uma validação de que o alerta de reflexo foi criado. Clique no botão **Em aberto** para abrir o Reflex.

9. Isso levará você à **experiência do Reflex** formal. A partir daqui, você poderá monitorar o fluxo de dados em tempo real, exibir os Dados que são usados para dar suporte ao Reflex e criar Gatilhos adicionais do mesmo fluxo.
 
## Tarefa 2: Testar alerta de email de experiência do Reflex

1. Na experiência do Reflex, clique no ícone de lápis ao lado do nome do evento e renomeie- o como **CTR is less than 20**.

2. Vamos também atualizar o **Título** e a **Mensagem**, que podem ser encontrados na seção **Ação** no lado direito. Atualize as duas áreas para corresponder à imagem abaixo e clique em **Salvar e atualizar**.
 
3. Na mesma seção **Ação**, no lado direito, clique no botão **Enviar uma ação de teste** para obter uma mensagem de exemplo em equipes do Reflex.

4. Abra uma guia Novo no Navegador Edge do Ambiente e vá para **Teams.Microsoft.com**.

5. Se for solicitado, entre com suas credenciais do ambiente. Uma mensagem para iniciar uma avaliação pode aparecer e você precisará aceitar isso.

6. Você deve ter uma mensagem dentro das equipes informando que a CTR é menor que 20.

7. Navegue até a experiência do Reflex e vamos criar outro gatilho.

## Tarefa 3: Criar um objeto do Reflex do fluxo de dados

1. Selecione o objeto rotulado **KQL Source Event** e selecione **Nova regra**.

2. Clique no ícone de **lápis** e dê a esta regra o nome **Clicks Greater Than 30,000** (você pode escolher um valor aqui que esteja mais de acordo com a quantidade de dados que você transmitiu).
 
3. Para começar, você precisa monitorar uma das colunas do fluxo de dados. Para fazer isso, precisamos configurar as seções Condição e Ação. Clique na guia Definição da regra para definir as condições e a ação.

4. Na página Definição que é aberta, em **Condição**, selecione as seguintes propriedades:

    - **Operação** = É maior que
    - **Coluna** = clicks
    - **Valor** = 30000

5. Na **Ação**, selecione as propriedades abaixo:

    - **Tipo** = Mensagem do Teams
    - **Destinatário(s)** = {sua id de usuário aqui}
 
6. Por fim, clique em **Salvar e iniciar** para iniciar esta regra.

7. Agora você tem dois gatilhos que estão monitorando o mesmo fluxo de dados.


# Limpar recursos

## Tarefa 4: Limpar espaço de trabalho

1. Este é o último laboratório e a última parte do Real-Time Analytics in a Day. Se você concluiu o laboratório e não tem mais perguntas para o instrutor sobre o conteúdo, ajude-nos desalocando o espaço de trabalho. Navegue até o espaço de trabalho **RTI_username**.
 
2. Clique nas **Configurações de workspace** no canto superior direito.

3. Nas configurações gerais do espaço de trabalho **Geral**, role para baixo e clique no botão **Remover este workspace**.

4. Laboratório e aula concluídos!
 
# Resumo

Neste laboratório, nós fizemos o passo a passo usando o Data Activator. Com esse recurso, você pode se conectar diretamente a dashboards ou a fluxos de dados em tempo real e criar gatilhos nesses dados. Esses gatilhos podem então ser configurados com condições de detecção e, uma vez que essas condições sejam atendidas, e ações podem ser realizadas. Neste laboratório, usamos a capacidade de enviar um email quando certas condições fossem atendidas em nossos gatilhos. O Data Activator ainda está em versão preliminar, então novas ações podem ser disponibilizadas no futuro.

# Referências

O Fabric Real-Time Intelligence in a Day (RTIIAD) apresenta algumas das principais funções disponíveis no Microsoft Fabric. No menu do serviço, a seção Ajuda (?) tem links para ótimos recursos.
 
Veja aqui mais alguns recursos que ajudarão você com as próximas etapas do Microsoft Fabric.

- Veja a postagem do blog para ler o anúncio completo da [GA do Microsof t Fabric](https://aka.ms/Fabric-Hero-Blog-Ignite23)

- Explore o Fabric por meio do [Tour Guiado](https://aka.ms/Fabric-GuidedTour)

- Inscreva-se na [avaliação gratuita do Microsoft Fabric](https://aka.ms/try-fabric)

- Visite [o site do Microsoft Fabric](https://aka.ms/microsoft-fabric)

- Aprenda novas habilidades explorando os [módulos de Aprendizagem do Fabric](https://aka.ms/learn-fabric)

- Explore a [documentação técnica do Fabric](https://aka.ms/fabric-docs)

- Leia o livro eletrônico [gratuito sobre como começar a usar o Fabric](https://aka.ms/fabric-get-started-ebook)

- Participe da [comunidade do Fabric](https://aka.ms/fabric-community) para postar suas perguntas, compartilhar seus comentários e aprender com outras pessoas 

Leia os blogs de comunicados de experiências do Fabric em mais detalhes:

- [Experiência do Data Factory no blog do Fabric](https://aka.ms/Fabric-Data-Factory-Blog)

- [Experiência do Synapse Data Engineering no blog do Fabric](https://aka.ms/Fabric-DE-Blog)

- [Experiência do Synapse Data Science no blog do Fabric](https://aka.ms/Fabric-DS-Blog)

- [Experiência do Synapse Data Warehousing no blog do Fabric](https://aka.ms/Fabric-DW-Blog)

- [Experiência do Real-Time Intelligence no blog do Fabric](https://blog.fabric.microsoft.com/en-us/blog/category/real-time-intelligence)

- [Blog de anúncio do Power BI](https://aka.ms/Fabric-PBI-Blog)

- [Experiência do Data Activator no blog do Fabric](https://aka.ms/Fabric-DA-Blog)

- [Administração e governança no blog do Fabric](https://aka.ms/Fabric-Admin-Gov-Blog)

- [OneLake no blog do Fabric](https://aka.ms/Fabric-OneLake-Blog)

- [Blog de integração do Dataverse e Microsoft Fabric](https://aka.ms/Dataverse-Fabric-Blog)


© 2024 Microsoft Corporation. Todos os direitos reservados.

Ao usar esta demonstração/este laboratório, você concorda com os seguintes termos:

A tecnologia/funcionalidade descrita nesta demonstração/neste laboratório é fornecida pela Microsoft Corporation para obter seus comentários e oferecer uma experiência de aprendizado.
Você pode usar a demonstração/o laboratório somente para avaliar tais funcionalidades e recursos de tecnologia e fornecer comentários à Microsoft. Você não pode usá-los para nenhuma outra finalidade. Você não pode modificar, copiar, distribuir, transmitir, exibir, executar, reproduzir, publicar, licenciar, criar obras derivadas, transferir nem vender esta demonstração/este laboratório ou qualquer parte deles.

A CÓPIA OU A REPRODUÇÃO DA DEMONSTRAÇÃO/DO LABORATÓRIO (OU DE QUALQUER PARTE DELES) EM QUALQUER OUTRO SERVIDOR OU LOCAL PARA REPRODUÇÃO OU REDISTRIBUIÇÃO ADICIONAL É EXPRESSAMENTE PROIBIDA.

ESTA DEMONSTRAÇÃO/LABORATÓRIO FORNECE DETERMINADAS TECNOLOGIAS DE
SOFTWARE/RECURSOS E FUNCIONALIDADE DO PRODUTO, INCLUINDO POTENCIAIS NOVOS RECURSOS E CONCEITOS, EM UM AMBIENTE SIMULADO SEM CONFIGURAÇÃO OU INSTALAÇÃO COMPLEXA PARA A FINALIDADE DESCRITA ACIMA. A TECNOLOGIA/CONCEITOS REPRESENTADOS NESTA DEMONSTRAÇÃO/LABORATÓRIO PODEM NÃO REPRESENTAR A FUNCIONALIDADE
COMPLETA DO RECURSO E PODE NÃO FUNCIONAR DA MESMA FORMA DO QUE UMA VERSÃO FINAL. NÓS TAMBÉM PODEMOS NÃO LANÇAR UMA VERSÃO FINAL DE TAIS RECURSOS OU CONCEITOS. SUA EXPERIÊNCIA COM O USO DE TAIS RECURSOS E FUNCIONALIDADES EM UM AMBIENTE FÍSICO TAMBÉM PODEM SER DIFERENTE.
 
**COMENTÁRIOS**. Caso você forneça comentários sobre os recursos de tecnologia, as funcionalidades e/ou os conceitos descritos nesta demonstração/neste laboratório à Microsoft, você concederá à Microsoft, sem encargos, o direito de usar, compartilhar e comercializar seus comentários de qualquer forma e para qualquer finalidade. Você também concede a terceiros, sem encargos, quaisquer direitos de patente necessários para que seus produtos, suas tecnologias e seus serviços usem ou interajam com partes específicas de um software ou um serviço da Microsoft que inclua os comentários. Você não fornecerá comentários que estejam
sujeitos a uma licença que exija que a Microsoft licencie seu software ou sua documentação para terceiros em virtude da inclusão de seus comentários neles. Esses direitos continuarão em vigor após o término do contrato.

A MICROSOFT CORPORATION SE ISENTA DE TODAS AS GARANTIAS E CONDIÇÕES COM RELAÇÃO A DEMONSTRAÇÃO/LABORATÓRIO, INCLUINDO TODAS AS GARANTIAS E CONDIÇÕES DE COMERCIALIZAÇÃO, SEJAM EXPRESSAS, IMPLÍCITAS OU ESTATUTÁRIAS, ADEQUAÇÃO A UM DETERMINADO FIM, TÍTULO E NÃO VIOLAÇÃO. A MICROSOFT NÃO DECLARA NEM GARANTE A PRECISÃO DOS RESULTADOS DERIVADOS DO USO DA DEMONSTRAÇÃO/DO LABORATÓRIO NEM A ADEQUAÇÃO DAS INFORMAÇÕES CONTIDAS NA DEMONSTRAÇÃO/NO LABORATÓRIO A QUALQUER FINALIDADE.

# AVISO DE ISENÇÃO DE RESPONSABILIDADE

Esta demonstração/este laboratório contém apenas uma parte dos novos recursos e aprimoramentos do Microsoft Power BI. Alguns dos recursos podem ser alterados em versões futuras do produto. Nesta demonstração/neste laboratório, você aprenderá sobre alguns dos novos recursos, mas não todos.
 
