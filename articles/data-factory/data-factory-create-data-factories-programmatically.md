---
title: "Vytvoření datových kanálů pomocí sady Azure .NET SDK | Microsoft Docs"
description: "Naučte se vytvořit prostřednictvím kódu programu, sledovat a spravovat objekty pro vytváření dat Azure pomocí sady SDK Data Factory."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: b0a357be-3040-4789-831e-0d0a32a0bda5
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 9d9dac75321c5d4e079f49320d9b7c6f56e48754
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="create-monitor-and-manage-azure-data-factories-using-azure-data-factory-net-sdk"></a><span data-ttu-id="34aa0-103">Vytvoření, sledovat a spravovat Azure data Factory pomocí .NET SDK služby Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="34aa0-103">Create, monitor, and manage Azure data factories using Azure Data Factory .NET SDK</span></span>
## <a name="overview"></a><span data-ttu-id="34aa0-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="34aa0-104">Overview</span></span>
<span data-ttu-id="34aa0-105">Můžete vytvořit, sledovat a spravovat Azure data Factory programově pomocí .NET SDK služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="34aa0-105">You can create, monitor, and manage Azure data factories programmatically using Data Factory .NET SDK.</span></span> <span data-ttu-id="34aa0-106">Tento článek obsahuje návod, který můžete přejít k vytvoření ukázkové aplikace konzoly .NET, která vytvoří a sleduje služby data factory.</span><span class="sxs-lookup"><span data-stu-id="34aa0-106">This article contains a walkthrough that you can follow to create a sample .NET console application that creates and monitors a data factory.</span></span> 

