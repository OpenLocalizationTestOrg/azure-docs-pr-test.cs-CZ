---
title: "Vytváření clusterů systému Hadoop pomocí šablon - Azure HDInsight | Microsoft Docs"
description: "Naučte se vytvářet clustery pro HDInsight pomocí šablony Resource Manageru"
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00a80dea-011f-44f0-92a4-25d09db9d996
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: jgao
ms.openlocfilehash: b2cdc954530daea2a641599c946ce3787149e762
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="create-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a><span data-ttu-id="52820-103">Vytvoření clusterů systému Hadoop v HDInsight pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="52820-103">Create Hadoop clusters in HDInsight by using Resource Manager templates</span></span>
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="52820-104">V tomto článku se dozvíte několik způsobů, jak vytvořit clustery se Azure HDInsight pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="52820-104">In this article, you learn several ways to create Azure HDInsight clusters with Azure Resource Manager templates.</span></span> <span data-ttu-id="52820-105">Další informace najdete v tématu [nasazení aplikace pomocí šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="52820-105">For more information, see [Deploy an application with Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span> <span data-ttu-id="52820-106">Další informace o dalších funkcí a nástrojů pro vytváření clusteru, klikněte na tlačítko volič karty v horní této stránce nebo v tématu [metody vytváření clusterů](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).</span><span class="sxs-lookup"><span data-stu-id="52820-106">To learn about other cluster creation tools and features, click the tab selector on the top of this page or see [Cluster creation methods](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="52820-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="52820-107">Prerequisites</span></span>
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="52820-108">Podle pokynů v tomto článku, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="52820-108">To follow the instructions in this article, you'll need:</span></span>

* <span data-ttu-id="52820-109">[Předplatné](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="52820-109">An [Azure subscription](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="52820-110">Prostředí Azure PowerShell nebo Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="52820-110">Azure PowerShell and/or Azure CLI.</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="resource-manager-templates"></a><span data-ttu-id="52820-111">Šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="52820-111">Resource Manager templates</span></span>
<span data-ttu-id="52820-112">Šablonu Resource Manager umožňuje snadné vytváření následující pro vaši aplikaci v rámci jediné koordinované operace:</span><span class="sxs-lookup"><span data-stu-id="52820-112">A Resource Manager template makes it easy to create the following for your application in a single, coordinated operation:</span></span>
* <span data-ttu-id="52820-113">Clustery prostředí HDInsight a jejich závislé prostředky (například výchozí účet úložiště)</span><span class="sxs-lookup"><span data-stu-id="52820-113">HDInsight clusters and their dependent resources (such as the default storage account)</span></span>
* <span data-ttu-id="52820-114">Další prostředky (například Azure SQL Database k použití Apache Sqoop)</span><span class="sxs-lookup"><span data-stu-id="52820-114">Other resources (such as Azure SQL Database to use Apache Sqoop)</span></span>

<span data-ttu-id="52820-115">V šabloně definujete prostředky, které jsou potřebné pro aplikaci.</span><span class="sxs-lookup"><span data-stu-id="52820-115">In the template, you define the resources that are needed for the application.</span></span> <span data-ttu-id="52820-116">Můžete určit taky parametry nasazení pro vstupní hodnoty pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="52820-116">You also specify deployment parameters to input values for different environments.</span></span> <span data-ttu-id="52820-117">Šablona se skládá z JSON a výrazy, které můžete použít k vytvoření hodnot pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="52820-117">The template consists of JSON and expressions that you use to construct values for your deployment.</span></span>

<span data-ttu-id="52820-118">Můžete najít ukázky šablony HDInsight v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/?term=hdinsight).</span><span class="sxs-lookup"><span data-stu-id="52820-118">You can find HDInsight template samples at [Azure Quickstart Templates](https://azure.microsoft.com/resources/templates/?term=hdinsight).</span></span> <span data-ttu-id="52820-119">Použít napříč platformami [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) s [Resource Manager rozšíření](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) nebo textovém editoru a uložit šablonu do souboru na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="52820-119">Use cross-platform [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) with the [Resource Manager extension](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) or a text editor to save the template into a file on your workstation.</span></span> <span data-ttu-id="52820-120">Zjistíte, jak volat šablony pomocí různých metod.</span><span class="sxs-lookup"><span data-stu-id="52820-120">You learn how to call the template by using different methods.</span></span>

<span data-ttu-id="52820-121">Další informace o šablonách Resource Manager najdete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="52820-121">For more information about Resource Manager templates, see the following articles:</span></span>

* [<span data-ttu-id="52820-122">Vytváření šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="52820-122">Author Azure Resource Manager templates</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)
* [<span data-ttu-id="52820-123">Nasazení aplikace pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="52820-123">Deploy an application with Azure Resource Manager templates</span></span>](../azure-resource-manager/resource-group-template-deploy.md)

## <a name="generate-templates"></a><span data-ttu-id="52820-124">Generování šablon</span><span class="sxs-lookup"><span data-stu-id="52820-124">Generate templates</span></span>

<span data-ttu-id="52820-125">Pomocí portálu Azure, můžete konfigurovat vlastnosti clusteru a potom uložte šablonu ještě před nasazením.</span><span class="sxs-lookup"><span data-stu-id="52820-125">By using the Azure portal, you can configure all the properties of a cluster and then save the template before deploying it.</span></span> <span data-ttu-id="52820-126">Pak můžete znovu použít šablonu.</span><span class="sxs-lookup"><span data-stu-id="52820-126">You can then reuse the template.</span></span>

<span data-ttu-id="52820-127">**Ke generování šablony pomocí portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="52820-127">**To generate a template by using the Azure portal**</span></span>

1. <span data-ttu-id="52820-128">Přihlaste se k webu [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="52820-128">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="52820-129">Klikněte na tlačítko **nový** v levé nabídce klikněte na tlačítko **Intelligence + analýzy**a potom klikněte na **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="52820-129">Click **New** on the left menu, click **Intelligence + analytics**, and then click **HDInsight**.</span></span>
3. <span data-ttu-id="52820-130">Postupujte podle pokynů a zadejte vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="52820-130">Follow the instructions to enter properties.</span></span> <span data-ttu-id="52820-131">Můžete použít buď **rychle vytvořit** nebo **vlastní** možnost.</span><span class="sxs-lookup"><span data-stu-id="52820-131">You can use either the **Quick create** or the **Custom** option.</span></span>
4. <span data-ttu-id="52820-132">Na **Souhrn** , klikněte na **stáhnout šablonu a parametry**:</span><span class="sxs-lookup"><span data-stu-id="52820-132">On the **Summary** tab, click **Download template and parameters**:</span></span>

    ![Vytvoření stažení šablony správce prostředků clusteru HDInsight Hadoop](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download.png)

    <span data-ttu-id="52820-134">Zobrazí seznam soubor šablony, soubor parametrů a ukázky kódu, které jsou používány k nasazení šablony:</span><span class="sxs-lookup"><span data-stu-id="52820-134">You see a list of the template file, parameters file, and code samples used to deploy the template:</span></span>

    ![HDInsight Hadoop vytvoření clusteru možnosti stahování šablony Resource Manageru](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download-options.png)

    <span data-ttu-id="52820-136">Tady můžete šablonu stáhnout, uložit do knihovny šablony nebo nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="52820-136">From here, you can download the template, save it to your template library, or deploy the template.</span></span>

    <span data-ttu-id="52820-137">Chcete-li získat přístup k šabloně v knihovně, klikněte na tlačítko **další služby** z levé nabídce a pak klikněte na tlačítko **šablony** (v části **jiných** kategorie).</span><span class="sxs-lookup"><span data-stu-id="52820-137">To access a template in your library, click **More services** from the left menu, and then click **Templates** (under the **Other** category).</span></span>

    > [!Note]
    > <span data-ttu-id="52820-138">Soubor šablony a parametry se musí použít společně.</span><span class="sxs-lookup"><span data-stu-id="52820-138">The template and parameters file must be used together.</span></span> <span data-ttu-id="52820-139">Jinak můžete získat neočekávané výsledky.</span><span class="sxs-lookup"><span data-stu-id="52820-139">Otherwise, you might get unexpected results.</span></span> <span data-ttu-id="52820-140">Například výchozí **clusterKind** hodnota vlastnosti je vždy **hadoop**, bez ohledu na tom, co jste zadejte před stažením šablony.</span><span class="sxs-lookup"><span data-stu-id="52820-140">For example, the default **clusterKind** property value is always **hadoop**, despite what you specify before you download the template.</span></span>



## <a name="deploy-with-powershell"></a><span data-ttu-id="52820-141">Nasazení s využitím PowerShellu</span><span class="sxs-lookup"><span data-stu-id="52820-141">Deploy with PowerShell</span></span>

<span data-ttu-id="52820-142">Tento postup vytvoří Hadoop cluster v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="52820-142">This procedure creates a Hadoop cluster in HDInsight.</span></span>

1. <span data-ttu-id="52820-143">Uložení souboru JSON v [příloha](#appx-a-arm-template) do pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="52820-143">Save the JSON file in the [Appendix](#appx-a-arm-template) to your workstation.</span></span> <span data-ttu-id="52820-144">Ve skriptu prostředí PowerShell, je název souboru `C:\HDITutorials-ARM\hdinsight-arm-template.json`.</span><span class="sxs-lookup"><span data-stu-id="52820-144">In the PowerShell script, the file name is `C:\HDITutorials-ARM\hdinsight-arm-template.json`.</span></span>
2. <span data-ttu-id="52820-145">V případě potřeby nastavte parametry a proměnné.</span><span class="sxs-lookup"><span data-stu-id="52820-145">Set the parameters and variables if needed.</span></span>
3. <span data-ttu-id="52820-146">Spustíte šablonu pomocí následujícího skriptu prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="52820-146">Run the template by using the following PowerShell script:</span></span>

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>"
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and variables
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage account
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

    <span data-ttu-id="52820-147">Skript prostředí PowerShell lze konfigurovat pouze název clusteru.</span><span class="sxs-lookup"><span data-stu-id="52820-147">The PowerShell script configures only the cluster name.</span></span> <span data-ttu-id="52820-148">Název účtu úložiště je pevně zakódovaná v šabloně.</span><span class="sxs-lookup"><span data-stu-id="52820-148">The storage account name is hard-coded in the template.</span></span> <span data-ttu-id="52820-149">Zobrazí se výzva k zadání hesla uživatele clusteru.</span><span class="sxs-lookup"><span data-stu-id="52820-149">You are prompted to enter the cluster user password.</span></span> <span data-ttu-id="52820-150">(Výchozí uživatelské jméno **správce**.) Také budete vyzváni k zadání hesla uživatele SSH.</span><span class="sxs-lookup"><span data-stu-id="52820-150">(The default username is **admin**.) You are also prompted to enter the SSH user password.</span></span> <span data-ttu-id="52820-151">(Výchozí uživatelské jméno SSH **sshuser**.)</span><span class="sxs-lookup"><span data-stu-id="52820-151">(The default SSH username is **sshuser**.)</span></span>  

<span data-ttu-id="52820-152">Další informace najdete v tématu [nasadit v prostředí PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span><span class="sxs-lookup"><span data-stu-id="52820-152">For more information, see  [Deploy with PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span></span>

## <a name="deploy-with-cli"></a><span data-ttu-id="52820-153">Nasazení pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="52820-153">Deploy with CLI</span></span>
<span data-ttu-id="52820-154">Následující příklad používá rozhraní příkazového řádku Azure (CLI).</span><span class="sxs-lookup"><span data-stu-id="52820-154">The following sample uses Azure command-line interface (CLI).</span></span> <span data-ttu-id="52820-155">Vytvoří cluster a jeho účet závislého úložiště a kontejneru voláním šablony Resource Manageru:</span><span class="sxs-lookup"><span data-stu-id="52820-155">It creates a cluster and its dependent storage account and container by calling a Resource Manager template:</span></span>

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"

<span data-ttu-id="52820-156">Zobrazí se výzva k zadání:</span><span class="sxs-lookup"><span data-stu-id="52820-156">You are prompted to enter:</span></span>
* <span data-ttu-id="52820-157">Název clusteru.</span><span class="sxs-lookup"><span data-stu-id="52820-157">The cluster name.</span></span>
* <span data-ttu-id="52820-158">Heslo uživatele clusteru.</span><span class="sxs-lookup"><span data-stu-id="52820-158">The cluster user password.</span></span> <span data-ttu-id="52820-159">(Výchozí uživatelské jméno **správce**.)</span><span class="sxs-lookup"><span data-stu-id="52820-159">(The default username is **admin**.)</span></span>
* <span data-ttu-id="52820-160">Heslo uživatele SSH.</span><span class="sxs-lookup"><span data-stu-id="52820-160">The SSH user password.</span></span> <span data-ttu-id="52820-161">(Výchozí uživatelské jméno SSH **sshuser**.)</span><span class="sxs-lookup"><span data-stu-id="52820-161">(The default SSH username is **sshuser**.)</span></span>

<span data-ttu-id="52820-162">Následující kód obsahuje vložené parametry:</span><span class="sxs-lookup"><span data-stu-id="52820-162">The following code provides inline parameters:</span></span>

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

## <a name="deploy-with-the-rest-api"></a><span data-ttu-id="52820-163">Nasazení pomocí rozhraní REST API</span><span class="sxs-lookup"><span data-stu-id="52820-163">Deploy with the REST API</span></span>
<span data-ttu-id="52820-164">V tématu [nasazení pomocí rozhraní REST API](../azure-resource-manager/resource-group-template-deploy-rest.md).</span><span class="sxs-lookup"><span data-stu-id="52820-164">See [Deploy with the REST API](../azure-resource-manager/resource-group-template-deploy-rest.md).</span></span>

## <a name="deploy-with-visual-studio"></a><span data-ttu-id="52820-165">Nasazení s využitím sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="52820-165">Deploy with Visual Studio</span></span>
 <span data-ttu-id="52820-166">Pomocí sady Visual Studio vytvořte projekt skupiny prostředků a nasaďte ho do Azure v uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="52820-166">Use Visual Studio to create a resource group project and deploy it to Azure through the user interface.</span></span> <span data-ttu-id="52820-167">Vyberete typ zdroje, které chcete zahrnout do projektu.</span><span class="sxs-lookup"><span data-stu-id="52820-167">You select the type of resources to include in your project.</span></span> <span data-ttu-id="52820-168">Tyto prostředky se automaticky přidají do šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="52820-168">Those resources are automatically added to the Resource Manager template.</span></span> <span data-ttu-id="52820-169">Projekt také obsahuje skript prostředí PowerShell k nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="52820-169">The project also provides a PowerShell script to deploy the template.</span></span>

<span data-ttu-id="52820-170">Úvod do skupiny prostředků pomocí sady Visual Studio, najdete v části [vytvoření a nasazení skupin prostředků Azure pomocí sady Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="52820-170">For an introduction to using Visual Studio with resource groups, see [Creating and deploying Azure resource groups through Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="52820-171">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="52820-171">Troubleshoot</span></span>

<span data-ttu-id="52820-172">Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="52820-172">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="52820-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="52820-173">Next steps</span></span>
<span data-ttu-id="52820-174">V tomto článku jste se naučili několik způsobů, jak vytvořit cluster služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="52820-174">In this article, you have learned several ways to create an HDInsight cluster.</span></span> <span data-ttu-id="52820-175">Další informace naleznete v následujících článcích:</span><span class="sxs-lookup"><span data-stu-id="52820-175">To learn more, see the following articles:</span></span>

* <span data-ttu-id="52820-176">Příklad nasazení prostředků prostřednictvím klientské knihovny .NET, naleznete v části [nasadit prostředky pomocí knihovny .NET a šablonu](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="52820-176">For an example of deploying resources through the .NET client library, see [Deploy resources by using .NET libraries and a template](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="52820-177">Podrobný příklad nasazení aplikace naleznete v tématu [zřídit a nasadit mikroslužeb předvídatelné v Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).</span><span class="sxs-lookup"><span data-stu-id="52820-177">For an in-depth example of deploying an application, see [Provision and deploy microservices predictably in Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).</span></span>
* <span data-ttu-id="52820-178">Pokyny pro nasazení řešení do různých prostředí najdete v článku věnovaném [testovacím a vývojovým prostředím v Microsoft Azure](../solution-dev-test-environments.md).</span><span class="sxs-lookup"><span data-stu-id="52820-178">For guidance on deploying your solution to different environments, see [Development and test environments in Microsoft Azure](../solution-dev-test-environments.md).</span></span>
* <span data-ttu-id="52820-179">Další informace o části šablony Azure Resource Manageru najdete v tématu [vytváření šablon](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="52820-179">To learn about the sections of the Azure Resource Manager template, see [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="52820-180">Seznam funkcí v šablonu Azure Resource Manager můžete použít, najdete v části [funkce šablon](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="52820-180">For a list of the functions you can use in an Azure Resource Manager template, see [Template functions](../azure-resource-manager/resource-group-template-functions.md).</span></span>

## <a name="appendix-resource-manager-template-to-create-a-hadoop-cluster"></a><span data-ttu-id="52820-181">Dodatek: Šablony Resource Manageru k vytvoření clusteru Hadoop</span><span class="sxs-lookup"><span data-stu-id="52820-181">Appendix: Resource Manager template to create a Hadoop cluster</span></span>
<span data-ttu-id="52820-182">Následující šablony Azure Resource Manager vytvoří cluster systémem Linux Hadoop se účet závislého úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="52820-182">The following Azure Resource Manager template creates a Linux-based Hadoop cluster with the dependent Azure storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="52820-183">Tato ukázka obsahuje informace o konfiguraci pro metaúložiště Hive a metaúložiště Oozie.</span><span class="sxs-lookup"><span data-stu-id="52820-183">This sample includes configuration information for Hive metastore and Oozie metastore.</span></span> <span data-ttu-id="52820-184">Odebrat oddíl nebo nakonfigurujte části před použitím šablony.</span><span class="sxs-lookup"><span data-stu-id="52820-184">Remove the section or configure the section before using the template.</span></span>
>
>

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "sshUserName": {
        "type": "string",
        "defaultValue": "sshuser",
        "metadata": {
            "description": "These credentials can be used to remotely access the cluster."
        }
        },
        "sshPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "location": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Australia East",
            "Australia Southeast"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterType": {
        "type": "string",
        "defaultValue": "hadoop",
        "allowedValues": [
            "hadoop",
            "hbase",
            "storm",
            "spark"
        ],
        "metadata": {
            "description": "The type of the HDInsight cluster to create."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
    "variables": {
        "defaultApiVersion": "2015-05-01-preview",
        "clusterApiVersion": "2015-03-01-preview",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
        {
        "name": "[parameters('clusterName')]",
        "type": "Microsoft.HDInsight/clusters",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('clusterApiVersion')]",
        "dependsOn": [ "[concat('Microsoft.Storage/storageAccounts/',variables('clusterStorageAccountName'))]" ],
        "tags": {

        },
        "properties": {
            "clusterVersion": "3.4",
            "osType": "Linux",
            "tier": "standard",
            "clusterDefinition": {
            "kind": "[parameters('clusterType')]",
            "configurations": {
                "gateway": {
                "restAuthCredential.isEnabled": true,
                "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                },
                "hive-site": {
                    "javax.jdo.option.ConnectionDriverName": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "javax.jdo.option.ConnectionURL": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "javax.jdo.option.ConnectionUserName": "johndole",
                    "javax.jdo.option.ConnectionPassword": "myPassword$"
                },
                "hive-env": {
                    "hive_database": "Existing MSSQL Server database with SQL authentication",
                    "hive_database_name": "myhive20160901",
                    "hive_database_type": "mssql",
                    "hive_existing_mssql_server_database": "myhive20160901",
                    "hive_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "hive_hostname": "myadla0901dbserver.database.windows.net"
                },
                "oozie-site": {
                    "oozie.service.JPAService.jdbc.driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "oozie.service.JPAService.jdbc.url": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "oozie.service.JPAService.jdbc.username": "johndole",
                    "oozie.service.JPAService.jdbc.password": "myPassword$",
                    "oozie.db.schema.name": "oozie"
                },
                "oozie-env": {
                    "oozie_database": "Existing MSSQL Server database with SQL authentication",
                    "oozie_database_name": "myhive20160901",
                    "oozie_database_type": "mssql",
                    "oozie_existing_mssql_server_database": "myhive20160901",
                    "oozie_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "oozie_hostname": "myadla0901dbserver.database.windows.net"
                }            
            }
            },
            "storageProfile": {
            "storageaccounts": [
                {
                "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                "isDefault": true,
                "container": "[parameters('clusterName')]",
                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                }
            ]
            },
            "computeProfile": {
            "roles": [
                {
                "name": "headnode",
                "targetInstanceCount": "2",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                },
                {
                "name": "workernode",
                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                }
            ]
            }
        }
        }
    ],
    "outputs": {
        "cluster": {
        "type": "object",
        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
        }
    }
    }

## <a name="appendix-resource-manager-template-to-create-a-spark-cluster"></a><span data-ttu-id="52820-185">Dodatek: Šablony Resource Manageru k vytvoření clusteru Spark</span><span class="sxs-lookup"><span data-stu-id="52820-185">Appendix: Resource Manager template to create a Spark cluster</span></span>

<span data-ttu-id="52820-186">Tato část obsahuje šablonu Resource Manager, který můžete použít k vytvoření clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="52820-186">This section provides a Resource Manager template that you can use to create an HDInsight Spark cluster.</span></span> <span data-ttu-id="52820-187">Tato šablona zahrnuje konfigurace pro `spark-defaults` a `spark-thrift-sparkconf` (pro clustery Spark 1.6) a `spark2-defaults` a `spark2-thrift-sparkconf` (pro clustery Spark 2).</span><span class="sxs-lookup"><span data-stu-id="52820-187">This template includes configurations for `spark-defaults` and `spark-thrift-sparkconf` (for Spark 1.6 clusters) and `spark2-defaults` and `spark2-thrift-sparkconf` (for Spark 2 clusters).</span></span> <span data-ttu-id="52820-188">Kromě toho HDInsight vypočítá a nastaví konfigurace, jako `spark.executor.instances`, `spark.executor.memory`, a `spark.executor.cores` na základě velikosti clusteru.</span><span class="sxs-lookup"><span data-stu-id="52820-188">In addition to this, HDInsight calculates and sets configurations such as `spark.executor.instances`, `spark.executor.memory`, and `spark.executor.cores` based on the cluster size.</span></span> 

<span data-ttu-id="52820-189">Pokud nastavíte všechny jeden parametr v části v rámci samotné šablony, HDInsight nepodporuje výpočtu a nastavit další parametry do stejné části.</span><span class="sxs-lookup"><span data-stu-id="52820-189">If you set any one parameter in a section as part of the template itself, HDInsight does not calculate and set the other parameters of the same section.</span></span> <span data-ttu-id="52820-190">Například parametr `spark.executor.instances` probíhá `spark-defaults` konfigurace.</span><span class="sxs-lookup"><span data-stu-id="52820-190">For example, parameter `spark.executor.instances` is in the  `spark-defaults` configuration.</span></span> <span data-ttu-id="52820-191">Pokud nastavíte parametr jiné (například `spark.yarn.exector.memoryOverhead`) v `spark-defaults` konfigurace, HDInsight nepodporuje výpočtu a nastavit `spark.executor.instances` také parametr.</span><span class="sxs-lookup"><span data-stu-id="52820-191">If you set another parameter (for example, `spark.yarn.exector.memoryOverhead`) in the `spark-defaults` configuration, HDInsight does not calculate and set the `spark.executor.instances` parameter as well.</span></span>

    {
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "parameters": {
        "clusterName": {
            "type": "string",
            "metadata": {
                "description": "The name of the HDInsight cluster to create."
            }
        },
        "clusterLoginUserName": {
            "type": "string",
            "defaultValue": "admin",
            "metadata": {
                "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
            }
        },
        "clusterLoginPassword": {
            "type": "securestring",
            "metadata": {
                "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus",
            "metadata": {
                "description": "The location where all azure resources will be deployed."
            }
        },
        "clusterVersion": {
            "type": "string",
            "defaultValue": "3.5",
            "metadata": {
                "description": "HDInsight cluster version."
            }
        },
        "clusterWorkerNodeCount": {
            "type": "int",
            "defaultValue": 4,
            "metadata": {
                "description": "The number of nodes in the HDInsight cluster."
            }
        },
        "clusterKind": {
            "type": "string",
            "defaultValue": "SPARK",
            "metadata": {
                "description": "The type of the HDInsight cluster to create."
            }
        },
        "sshUserName": {
            "type": "string",
            "defaultValue": "sshuser",
            "metadata": {
                "description": "These credentials can be used to remotely access the cluster."
            }
        },
        "sshPassword": {
            "type": "securestring",
            "metadata": {
                "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        }
    },
    "variables": {
        "defaultApiVersion": "2017-06-01",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
    {
            "apiVersion": "2015-03-01-preview",
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "dependsOn": [],
            "properties": {
                "clusterVersion": "[parameters('clusterVersion')]",
                "osType": "Linux",
                "tier": "standard",
                "clusterDefinition": {
                    "kind": "[parameters('clusterKind')]",
                    "configurations": {
                        "gateway": {
                            "restAuthCredential.isEnabled": true,
                            "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                            "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                        },
                        "spark-defaults": {
                            "spark.executor.cores": "2"
                        },
                        "spark-thrift-sparkconf": {
                            "spark.yarn.executor.memoryOverhead": "896"
                        }
                    }
                },
                "storageProfile": {
                    "storageaccounts": [
                        {
                            "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                            "isDefault": true,
                            "container": "[parameters('clusterName')]",
                            "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                        }
                    ]
                },
                "computeProfile": {
                    "roles": [
                        {
                            "name": "headnode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 2,
                            "hardwareProfile": {
                                "vmSize": "Standard_D12"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                }
                            },
                            "virtualNetworkProfile": null,
                            "scriptActions": []
                        },
                        {
                            "name": "workernode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 4,
                            "hardwareProfile": {
                                "vmSize": "Standard_D4"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                    }
                                },
                                "virtualNetworkProfile": null,
                                "scriptActions": []
                            }
                        ]
                    }
                }
            }
        ]
    }
