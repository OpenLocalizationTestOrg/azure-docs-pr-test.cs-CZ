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
ms.openlocfilehash: 30c7cd1ba455d7b1bc93d76e7ee79455bb52aae9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a><span data-ttu-id="5df71-103">Kurz: Vytvoření kanálu s aktivitou kopírování pomocí rozhraní .NET API</span><span class="sxs-lookup"><span data-stu-id="5df71-103">Tutorial: Create a pipeline with Copy Activity using .NET API</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5df71-104">Přehled a požadavky</span><span class="sxs-lookup"><span data-stu-id="5df71-104">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="5df71-105">Průvodce kopírováním</span><span class="sxs-lookup"><span data-stu-id="5df71-105">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="5df71-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5df71-106">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="5df71-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5df71-107">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="5df71-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5df71-108">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="5df71-109">Šablona Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="5df71-109">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="5df71-110">REST API</span><span class="sxs-lookup"><span data-stu-id="5df71-110">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="5df71-111">.NET API</span><span class="sxs-lookup"><span data-stu-id="5df71-111">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)

<span data-ttu-id="5df71-112">V tomto článku se naučíte, jak používat rozhraní [.NET API](https://portal.azure.com), abyste vytvořili datovou továrnu s kanálem, který kopíruje data z úložiště objektů blob v Azure do databáze SQL v Azure.</span><span class="sxs-lookup"><span data-stu-id="5df71-112">In this article, you learn how to use [.NET API](https://portal.azure.com) to create a data factory with a pipeline that copies data from an Azure blob storage to an Azure SQL database.</span></span> <span data-ttu-id="5df71-113">Pokud s Azure Data Factory začínáte, přečtěte si článek [Seznámení se službou Azure Data Factory](data-factory-introduction.md), než s tímto kurzem začnete.</span><span class="sxs-lookup"><span data-stu-id="5df71-113">If you are new to Azure Data Factory, read through the [Introduction to Azure Data Factory](data-factory-introduction.md) article before doing this tutorial.</span></span>   

<span data-ttu-id="5df71-114">V tomto kurzu vytvoříte kanál s jednou aktivitou: aktivita kopírování.</span><span class="sxs-lookup"><span data-stu-id="5df71-114">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="5df71-115">Aktivita kopírování kopíruje data z podporovaného úložiště dat do podporovaného úložiště dat jímky.</span><span class="sxs-lookup"><span data-stu-id="5df71-115">The copy activity copies data from a supported data store to a supported sink data store.</span></span> <span data-ttu-id="5df71-116">Seznam úložišť dat podporovaných jako zdroje a jímky najdete v tématu [podporovaná úložiště dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="5df71-116">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="5df71-117">Aktivita používá globálně dostupnou službu, která může kopírovat data mezi různými úložišti dat zabezpečeným, spolehlivým a škálovatelným způsobem.</span><span class="sxs-lookup"><span data-stu-id="5df71-117">The activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="5df71-118">Další informace o aktivitě kopírování najdete v tématu [Aktivity pohybu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="5df71-118">For more information about the Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="5df71-119">Kanál může obsahovat víc než jednu aktivitu.</span><span class="sxs-lookup"><span data-stu-id="5df71-119">A pipeline can have more than one activity.</span></span> <span data-ttu-id="5df71-120">A dvě aktivity můžete zřetězit (spustit jednu aktivitu po druhé) nastavením výstupní datové sady jedné aktivity jako vstupní datové sady druhé aktivity.</span><span class="sxs-lookup"><span data-stu-id="5df71-120">And, you can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="5df71-121">Další informace naleznete, když přejdete na [více aktivit v kanálu](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="5df71-121">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span> 

> [!NOTE] 
> <span data-ttu-id="5df71-122">Úplnou dokumentaci k rozhraní .NET API pro datovou továrnu najdete v [referencích k rozhraní .NET API služby Data Factory](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).</span><span class="sxs-lookup"><span data-stu-id="5df71-122">For complete documentation on .NET API for Data Factory, see [Data Factory .NET API Reference](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).</span></span>
> 
> <span data-ttu-id="5df71-123">Datový kanál v tomto kurzu kopíruje data ze zdrojového úložiště dat do cílového úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="5df71-123">The data pipeline in this tutorial copies data from a source data store to a destination data store.</span></span> <span data-ttu-id="5df71-124">Kurz předvádějící způsoby transformace dat pomocí Azure Data Factory najdete v tématu popisujícím [kurz vytvoření kanálu, který umožňuje transformovat data pomocí clusteru Hadoop](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="5df71-124">For a tutorial on how to transform data using Azure Data Factory, see [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5df71-125">Požadavky</span><span class="sxs-lookup"><span data-stu-id="5df71-125">Prerequisites</span></span>
* <span data-ttu-id="5df71-126">Projděte si [Přehled a požadavky kurzu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md), kde najdete informace o kurzu, a proveďte **nutné** kroky.</span><span class="sxs-lookup"><span data-stu-id="5df71-126">Go through [Tutorial Overview and Pre-requisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) to get an overview of the tutorial and complete the **prerequisite** steps.</span></span>
* <span data-ttu-id="5df71-127">Visual Studio 2012 nebo 2013 nebo 2015</span><span class="sxs-lookup"><span data-stu-id="5df71-127">Visual Studio 2012 or 2013 or 2015</span></span>
* <span data-ttu-id="5df71-128">Stáhněte sadu [Azure .NET SDK](http://azure.microsoft.com/downloads/) a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="5df71-128">Download and install [Azure .NET SDK](http://azure.microsoft.com/downloads/)</span></span>
* <span data-ttu-id="5df71-129">Azure Powershell</span><span class="sxs-lookup"><span data-stu-id="5df71-129">Azure PowerShell.</span></span> <span data-ttu-id="5df71-130">Podle pokynů v článku [Instalace a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md) si na počítač nainstalujte prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5df71-130">Follow instructions in [How to install and configure Azure PowerShell](../powershell-install-configure.md) article to install Azure PowerShell on your computer.</span></span> <span data-ttu-id="5df71-131">K vytvoření aplikace v Azure Active Directory použijete Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5df71-131">You use Azure PowerShell to create an Azure Active Directory application.</span></span>

### <a name="create-an-application-in-azure-active-directory"></a><span data-ttu-id="5df71-132">Vytvoření aplikace v Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="5df71-132">Create an application in Azure Active Directory</span></span>
<span data-ttu-id="5df71-133">Vytvořte aplikaci Azure Active Directory, vytvořte pro ni instanční objekt a přiřaďte ho roli **Přispěvatel Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="5df71-133">Create an Azure Active Directory application, create a service principal for the application, and assign it to the **Data Factory Contributor** role.</span></span>

1. <span data-ttu-id="5df71-134">Spusťte **PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="5df71-134">Launch **PowerShell**.</span></span>
2. <span data-ttu-id="5df71-135">Spusťte následující příkaz a zadejte uživatelské jméno a heslo, které používáte k přihlášení na web Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="5df71-135">Run the following command and enter the user name and password that you use to sign in to the Azure portal.</span></span>

    ```PowerShell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="5df71-136">Spuštěním následujícího příkazu zobrazíte všechna předplatná pro tento účet.</span><span class="sxs-lookup"><span data-stu-id="5df71-136">Run the following command to view all the subscriptions for this account.</span></span>

    ```PowerShell
    Get-AzureRmSubscription
    ```
4. <span data-ttu-id="5df71-137">Spuštěním následujícího příkazu vyberte předplatné, se kterým chcete pracovat.</span><span class="sxs-lookup"><span data-stu-id="5df71-137">Run the following command to select the subscription that you want to work with.</span></span> <span data-ttu-id="5df71-138">Místo **&lt;NameOfAzureSubscription**&gt; zadejte název svého předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="5df71-138">Replace **&lt;NameOfAzureSubscription**&gt; with the name of your Azure subscription.</span></span>

    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="5df71-139">Poznamenejte si **SubscriptionId** a **TenantId** z výstupu tohoto příkazu.</span><span class="sxs-lookup"><span data-stu-id="5df71-139">Note down **SubscriptionId** and **TenantId** from the output of this command.</span></span>

5. <span data-ttu-id="5df71-140">Spuštěním následujícího příkazu v PowerShellu vytvořte skupinu prostředků Azure s názvem **ADFTutorialResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="5df71-140">Create an Azure resource group named **ADFTutorialResourceGroup** by running the following command in the PowerShell.</span></span>

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

    <span data-ttu-id="5df71-141">Pokud skupina prostředků už existuje, určete, jestli se má aktualizovat (Y), nebo ponechat tak, jak je (N).</span><span class="sxs-lookup"><span data-stu-id="5df71-141">If the resource group already exists, you specify whether to update it (Y) or keep it as (N).</span></span>

    <span data-ttu-id="5df71-142">Pokud používáte jinou skupinu prostředků, použijte v postupech v tomto kurzu místo skupiny ADFTutorialResourceGroup název vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="5df71-142">If you use a different resource group, you need to use the name of your resource group in place of ADFTutorialResourceGroup in this tutorial.</span></span>
6. <span data-ttu-id="5df71-143">Vytvořte aplikaci Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="5df71-143">Create an Azure Active Directory application.</span></span>

    ```PowerShell
    $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"
    ```

    <span data-ttu-id="5df71-144">Pokud se zobrazí následující chyba, zadejte jinou adresu URL a spusťte příkaz znovu.</span><span class="sxs-lookup"><span data-stu-id="5df71-144">If you get the following error, specify a different URL and run the command again.</span></span>
    
    ```PowerShell
    Another object with the same value for property identifierUris already exists.
    ```
7. <span data-ttu-id="5df71-145">Vytvořte instanční objekt služby AD.</span><span class="sxs-lookup"><span data-stu-id="5df71-145">Create the AD service principal.</span></span>

    ```PowerShell
    New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    ```
8. <span data-ttu-id="5df71-146">Přidejte instanční objekt k roli **Přispěvatel Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="5df71-146">Add service principal to the **Data Factory Contributor** role.</span></span>

    ```PowerShell
    New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    ```
9. <span data-ttu-id="5df71-147">Získejte ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="5df71-147">Get the application ID.</span></span>

    ```PowerShell
    $azureAdApplication 
    ```
    <span data-ttu-id="5df71-148">Poznamenejte si ID aplikace (applicationID) ve výstupu.</span><span class="sxs-lookup"><span data-stu-id="5df71-148">Note down the application ID (applicationID) from the output.</span></span>

<span data-ttu-id="5df71-149">Z těchto kroků byste měli mít tyto čtyři hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5df71-149">You should have following four values from these steps:</span></span>

* <span data-ttu-id="5df71-150">ID tenanta</span><span class="sxs-lookup"><span data-stu-id="5df71-150">Tenant ID</span></span>
* <span data-ttu-id="5df71-151">ID předplatného</span><span class="sxs-lookup"><span data-stu-id="5df71-151">Subscription ID</span></span>
* <span data-ttu-id="5df71-152">ID aplikace</span><span class="sxs-lookup"><span data-stu-id="5df71-152">Application ID</span></span>
* <span data-ttu-id="5df71-153">Heslo (zadané v prvním příkazu)</span><span class="sxs-lookup"><span data-stu-id="5df71-153">Password (specified in the first command)</span></span>

## <a name="walkthrough"></a><span data-ttu-id="5df71-154">Názorný postup</span><span class="sxs-lookup"><span data-stu-id="5df71-154">Walkthrough</span></span>
1. <span data-ttu-id="5df71-155">Pomocí sady Visual Studio 2012/2013/2015 vytvořte konzolovou aplikaci C# .NET.</span><span class="sxs-lookup"><span data-stu-id="5df71-155">Using Visual Studio 2012/2013/2015, create a C# .NET console application.</span></span>
   1. <span data-ttu-id="5df71-156">Spusťte **Visual Studio** 2012/2013/2015.</span><span class="sxs-lookup"><span data-stu-id="5df71-156">Launch **Visual Studio** 2012/2013/2015.</span></span>
   2. <span data-ttu-id="5df71-157">Klikněte na **Soubor**, přejděte na **Nový** a klikněte na **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="5df71-157">Click **File**, point to **New**, and click **Project**.</span></span>
   3. <span data-ttu-id="5df71-158">Rozbalte **Šablony** a vyberte **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="5df71-158">Expand **Templates**, and select **Visual C#**.</span></span> <span data-ttu-id="5df71-159">V tomto názorném postupu použijete C#, ale mohli byste využít libovolný jazyk .NET.</span><span class="sxs-lookup"><span data-stu-id="5df71-159">In this walkthrough, you use C#, but you can use any .NET language.</span></span>
   4. <span data-ttu-id="5df71-160">V seznamu typů projektů napravo vyberte **Konzolová aplikace**.</span><span class="sxs-lookup"><span data-stu-id="5df71-160">Select **Console Application** from the list of project types on the right.</span></span>
   5. <span data-ttu-id="5df71-161">Jako název zadejte **DataFactoryAPITestApp**.</span><span class="sxs-lookup"><span data-stu-id="5df71-161">Enter **DataFactoryAPITestApp** for the Name.</span></span>
   6. <span data-ttu-id="5df71-162">Jako umístění vyberte **C:\ADFGetStarted**.</span><span class="sxs-lookup"><span data-stu-id="5df71-162">Select **C:\ADFGetStarted** for the Location.</span></span>
   7. <span data-ttu-id="5df71-163">Projekt vytvoříte kliknutím na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5df71-163">Click **OK** to create the project.</span></span>
2. <span data-ttu-id="5df71-164">Klikněte na **Nástroje**, přejděte na **Správce balíčků NuGet** a klikněte na **Konzola Správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="5df71-164">Click **Tools**, point to **NuGet Package Manager**, and click **Package Manager Console**.</span></span>
3. <span data-ttu-id="5df71-165">V **Konzole Správce balíčků** postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="5df71-165">In the **Package Manager Console**, do the following steps:</span></span>
   1. <span data-ttu-id="5df71-166">Spusťte následující příkaz a nainstalujte balíček služby Data Factory: `Install-Package Microsoft.Azure.Management.DataFactories`</span><span class="sxs-lookup"><span data-stu-id="5df71-166">Run the following command to install Data Factory package: `Install-Package Microsoft.Azure.Management.DataFactories`</span></span>
   2. <span data-ttu-id="5df71-167">Spusťte následující příkaz pro instalaci balíčku Azure Active Directory (v kódu použijete rozhraní API Active Directory): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span><span class="sxs-lookup"><span data-stu-id="5df71-167">Run the following command to install Azure Active Directory package (you use Active Directory API in the code): `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`</span></span>
4. <span data-ttu-id="5df71-168">Do souboru **App.config** přidejte následující část **appSetttings**.</span><span class="sxs-lookup"><span data-stu-id="5df71-168">Add the following **appSetttings** section to the **App.config** file.</span></span> <span data-ttu-id="5df71-169">Tyto nastavení používá pomocná metoda: **GetAuthorizationHeader**.</span><span class="sxs-lookup"><span data-stu-id="5df71-169">These settings are used by the helper method: **GetAuthorizationHeader**.</span></span>

    <span data-ttu-id="5df71-170">Nahraďte hodnoty pro **&lt;ID aplikace&gt;**, **&lt;Heslo&gt;**, **&lt;ID předplatného&gt;** a **&lt;ID tenanta&gt;** vlastními hodnotami.</span><span class="sxs-lookup"><span data-stu-id="5df71-170">Replace values for **&lt;Application ID&gt;**, **&lt;Password&gt;**, **&lt;Subscription ID&gt;**, and **&lt;tenant ID&gt;** with your own values.</span></span>

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

5. <span data-ttu-id="5df71-171">Do zdrojového souboru (Program.cs) v projektu přidejte následující příkazy **using**.</span><span class="sxs-lookup"><span data-stu-id="5df71-171">Add the following **using** statements to the source file (Program.cs) in the project.</span></span>

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

6. <span data-ttu-id="5df71-172">Do metody **Main** přidejte následující kód, který vytvoří instanci třídy **DataPipelineManagementClient**.</span><span class="sxs-lookup"><span data-stu-id="5df71-172">Add the following code that creates an instance of **DataPipelineManagementClient** class to the **Main** method.</span></span> <span data-ttu-id="5df71-173">Tento objekt použijete k vytvoření objektu pro vytváření dat, propojené služby, vstupních a výstupních datových sad a kanálu.</span><span class="sxs-lookup"><span data-stu-id="5df71-173">You use this object to create a data factory, a linked service, input and output datasets, and a pipeline.</span></span> <span data-ttu-id="5df71-174">Použijete ho také k monitorování řezů datových sad při spuštění.</span><span class="sxs-lookup"><span data-stu-id="5df71-174">You also use this object to monitor slices of a dataset at runtime.</span></span>

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
   > <span data-ttu-id="5df71-175">Hodnotu **resourceGroupName** nahraďte názvem skupiny prostředků Azure.</span><span class="sxs-lookup"><span data-stu-id="5df71-175">Replace the value of **resourceGroupName** with the name of your Azure resource group.</span></span>
   >
   > <span data-ttu-id="5df71-176">Aktualizujte název datové továrny (dataFactoryName) tak, aby byl jedinečný.</span><span class="sxs-lookup"><span data-stu-id="5df71-176">Update name of the data factory (dataFactoryName) to be unique.</span></span> <span data-ttu-id="5df71-177">Název objektu pro vytváření dat musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="5df71-177">Name of the data factory must be globally unique.</span></span> <span data-ttu-id="5df71-178">V tématu [Objekty pro vytváření dat – pravidla pojmenování](data-factory-naming-rules.md) najdete pravidla pojmenování artefaktů služby Data Factory.</span><span class="sxs-lookup"><span data-stu-id="5df71-178">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>

7. <span data-ttu-id="5df71-179">Do metody **Main** přidejte následující kód, který vytvoří **objekt pro vytváření dat**.</span><span class="sxs-lookup"><span data-stu-id="5df71-179">Add the following code that creates a **data factory** to the **Main** method.</span></span>

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

    <span data-ttu-id="5df71-180">Objekt pro vytváření dat může mít jeden nebo víc kanálů.</span><span class="sxs-lookup"><span data-stu-id="5df71-180">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="5df71-181">Kanál může obsahovat jednu nebo víc aktivit.</span><span class="sxs-lookup"><span data-stu-id="5df71-181">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="5df71-182">Může obsahovat například aktivitu kopírování, která slouží ke kopírování dat ze zdrojového do cílového úložiště dat, a aktivitu HDInsight Hive pro spuštění skriptu Hive, který umožňuje transformovat vstupní data na výstupní data produktu.</span><span class="sxs-lookup"><span data-stu-id="5df71-182">For example, a Copy Activity to copy data from a source to a destination data store and a HDInsight Hive activity to run a Hive script to transform input data to product output data.</span></span> <span data-ttu-id="5df71-183">V tomto kroku začneme vytvořením objektu pro vytváření dat.</span><span class="sxs-lookup"><span data-stu-id="5df71-183">Let's start with creating the data factory in this step.</span></span>
8. <span data-ttu-id="5df71-184">Do metody **Main** přidejte následující kód, který vytvoří **propojenou službu Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="5df71-184">Add the following code that creates an **Azure Storage linked service** to the **Main** method.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="5df71-185">Položky **storageaccountname** a **accountkey** nahraďte názvem svého účtu Azure Storage a jeho klíčem.</span><span class="sxs-lookup"><span data-stu-id="5df71-185">Replace **storageaccountname** and **accountkey** with name and key of your Azure Storage account.</span></span>

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

    <span data-ttu-id="5df71-186">V datové továrně vytvoříte propojené služby, abyste svá úložiště dat a výpočetní služby spojili s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="5df71-186">You create linked services in a data factory to link your data stores and compute services to the data factory.</span></span> <span data-ttu-id="5df71-187">V tomto kurzu nebudete používat žádnou výpočetní službu jako je Azure HDInsight nebo Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="5df71-187">In this tutorial, you don't use any compute service such as Azure HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="5df71-188">Budete používat dvě úložiště dat typu Azure Storage (zdroj) a Azure SQL Database (cíl).</span><span class="sxs-lookup"><span data-stu-id="5df71-188">You use two data stores of type Azure Storage (source) and Azure SQL Database (destination).</span></span> 

    <span data-ttu-id="5df71-189">Vytvoříte tedy dvě propojené služby s názvem AzureStorageLinkedService a AzureSqlLinkedService typu: AzureStorage a AzureSqlDatabase.</span><span class="sxs-lookup"><span data-stu-id="5df71-189">Therefore, you create two linked services named AzureStorageLinkedService and AzureSqlLinkedService of types: AzureStorage and AzureSqlDatabase.</span></span>  

    <span data-ttu-id="5df71-190">Služba AzureStorageLinkedService propojí váš účet služby Azure Storage s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="5df71-190">The AzureStorageLinkedService links your Azure storage account to the data factory.</span></span> <span data-ttu-id="5df71-191">Tento účet úložiště je ten, ve kterém jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili kontejner a nahráli do něj data.</span><span class="sxs-lookup"><span data-stu-id="5df71-191">This storage account is the one in which you created a container and uploaded the data as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
9. <span data-ttu-id="5df71-192">Do metody **Main** přidejte následující kód, který vytvoří **propojenou službu Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="5df71-192">Add the following code that creates an **Azure SQL linked service** to the **Main** method.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="5df71-193">Položky **servername**, **databasename**, **username** a **password** nahraďte názvem serveru SQL Azure, názvem databáze, uživatelským jménem a heslem.</span><span class="sxs-lookup"><span data-stu-id="5df71-193">Replace **servername**, **databasename**, **username**, and **password** with names of your Azure SQL server, database, user, and password.</span></span>

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

    <span data-ttu-id="5df71-194">Služba AzureSqlLinkedService propojí službu Azure SQL Database s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="5df71-194">AzureSqlLinkedService links your Azure SQL database to the data factory.</span></span> <span data-ttu-id="5df71-195">Data kopírovaná z úložiště objektů blob se ukládají do této databáze.</span><span class="sxs-lookup"><span data-stu-id="5df71-195">The data that is copied from the blob storage is stored in this database.</span></span> <span data-ttu-id="5df71-196">V této databázi jste v rámci [požadavků](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) vytvořili tabulku emp.</span><span class="sxs-lookup"><span data-stu-id="5df71-196">You created the emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
10. <span data-ttu-id="5df71-197">Do metody **Main** přidejte následující kód, který vytvoří **vstupní a výstupní datové sady**.</span><span class="sxs-lookup"><span data-stu-id="5df71-197">Add the following code that creates **input and output datasets** to the **Main** method.</span></span>

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
    
    <span data-ttu-id="5df71-198">V předchozím kroku jste vytvořili propojené služby, abyste propojili účet úložiště Azure a Azure SQL Database s datovou továrnou.</span><span class="sxs-lookup"><span data-stu-id="5df71-198">In the previous step, you created linked services to link your Azure Storage account and Azure SQL database to your data factory.</span></span> <span data-ttu-id="5df71-199">V tomto kroku nadefinujete dvě datové sady s názvem InputDataset a OutputDataset, které představují vstupní a výstupní data uložená v úložištích dat, na která odkazují služby AzureStorageLinkedService a AzureSqlLinkedService.</span><span class="sxs-lookup"><span data-stu-id="5df71-199">In this step, you define two datasets named InputDataset and OutputDataset that represent input and output data that is stored in the data stores referred by AzureStorageLinkedService and AzureSqlLinkedService respectively.</span></span>

    <span data-ttu-id="5df71-200">Propojená služba úložiště Azure určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="5df71-200">The Azure storage linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure storage account.</span></span> <span data-ttu-id="5df71-201">A vstupní datová sada objektu blob (InputDataset) určuje kontejner a složku obsahující vstupní data.</span><span class="sxs-lookup"><span data-stu-id="5df71-201">And, the input blob dataset (InputDataset) specifies the container and the folder that contains the input data.</span></span>  

    <span data-ttu-id="5df71-202">Podobně také propojená služba Azure SQL Database určuje připojovací řetězec, který služba Data Factory používá za běhu, aby se připojila k vašemu účtu Azure SQL database.</span><span class="sxs-lookup"><span data-stu-id="5df71-202">Similarly, the Azure SQL Database linked service specifies the connection string that Data Factory service uses at run time to connect to your Azure SQL database.</span></span> <span data-ttu-id="5df71-203">A výstupní datová sada tabulky SQL (OutputDataset) určuje tabulku v databázi, do které se kopírují data z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="5df71-203">And, the output SQL table dataset (OututDataset) specifies the table in the database to which the data from the blob storage is copied.</span></span>

    <span data-ttu-id="5df71-204">V tomto kroku vytvoříte datovou sadu s názvem InputDataset, která odkazuje na soubor blob (emp.txt) v kořenové složce kontejneru objektů blob (adftutorial), který se nachází ve službě Azure Storage reprezentované propojenou službou AzureStorageLinkedService.</span><span class="sxs-lookup"><span data-stu-id="5df71-204">In this step, you create a dataset named InputDataset that points to a blob file (emp.txt) in the root folder of a blob container (adftutorial) in the Azure Storage represented by the AzureStorageLinkedService linked service.</span></span> <span data-ttu-id="5df71-205">Pokud neurčíte hodnotu fileName (nebo ji přeskočíte), data ze všech objektů blob ve vstupní složce se zkopírují do cíle.</span><span class="sxs-lookup"><span data-stu-id="5df71-205">If you don't specify a value for the fileName (or skip it), data from all blobs in the input folder are copied to the destination.</span></span> <span data-ttu-id="5df71-206">V tomto kurzu určíte hodnotu fileName.</span><span class="sxs-lookup"><span data-stu-id="5df71-206">In this tutorial, you specify a value for the fileName.</span></span>    

    <span data-ttu-id="5df71-207">V tomto kroku vytvoříte výstupní datovou sadu s názvem **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="5df71-207">In this step, you create an output dataset named **OutputDataset**.</span></span> <span data-ttu-id="5df71-208">Tato datová sada odkazuje na tabulku SQL ve službě Azure SQL Database, kterou reprezentuje **AzureSqlLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="5df71-208">This dataset points to a SQL table in the Azure SQL database represented by **AzureSqlLinkedService**.</span></span>
11. <span data-ttu-id="5df71-209">Do metody **Main** přidejte následující kód, který **vytvoří a aktivuje kanál**.</span><span class="sxs-lookup"><span data-stu-id="5df71-209">Add the following code that **creates and activates a pipeline** to the **Main** method.</span></span> <span data-ttu-id="5df71-210">V tomto kroku vytvoříte kanál s **aktivitou kopírování**, která používá **InputDataset** jako vstup a **OutputDataset** jako výstup.</span><span class="sxs-lookup"><span data-stu-id="5df71-210">In this step, you create a pipeline with a **copy activity** that uses **InputDataset** as an input and **OutputDataset** as an output.</span></span>

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

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
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

    <span data-ttu-id="5df71-211">Je třeba počítat s následujícím:</span><span class="sxs-lookup"><span data-stu-id="5df71-211">Note the following points:</span></span>
   
    - <span data-ttu-id="5df71-212">V části aktivit je jenom jedna aktivita, jejíž vlastnost **type** je nastavená na **Copy**.</span><span class="sxs-lookup"><span data-stu-id="5df71-212">In the activities section, there is only one activity whose **type** is set to **Copy**.</span></span> <span data-ttu-id="5df71-213">Další informace o aktivitě kopírování najdete v tématu [Aktivity pohybu dat](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="5df71-213">For more information about the copy activity, see [data movement activities](data-factory-data-movement-activities.md).</span></span> <span data-ttu-id="5df71-214">V řešeních služby Data Factory můžete také použít [aktivity transformace dat](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="5df71-214">In Data Factory solutions, you can also use [data transformation activities](data-factory-data-transformation-activities.md).</span></span>
    - <span data-ttu-id="5df71-215">Vstup aktivity je nastavený na **InputDataset** a výstup aktivity je nastavený na **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="5df71-215">Input for the activity is set to **InputDataset** and output for the activity is set to **OutputDataset**.</span></span> 
    - <span data-ttu-id="5df71-216">V části **typeProperties** je jako typ zdroje určen **BlobSource** a jako typ jímky **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="5df71-216">In the **typeProperties** section, **BlobSource** is specified as the source type and **SqlSink** is specified as the sink type.</span></span> <span data-ttu-id="5df71-217">Úplný seznam úložišť dat podporovaných aktivitou kopírování jako zdroje a jímky najdete v tématu [podporovaných úložištích dat](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="5df71-217">For a complete list of data stores supported by the copy activity as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="5df71-218">Kliknutím na odkaz v tabulce se dozvíte, jak použít konkrétní podporovaná úložiště dat jako zdroj/jímku.</span><span class="sxs-lookup"><span data-stu-id="5df71-218">To learn how to use a specific supported data store as a source/sink, click the link in the table.</span></span>  
   
    <span data-ttu-id="5df71-219">Výstupní datové sady v současné době řídí plán.</span><span class="sxs-lookup"><span data-stu-id="5df71-219">Currently, output dataset is what drives the schedule.</span></span> <span data-ttu-id="5df71-220">V tomto kurzu je výstupní datová sada nakonfigurovaná tak, aby vytvářela řez jednou za hodinu.</span><span class="sxs-lookup"><span data-stu-id="5df71-220">In this tutorial, output dataset is configured to produce a slice once an hour.</span></span> <span data-ttu-id="5df71-221">Kanál má čas spuštění a čas ukončení nastavený jeden den od sebe, což je 24 hodin.</span><span class="sxs-lookup"><span data-stu-id="5df71-221">The pipeline has a start time and end time that are one day apart, which is 24 hours.</span></span> <span data-ttu-id="5df71-222">Proto kanál vytvoří 24 řezů výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5df71-222">Therefore, 24 slices of output dataset are produced by the pipeline.</span></span>
12. <span data-ttu-id="5df71-223">Do metody **Main** přidejte následující kód pro získání stavu datového řezu výstupní datové sady.</span><span class="sxs-lookup"><span data-stu-id="5df71-223">Add the following code to the **Main** method to get the status of a data slice of the output dataset.</span></span> <span data-ttu-id="5df71-224">V této ukázce se očekává jenom jeden řez.</span><span class="sxs-lookup"><span data-stu-id="5df71-224">There is only slice expected in this sample.</span></span>

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

13. <span data-ttu-id="5df71-225">Do metody **Main** přidejte následující kód pro získání běhových dat pro datový řez.</span><span class="sxs-lookup"><span data-stu-id="5df71-225">Add the following code to get run details for a data slice to the **Main** method.</span></span>

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

    Console.WriteLine("\nPress any key to exit.");
    Console.ReadKey();
    ```

14. <span data-ttu-id="5df71-226">Do třídy **Program** přidejte následující pomocnou metodu, kterou používá metoda **Main**.</span><span class="sxs-lookup"><span data-stu-id="5df71-226">Add the following helper method used by the **Main** method to the **Program** class.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5df71-227">Když následující kód kopírujete a vkládáte, ujistěte se, že je tento zkopírovaný kód na stejné úrovní jako metoda Main.</span><span class="sxs-lookup"><span data-stu-id="5df71-227">When you copy and paste the following code, make sure that the copied code is at the same level as the Main method.</span></span>

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

15. <span data-ttu-id="5df71-228">V Průzkumníku řešení rozbalte projekt (DataFactoryAPITestApp), klikněte pravým tlačítkem na **Odkazy** a potom klikněte na **Přidat odkaz**.</span><span class="sxs-lookup"><span data-stu-id="5df71-228">In the Solution Explorer, expand the project (DataFactoryAPITestApp), right-click **References**, and click **Add Reference**.</span></span> <span data-ttu-id="5df71-229">Zaškrtněte políčko pro sestavení **System.Configuration**.</span><span class="sxs-lookup"><span data-stu-id="5df71-229">Select check box for **System.Configuration** assembly.</span></span> <span data-ttu-id="5df71-230">Pak klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="5df71-230">and click **OK**.</span></span>
16. <span data-ttu-id="5df71-231">Sestavte konzolovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="5df71-231">Build the console application.</span></span> <span data-ttu-id="5df71-232">Klikněte v nabídce na **Sestavit** a potom klikněte na **Sestavit řešení**.</span><span class="sxs-lookup"><span data-stu-id="5df71-232">Click **Build** on the menu and click **Build Solution**.</span></span>
17. <span data-ttu-id="5df71-233">Potvrďte, že kontejner **adftutorial** v Azure Blob Storage obsahuje alespoň jeden soubor.</span><span class="sxs-lookup"><span data-stu-id="5df71-233">Confirm that there is at least one file in the **adftutorial** container in your Azure blob storage.</span></span> <span data-ttu-id="5df71-234">Pokud ne, vytvořte v Poznámkovém bloku soubor **Emp.txt** s následujícím obsahem a nahrajte ho do kontejneru adftutorial.</span><span class="sxs-lookup"><span data-stu-id="5df71-234">If not, create **Emp.txt** file in Notepad with the following content and upload it to the adftutorial container.</span></span>

    ```
    John, Doe
    Jane, Doe
    ```
18. <span data-ttu-id="5df71-235">Ukázku spusťte kliknutím na **Ladit** -> **Spustit ladění** v nabídce.</span><span class="sxs-lookup"><span data-stu-id="5df71-235">Run the sample by clicking **Debug** -> **Start Debugging** on the menu.</span></span> <span data-ttu-id="5df71-236">Když se zobrazí **Získávání běhových podrobností o datovém řezu**, počkejte několik minut a stiskněte **ENTER**.</span><span class="sxs-lookup"><span data-stu-id="5df71-236">When you see the **Getting run details of a data slice**, wait for a few minutes, and press **ENTER**.</span></span>
19. <span data-ttu-id="5df71-237">Pomocí webu Azure Portal ověřte, že je objekt pro vytváření dat **APITutorialFactory** vytvořený s těmito artefakty:</span><span class="sxs-lookup"><span data-stu-id="5df71-237">Use the Azure portal to verify that the data factory **APITutorialFactory** is created with the following artifacts:</span></span>
   * <span data-ttu-id="5df71-238">Propojená služba: **LinkedService_AzureStorage**</span><span class="sxs-lookup"><span data-stu-id="5df71-238">Linked service: **LinkedService_AzureStorage**</span></span>
   * <span data-ttu-id="5df71-239">Datová sada: **InputDataset** a **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="5df71-239">Dataset: **InputDataset** and **OutputDataset**.</span></span>
   * <span data-ttu-id="5df71-240">Kanál: **PipelineBlobSample**</span><span class="sxs-lookup"><span data-stu-id="5df71-240">Pipeline: **PipelineBlobSample**</span></span>
20. <span data-ttu-id="5df71-241">Ověřte, že se v tabulce **emp** vytvořily dva záznamy zaměstnanců v zadané databázi Azure SQL.</span><span class="sxs-lookup"><span data-stu-id="5df71-241">Verify that the two employee records are created in the **emp** table in the specified Azure SQL database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5df71-242">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5df71-242">Next steps</span></span>
<span data-ttu-id="5df71-243">Úplnou dokumentaci k rozhraní .NET API pro datovou továrnu najdete v [referencích k rozhraní .NET API služby Data Factory](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).</span><span class="sxs-lookup"><span data-stu-id="5df71-243">For complete documentation on .NET API for Data Factory, see [Data Factory .NET API Reference](/dotnet/api/index?view=azuremgmtdatafactories-4.12.1).</span></span>

<span data-ttu-id="5df71-244">V tomto kurzu jste v operaci kopírování použili úložiště objektů blob jako zdrojové úložiště dat a databázi Azure SQL jako cílové úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="5df71-244">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="5df71-245">Následující tabulka obsahuje seznam úložišť dat podporovaných jako zdroje a cíle aktivitou kopírování:</span><span class="sxs-lookup"><span data-stu-id="5df71-245">The following table provides a list of data stores supported as sources and destinations by the copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="5df71-246">Kliknutím na odkaz úložiště dat v tabulce získáte další informace o kopírování dat do nebo z úložiště dat.</span><span class="sxs-lookup"><span data-stu-id="5df71-246">To learn about how to copy data to/from a data store, click the link for the data store in the table.</span></span>