> [!NOTE]
> <span data-ttu-id="34aa0-107">Tento článek nepopisuje všechny možnosti rozhraní .NET API služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="34aa0-107">This article does not cover all the Data Factory .NET API.</span></span> <span data-ttu-id="34aa0-108">V tématu [Data Factory .NET API – referenční informace](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) úplnou dokumentaci o .NET API pro Data Factory.</span><span class="sxs-lookup"><span data-stu-id="34aa0-108">See [Data Factory .NET API Reference](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) for comprehensive documentation on .NET API for Data Factory.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="34aa0-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="34aa0-109">Prerequisites</span></span>
* <span data-ttu-id="34aa0-110">Visual Studio 2012 nebo 2013 nebo 2015</span><span class="sxs-lookup"><span data-stu-id="34aa0-110">Visual Studio 2012 or 2013 or 2015</span></span>
* <span data-ttu-id="34aa0-111">Stáhněte a nainstalujte [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="34aa0-111">Download and install [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span></span>
* <span data-ttu-id="34aa0-112">Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="34aa0-112">Azure PowerShell.</span></span> <span data-ttu-id="34aa0-113">Podle pokynů v článku [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) si na počítač nainstalujte prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="34aa0-113">Follow instructions in [How to install and configure Azure PowerShell](/powershell/azure/overview) article to install Azure PowerShell on your computer.</span></span> <span data-ttu-id="34aa0-114">K vytvoření aplikace v Azure Active Directory použijete Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="34aa0-114">You use Azure PowerShell to create an Azure Active Directory application.</span></span>

### <a name="create-an-application-in-azure-active-directory"></a><span data-ttu-id="34aa0-115">Vytvoření aplikace v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="34aa0-115">Create an application in Azure Active Directory</span></span>
<span data-ttu-id="34aa0-116">Vytvořte aplikaci Azure Active Directory, vytvořte pro ni instanční objekt a přiřaďte ho roli **Přispěvatel Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-116">Create an Azure Active Directory application, create a service principal for the application, and assign it to the **Data Factory Contributor** role.</span></span>

1. <span data-ttu-id="34aa0-117">Spusťte **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-117">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="34aa0-118">Spusťte následující příkaz a zadejte uživatelské jméno a heslo, které používáte k přihlášení na web Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="34aa0-118">Run the following command and enter the user name and password that you use to sign in to the Azure portal.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="34aa0-119">Spuštěním následujícího příkazu zobrazíte všechna předplatná pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="34aa0-119">Run the following command to view all the subscriptions for this account.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. <span data-ttu-id="34aa0-120">Spuštěním následujícího příkazu vyberte předplatné, se kterým chcete pracovat.</span><span class="sxs-lookup"><span data-stu-id="34aa0-120">Run the following command to select the subscription that you want to work with.</span></span> <span data-ttu-id="34aa0-121">Místo **&lt;NameOfAzureSubscription**&gt; zadejte název svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="34aa0-121">Replace **&lt;NameOfAzureSubscription**&gt; with the name of your Azure subscription.</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="34aa0-122">Poznamenejte si **SubscriptionId** a **TenantId** z výstupu tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="34aa0-122">Note down **SubscriptionId** and **TenantId** from the output of this command.</span></span>

5. <span data-ttu-id="34aa0-123">Spuštěním následujícího příkazu v PowerShellu vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-123">Create an Azure resource group named **ADFTutorialResourceGroup** by running the following command in the PowerShell.</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    <span data-ttu-id="34aa0-124">Pokud skupina prostředků už existuje, určete, jestli se má aktualizovat (Y), nebo ponechat tak, jak je (N).</span><span class="sxs-lookup"><span data-stu-id="34aa0-124">If the resource group already exists, you specify whether to update it (Y) or keep it as (N).</span></span>

    <span data-ttu-id="34aa0-125">Pokud používáte jinou skupinu prostředků, použijte v postupech v tomto kurzu místo skupiny ADFTutorialResourceGroup název vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="34aa0-125">If you use a different resource group, you need to use the name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>
6. <span data-ttu-id="34aa0-126">Vytvořte aplikaci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="34aa0-126">Create an Azure Active Directory application.</span></span>

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFDotNetWalkthroughApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfdotnetwalkthroughapp.org/example" -Password "Pass@word1"
    ```

    <span data-ttu-id="34aa0-127">Pokud se zobrazí následující chyba, zadejte jinou adresu URL a spusťte příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="34aa0-127">If you get the following error, specify a different URL and run the command again.</span></span>
    
    ```PowerShell
    Another object with the same value for property identifierUris already exists.
    ```
7. <span data-ttu-id="34aa0-128">Vytvořte instanční objekt služby AD.</span><span class="sxs-lookup"><span data-stu-id="34aa0-128">Create the AD service principal.</span></span>

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. <span data-ttu-id="34aa0-129">Přidejte instanční objekt k roli **Přispěvatel Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-129">Add service principal to the **Data Factory Contributor** role.</span></span>

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. <span data-ttu-id="34aa0-130">Získejte ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="34aa0-130">Get the application ID.</span></span>

    ```PowerShell
    $azureAdApplication 
    ```
    <span data-ttu-id="34aa0-131">Poznamenejte si ID aplikace (applicationID) ve výstupu.</span><span class="sxs-lookup"><span data-stu-id="34aa0-131">Note down the application ID (applicationID) from the output.</span></span>

<span data-ttu-id="34aa0-132">Z těchto kroků byste měli mít tyto čtyři hodnoty:</span><span class="sxs-lookup"><span data-stu-id="34aa0-132">You should have following four values from these steps:</span></span>

* <span data-ttu-id="34aa0-133">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="34aa0-133">Tenant ID</span></span>
* <span data-ttu-id="34aa0-134">ID předplatného</span><span class="sxs-lookup"><span data-stu-id="34aa0-134">Subscription ID</span></span>
* <span data-ttu-id="34aa0-135">ID aplikace</span><span class="sxs-lookup"><span data-stu-id="34aa0-135">Application ID</span></span>
* <span data-ttu-id="34aa0-136">Heslo (zadané v prvním příkazu)</span><span class="sxs-lookup"><span data-stu-id="34aa0-136">Password (specified in the first command)</span></span>

## <a name="walkthrough"></a><span data-ttu-id="34aa0-137">Názorný postup</span><span class="sxs-lookup"><span data-stu-id="34aa0-137">Walkthrough</span></span>
<span data-ttu-id="34aa0-138">V tomto návodu vytvoříte objekt pro vytváření dat kanál, který obsahuje aktivitu kopírování.</span><span class="sxs-lookup"><span data-stu-id="34aa0-138">In the walkthrough, you create a data factory with a pipeline that contains a copy activity.</span></span> <span data-ttu-id="34aa0-139">Aktivita kopírování kopíruje data ze složky ve službě Azure blob storage do jiné složky v stejné úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="34aa0-139">The copy activity copies data from a folder in your Azure blob storage to another folder in the same blob storage.</span></span> 

<span data-ttu-id="34aa0-140">Aktivita kopírování provádí přesun dat ve službě Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="34aa0-140">The Copy Activity performs the data movement in Azure Data Factory.</span></span> <span data-ttu-id="34aa0-141">Aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem.</span><span class="sxs-lookup"><span data-stu-id="34aa0-141">The activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="34aa0-142">Podrobnosti o aktivitě kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="34aa0-142">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about the Copy Activity.</span></span>

1. <span data-ttu-id="34aa0-143">Pomocí sady Visual Studio 2012/2013/2015 vytvořte konzolovou aplikaci C# .NET.</span><span class="sxs-lookup"><span data-stu-id="34aa0-143">Using Visual Studio 2012/2013/2015, create a C# .NET console application.</span></span>
   1. <span data-ttu-id="34aa0-144">Spusťte **Visual Studio** 2012/2013/2015.</span><span class="sxs-lookup"><span data-stu-id="34aa0-144">Launch **Visual Studio** 2012/2013/2015.</span></span>
   2. <span data-ttu-id="34aa0-145">Klikněte na **Soubor**, přejděte na **Nový** a klikněte na **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-145">Click **File**, point to **New**, and click **Project**.</span></span>
   3. <span data-ttu-id="34aa0-146">Rozbalte **Šablony** a vyberte **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-146">Expand **Templates**, and select **Visual C#**.</span></span> <span data-ttu-id="34aa0-147">V tomto názorném postupu použijete C#, ale mohli byste využít libovolný jazyk .NET.</span><span class="sxs-lookup"><span data-stu-id="34aa0-147">In this walkthrough, you use C#, but you can use any .NET language.</span></span>
   4. <span data-ttu-id="34aa0-148">V seznamu typů projektů napravo vyberte **Konzolová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-148">Select **Console Application** from the list of project types on the right.</span></span>
   5. <span data-ttu-id="34aa0-149">Jako název zadejte **DataFactoryAPITestApp**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-149">Enter **DataFactoryAPITestApp** for the Name.</span></span>
   6. <span data-ttu-id="34aa0-150">Jako umístění vyberte **C:\ADFGetStarted**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-150">Select **C:\ADFGetStarted** for the Location.</span></span>
   7. <span data-ttu-id="34aa0-151">Projekt vytvoříte kliknutím na **OK**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-151">Click **OK** to create the project.</span></span>
2. <span data-ttu-id="34aa0-152">Klikněte na **Nástroje**, přejděte na **Správce balíčků NuGet** a klikněte na **Konzola Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-152">Click **Tools**, point to **NuGet Package Manager**, and click **Package Manager Console**.</span></span>
3. <span data-ttu-id="34aa0-153">V **Konzole Správce balíčků** postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="34aa0-153">In the **Package Manager Console**, do the following steps:</span></span>
   1. <span data-ttu-id="34aa0-154">Spusťte následující příkaz a nainstalujte balíček služby Data Factory: `Install-Package Microsoft.Azure.Management.DataFactories`</span><span class="sxs-lookup"><span data-stu-id="34aa0-154">Run the following command to install Data Factory package: `Install-Package Microsoft.Azure.Management.DataFactories`</span></span>
   2. <span data-ttu-id="34aa0-155">Spusťte následující příkaz pro instalaci balíčku Azure Active Directory (v kódu použijete rozhraní API Active Directory): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span><span class="sxs-lookup"><span data-stu-id="34aa0-155">Run the following command to install Azure Active Directory package (you use Active Directory API in the code): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span></span>
4. <span data-ttu-id="34aa0-156">Nahraďte obsah **App.config** v projektu s následujícím obsahem:</span><span class="sxs-lookup"><span data-stu-id="34aa0-156">Replace the contents of **App.config** file in the project with the following content:</span></span> 
    
    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <add key="ApplicationId" value="your application ID" />
            <add key="Password" value="Password you used while creating the AAD application" />
            <add key="SubscriptionId" value= "Subscription ID" />
            <add key="ActiveDirectoryTenantId" value="Tenant ID" />
        </appSettings>
    </configuration>
    ```
5. <span data-ttu-id="34aa0-157">V souboru App.Config aktualizujte hodnoty pro  **&lt;ID aplikace&gt;**,  **&lt;heslo&gt;**,  **&lt;předplatného ID&gt;**, a  **&lt;ID klienta&gt;**  vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="34aa0-157">In the App.Config file, update values for **&lt;Application ID&gt;**, **&lt;Password&gt;**, **&lt;Subscription ID&gt;**, and **&lt;tenant ID&gt;** with your own values.</span></span>
6. <span data-ttu-id="34aa0-158">Přidejte následující **pomocí** příkazy **Program.cs** v projektu.</span><span class="sxs-lookup"><span data-stu-id="34aa0-158">Add the following **using** statements to the **Program.cs** file in the project.</span></span>

    ```csharp
    using System.Configuration;
    using System.Collections.ObjectModel;
    using System.Threading;
    using System.Threading.Tasks;

    using Microsoft.Azure;
    using Microsoft.Azure.Management.DataFactories;
    using Microsoft.Azure.Management.DataFactories.Models;
    using Microsoft.Azure.Management.DataFactories.Common.Models;

    using Microsoft.IdentityModel.Clients.ActiveDirectory;

    ```
6. <span data-ttu-id="34aa0-159">Do metody **Main** přidejte následující kód, který vytvoří instanci třídy **DataPipelineManagementClient**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-159">Add the following code that creates an instance of **DataPipelineManagementClient** class to the **Main** method.</span></span> <span data-ttu-id="34aa0-160">Tento objekt použijete k vytvoření objektu pro vytváření dat, propojené služby, vstupních a výstupních datových sad a kanálu.</span><span class="sxs-lookup"><span data-stu-id="34aa0-160">You use this object to create a data factory, a linked service, input and output datasets, and a pipeline.</span></span> <span data-ttu-id="34aa0-161">Použijete ho také k monitorování řezů datových sad při spuštění.</span><span class="sxs-lookup"><span data-stu-id="34aa0-161">You also use this object to monitor slices of a dataset at runtime.</span></span>

    ```csharp
    // create data factory management client

    //IMPORTANT: specify the name of Azure resource group here
    string resourceGroupName = "ADFTutorialResourceGroup";

    //IMPORTANT: the name of the data factory must be globally unique.
    // Therefore, update this value. For example:APITutorialFactory05122017
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="34aa0-162">Hodnotu **resourceGroupName** nahraďte názvem skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="34aa0-162">Replace the value of **resourceGroupName** with the name of your Azure resource group.</span></span> <span data-ttu-id="34aa0-163">Můžete vytvořit skupinu prostředků pomocí [New-AzureResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) rutiny.</span><span class="sxs-lookup"><span data-stu-id="34aa0-163">You can create a resource group using the [New-AzureResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet.</span></span>
   >
   > <span data-ttu-id="34aa0-164">Aktualizujte název datové továrny (dataFactoryName) tak, aby byl jedinečný.</span><span class="sxs-lookup"><span data-stu-id="34aa0-164">Update name of the data factory (dataFactoryName) to be unique.</span></span> <span data-ttu-id="34aa0-165">Název objektu pro vytváření dat musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="34aa0-165">Name of the data factory must be globally unique.</span></span> <span data-ttu-id="34aa0-166">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="34aa0-166">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
7. <span data-ttu-id="34aa0-167">Do metody **Main** přidejte následující kód, který vytvoří **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-167">Add the following code that creates a **data factory** to the **Main** method.</span></span>

    ```csharp
    // create a data factory
    Console.WriteLine("Creating a data factory");
    client.DataFactories.CreateOrUpdate(resourceGroupName,
        new DataFactoryCreateOrUpdateParameters()
        {
            DataFactory = new DataFactory()
            {
                Name = dataFactoryName,
                Location = "westus",
                Properties = new DataFactoryProperties()
            }
        }
    );
    ```
8. <span data-ttu-id="34aa0-168">Do metody **Main** přidejte následující kód, který vytvoří **propojenou službu Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-168">Add the following code that creates an **Azure Storage linked service** to the **Main** method.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="34aa0-169">Položky **storageaccountname** a **accountkey** nahraďte názvem svého účtu Azure Storage a jeho klíčem.</span><span class="sxs-lookup"><span data-stu-id="34aa0-169">Replace **storageaccountname** and **accountkey** with name and key of your Azure Storage account.</span></span>

    ```csharp
    // create a linked service for input data store: Azure Storage
    Console.WriteLine("Creating Azure Storage linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "AzureStorageLinkedService",
                Properties = new LinkedServiceProperties
                (
                    new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                )
            }
        }
    );
    ```
9. <span data-ttu-id="34aa0-170">Do metody **Main** přidejte následující kód, který vytvoří **vstupní a výstupní datové sady**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-170">Add the following code that creates **input and output datasets** to the **Main** method.</span></span>

    <span data-ttu-id="34aa0-171">**FolderPath** vstupního objektu blob je nastavený na **adftutorial /** kde **adftutorial** je název kontejneru ve službě blob storage.</span><span class="sxs-lookup"><span data-stu-id="34aa0-171">The **FolderPath** for the input blob is set to **adftutorial/** where **adftutorial** is the name of the container in your blob storage.</span></span> <span data-ttu-id="34aa0-172">Pokud tento kontejner ve službě Azure blob storage neexistuje, vytvořit kontejner s tímto názvem: **adftutorial** a odešlete textový soubor do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="34aa0-172">If this container does not exist in your Azure blob storage, create a container with this name: **adftutorial** and upload a text file to the container.</span></span>

    <span data-ttu-id="34aa0-173">FolderPath pro výstup objektů blob je nastavena na: **adftutorial/apifactoryoutput / {řezu}** kde **řez** je počítáno dynamicky podle hodnotu **SliceStart** () Spusťte každý řez datum a čas.)</span><span class="sxs-lookup"><span data-stu-id="34aa0-173">The FolderPath for the output blob is set to: **adftutorial/apifactoryoutput/{Slice}** where **Slice** is dynamically calculated based on the value of **SliceStart** (start date-time of each slice.)</span></span>

    ```csharp
    // create input and output datasets
    Console.WriteLine("Creating input and output datasets");
    string Dataset_Source = "DatasetBlobSource";
    string Dataset_Destination = "DatasetBlobDestination";
    
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Source,
            Properties = new DatasetProperties()
            {
                LinkedServiceName = "AzureStorageLinkedService",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/",
                    FileName = "emp.txt"
                },
                External = true,
                Availability = new Availability()
                {
                    Frequency = SchedulePeriod.Hour,
                    Interval = 1,
                },
    
                Policy = new Policy()
                {
                    Validation = new ValidationPolicy()
                    {
                        MinimumRows = 1
                    }
                }
            }
        }
    });
    
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Destination,
            Properties = new DatasetProperties()
            {
    
                LinkedServiceName = "AzureStorageLinkedService",
                TypeProperties = new AzureBlobDataset()
                {
                    FolderPath = "adftutorial/apifactoryoutput/{Slice}",
                    PartitionedBy = new Collection<Partition>()
                    {
                        new Partition()
                        {
                            Name = "Slice",
                            Value = new DateTimePartitionValue()
                            {
                                Date = "SliceStart",
                                Format = "yyyyMMdd-HH"
                            }
                        }
                    }
                },
    
                Availability = new Availability()
                {
                    Frequency = SchedulePeriod.Hour,
                    Interval = 1,
                },
            }
        }
    });
    ```
10. <span data-ttu-id="34aa0-174">Do metody **Main** přidejte následující kód, který **vytvoří a aktivuje kanál**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-174">Add the following code that **creates and activates a pipeline** to the **Main** method.</span></span> <span data-ttu-id="34aa0-175">Tento kanál má aktivitu **CopyActivity**, která jako zdroj používá **BlobSource** a jako jímku používá **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-175">This pipeline has a **CopyActivity** that takes **BlobSource** as a source and **BlobSink** as a sink.</span></span>

    <span data-ttu-id="34aa0-176">Aktivita kopírování provádí přesun dat ve službě Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="34aa0-176">The Copy Activity performs the data movement in Azure Data Factory.</span></span> <span data-ttu-id="34aa0-177">Aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem.</span><span class="sxs-lookup"><span data-stu-id="34aa0-177">The activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="34aa0-178">Podrobnosti o aktivitě kopírování najdete v článku [Aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="34aa0-178">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about the Copy Activity.</span></span>

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2014, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
    string PipelineName = "PipelineBlobSample";
    
    client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
    new PipelineCreateOrUpdateParameters()
    {
        Pipeline = new Pipeline()
        {
            Name = PipelineName,
            Properties = new PipelineProperties()
            {
                Description = "Demo Pipeline for data transfer between blobs",
    
                // Initial value for pipeline's active period. With this, you won't need to set slice status
                Start = PipelineActivePeriodStartTime,
                End = PipelineActivePeriodEndTime,
    
                Activities = new List<Activity>()
                {
                    new Activity()
                    {
                        Name = "BlobToBlob",
                        Inputs = new List<ActivityInput>()
                        {
                            new ActivityInput()
                {
                                Name = Dataset_Source
                            }
                        },
                        Outputs = new List<ActivityOutput>()
                        {
                            new ActivityOutput()
                            {
                                Name = Dataset_Destination
                            }
                        },
                        TypeProperties = new CopyActivity()
                        {
                            Source = new BlobSource(),
                            Sink = new BlobSink()
                            {
                                WriteBatchSize = 10000,
                                WriteBatchTimeout = TimeSpan.FromMinutes(10)
                            }
                        }
                    }
    
                },
            }
        }
    });
    ```
