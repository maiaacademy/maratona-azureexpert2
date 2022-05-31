# Maratona Azure Expert 2.0

**Sejam muito bem-vindo(a) a Maratona Azure Expert 2.0**

Um evento ao Vivo e 100% gratuito

Este é o repositório que contém as atividades, Hands-on e desafios da Maratona.

## Desafio 1 - Criação do ambiente de Aplicação (On-premises) no Azure (tempo estimado de 30 a 60 minutos)

Para realizar as atividades do Hands-on estamos utilizando o Portal do Azure em idioma Inglês a fim de manter o padrão e não haver erros, e se necessário você pode abrir o Google Tradutor e traduzir caso tenha alguma dificuldade no entendimento.

## Requisitos para criação do ambiente na sua assinatura do Azure

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

    > **Note:** If you are using an Azure Pass subscription, you may not meet the vCPU quotas above. In this case, you can still complete the lab.

## Crição do ambiente da Aplicação completa (On-premises) para Cloud Azure

1. Deploy the template **SmartHotelHost.json** to a new resource group. This template deploys a virtual machine running nested Hyper-V, with 4 nested VMs. This comprises the 'on-premises' environment which you will assess and migrate during this lab.

    You can deploy the template by selecting the 'Deploy to Azure' button below. You will need to create a new resource group **SmartHotel**. You will also need to select a location **East US** or **West US** or **Central US** or other close to you to deploy the template to. Then choose **Review + create** followed by **Create**. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fcloudworkshop.blob.core.windows.net%2Fline-of-business-application-migration%2Fsept-2020%2FSmartHotelHost.json" target="_blank">![Button to deploy the SmartHotelHost template to Azure.](/AllFiles/Images/deploy-to-azure.png)</a>

    > **Note:** The template will take around 6-7 minutes to deploy. Once template deployment is complete, several additional scripts are executed to bootstrap the lab environment. **Allow at least 1 hour from the start of template deployment for the scripts to run.**

### Verifique o andamento da criação do ambiente da aplicação

1. Navigate to the **SmartHotelHost** VM that was deployed by the template in the previous step.

1. In a separate browser tab, navigate to the **Azure portal**. In the global search box, enter **SmartHotelHost**, then select the SmartHotelHost virtual machine.

1. Select **Connect**, select **RDP**, then download the RDP file and connect to the virtual machine using.
        - Username: **demouser**
        - Password: **demo!pass123**

1. In **Server Manager**, select Tools, then **Hyper-V Manager** (if Server Manager does not open automatically, open it by selecting Start, then Server Manager). In Hyper-V Manager, select SMARTHOTELHOST. You should now see a list of the four VMs that comprise the on-premises SmartHotel application.

    > **Note:** If the SmartHotel application VMs in Hyper-V Manager is not shown, wait 30 minutes and try again. It takes **at least 1 hour** from the start of template deployment. You can also check the CPU, network and disk activity levels for the SmartHotelHost VM in the Azure portal, to see if the provisioning is still active.

2. Make a note of the public IP address.

3. Open a browser tab and navigate to **http://\<SmartHotelHostIP-Address\>**. You should see the SmartHotel application, which is running on nested VMs within Hyper-V on the SmartHotelHost. (The application doesn't do much: you can refresh the page to see the list of guests or select 'CheckIn' or 'CheckOut' to toggle their status.)

    ![Browser screenshot showing the SmartHotel application.](/AllFiles/Images/smarthotel.png)

1. Assim que a Aplicação estiver funcionando para completar o desafio você vai postar o **Print das evidências do ambiente e a imagem do Badge** ([Clique aqui para baixar](https://guilhermemaia.com/badge-maratona)) **com a Hastash** #MaratonaAzureExpert2 #Desafio1 **no Linkedin**.

## Arquitetura do Projeto Hands-on da Migração da Aplicação completa (On-premises) para o Azure

A Aplicação SmartHotel é composta por uma arquitetura em N camadas (N-Tier) no Hyper-V.

- **Database tier** É a VM **smarthotelSQL1** da camada de Dados, executa o Windows Server 2016 e SQL Server 2017.

- **Application tier** É a VM **smarthotelweb2** da camada de Aplicação, executa o Windows Server 2012 R2 e IIS Server.

- **Web tier** É a VM **smarthotelweb1** da camada Web, executa o Windows Server 2012 R2 e IIS Server.

- **Web proxy** É a VM **UbuntuWAF** da camada de Proxy e Firewall (WAF), executa o Nginx no Ubuntu 18.04 LTS.

![A slide shows the on-premises SmartHotel application architecture.](/AllFiles/Images/overview.png)

1. Continua na próxima aula. Quarta (01/06) às 20h00 (horário de Brasília)

Inscreva-se para receber os materiais extras e o link das próximas aulas: [Clicando aqui no link](htts://guilhermemaia.com/inscricoes-maratona-jun22).

