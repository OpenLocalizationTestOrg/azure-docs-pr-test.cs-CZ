---
title: "aaaCreate datových kanálů pomocí sady Azure .NET SDK | Microsoft Docs"
description: "Zjistěte, jak tooprogrammatically vytvořit, sledovat a spravovat objekty pro vytváření dat Azure pomocí sady SDK Data Factory."
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
ms.openlocfilehash: 190b5f99edbb3c27e1e8efb8990b9e601b22458f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-monitor-and-manage-azure-data-factories-using-azure-data-factory-net-sdk"></a><span data-ttu-id="66b4c-103">Vytvoření, sledovat a spravovat Azure data Factory pomocí .NET SDK služby Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="66b4c-103">Create, monitor, and manage Azure data factories using Azure Data Factory .NET SDK</span></span>
## <a name="overview"></a><span data-ttu-id="66b4c-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="66b4c-104">Overview</span></span>
<span data-ttu-id="66b4c-105">Můžete vytvořit, sledovat a spravovat Azure data Factory programově pomocí .NET SDK služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="66b4c-105">You can create, monitor, and manage Azure data factories programmatically using Data Factory .NET SDK.</span></span> <span data-ttu-id="66b4c-106">Tento článek obsahuje návod, který může sledovat toocreate ukázkové aplikace konzoly .NET, která vytvoří a sleduje služby data factory.</span><span class="sxs-lookup"><span data-stu-id="66b4c-106">This article contains a walkthrough that you can follow toocreate a sample .NET console application that creates and monitors a data factory.</span></span> 