12. <span data-ttu-id="34aa0-179">Do metody **Main** přidejte následující kód pro získání stavu datového řezu výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="34aa0-179">Add the following code to the **Main** method to get the status of a data slice of the output dataset.</span></span> <span data-ttu-id="34aa0-180">Není v této ukázce se očekává pouze jeden řez.</span><span class="sxs-lookup"><span data-stu-id="34aa0-180">There is only one slice expected in this sample.</span></span>

    ```csharp
    // Pulling status within a timeout threshold
    DateTime start = DateTime.Now;
    bool done = false;
    
    while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
    {
        Console.WriteLine("Pulling the slice status");
        // wait before the next status check
        Thread.Sleep(1000 * 12);
    
        var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
            new DataSliceListParameters()
            {
                DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
            });
    
        foreach (DataSlice slice in datalistResponse.DataSlices)
        {
            if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
            {
                Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                done = true;
                break;
            }
            else
            {
                Console.WriteLine("Slice status is: {0}", slice.State);
            }
        }
    }
    ```
13. <span data-ttu-id="34aa0-181">**(volitelné)**  Přidejte následující kód, který získat podrobnosti o pro datový řez pro spuštění **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="34aa0-181">**(optional)** Add the following code to get run details for a data slice to the **Main** method.</span></span>

    ```csharp
    Console.WriteLine("Getting run details of a data slice");
    
    // give it a few minutes for the output slice to be ready
    Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
    Console.ReadKey();
    
    var datasliceRunListResponse = client.DataSliceRuns.List(
        resourceGroupName,
        dataFactoryName,
        Dataset_Destination,
        new DataSliceRunListParameters()
        {
            DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
        });
    
    foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
    {
        Console.WriteLine("Status: \t\t{0}", run.Status);
        Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
        Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
        Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
        Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
        Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
        Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
    }
    
    Console.WriteLine("\nPress any key to exit.");
    Console.ReadKey();
    ```
