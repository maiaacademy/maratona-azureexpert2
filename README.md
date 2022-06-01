# Maratona Azure Expert 2.0

Este é o repositório que contém os materiais de apoio da Maratona.

**Fique por dentro da programação do evento**, **[clicando aqui]**(https://guilhermemaia.com/programacao-maratona-jun22/) 

- **AULA 1** - Segunda (30/05) às 20h00
- **AULA 2** - Quarta (01/06) às 20h00
- **AULA 3** - Sexta (03/06) às 20h00
- **AULA 4** - Domingo (05/06) às 20h00

**Aproveite e defina o lembrete das aulas** no Youtube para ser notificado quando a aula começar.

Para realizar as atividades do Hands-on estamos utilizando o Portal do Azure no idioma Inglês a fim de manter o padrão e não haver erros, e se necessário você pode abrir o Google Tradutor e traduzir caso tenha alguma dificuldade no entendimento.

## Desafio 1 - Criação do ambiente de Aplicação (On-premises) no Azure (tempo estimado de 30 a 60 minutos)

Neste desafio você vai criar o ambiente da aplicação que vamos utilizar no projeto de migração de uma aplicação completa (On-premises) para Cloud.

### Requisitos para criação do ambiente na sua assinatura do Azure

1. You will need Owner or Contributor permissions for an Azure subscription to use in the lab.

2. Your subscription must have sufficient unused quota to deploy the VMs used in this lab. To check your quota:

    - Log in to the [Azure portal](https://portal.azure.com), select **All services** then **Subscriptions**. Select your subscription, then choose **Usage + quotas**.
  
    - From the **Select a provider** drop-down, select **Microsoft.Compute**.
  
    - From the **All service quotas** drop down, select **Standard DSv3 Family vCPUs**, **Standard FSv2 Family vCPUs** and **Total Regional vCPUs**.
  
    - From the **All locations** drop down, select the location where you will deploy the lab.
  
    - From the last drop-down, select **Show all**.
  
    - Check that the selected quotas have sufficient unused capacity:
  
        - Standard DSv3 Family vCPUs: **at least 8 vCPUs**.
  
        - Standard FSv2 Family vCPUs: **at least 6 vCPUs**.

        - Total Regional vCPUs: **at least 14 vCPUs**.

     > **NOTA:** Se você está utilizando uma **assinatura gratuita (Trial)** ou **Azure Pass** você não vai conseguir aumentar o limite de processadores (vCPUs) e sendo necessário seguir as instruções a seguir**

### Criação do ambiente da Aplicação completa (On-premises) para Cloud

1. Deploy the template **SmartHotelHost.json** to a new resource group. This template deploys a virtual machine running nested Hyper-V, with 4 nested VMs. This comprises the 'on-premises' environment which you will assess and migrate during this lab.

    You can deploy the template by selecting the 'Deploy to Azure' button below. You will need to create a new resource group **SmartHotel**. You will also need to select a location **East US**, **Central US**, **Australia ...**, **Canada ...**, **Europe ...** or other close to you to deploy the template to. Then choose **Review + create** followed by **Create**. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fcloudworkshop.blob.core.windows.net%2Fline-of-business-application-migration%2Fsept-2020%2FSmartHotelHost.json" target="_blank">![Button to deploy the SmartHotelHost template to Azure.](/AllFiles/Images/deploy-to-azure.png)</a>

    > **NOTA IMPORTANTE:** Em caso de restrição de **Quota de vCPUs** ou **Restrição da região** mesmo selecionando outras regiões, você vai precisar alterar a quantidade de processadores devido aos limites da assinatura Trial ou Azure Pass, iniciando novamente o processo de criação do template, selecionando em **Edit template**, localizar a **linha 48** que contém **Standard_D8s_v3** e vai substituir para **Standard_D4s_v3**, e em seguida selecione **Save** e continue o processo de criação pelo template.

    > **NOTA:** O Template uma vez executado levará cerca de 7 a 10 minutos para ser implantado. Após a conclusão da implantação do modelo, vários scripts adicionais são executados para configurar o ambiente da aplicação completa. **Aguarde pelo menos 1 hora a partir do início da implantação Template para que os scripts sejam executados.**

### Verifique o andamento da criação do ambiente da aplicação

1. Navigate to the **SmartHotelHost** VM that was deployed by the template in the previous step.

1. In a separate browser tab, navigate to the **Azure portal**. In the global search box, enter **SmartHotelHost**, then select the SmartHotelHost virtual machine.

1. Select **Connect**, select **RDP**, then download the RDP file and connect to the virtual machine using.
        - Username: **demouser**
        - Password: **demo!pass123**

1. In **Server Manager**, select Tools, then **Hyper-V Manager** (if Server Manager does not open automatically, open it by selecting Start, then Server Manager). In Hyper-V Manager, select SMARTHOTELHOST. You should now see a list of the four VMs that comprise the on-premises SmartHotel application.

     > **NOTA:** Se as VMs não estiverem aparecendo no Hyper-V Manager, aguarde 30 minutos e tente novamente. Leva **pelo menos 1 hora** desde o início da implantação do modelo. Você também pode verificar as atividade de CPU, rede e disco da VM **SmartHotelHost** no Portal do Azure, para ver se a criação ainda está andamento.

2. Make a note of the public IP address.

3. Open a browser tab and navigate to **http://\<SmartHotelHostIP-Address\>**. You should see the SmartHotel application, which is running on nested VMs within Hyper-V on the SmartHotelHost. (The application doesn't do much: you can refresh the page to see the list of guests or select 'CheckIn' or 'CheckOut' to toggle their status.)

    ![Browser screenshot showing the SmartHotel application.](/AllFiles/Images/smarthotel.png)

Assim que a Aplicação estiver funcionando para completar o desafio você vai postar o **Print das evidências do ambiente e a imagem do Badge** da Maratona ([Clique aqui para baixar](https://guilhermemaia.com/badge-maratona)) **com a Hastash** #MaratonaAzureExpert2 #Desafio1 **no Linkedin**.

## Arquitetura do Projeto Hands-on da Migração da Aplicação completa (On-premises) para o Azure

A Aplicação SmartHotel é composta por uma arquitetura em N camadas (N-Tier) no Hyper-V.

- **Database tier** **SmartHotelSQL1** é a VM da camada de Dados, executa o Windows Server 2016 e SQL Server 2017.

- **Application tier** **SmartHotelWeb2** é a VM da camada de Aplicação, executa o Windows Server 2012 R2 e IIS Server.

- **Web tier** **SmartHotelWeb1** é a VM da camada Web, executa o Windows Server 2012 R2 e IIS Server.

- **Web proxy** **UbuntuWAF** é a VM da camada de Proxy e Firewall (WAF), executa o Nginx no Ubuntu 18.04 LTS.

![A slide shows the on-premises SmartHotel application architecture.](/AllFiles/Images/overview.png)

## Desafio 2 - Implantação do ambiente no Azure do Z (tempo estimado de 60 minutos a 120 minutos)

...

Inscreva-se para receber o link das próximas aulas: [Clicando aqui](https://guilhermemaia.com/inscricoes-maratona-jun22).