> [!NOTE]
> <span data-ttu-id="66b4c-107">Tento článek nepopisuje všechny hello Data Factory .NET API.</span><span class="sxs-lookup"><span data-stu-id="66b4c-107">This article does not cover all hello Data Factory .NET API.</span></span> <span data-ttu-id="66b4c-108">V tématu [Data Factory .NET API – referenční informace](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) úplnou dokumentaci o .NET API pro Data Factory.</span><span class="sxs-lookup"><span data-stu-id="66b4c-108">See [Data Factory .NET API Reference](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1) for comprehensive documentation on .NET API for Data Factory.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="66b4c-109">Požadavky</span><span class="sxs-lookup"><span data-stu-id="66b4c-109">Prerequisites</span></span>
* <span data-ttu-id="66b4c-110">Visual Studio 2012 nebo 2013 nebo 2015</span><span class="sxs-lookup"><span data-stu-id="66b4c-110">Visual Studio 2012 or 2013 or 2015</span></span>
* <span data-ttu-id="66b4c-111">Stáhněte a nainstalujte [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="66b4c-111">Download and install [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span></span>
* <span data-ttu-id="66b4c-112">Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="66b4c-112">Azure PowerShell.</span></span> <span data-ttu-id="66b4c-113">Postupujte podle pokynů v [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článek tooinstall prostředí Azure PowerShell ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="66b4c-113">Follow instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article tooinstall Azure PowerShell on your computer.</span></span> <span data-ttu-id="66b4c-114">Používáte prostředí Azure PowerShell toocreate aplikaci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="66b4c-114">You use Azure PowerShell toocreate an Azure Active Directory application.</span></span>

### <a name="create-an-application-in-azure-active-directory"></a><span data-ttu-id="66b4c-115">Vytvoření aplikace v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="66b4c-115">Create an application in Azure Active Directory</span></span>
<span data-ttu-id="66b4c-116">Vytvoření aplikace Azure Active Directory, vytvořit objekt služby pro aplikaci hello a přiřaďte ho toohello **Data Factory Přispěvatel** role.</span><span class="sxs-lookup"><span data-stu-id="66b4c-116">Create an Azure Active Directory application, create a service principal for hello application, and assign it toohello **Data Factory Contributor** role.</span></span>

1. <span data-ttu-id="66b4c-117">Spusťte **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="66b4c-117">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="66b4c-118">Spusťte následující příkaz hello a zadejte hello uživatelské jméno a heslo použít toosign v toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="66b4c-118">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="66b4c-119">Spusťte následující příkaz tooview hello všechny hello předplatná pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="66b4c-119">Run hello following command tooview all hello subscriptions for this account.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. <span data-ttu-id="66b4c-120">Spusťte následující příkaz tooselect hello předplatné, které chcete toowork s hello.</span><span class="sxs-lookup"><span data-stu-id="66b4c-120">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="66b4c-121">Nahraďte  **&lt;NameOfAzureSubscription** &gt; s názvem hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="66b4c-121">Replace **&lt;NameOfAzureSubscription**&gt; with hello name of your Azure subscription.</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="66b4c-122">Poznamenejte si **SubscriptionId** a **TenantId** z hello výstup tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="66b4c-122">Note down **SubscriptionId** and **TenantId** from hello output of this command.</span></span>

5. <span data-ttu-id="66b4c-123">Vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup** tak, že spustíte následující příkaz v hello prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="66b4c-123">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command in hello PowerShell.</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    <span data-ttu-id="66b4c-124">Pokud skupina prostředků hello již existuje, zadejte zda tooupdate ho (Y) nebo jej zachovat jako (ne).</span><span class="sxs-lookup"><span data-stu-id="66b4c-124">If hello resource group already exists, you specify whether tooupdate it (Y) or keep it as (N).</span></span>

    <span data-ttu-id="66b4c-125">Pokud používáte jiné skupině prostředků, musíte v tomto kurzu toouse hello název vaší skupiny prostředků místo skupiny ADFTutorialResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="66b4c-125">If you use a different resource group, you need toouse hello name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>
6. <span data-ttu-id="66b4c-126">Vytvořte aplikaci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="66b4c-126">Create an Azure Active Directory application.</span></span>

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFDotNetWalkthroughApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfdotnetwalkthroughapp.org/example" -Password "Pass@word1"
    ```

    <span data-ttu-id="66b4c-127">Pokud dojde k následující chybě hello, zadejte jinou adresu URL a znovu spusťte příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="66b4c-127">If you get hello following error, specify a different URL and run hello command again.</span></span>
    
    ```PowerShell
    Another object with hello same value for property identifierUris already exists.
    ```
7. <span data-ttu-id="66b4c-128">Vytvořte hello objekt služby AD.</span><span class="sxs-lookup"><span data-stu-id="66b4c-128">Create hello AD service principal.</span></span>

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. <span data-ttu-id="66b4c-129">Přidání služby hlavní toohello **Data Factory Přispěvatel** role.</span><span class="sxs-lookup"><span data-stu-id="66b4c-129">Add service principal toohello **Data Factory Contributor** role.</span></span>

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. <span data-ttu-id="66b4c-130">Získání ID hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="66b4c-130">Get hello application ID.</span></span>

    ```PowerShell
    $azureAdApplication 
    ```
    <span data-ttu-id="66b4c-131">Poznamenejte si ID aplikace hello (applicationID) z výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="66b4c-131">Note down hello application ID (applicationID) from hello output.</span></span>

<span data-ttu-id="66b4c-132">Z těchto kroků byste měli mít tyto čtyři hodnoty:</span><span class="sxs-lookup"><span data-stu-id="66b4c-132">You should have following four values from these steps:</span></span>

* <span data-ttu-id="66b4c-133">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="66b4c-133">Tenant ID</span></span>
* <span data-ttu-id="66b4c-134">ID předplatného</span><span class="sxs-lookup"><span data-stu-id="66b4c-134">Subscription ID</span></span>
* <span data-ttu-id="66b4c-135">ID aplikace</span><span class="sxs-lookup"><span data-stu-id="66b4c-135">Application ID</span></span>
* <span data-ttu-id="66b4c-136">Heslo (zadané v první příkaz hello)</span><span class="sxs-lookup"><span data-stu-id="66b4c-136">Password (specified in hello first command)</span></span>

## <a name="walkthrough"></a><span data-ttu-id="66b4c-137">Názorný postup</span><span class="sxs-lookup"><span data-stu-id="66b4c-137">Walkthrough</span></span>
<span data-ttu-id="66b4c-138">V Průvodci hello vytvoříte objekt pro vytváření dat kanál, který obsahuje aktivitu kopírování.</span><span class="sxs-lookup"><span data-stu-id="66b4c-138">In hello walkthrough, you create a data factory with a pipeline that contains a copy activity.</span></span> <span data-ttu-id="66b4c-139">Hello aktivity kopírování kopíruje data ze složky ve složce tooanother úložiště objektů blob v Azure aplikace hello stejné úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="66b4c-139">hello copy activity copies data from a folder in your Azure blob storage tooanother folder in hello same blob storage.</span></span> 

<span data-ttu-id="66b4c-140">Hello aktivita kopírování provádí přesun dat hello v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="66b4c-140">hello Copy Activity performs hello data movement in Azure Data Factory.</span></span> <span data-ttu-id="66b4c-141">Hello aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelné způsobem.</span><span class="sxs-lookup"><span data-stu-id="66b4c-141">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="66b4c-142">V tématu [aktivity přesunu dat](data-factory-data-movement-activities.md) článku podrobnosti o aktivitě kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="66b4c-142">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about hello Copy Activity.</span></span>

1. <span data-ttu-id="66b4c-143">Pomocí sady Visual Studio 2012/2013/2015 vytvořte konzolovou aplikaci C# .NET.</span><span class="sxs-lookup"><span data-stu-id="66b4c-143">Using Visual Studio 2012/2013/2015, create a C# .NET console application.</span></span>
   1. <span data-ttu-id="66b4c-144">Spusťte **Visual Studio** 2012/2013/2015.</span><span class="sxs-lookup"><span data-stu-id="66b4c-144">Launch **Visual Studio** 2012/2013/2015.</span></span>
   2. <span data-ttu-id="66b4c-145">Klikněte na tlačítko **soubor**, bod příliš**nový**a klikněte na tlačítko **projektu**.</span><span class="sxs-lookup"><span data-stu-id="66b4c-145">Click **File**, point too**New**, and click **Project**.</span></span>
   3. <span data-ttu-id="66b4c-146">Rozbalte **Šablony** a vyberte **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="66b4c-146">Expand **Templates**, and select **Visual C#**.</span></span> <span data-ttu-id="66b4c-147">V tomto názorném postupu použijete C#, ale mohli byste využít libovolný jazyk .NET.</span><span class="sxs-lookup"><span data-stu-id="66b4c-147">In this walkthrough, you use C#, but you can use any .NET language.</span></span>
   4. <span data-ttu-id="66b4c-148">Vyberte **konzolové aplikace** hello seznamu typů projektu na hello správné.</span><span class="sxs-lookup"><span data-stu-id="66b4c-148">Select **Console Application** from hello list of project types on hello right.</span></span>
   5. <span data-ttu-id="66b4c-149">Zadejte **DataFactoryAPITestApp** pro hello název.</span><span class="sxs-lookup"><span data-stu-id="66b4c-149">Enter **DataFactoryAPITestApp** for hello Name.</span></span>
   6. <span data-ttu-id="66b4c-150">Vyberte **C:\ADFGetStarted** pro hello umístění.</span><span class="sxs-lookup"><span data-stu-id="66b4c-150">Select **C:\ADFGetStarted** for hello Location.</span></span>
   7. <span data-ttu-id="66b4c-151">Klikněte na tlačítko **OK** toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="66b4c-151">Click **OK** toocreate hello project.</span></span>
2. <span data-ttu-id="66b4c-152">Klikněte na tlačítko **nástroje**, bod příliš**Správce balíčků NuGet**a klikněte na tlačítko **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="66b4c-152">Click **Tools**, point too**NuGet Package Manager**, and click **Package Manager Console**.</span></span>
3. <span data-ttu-id="66b4c-153">V hello **Konzola správce balíčků**, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="66b4c-153">In hello **Package Manager Console**, do hello following steps:</span></span>
   1. <span data-ttu-id="66b4c-154">Spusťte následující příkaz tooinstall Data Factory balíček hello:`Install-Package Microsoft.Azure.Management.DataFactories`</span><span class="sxs-lookup"><span data-stu-id="66b4c-154">Run hello following command tooinstall Data Factory package: `Install-Package Microsoft.Azure.Management.DataFactories`</span></span>
   2. <span data-ttu-id="66b4c-155">Spusťte následující příkaz tooinstall Azure Active Directory balíčku (použít Active Directory API v kódu hello) hello:`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span><span class="sxs-lookup"><span data-stu-id="66b4c-155">Run hello following command tooinstall Azure Active Directory package (you use Active Directory API in hello code): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span></span>
4. <span data-ttu-id="66b4c-156">Nahraďte obsah hello **App.config** soubor v projektu hello s hello následující obsah:</span><span class="sxs-lookup"><span data-stu-id="66b4c-156">Replace hello contents of **App.config** file in hello project with hello following content:</span></span> 
    
    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="ActiveDirectoryEndpoint" value="https://login.microsoftonline.com/" />
            <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
            <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

            <add key="ApplicationId" value="your application ID" />
            <add key="Password" value="Password you used while creating hello AAD application" />
            <add key="SubscriptionId" value= "Subscription ID" />
            <add key="ActiveDirectoryTenantId" value="Tenant ID" />
        </appSettings>
    </configuration>
    ```
5. <span data-ttu-id="66b4c-157">V souboru App.Config hello aktualizujte hodnoty pro  **&lt;ID aplikace&gt;**,  **&lt;heslo&gt;**,  **&lt; ID předplatného&gt;**, a  **&lt;ID klienta&gt;**  vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="66b4c-157">In hello App.Config file, update values for **&lt;Application ID&gt;**, **&lt;Password&gt;**, **&lt;Subscription ID&gt;**, and **&lt;tenant ID&gt;** with your own values.</span></span>
6. <span data-ttu-id="66b4c-158">Přidejte následující hello **pomocí** příkazy toohello **Program.cs** soubor v projektu hello.</span><span class="sxs-lookup"><span data-stu-id="66b4c-158">Add hello following **using** statements toohello **Program.cs** file in hello project.</span></span>

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
6. <span data-ttu-id="66b4c-159">Přidejte následující kód, který vytvoří instanci hello **DataPipelineManagementClient** třída toohello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="66b4c-159">Add hello following code that creates an instance of **DataPipelineManagementClient** class toohello **Main** method.</span></span> <span data-ttu-id="66b4c-160">Tento objekt toocreate použijete objekt pro vytváření dat, propojené služby, vstupní a výstupní datové sady a kanál.</span><span class="sxs-lookup"><span data-stu-id="66b4c-160">You use this object toocreate a data factory, a linked service, input and output datasets, and a pipeline.</span></span> <span data-ttu-id="66b4c-161">Tento objekt toomonitor řezy datové sady se také použít za běhu.</span><span class="sxs-lookup"><span data-stu-id="66b4c-161">You also use this object toomonitor slices of a dataset at runtime.</span></span>

    ```csharp
    // create data factory management client

    //IMPORTANT: specify hello name of Azure resource group here
    string resourceGroupName = "ADFTutorialResourceGroup";

    //IMPORTANT: hello name of hello data factory must be globally unique.
    // Therefore, update this value. For example:APITutorialFactory05122017
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="66b4c-162">Nahraďte hodnotu hello **resourceGroupName** s hello název vaší skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="66b4c-162">Replace hello value of **resourceGroupName** with hello name of your Azure resource group.</span></span> <span data-ttu-id="66b4c-163">Můžete vytvořit skupinu prostředků pomocí hello [New-AzureResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) rutiny.</span><span class="sxs-lookup"><span data-stu-id="66b4c-163">You can create a resource group using hello [New-AzureResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet.</span></span>
   >
   > <span data-ttu-id="66b4c-164">Aktualizujte název hello data factory (dataFactoryName) toobe jedinečný.</span><span class="sxs-lookup"><span data-stu-id="66b4c-164">Update name of hello data factory (dataFactoryName) toobe unique.</span></span> <span data-ttu-id="66b4c-165">Název objektu pro vytváření dat hello musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="66b4c-165">Name of hello data factory must be globally unique.</span></span> <span data-ttu-id="66b4c-166">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="66b4c-166">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
7. <span data-ttu-id="66b4c-167">Přidejte následující kód, který vytvoří hello **objekt pro vytváření dat** toohello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="66b4c-167">Add hello following code that creates a **data factory** toohello **Main** method.</span></span>

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
8. <span data-ttu-id="66b4c-168">Přidejte následující kód, který vytvoří hello **propojená služba Azure Storage** toohello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="66b4c-168">Add hello following code that creates an **Azure Storage linked service** toohello **Main** method.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="66b4c-169">Položky **storageaccountname** a **accountkey** nahraďte názvem svého účtu Azure Storage a jeho klíčem.</span><span class="sxs-lookup"><span data-stu-id="66b4c-169">Replace **storageaccountname** and **accountkey** with name and key of your Azure Storage account.</span></span>

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
9. <span data-ttu-id="66b4c-170">Přidejte následující kód, který vytvoří hello **vstupní a výstupní datové sady** toohello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="66b4c-170">Add hello following code that creates **input and output datasets** toohello **Main** method.</span></span>

    <span data-ttu-id="66b4c-171">Hello **FolderPath** hello vstupního objektu blob je nastavený příliš**adftutorial /** kde **adftutorial** je název hello hello kontejneru ve službě blob storage.</span><span class="sxs-lookup"><span data-stu-id="66b4c-171">hello **FolderPath** for hello input blob is set too**adftutorial/** where **adftutorial** is hello name of hello container in your blob storage.</span></span> <span data-ttu-id="66b4c-172">Pokud tento kontejner ve službě Azure blob storage neexistuje, vytvořit kontejner s tímto názvem: **adftutorial** a nahrát textový soubor toohello kontejner.</span><span class="sxs-lookup"><span data-stu-id="66b4c-172">If this container does not exist in your Azure blob storage, create a container with this name: **adftutorial** and upload a text file toohello container.</span></span>

    <span data-ttu-id="66b4c-173">výstup Hello FolderPath pro hello objektu blob je nastavena na: **adftutorial/apifactoryoutput / {řezu}** kde **řez** je hodnota dynamicky počítané na základě hello **SliceStart**(spustit datum a čas každý řez.)</span><span class="sxs-lookup"><span data-stu-id="66b4c-173">hello FolderPath for hello output blob is set to: **adftutorial/apifactoryoutput/{Slice}** where **Slice** is dynamically calculated based on hello value of **SliceStart** (start date-time of each slice.)</span></span>

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
10. <span data-ttu-id="66b4c-174">Přidat hello následující kód, který **vytvoří a aktivuje kanálu** toohello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="66b4c-174">Add hello following code that **creates and activates a pipeline** toohello **Main** method.</span></span> <span data-ttu-id="66b4c-175">Tento kanál má aktivitu **CopyActivity**, která jako zdroj používá **BlobSource** a jako jímku používá **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="66b4c-175">This pipeline has a **CopyActivity** that takes **BlobSource** as a source and **BlobSink** as a sink.</span></span>

    <span data-ttu-id="66b4c-176">Hello aktivita kopírování provádí přesun dat hello v Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="66b4c-176">hello Copy Activity performs hello data movement in Azure Data Factory.</span></span> <span data-ttu-id="66b4c-177">Hello aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelné způsobem.</span><span class="sxs-lookup"><span data-stu-id="66b4c-177">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="66b4c-178">V tématu [aktivity přesunu dat](data-factory-data-movement-activities.md) článku podrobnosti o aktivitě kopírování hello.</span><span class="sxs-lookup"><span data-stu-id="66b4c-178">See [Data Movement Activities](data-factory-data-movement-activities.md) article for details about hello Copy Activity.</span></span>

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
    
                // Initial value for pipeline's active period. With this, you won't need tooset slice status
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
12. <span data-ttu-id="66b4c-179">Přidejte následující kód toohello hello **hlavní** metoda tooget hello stav datový řez ze hello výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="66b4c-179">Add hello following code toohello **Main** method tooget hello status of a data slice of hello output dataset.</span></span> <span data-ttu-id="66b4c-180">Není v této ukázce se očekává pouze jeden řez.</span><span class="sxs-lookup"><span data-stu-id="66b4c-180">There is only one slice expected in this sample.</span></span>

    ```csharp
    // Pulling status within a timeout threshold
    DateTime start = DateTime.Now;
    bool done = false;
    
    while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
    {
        Console.WriteLine("Pulling hello slice status");
        // wait before hello next status check
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
13. <span data-ttu-id="66b4c-181">**(volitelné)**  Přidat hello následující kód tooget spustit podrobnosti datový řez toohello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="66b4c-181">**(optional)** Add hello following code tooget run details for a data slice toohello **Main** method.</span></span>

    ```csharp
    Console.WriteLine("Getting run details of a data slice");
    
    // give it a few minutes for hello output slice toobe ready
    Console.WriteLine("\nGive it a few minutes for hello output slice toobe ready and press any key.");
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
    
    Console.WriteLine("\nPress any key tooexit.");
    Console.ReadKey();
    ```
14. <span data-ttu-id="66b4c-182">Přidejte následující metodu helper používané hello hello **hlavní** metoda toohello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="66b4c-182">Add hello following helper method used by hello **Main** method toohello **Program** class.</span></span> <span data-ttu-id="66b4c-183">Tato metoda otevře dialogové okno, který umožňuje zadat **uživatelské jméno** a **heslo** použijete toolog tooAzure portálu.</span><span class="sxs-lookup"><span data-stu-id="66b4c-183">This method pops a dialog box that that lets you provide **user name** and **password** that you use toolog in tooAzure portal.</span></span>

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

        throw new InvalidOperationException("Failed tooacquire token");
    }
    ```

15. <span data-ttu-id="66b4c-184">V Průzkumníku řešení hello, rozbalte projekt hello: **DataFactoryAPITestApp**, klikněte pravým tlačítkem na **odkazy**a klikněte na tlačítko **přidat odkaz na**.</span><span class="sxs-lookup"><span data-stu-id="66b4c-184">In hello Solution Explorer, expand hello project: **DataFactoryAPITestApp**, right-click **References**, and click **Add Reference**.</span></span> <span data-ttu-id="66b4c-185">Zaškrtněte políčko u `System.Configuration` sestavení a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="66b4c-185">Select check box for `System.Configuration` assembly and click **OK**.</span></span>
15. <span data-ttu-id="66b4c-186">Vytvoření konzolové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="66b4c-186">Build hello console application.</span></span> <span data-ttu-id="66b4c-187">Klikněte na tlačítko **sestavení** na hello nabídky a klikněte na **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="66b4c-187">Click **Build** on hello menu and click **Build Solution**.</span></span>
16. <span data-ttu-id="66b4c-188">Potvrďte, že existuje alespoň jeden soubor hello adftutorial kontejneru ve službě Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="66b4c-188">Confirm that there is at least one file in hello adftutorial container in your Azure blob storage.</span></span> <span data-ttu-id="66b4c-189">Pokud ne, vytvořte soubor Emp.txt v poznámkovém bloku s hello následující obsahu a nahrajte ho toohello adftutorial kontejneru.</span><span class="sxs-lookup"><span data-stu-id="66b4c-189">If not, create Emp.txt file in Notepad with hello following content and upload it toohello adftutorial container.</span></span>

    ```
    John, Doe
    Jane, Doe
    ```
17. <span data-ttu-id="66b4c-190">Hello ukázku spustit kliknutím **ladění** -> **spustit ladění** v nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="66b4c-190">Run hello sample by clicking **Debug** -> **Start Debugging** on hello menu.</span></span> <span data-ttu-id="66b4c-191">Až se zobrazí hello **získávání spustit podrobnosti datový řez**, počkejte několik minut a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="66b4c-191">When you see hello **Getting run details of a data slice**, wait for a few minutes, and press **ENTER**.</span></span>
18. <span data-ttu-id="66b4c-192">Použití této datové továrně hello hello Azure portálu tooverify **APITutorialFactory** je vytvořena s hello artefakty následující:</span><span class="sxs-lookup"><span data-stu-id="66b4c-192">Use hello Azure portal tooverify that hello data factory **APITutorialFactory** is created with hello following artifacts:</span></span>
    * <span data-ttu-id="66b4c-193">Propojená služba: **AzureStorageLinkedService**</span><span class="sxs-lookup"><span data-stu-id="66b4c-193">Linked service: **AzureStorageLinkedService**</span></span>
    * <span data-ttu-id="66b4c-194">Datová sada: **DatasetBlobSource** a **DatasetBlobDestination**.</span><span class="sxs-lookup"><span data-stu-id="66b4c-194">Dataset: **DatasetBlobSource** and **DatasetBlobDestination**.</span></span>
    * <span data-ttu-id="66b4c-195">Kanál: **PipelineBlobSample**</span><span class="sxs-lookup"><span data-stu-id="66b4c-195">Pipeline: **PipelineBlobSample**</span></span>
19. <span data-ttu-id="66b4c-196">Ověřte, že výstupní soubor je vytvořen v hello **apifactoryoutput** složky v hello **adftutorial** kontejneru.</span><span class="sxs-lookup"><span data-stu-id="66b4c-196">Verify that an output file is created in hello **apifactoryoutput** folder in hello **adftutorial** container.</span></span>

## <a name="get-a-list-of-failed-data-slices"></a><span data-ttu-id="66b4c-197">Získat seznam neúspěšných datové řezy</span><span class="sxs-lookup"><span data-stu-id="66b4c-197">Get a list of failed data slices</span></span> 

```csharp
// Parse hello resource path
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

## <a name="next-steps"></a><span data-ttu-id="66b4c-198">Další kroky</span><span class="sxs-lookup"><span data-stu-id="66b4c-198">Next steps</span></span>
<span data-ttu-id="66b4c-199">Viz následující ukázka pro vytvoření kanálu pomocí sady .NET SDK, který kopíruje data z Azure blob storage tooan Azure SQL database hello:</span><span class="sxs-lookup"><span data-stu-id="66b4c-199">See hello following example for creating a pipeline using .NET SDK that copies data from an Azure blob storage tooan Azure SQL database:</span></span> 

- [<span data-ttu-id="66b4c-200">Vytvoření kanálu toocopy dat z úložiště objektů Blob tooSQL databáze</span><span class="sxs-lookup"><span data-stu-id="66b4c-200">Create a pipeline toocopy data from Blob Storage tooSQL Database</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