14. <span data-ttu-id="34aa0-182">Do třídy **Program** přidejte následující pomocnou metodu, kterou používá metoda **Main**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-182">Add the following helper method used by the **Main** method to the **Program** class.</span></span> <span data-ttu-id="34aa0-183">Tato metoda otevře dialogové okno, který umožňuje zadat **uživatelské jméno** a **heslo** který používáte k přihlášení k portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="34aa0-183">This method pops a dialog box that that lets you provide **user name** and **password** that you use to log in to Azure portal.</span></span>

    ```csharp
    public static async Task<string> GetAuthorizationHeader()
    {
        AuthenticationContext context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);
        ClientCredential credential = new ClientCredential(
            ConfigurationManager.AppSettings["ApplicationId"],
            ConfigurationManager.AppSettings["Password"]);
        AuthenticationResult result = await context.AcquireTokenAsync(
            resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
            clientCredential: credential);

        if (result != null)
            return result.AccessToken;

        throw new InvalidOperationException("Failed to acquire token");
    }
    ```

15. <span data-ttu-id="34aa0-184">V Průzkumníku řešení rozbalte projekt: **DataFactoryAPITestApp**, klikněte pravým tlačítkem na **odkazy**a klikněte na tlačítko **přidat odkaz na**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-184">In the Solution Explorer, expand the project: **DataFactoryAPITestApp**, right-click **References**, and click **Add Reference**.</span></span> <span data-ttu-id="34aa0-185">Zaškrtněte políčko u `System.Configuration` sestavení a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-185">Select check box for `System.Configuration` assembly and click **OK**.</span></span>
15. <span data-ttu-id="34aa0-186">Sestavte konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="34aa0-186">Build the console application.</span></span> <span data-ttu-id="34aa0-187">Klikněte v nabídce na **Sestavit** a potom klikněte na **Sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-187">Click **Build** on the menu and click **Build Solution**.</span></span>
16. <span data-ttu-id="34aa0-188">Potvrďte, že existuje alespoň jeden soubor adftutorial kontejneru ve službě Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="34aa0-188">Confirm that there is at least one file in the adftutorial container in your Azure blob storage.</span></span> <span data-ttu-id="34aa0-189">Pokud ne, vytvořte soubor Emp.txt v poznámkovém bloku s následujícím obsahem a nahrajte ho do kontejneru adftutorial.</span><span class="sxs-lookup"><span data-stu-id="34aa0-189">If not, create Emp.txt file in Notepad with the following content and upload it to the adftutorial container.</span></span>

    ```
    John, Doe
    Jane, Doe
    ```
