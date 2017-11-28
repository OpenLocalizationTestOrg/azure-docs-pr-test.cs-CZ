---
title: "Kurz: Vytvoření kanálu s aktivitou kopírování pomocí rozhraní .NET API | Dokumentace Microsoftu"
description: "V tomto kurzu vytvoříte kanál Azure Data Factory s aktivitou kopírování pomocí rozhraní .NET API."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 58fc4007-b46d-4c8e-a279-cb9e479b3e2b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 52f72da54cdd80691e09d7453bf6730454c4089e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a><span data-ttu-id="fdf79-103">Kurz: Vytvoření kanálu s aktivitou kopírování pomocí rozhraní .NET API</span><span class="sxs-lookup"><span data-stu-id="fdf79-103">Tutorial: Create a pipeline with Copy Activity using .NET API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fdf79-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="fdf79-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="fdf79-105">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="fdf79-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="fdf79-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fdf79-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="fdf79-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fdf79-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="fdf79-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="fdf79-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="fdf79-109">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="fdf79-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="fdf79-110">REST API</span><span class="sxs-lookup"><span data-stu-id="fdf79-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="fdf79-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="fdf79-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

<span data-ttu-id="fdf79-112">V tomto článku se dozvíte, jak toouse [.NET API](https://portal.azure.com) toocreate objekt pro vytváření dat s kanál, který kopíruje data z databáze Azure SQL tooan úložiště objektů blob v Azure.</span><span class="sxs-lookup"><span data-stu-id="fdf79-112">In this article, you learn how toouse [.NET API](https://portal.azure.com) toocreate a data factory with a pipeline that copies data from an Azure blob storage tooan Azure SQL database.</span></span> <span data-ttu-id="fdf79-113">Pokud jste tooAzure nový objekt pro vytváření dat, pročtěte hello [Úvod tooAzure Data Factory](data-factory-introduction.md) článek před provedením tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="fdf79-113">If you are new tooAzure Data Factory, read through hello [Introduction tooAzure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="fdf79-114">V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování.</span><span class="sxs-lookup"><span data-stu-id="fdf79-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="fdf79-115">Aktivita kopírování Hello zkopíruje data z úložiště dat podporovaných podřízený tooa podporované datové úložiště.</span><span class="sxs-lookup"><span data-stu-id="fdf79-115">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="fdf79-116">Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="fdf79-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="fdf79-117">Hello aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelné způsobem.</span><span class="sxs-lookup"><span data-stu-id="fdf79-117">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="fdf79-118">Další informace o hello aktivitě kopírování najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="fdf79-118">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="fdf79-119">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="fdf79-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="fdf79-120">A dvě aktivity (spustit aktivitu po jiné) můžete řetězu nastavením hello výstupní datovou sadu jednu aktivitu jako hello vstupní datové sady hello dalších aktivit.</span><span class="sxs-lookup"><span data-stu-id="fdf79-120">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="fdf79-121">Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="fdf79-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span> 

> [!NOTE] 
> <span data-ttu-id="fdf79-122">Úplnou dokumentaci k rozhraní .NET API pro datovou továrnu najdete v [referencích k rozhraní .NET API služby Data Factory](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).</span><span class="sxs-lookup"><span data-stu-id="fdf79-122">For complete documentation on .NET API for Data Factory, see [Data Factory .NET API Reference](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).</span></span>
> 
> <span data-ttu-id="fdf79-123">Hello datovém kanálu v tomto kurzu kopíruje data ze zdroje dat úložiště tooa cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="fdf79-123">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="fdf79-124">Kurz týkající se jak tootransform dat pomocí Azure Data Factory najdete v části [kurz: sestavení dat tootransform kanálu pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="fdf79-124">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fdf79-125">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fdf79-125">Prerequisites</span></span>
* <span data-ttu-id="fdf79-126">Projděte [přehled kurzu a předpoklady](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) tooget přehled kurzu hello a dokončení hello **požadavek** kroky.</span><span class="sxs-lookup"><span data-stu-id="fdf79-126">Go through [Tutorial Overview and Pre-requisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) tooget an overview of hello tutorial and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="fdf79-127">Visual Studio 2012 nebo 2013 nebo 2015</span><span class="sxs-lookup"><span data-stu-id="fdf79-127">Visual Studio 2012 or 2013 or 2015</span></span>
* <span data-ttu-id="fdf79-128">Stáhněte sadu [Azure .NET SDK](http://azure.microsoft.com/downloads/) a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="fdf79-128">Download and install [Azure .NET SDK](http://azure.microsoft.com/downloads/)</span></span>
* <span data-ttu-id="fdf79-129">Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="fdf79-129">Azure PowerShell.</span></span> <span data-ttu-id="fdf79-130">Postupujte podle pokynů v [jak tooinstall a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md) článek tooinstall prostředí Azure PowerShell ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="fdf79-130">Follow instructions in [How tooinstall and configure Azure PowerShell](../powershell-install-configure.md) article tooinstall Azure PowerShell on your computer.</span></span> <span data-ttu-id="fdf79-131">Používáte prostředí Azure PowerShell toocreate aplikaci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fdf79-131">You use Azure PowerShell toocreate an Azure Active Directory application.</span></span>

### <a name="create-an-application-in-azure-active-directory"></a><span data-ttu-id="fdf79-132">Vytvoření aplikace v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="fdf79-132">Create an application in Azure Active Directory</span></span>
<span data-ttu-id="fdf79-133">Vytvoření aplikace Azure Active Directory, vytvořit objekt služby pro aplikaci hello a přiřaďte ho toohello **Data Factory Přispěvatel** role.</span><span class="sxs-lookup"><span data-stu-id="fdf79-133">Create an Azure Active Directory application, create a service principal for hello application, and assign it toohello **Data Factory Contributor** role.</span></span>

1. <span data-ttu-id="fdf79-134">Spusťte **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-134">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="fdf79-135">Spusťte následující příkaz hello a zadejte hello uživatelské jméno a heslo použít toosign v toohello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fdf79-135">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="fdf79-136">Spusťte následující příkaz tooview hello všechny hello předplatná pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="fdf79-136">Run hello following command tooview all hello subscriptions for this account.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. <span data-ttu-id="fdf79-137">Spusťte následující příkaz tooselect hello předplatné, které chcete toowork s hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-137">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="fdf79-138">Nahraďte  **&lt;NameOfAzureSubscription** &gt; s názvem hello předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="fdf79-138">Replace **&lt;NameOfAzureSubscription**&gt; with hello name of your Azure subscription.</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="fdf79-139">Poznamenejte si **SubscriptionId** a **TenantId** z hello výstup tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="fdf79-139">Note down **SubscriptionId** and **TenantId** from hello output of this command.</span></span>

5. <span data-ttu-id="fdf79-140">Vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup** tak, že spustíte následující příkaz v hello prostředí PowerShell hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-140">Create an Azure resource group named **ADFTutorialResourceGroup** by running hello following command in hello PowerShell.</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    <span data-ttu-id="fdf79-141">Pokud skupina prostředků hello již existuje, zadejte zda tooupdate ho (Y) nebo jej zachovat jako (ne).</span><span class="sxs-lookup"><span data-stu-id="fdf79-141">If hello resource group already exists, you specify whether tooupdate it (Y) or keep it as (N).</span></span>

    <span data-ttu-id="fdf79-142">Pokud používáte jiné skupině prostředků, musíte v tomto kurzu toouse hello název vaší skupiny prostředků místo skupiny ADFTutorialResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="fdf79-142">If you use a different resource group, you need toouse hello name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>
6. <span data-ttu-id="fdf79-143">Vytvořte aplikaci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="fdf79-143">Create an Azure Active Directory application.</span></span>

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"
    ```

    <span data-ttu-id="fdf79-144">Pokud dojde k následující chybě hello, zadejte jinou adresu URL a znovu spusťte příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-144">If you get hello following error, specify a different URL and run hello command again.</span></span>
    
    ```PowerShell
    Another object with hello same value for property identifierUris already exists.
    ```
7. <span data-ttu-id="fdf79-145">Vytvořte hello objekt služby AD.</span><span class="sxs-lookup"><span data-stu-id="fdf79-145">Create hello AD service principal.</span></span>

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. <span data-ttu-id="fdf79-146">Přidání služby hlavní toohello **Data Factory Přispěvatel** role.</span><span class="sxs-lookup"><span data-stu-id="fdf79-146">Add service principal toohello **Data Factory Contributor** role.</span></span>

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. <span data-ttu-id="fdf79-147">Získání ID hello aplikace.</span><span class="sxs-lookup"><span data-stu-id="fdf79-147">Get hello application ID.</span></span>

    ```PowerShell
    $azureAdApplication 
    ```
    <span data-ttu-id="fdf79-148">Poznamenejte si ID aplikace hello (applicationID) z výstupu hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-148">Note down hello application ID (applicationID) from hello output.</span></span>

<span data-ttu-id="fdf79-149">Z těchto kroků byste měli mít tyto čtyři hodnoty:</span><span class="sxs-lookup"><span data-stu-id="fdf79-149">You should have following four values from these steps:</span></span>

* <span data-ttu-id="fdf79-150">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="fdf79-150">Tenant ID</span></span>
* <span data-ttu-id="fdf79-151">ID předplatného</span><span class="sxs-lookup"><span data-stu-id="fdf79-151">Subscription ID</span></span>
* <span data-ttu-id="fdf79-152">ID aplikace</span><span class="sxs-lookup"><span data-stu-id="fdf79-152">Application ID</span></span>
* <span data-ttu-id="fdf79-153">Heslo (zadané v první příkaz hello)</span><span class="sxs-lookup"><span data-stu-id="fdf79-153">Password (specified in hello first command)</span></span>

## <a name="walkthrough"></a><span data-ttu-id="fdf79-154">Názorný postup</span><span class="sxs-lookup"><span data-stu-id="fdf79-154">Walkthrough</span></span>
1. <span data-ttu-id="fdf79-155">Pomocí sady Visual Studio 2012/2013/2015 vytvořte konzolovou aplikaci C# .NET.</span><span class="sxs-lookup"><span data-stu-id="fdf79-155">Using Visual Studio 2012/2013/2015, create a C# .NET console application.</span></span>
   1. <span data-ttu-id="fdf79-156">Spusťte **Visual Studio** 2012/2013/2015.</span><span class="sxs-lookup"><span data-stu-id="fdf79-156">Launch **Visual Studio** 2012/2013/2015.</span></span>
   2. <span data-ttu-id="fdf79-157">Klikněte na tlačítko **soubor**, bod příliš**nový**a klikněte na tlačítko **projektu**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-157">Click **File**, point too**New**, and click **Project**.</span></span>
   3. <span data-ttu-id="fdf79-158">Rozbalte **Šablony** a vyberte **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-158">Expand **Templates**, and select **Visual C#**.</span></span> <span data-ttu-id="fdf79-159">V tomto názorném postupu použijete C#, ale mohli byste využít libovolný jazyk .NET.</span><span class="sxs-lookup"><span data-stu-id="fdf79-159">In this walkthrough, you use C#, but you can use any .NET language.</span></span>
   4. <span data-ttu-id="fdf79-160">Vyberte **konzolové aplikace** hello seznamu typů projektu na hello správné.</span><span class="sxs-lookup"><span data-stu-id="fdf79-160">Select **Console Application** from hello list of project types on hello right.</span></span>
   5. <span data-ttu-id="fdf79-161">Zadejte **DataFactoryAPITestApp** pro hello název.</span><span class="sxs-lookup"><span data-stu-id="fdf79-161">Enter **DataFactoryAPITestApp** for hello Name.</span></span>
   6. <span data-ttu-id="fdf79-162">Vyberte **C:\ADFGetStarted** pro hello umístění.</span><span class="sxs-lookup"><span data-stu-id="fdf79-162">Select **C:\ADFGetStarted** for hello Location.</span></span>
   7. <span data-ttu-id="fdf79-163">Klikněte na tlačítko **OK** toocreate hello projektu.</span><span class="sxs-lookup"><span data-stu-id="fdf79-163">Click **OK** toocreate hello project.</span></span>
2. <span data-ttu-id="fdf79-164">Klikněte na tlačítko **nástroje**, bod příliš**Správce balíčků NuGet**a klikněte na tlačítko **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-164">Click **Tools**, point too**NuGet Package Manager**, and click **Package Manager Console**.</span></span>
3. <span data-ttu-id="fdf79-165">V hello **Konzola správce balíčků**, hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="fdf79-165">In hello **Package Manager Console**, do hello following steps:</span></span>
   1. <span data-ttu-id="fdf79-166">Spusťte následující příkaz tooinstall Data Factory balíček hello:`Install-Package Microsoft.Azure.Management.DataFactories`</span><span class="sxs-lookup"><span data-stu-id="fdf79-166">Run hello following command tooinstall Data Factory package: `Install-Package Microsoft.Azure.Management.DataFactories`</span></span>
   2. <span data-ttu-id="fdf79-167">Spusťte následující příkaz tooinstall Azure Active Directory balíčku (použít Active Directory API v kódu hello) hello:`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span><span class="sxs-lookup"><span data-stu-id="fdf79-167">Run hello following command tooinstall Azure Active Directory package (you use Active Directory API in hello code): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span></span>
4. <span data-ttu-id="fdf79-168">Přidejte následující hello **appSetttings** části toohello **App.config** souboru.</span><span class="sxs-lookup"><span data-stu-id="fdf79-168">Add hello following **appSetttings** section toohello **App.config** file.</span></span> <span data-ttu-id="fdf79-169">Tato nastavení jsou používána ve hello Pomocná metoda: **GetAuthorizationHeader**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-169">These settings are used by hello helper method: **GetAuthorizationHeader**.</span></span>

    <span data-ttu-id="fdf79-170">Nahraďte hodnoty pro **&lt;ID aplikace&gt;**, **&lt;Heslo&gt;**, **&lt;ID předplatného&gt;** a **&lt;ID tenanta&gt;** vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="fdf79-170">Replace values for **&lt;Application ID&gt;**, **&lt;Password&gt;**, **&lt;Subscription ID&gt;**, and **&lt;tenant ID&gt;** with your own values.</span></span>

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

5. <span data-ttu-id="fdf79-171">Přidejte následující hello **pomocí** příkazy toohello zdrojový soubor (Program.cs) v projektu hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-171">Add hello following **using** statements toohello source file (Program.cs) in hello project.</span></span>

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

6. <span data-ttu-id="fdf79-172">Přidejte následující kód, který vytvoří instanci hello **DataPipelineManagementClient** třída toohello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="fdf79-172">Add hello following code that creates an instance of **DataPipelineManagementClient** class toohello **Main** method.</span></span> <span data-ttu-id="fdf79-173">Tento objekt toocreate použijete objekt pro vytváření dat, propojené služby, vstupní a výstupní datové sady a kanál.</span><span class="sxs-lookup"><span data-stu-id="fdf79-173">You use this object toocreate a data factory, a linked service, input and output datasets, and a pipeline.</span></span> <span data-ttu-id="fdf79-174">Tento objekt toomonitor řezy datové sady se také použít za běhu.</span><span class="sxs-lookup"><span data-stu-id="fdf79-174">You also use this object toomonitor slices of a dataset at runtime.</span></span>

    ```csharp
    // create data factory management client
    string resourceGroupName = "ADFTutorialResourceGroup";
    string dataFactoryName = "APITutorialFactory";

    TokenCloudCredentials aadTokenCredentials = new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader().Result);

    Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="fdf79-175">Nahraďte hodnotu hello **resourceGroupName** s hello název vaší skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="fdf79-175">Replace hello value of **resourceGroupName** with hello name of your Azure resource group.</span></span>
   >
   > <span data-ttu-id="fdf79-176">Aktualizujte název hello data factory (dataFactoryName) toobe jedinečný.</span><span class="sxs-lookup"><span data-stu-id="fdf79-176">Update name of hello data factory (dataFactoryName) toobe unique.</span></span> <span data-ttu-id="fdf79-177">Název objektu pro vytváření dat hello musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="fdf79-177">Name of hello data factory must be globally unique.</span></span> <span data-ttu-id="fdf79-178">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="fdf79-178">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>

7. <span data-ttu-id="fdf79-179">Přidejte následující kód, který vytvoří hello **objekt pro vytváření dat** toohello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="fdf79-179">Add hello following code that creates a **data factory** toohello **Main** method.</span></span>

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

    <span data-ttu-id="fdf79-180">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="fdf79-180">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="fdf79-181">Kanál může obsahovat jednu nebo víc aktivit.</span><span class="sxs-lookup"><span data-stu-id="fdf79-181">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="fdf79-182">Například aktivitu kopírování dat toocopy z tooa zdroje cílového úložiště dat a toorun aktivitu HDInsight Hive tootransform skriptu Hive vstupní data tooproduct výstupní data.</span><span class="sxs-lookup"><span data-stu-id="fdf79-182">For example, a Copy Activity toocopy data from a source tooa destination data store and a HDInsight Hive activity toorun a Hive script tootransform input data tooproduct output data.</span></span> <span data-ttu-id="fdf79-183">Začneme vytvořením objektu pro vytváření dat hello v tomto kroku.</span><span class="sxs-lookup"><span data-stu-id="fdf79-183">Let's start with creating hello data factory in this step.</span></span>
8. <span data-ttu-id="fdf79-184">Přidejte následující kód, který vytvoří hello **propojená služba Azure Storage** toohello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="fdf79-184">Add hello following code that creates an **Azure Storage linked service** toohello **Main** method.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="fdf79-185">Položky **storageaccountname** a **accountkey** nahraďte názvem svého účtu Azure Storage a jeho klíčem.</span><span class="sxs-lookup"><span data-stu-id="fdf79-185">Replace **storageaccountname** and **accountkey** with name and key of your Azure Storage account.</span></span>

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

    <span data-ttu-id="fdf79-186">Vytvoření propojených služeb v objektu pro vytváření toolink dat, data se ukládá a výpočetní služby toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="fdf79-186">You create linked services in a data factory toolink your data stores and compute services toohello data factory.</span></span> <span data-ttu-id="fdf79-187">V tomto kurzu nebudete používat žádnou výpočetní službu jako je Azure HDInsight nebo Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="fdf79-187">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="fdf79-188">Budete používat dvě úložiště dat typu Azure Storage (zdroj) a Azure SQL Database (cíl).</span><span class="sxs-lookup"><span data-stu-id="fdf79-188">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

    <span data-ttu-id="fdf79-189">Vytvoříte tedy dvě propojené služby s názvem AzureStorageLinkedService a AzureSqlLinkedService typu: AzureStorage a AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="fdf79-189">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

    <span data-ttu-id="fdf79-190">Hello AzureStorageLinkedService propojuje účet úložiště Azure toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="fdf79-190">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="fdf79-191">Tento účet úložiště se hello, jeden ve které jste vytvořili kontejner a objemu odeslaných dat hello jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="fdf79-191">This storage account is hello one in which you created a container and uploaded hello data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
9. <span data-ttu-id="fdf79-192">Přidejte následující kód, který vytvoří hello **propojená služba Azure SQL** toohello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="fdf79-192">Add hello following code that creates an **Azure SQL linked service** toohello **Main** method.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="fdf79-193">Položky **servername**, **databasename**, **username** a **password** nahraďte názvem serveru SQL Azure, názvem databáze, uživatelským jménem a heslem.</span><span class="sxs-lookup"><span data-stu-id="fdf79-193">Replace **servername**, **databasename**, **username**, and **password** with names of your Azure SQL server, database, user, and password.</span></span>

    ```csharp
    // create a linked service for output data store: Azure SQL Database
    Console.WriteLine("Creating Azure SQL Database linked service");
    client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new LinkedServiceCreateOrUpdateParameters()
        {
            LinkedService = new LinkedService()
            {
                Name = "AzureSqlLinkedService",
                Properties = new LinkedServiceProperties
                (
                    new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                )
            }
        }
    );
    ```

    <span data-ttu-id="fdf79-194">AzureSqlLinkedService propojuje službu Azure SQL database toohello datovou továrnu.</span><span class="sxs-lookup"><span data-stu-id="fdf79-194">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="fdf79-195">Hello data, která se zkopírují z úložiště objektů blob hello jsou uložena v této databázi.</span><span class="sxs-lookup"><span data-stu-id="fdf79-195">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="fdf79-196">Vytvořit hello tabulce emp v této databázi jako součást [požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="fdf79-196">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
10. <span data-ttu-id="fdf79-197">Přidejte následující kód, který vytvoří hello **vstupní a výstupní datové sady** toohello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="fdf79-197">Add hello following code that creates **input and output datasets** toohello **Main** method.</span></span>

    ```csharp
    // create input and output datasets
    Console.WriteLine("Creating input and output datasets");
    string Dataset_Source = "InputDataset";
    string Dataset_Destination = "OutputDataset";

    Console.WriteLine("Creating input dataset of type: Azure Blob");
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,

    new DatasetCreateOrUpdateParameters()
    {
        Dataset = new Dataset()
        {
            Name = Dataset_Source,
            Properties = new DatasetProperties()
            {
                Structure = new List<DataElement>()
                {
                    new DataElement() { Name = "FirstName", Type = "String" },
                    new DataElement() { Name = "LastName", Type = "String" }
                },
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

    Console.WriteLine("Creating output dataset of type: Azure SQL");
    client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
        new DatasetCreateOrUpdateParameters()
        {
            Dataset = new Dataset()
            {
                Name = Dataset_Destination,
                Properties = new DatasetProperties()
                {
                    Structure = new List<DataElement>()
                    {
                        new DataElement() { Name = "FirstName", Type = "String" },
                        new DataElement() { Name = "LastName", Type = "String" }
                    },
                    LinkedServiceName = "AzureSqlLinkedService",
                    TypeProperties = new AzureSqlTableDataset()
                    {
                        TableName = "emp"
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
    
    <span data-ttu-id="fdf79-198">V předchozím kroku hello jste vytvořili propojené služby toolink, účet úložiště Azure a Azure SQL database tooyour objekt pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="fdf79-198">In hello previous step, you created linked services toolink your Azure Storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="fdf79-199">V tomto kroku definujete dvě datové sady s názvem InputDataset a OutputDataset, které představují vstupní a výstupní data, která jsou uložená v úložištích dat hello odkazuje AzureStorageLinkedService a AzureSqlLinkedService.</span><span class="sxs-lookup"><span data-stu-id="fdf79-199">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in hello data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

    <span data-ttu-id="fdf79-200">Hello propojená služba úložiště Azure určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="fdf79-200">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="fdf79-201">A datovou sadu hello vstupního objektu blob (InputDataset) určuje hello kontejneru a složce hello, který obsahuje vstupní data hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-201">And, hello input blob dataset (InputDataset) specifies hello container and hello folder that contains hello input data.</span></span>  

    <span data-ttu-id="fdf79-202">Podobně hello služby Azure SQL Database propojené určuje hello připojovací řetězec, který používá služba Data Factory v době běhu tooconnect tooyour Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="fdf79-202">Similarly, hello Azure SQL Database linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure SQL database.</span></span> <span data-ttu-id="fdf79-203">A hello výstupní datovou sadu tabulky SQL (OututDataset) určuje hello tabulky v hello databáze hello toowhich zkopírování dat z úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-203">And, hello output SQL table dataset (OututDataset) specifies hello table in hello database toowhich hello data from hello blob storage is copied.</span></span>

    <span data-ttu-id="fdf79-204">V tomto kroku vytvoříte datové sady s názvem InputDataset, který ukazuje soubor blob tooa (emp.txt) v kořenové složce hello kontejner objektů blob (adftutorial) v hello Azure Storage reprezentované hello AzureStorageLinkedService propojené služby.</span><span class="sxs-lookup"><span data-stu-id="fdf79-204">In this step, you create a dataset named InputDataset that points tooa blob file (emp.txt) in hello root folder of a blob container (adftutorial) in hello Azure Storage represented by hello AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="fdf79-205">Pokud nechcete zadat hodnotu pro název souboru hello (nebo ji přeskočit), jsou data ze všech objektů BLOB ve složce vstupní hello zkopírovaný toohello cílový.</span><span class="sxs-lookup"><span data-stu-id="fdf79-205">If you don't specify a value for hello fileName (or skip it), data from all blobs in hello input folder are copied toohello destination.</span></span> <span data-ttu-id="fdf79-206">V tomto kurzu zadejte hodnotu pro název souboru hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-206">In this tutorial, you specify a value for hello fileName.</span></span>    

    <span data-ttu-id="fdf79-207">V tomto kroku vytvoříte výstupní datovou sadu s názvem **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-207">In this step, you create an output dataset named **OutputDataset**.</span></span> <span data-ttu-id="fdf79-208">Tato datová sada body tooa tabulky SQL ve hello Azure SQL database reprezentované **AzureSqlLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-208">This dataset points tooa SQL table in hello Azure SQL database represented by **AzureSqlLinkedService**.</span></span>
11. <span data-ttu-id="fdf79-209">Přidat hello následující kód, který **vytvoří a aktivuje kanálu** toohello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="fdf79-209">Add hello following code that **creates and activates a pipeline** toohello **Main** method.</span></span> <span data-ttu-id="fdf79-210">V tomto kroku vytvoříte kanál s **aktivitou kopírování**, která používá **InputDataset** jako vstup a **OutputDataset** jako výstup.</span><span class="sxs-lookup"><span data-stu-id="fdf79-210">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

    ```csharp
    // create a pipeline
    Console.WriteLine("Creating a pipeline");
    DateTime PipelineActivePeriodStartTime = new DateTime(2017, 5, 11, 0, 0, 0, 0, DateTimeKind.Utc);
    DateTime PipelineActivePeriodEndTime = new DateTime(2017, 5, 12, 0, 0, 0, 0, DateTimeKind.Utc);
    string PipelineName = "ADFTutorialPipeline";

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
                            Name = "BlobToAzureSql",
                            Inputs = new List<ActivityInput>()
                            {
                                new ActivityInput() {
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
                    }
                }
            }
        });
    ```

    <span data-ttu-id="fdf79-211">Všimněte si hello následující body:</span><span class="sxs-lookup"><span data-stu-id="fdf79-211">Note hello following points:</span></span>
   
    - <span data-ttu-id="fdf79-212">V části hello aktivit je jenom jedna aktivita jejichž **typ** je nastaven příliš**kopie**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-212">In hello activities section, there is only one activity whose **type** is set too**Copy**.</span></span> <span data-ttu-id="fdf79-213">Další informace o aktivitě kopírování hello najdete v tématu [aktivity přesunu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="fdf79-213">For more information about hello copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="fdf79-214">V řešeních služby Data Factory můžete také použít [aktivity transformace dat](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="fdf79-214">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="fdf79-215">Vstup aktivity hello nastaven příliš**InputDataset** a výstup hello aktivity je nastavený příliš**OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-215">Input for hello activity is set too**InputDataset** and output for hello activity is set too**OutputDataset**.</span></span> 
    - <span data-ttu-id="fdf79-216">V hello **rámci typeProperties** části **BlobSource** je zadán jako typ zdroje hello a **SqlSink** je zadán jako typ jímky hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-216">In hello **typeProperties** section, **BlobSource** is specified as hello source type and **SqlSink** is specified as hello sink type.</span></span> <span data-ttu-id="fdf79-217">Úplný seznam úložišť dat nepodporuje aktivity kopírování hello jako zdroje a jímky, najdete v části [podporovanými úložišti dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="fdf79-217">For a complete list of data stores supported by hello copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="fdf79-218">toolearn jak toouse konkrétní podporované datové úložiště jako zdroj/jímka, klikněte na odkaz hello v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-218">toolearn how toouse a specific supported data store as a source/sink, click hello link in hello table.</span></span>  
   
    <span data-ttu-id="fdf79-219">Výstupní datové sady v současné době je, jaké jednotky hello plán.</span><span class="sxs-lookup"><span data-stu-id="fdf79-219">Currently, output dataset is what drives hello schedule.</span></span> <span data-ttu-id="fdf79-220">V tomto kurzu výstupní datové sady je nakonfigurované tooproduce řez jednou za hodinu.</span><span class="sxs-lookup"><span data-stu-id="fdf79-220">In this tutorial, output dataset is configured tooproduce a slice once an hour.</span></span> <span data-ttu-id="fdf79-221">Hello kanálu má čas spuštění a čas ukončení, které jsou od sebe, jeden den, což je 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="fdf79-221">hello pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="fdf79-222">Proto 24 řezů výstupní datové sady vyprodukované hello kanálu.</span><span class="sxs-lookup"><span data-stu-id="fdf79-222">Therefore, 24 slices of output dataset are produced by hello pipeline.</span></span>
12. <span data-ttu-id="fdf79-223">Přidejte následující kód toohello hello **hlavní** metoda tooget hello stav datový řez ze hello výstupní datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="fdf79-223">Add hello following code toohello **Main** method tooget hello status of a data slice of hello output dataset.</span></span> <span data-ttu-id="fdf79-224">V této ukázce se očekává jenom jeden řez.</span><span class="sxs-lookup"><span data-stu-id="fdf79-224">There is only slice expected in this sample.</span></span>

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

13. <span data-ttu-id="fdf79-225">Přidejte následující kód tooget spustit podrobnosti datový řez toohello hello **hlavní** metoda.</span><span class="sxs-lookup"><span data-stu-id="fdf79-225">Add hello following code tooget run details for a data slice toohello **Main** method.</span></span>

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
            }
        );

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

14. <span data-ttu-id="fdf79-226">Přidejte následující metodu helper používané hello hello **hlavní** metoda toohello **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="fdf79-226">Add hello following helper method used by hello **Main** method toohello **Program** class.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fdf79-227">Když je kopírujete a vkládáte hello následující kód, ujistěte se, že hello zkopírovaný kód je ve stejné úrovni jako hello metodu Main hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-227">When you copy and paste hello following code, make sure that hello copied code is at hello same level as hello Main method.</span></span>

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

15. <span data-ttu-id="fdf79-228">V hello Průzkumníku řešení rozbalte projekt hello (DataFactoryAPITestApp), klikněte pravým tlačítkem na **odkazy**a klikněte na tlačítko **přidat odkaz na**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-228">In hello Solution Explorer, expand hello project (DataFactoryAPITestApp), right-click **References**, and click **Add Reference**.</span></span> <span data-ttu-id="fdf79-229">Zaškrtněte políčko pro sestavení **System.Configuration**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-229">Select check box for **System.Configuration** assembly.</span></span> <span data-ttu-id="fdf79-230">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-230">and click **OK**.</span></span>
16. <span data-ttu-id="fdf79-231">Vytvoření konzolové aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-231">Build hello console application.</span></span> <span data-ttu-id="fdf79-232">Klikněte na tlačítko **sestavení** na hello nabídky a klikněte na **sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-232">Click **Build** on hello menu and click **Build Solution**.</span></span>
17. <span data-ttu-id="fdf79-233">Potvrďte, že je alespoň jeden soubor v hello **adftutorial** kontejneru ve službě Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="fdf79-233">Confirm that there is at least one file in hello **adftutorial** container in your Azure blob storage.</span></span> <span data-ttu-id="fdf79-234">Pokud ne, vytvořte **Emp.txt** soubor v poznámkovém bloku s hello následující obsah a nahrajte ho toohello adftutorial kontejneru.</span><span class="sxs-lookup"><span data-stu-id="fdf79-234">If not, create **Emp.txt** file in Notepad with hello following content and upload it toohello adftutorial container.</span></span>

    ```
    John, Doe
    Jane, Doe
    ```
18. <span data-ttu-id="fdf79-235">Hello ukázku spustit kliknutím **ladění** -> **spustit ladění** v nabídce hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-235">Run hello sample by clicking **Debug** -> **Start Debugging** on hello menu.</span></span> <span data-ttu-id="fdf79-236">Až se zobrazí hello **získávání spustit podrobnosti datový řez**, počkejte několik minut a stiskněte klávesu **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-236">When you see hello **Getting run details of a data slice**, wait for a few minutes, and press **ENTER**.</span></span>
19. <span data-ttu-id="fdf79-237">Použití této datové továrně hello hello Azure portálu tooverify **APITutorialFactory** je vytvořena s hello artefakty následující:</span><span class="sxs-lookup"><span data-stu-id="fdf79-237">Use hello Azure portal tooverify that hello data factory **APITutorialFactory** is created with hello following artifacts:</span></span>
   * <span data-ttu-id="fdf79-238">Propojená služba: **LinkedService_AzureStorage**</span><span class="sxs-lookup"><span data-stu-id="fdf79-238">Linked service: **LinkedService_AzureStorage**</span></span>
   * <span data-ttu-id="fdf79-239">Datová sada: **InputDataset** a **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-239">Dataset: **InputDataset** and **OutputDataset**.</span></span>
   * <span data-ttu-id="fdf79-240">Kanál: **PipelineBlobSample**</span><span class="sxs-lookup"><span data-stu-id="fdf79-240">Pipeline: **PipelineBlobSample**</span></span>
20. <span data-ttu-id="fdf79-241">Ověřte, že hello dva záznamy zaměstnanců jsou vytvořeny v hello **emp** tabulky v hello zadat databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="fdf79-241">Verify that hello two employee records are created in hello **emp** table in hello specified Azure SQL database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fdf79-242">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fdf79-242">Next steps</span></span>
<span data-ttu-id="fdf79-243">Úplnou dokumentaci k rozhraní .NET API pro datovou továrnu najdete v [referencích k rozhraní .NET API služby Data Factory](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).</span><span class="sxs-lookup"><span data-stu-id="fdf79-243">For complete documentation on .NET API for Data Factory, see [Data Factory .NET API Reference](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).</span></span>

<span data-ttu-id="fdf79-244">V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="fdf79-244">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="fdf79-245">Hello následující tabulka obsahuje seznam úložiště dat, které jsou podporované jako zdroje a cíle pomocí aktivity kopírování hello:</span><span class="sxs-lookup"><span data-stu-id="fdf79-245">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="fdf79-246">toolearn o tom, jak toocopy data z datové úložiště, klikněte na odkaz hello hello úložiště dat v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-246">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span>