17. <span data-ttu-id="34aa0-190">Ukázku spusťte kliknutím na **Ladit** -> **Spustit ladění** v nabídce.</span><span class="sxs-lookup"><span data-stu-id="34aa0-190">Run the sample by clicking **Debug** -> **Start Debugging** on the menu.</span></span> <span data-ttu-id="34aa0-191">Když se zobrazí **Získávání běhových podrobností o datovém řezu**, počkejte několik minut a stiskněte **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-191">When you see the **Getting run details of a data slice**, wait for a few minutes, and press **ENTER**.</span></span>
18. <span data-ttu-id="34aa0-192">Pomocí webu Azure Portal ověřte, že je objekt pro vytváření dat **APITutorialFactory** vytvořený s těmito artefakty:</span><span class="sxs-lookup"><span data-stu-id="34aa0-192">Use the Azure portal to verify that the data factory **APITutorialFactory** is created with the following artifacts:</span></span>
    * <span data-ttu-id="34aa0-193">Propojená služba: **AzureStorageLinkedService**</span><span class="sxs-lookup"><span data-stu-id="34aa0-193">Linked service: **AzureStorageLinkedService**</span></span>
    * <span data-ttu-id="34aa0-194">Datová sada: **DatasetBlobSource** a **DatasetBlobDestination**.</span><span class="sxs-lookup"><span data-stu-id="34aa0-194">Dataset: **DatasetBlobSource** and **DatasetBlobDestination**.</span></span>
    * <span data-ttu-id="34aa0-195">Kanál: **PipelineBlobSample**</span><span class="sxs-lookup"><span data-stu-id="34aa0-195">Pipeline: **PipelineBlobSample**</span></span>
19. <span data-ttu-id="34aa0-196">Ověřte, že výstupní soubor je vytvořen v **apifactoryoutput** složku **adftutorial** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="34aa0-196">Verify that an output file is created in the **apifactoryoutput** folder in the **adftutorial** container.</span></span>

## <a name="get-a-list-of-failed-data-slices"></a><span data-ttu-id="34aa0-197">Získat seznam neúspěšných datové řezy</span><span class="sxs-lookup"><span data-stu-id="34aa0-197">Get a list of failed data slices</span></span> 

```csharp
// Parse the resource path
var ResourceGroupName = "ADFTutorialResourceGroup";
var DataFactoryName = "DataFactoryAPITestApp";

var parameters = new ActivityWindowsByDataFactoryListParameters(ResourceGroupName, DataFactoryName);
parameters.WindowState = "Failed";
var response = dataFactoryManagementClient.ActivityWindows.List(parameters);
do
{
    foreach (var activityWindow in response.ActivityWindowListResponseValue.ActivityWindows)
    {
        var row = string.Join(
            "\t",
            activityWindow.WindowStart.ToString(),
            activityWindow.WindowEnd.ToString(),
            activityWindow.RunStart.ToString(),
            activityWindow.RunEnd.ToString(),
            activityWindow.DataFactoryName,
            activityWindow.PipelineName,
            activityWindow.ActivityName,
            string.Join(",", activityWindow.OutputDatasets));
        Console.WriteLine(row);
    }

    if (response.NextLink != null)
    {
        response = dataFactoryManagementClient.ActivityWindows.ListNext(response.NextLink, parameters);
    }
    else
    {
        response = null;
    }
}
while (response != null);
```

## <a name="next-steps"></a><span data-ttu-id="34aa0-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="34aa0-198">Next steps</span></span>
<span data-ttu-id="34aa0-199">Podívejte se na následující příklad k vytvoření kanálu pomocí sady .NET SDK, který kopíruje data z Azure blob storage do Azure SQL database:</span><span class="sxs-lookup"><span data-stu-id="34aa0-199">See the following example for creating a pipeline using .NET SDK that copies data from an Azure blob storage to an Azure SQL database:</span></span> 

- [<span data-ttu-id="34aa0-200">Vytvoření kanálu pro zkopírování dat z úložiště objektů Blob do databáze SQL</span><span class="sxs-lookup"><span data-stu-id="34aa0-200">Create a pipeline to copy data from Blob Storage to SQL Database</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
