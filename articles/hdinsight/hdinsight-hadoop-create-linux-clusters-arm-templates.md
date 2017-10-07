---
title: "aaaCreate Hadoop clusterů pomocí šablon - Azure HDInsight | Microsoft Docs"
description: "Zjistěte, jak toocreate clusterů pro HDInsight pomocí šablony Resource Manageru"
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
ms.openlocfilehash: 92a6c1d888e401a11537dba34f188245ac17f448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a><span data-ttu-id="f8129-103">Vytvoření clusterů systému Hadoop v HDInsight pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="f8129-103">Create Hadoop clusters in HDInsight by using Resource Manager templates</span></span>
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="f8129-104">V tomto článku se dozvíte několik způsobů toocreate Azure HDInsight clustery s šablon Azure Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="f8129-104">In this article, you learn several ways toocreate Azure HDInsight clusters with Azure Resource Manager templates.</span></span> <span data-ttu-id="f8129-105">Další informace najdete v tématu [nasazení aplikace pomocí šablony Azure Resource Manageru](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="f8129-105">For more information, see [Deploy an application with Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span> <span data-ttu-id="f8129-106">toolearn o jiných nástrojů pro vytvoření clusteru a funkcí, klikněte na tlačítko hello karta selektor na hello horní části této stránky nebo najdete [metody vytváření clusterů](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).</span><span class="sxs-lookup"><span data-stu-id="f8129-106">toolearn about other cluster creation tools and features, click hello tab selector on hello top of this page or see [Cluster creation methods](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8129-107">Požadavky</span><span class="sxs-lookup"><span data-stu-id="f8129-107">Prerequisites</span></span>
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="f8129-108">toofollow hello pokyny v tomto článku, budete potřebovat:</span><span class="sxs-lookup"><span data-stu-id="f8129-108">toofollow hello instructions in this article, you'll need:</span></span>

* <span data-ttu-id="f8129-109">[Předplatné](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="f8129-109">An [Azure subscription](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="f8129-110">Prostředí Azure PowerShell nebo Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="f8129-110">Azure PowerShell and/or Azure CLI.</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="resource-manager-templates"></a><span data-ttu-id="f8129-111">Šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="f8129-111">Resource Manager templates</span></span>
<span data-ttu-id="f8129-112">Šablonu Resource Manager umožňuje snadno toocreate hello následující pro vaši aplikaci v rámci jediné koordinované operace:</span><span class="sxs-lookup"><span data-stu-id="f8129-112">A Resource Manager template makes it easy toocreate hello following for your application in a single, coordinated operation:</span></span>
* <span data-ttu-id="f8129-113">Clustery prostředí HDInsight a jejich závislé prostředky (například hello výchozí účet úložiště)</span><span class="sxs-lookup"><span data-stu-id="f8129-113">HDInsight clusters and their dependent resources (such as hello default storage account)</span></span>
* <span data-ttu-id="f8129-114">Další prostředky (například Azure SQL Database toouse Apache Sqoop)</span><span class="sxs-lookup"><span data-stu-id="f8129-114">Other resources (such as Azure SQL Database toouse Apache Sqoop)</span></span>

<span data-ttu-id="f8129-115">V šabloně hello definujete hello prostředky, které jsou potřebné pro aplikace hello.</span><span class="sxs-lookup"><span data-stu-id="f8129-115">In hello template, you define hello resources that are needed for hello application.</span></span> <span data-ttu-id="f8129-116">Je také zadat hodnoty tooinput parametry nasazení pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="f8129-116">You also specify deployment parameters tooinput values for different environments.</span></span> <span data-ttu-id="f8129-117">Šablona Hello se skládá z JSON a výrazy, že používáte tooconstruct hodnoty pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="f8129-117">hello template consists of JSON and expressions that you use tooconstruct values for your deployment.</span></span>

<span data-ttu-id="f8129-118">Můžete najít ukázky šablony HDInsight v [šablon Azure rychlý Start](https://azure.microsoft.com/resources/templates/?term=hdinsight).</span><span class="sxs-lookup"><span data-stu-id="f8129-118">You can find HDInsight template samples at [Azure Quickstart Templates](https://azure.microsoft.com/resources/templates/?term=hdinsight).</span></span> <span data-ttu-id="f8129-119">Použít napříč platformami [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) s hello [Resource Manager rozšíření](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) nebo textový editor toosave hello šablony do souboru na pracovní stanici.</span><span class="sxs-lookup"><span data-stu-id="f8129-119">Use cross-platform [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) with hello [Resource Manager extension](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) or a text editor toosave hello template into a file on your workstation.</span></span> <span data-ttu-id="f8129-120">Zjistíte, jak toocall hello šablony pomocí různých metod.</span><span class="sxs-lookup"><span data-stu-id="f8129-120">You learn how toocall hello template by using different methods.</span></span>

<span data-ttu-id="f8129-121">Další informace o šablonách Resource Manager najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="f8129-121">For more information about Resource Manager templates, see hello following articles:</span></span>

* [<span data-ttu-id="f8129-122">Vytváření šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="f8129-122">Author Azure Resource Manager templates</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)
* [<span data-ttu-id="f8129-123">Nasazení aplikace pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="f8129-123">Deploy an application with Azure Resource Manager templates</span></span>](../azure-resource-manager/resource-group-template-deploy.md)

## <a name="generate-templates"></a><span data-ttu-id="f8129-124">Generování šablon</span><span class="sxs-lookup"><span data-stu-id="f8129-124">Generate templates</span></span>

<span data-ttu-id="f8129-125">Pomocí hello portálu Azure můžete nakonfigurovat všechny vlastnosti hello clusteru a potom uložte hello šablonu ještě před nasazením.</span><span class="sxs-lookup"><span data-stu-id="f8129-125">By using hello Azure portal, you can configure all hello properties of a cluster and then save hello template before deploying it.</span></span> <span data-ttu-id="f8129-126">Můžete je znovu hello šablony.</span><span class="sxs-lookup"><span data-stu-id="f8129-126">You can then reuse hello template.</span></span>

<span data-ttu-id="f8129-127">**toogenerate a šablony pomocí hello portálu Azure**</span><span class="sxs-lookup"><span data-stu-id="f8129-127">**toogenerate a template by using hello Azure portal**</span></span>

1. <span data-ttu-id="f8129-128">Přihlaste se toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f8129-128">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f8129-129">Klikněte na tlačítko **nový** v levé nabídce hello, klikněte na tlačítko **Intelligence + analýzy**a potom klikněte na **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="f8129-129">Click **New** on hello left menu, click **Intelligence + analytics**, and then click **HDInsight**.</span></span>
3. <span data-ttu-id="f8129-130">Postupujte podle vlastnosti tooenter pokyny hello.</span><span class="sxs-lookup"><span data-stu-id="f8129-130">Follow hello instructions tooenter properties.</span></span> <span data-ttu-id="f8129-131">Můžete použít buď hello **rychle vytvořit** nebo hello **vlastní** možnost.</span><span class="sxs-lookup"><span data-stu-id="f8129-131">You can use either hello **Quick create** or hello **Custom** option.</span></span>
4. <span data-ttu-id="f8129-132">Na hello **Souhrn** , klikněte na **stáhnout šablonu a parametry**:</span><span class="sxs-lookup"><span data-stu-id="f8129-132">On hello **Summary** tab, click **Download template and parameters**:</span></span>

    ![Vytvoření stažení šablony správce prostředků clusteru HDInsight Hadoop](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download.png)

    <span data-ttu-id="f8129-134">Zobrazí seznam hello soubor šablony, soubor parametrů a kódu ukázky používá toodeploy hello šablonu:</span><span class="sxs-lookup"><span data-stu-id="f8129-134">You see a list of hello template file, parameters file, and code samples used toodeploy hello template:</span></span>

    ![HDInsight Hadoop vytvoření clusteru možnosti stahování šablony Resource Manageru](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download-options.png)

    <span data-ttu-id="f8129-136">Tady můžete stáhnout hello šablonu, uložit knihovna šablon tooyour nebo nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="f8129-136">From here, you can download hello template, save it tooyour template library, or deploy hello template.</span></span>

    <span data-ttu-id="f8129-137">tooaccess šablonu v knihovně, klikněte na tlačítko **další služby** z levé nabídce hello a pak klikněte na tlačítko **šablony** (v části hello **jiných** kategorie).</span><span class="sxs-lookup"><span data-stu-id="f8129-137">tooaccess a template in your library, click **More services** from hello left menu, and then click **Templates** (under hello **Other** category).</span></span>

    > [!Note]
    > <span data-ttu-id="f8129-138">soubor šablony a parametry Hello musí použít společně.</span><span class="sxs-lookup"><span data-stu-id="f8129-138">hello template and parameters file must be used together.</span></span> <span data-ttu-id="f8129-139">Jinak můžete získat neočekávané výsledky.</span><span class="sxs-lookup"><span data-stu-id="f8129-139">Otherwise, you might get unexpected results.</span></span> <span data-ttu-id="f8129-140">Například hello výchozí **clusterKind** hodnota vlastnosti je vždy **hadoop**, bez ohledu na tom, co jste zadejte před stažením šablony hello.</span><span class="sxs-lookup"><span data-stu-id="f8129-140">For example, hello default **clusterKind** property value is always **hadoop**, despite what you specify before you download hello template.</span></span>



## <a name="deploy-with-powershell"></a><span data-ttu-id="f8129-141">Nasazení s využitím PowerShellu</span><span class="sxs-lookup"><span data-stu-id="f8129-141">Deploy with PowerShell</span></span>

<span data-ttu-id="f8129-142">Tento postup vytvoří Hadoop cluster v HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f8129-142">This procedure creates a Hadoop cluster in HDInsight.</span></span>

1. <span data-ttu-id="f8129-143">Uložte soubor JSON hello v hello [příloha](#appx-a-arm-template) tooyour pracovní stanice.</span><span class="sxs-lookup"><span data-stu-id="f8129-143">Save hello JSON file in hello [Appendix](#appx-a-arm-template) tooyour workstation.</span></span> <span data-ttu-id="f8129-144">V hello skript prostředí PowerShell, je název souboru hello `C:\HDITutorials-ARM\hdinsight-arm-template.json`.</span><span class="sxs-lookup"><span data-stu-id="f8129-144">In hello PowerShell script, hello file name is `C:\HDITutorials-ARM\hdinsight-arm-template.json`.</span></span>
2. <span data-ttu-id="f8129-145">V případě potřeby nastavte hello parametry a proměnné.</span><span class="sxs-lookup"><span data-stu-id="f8129-145">Set hello parameters and variables if needed.</span></span>
3. <span data-ttu-id="f8129-146">Spusťte hello šablony pomocí hello následující skript prostředí PowerShell:</span><span class="sxs-lookup"><span data-stu-id="f8129-146">Run hello template by using hello following PowerShell script:</span></span>

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
        # Connect tooAzure
        ####################################
        #region - Connect tooAzure subscription
        Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and hello dependent storage account
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

    <span data-ttu-id="f8129-147">Hello skript prostředí PowerShell lze konfigurovat pouze hello název clusteru.</span><span class="sxs-lookup"><span data-stu-id="f8129-147">hello PowerShell script configures only hello cluster name.</span></span> <span data-ttu-id="f8129-148">název účtu úložiště Hello je pevně zakódovaná v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="f8129-148">hello storage account name is hard-coded in hello template.</span></span> <span data-ttu-id="f8129-149">Jste heslo uživatele clusteru výzvami tooenter hello.</span><span class="sxs-lookup"><span data-stu-id="f8129-149">You are prompted tooenter hello cluster user password.</span></span> <span data-ttu-id="f8129-150">(výchozí uživatelské jméno hello **správce**.) Také jste heslo uživatele SSH výzvami tooenter hello.</span><span class="sxs-lookup"><span data-stu-id="f8129-150">(hello default username is **admin**.) You are also prompted tooenter hello SSH user password.</span></span> <span data-ttu-id="f8129-151">(uživatelské jméno SSH výchozí hello **sshuser**.)</span><span class="sxs-lookup"><span data-stu-id="f8129-151">(hello default SSH username is **sshuser**.)</span></span>  

<span data-ttu-id="f8129-152">Další informace najdete v tématu [nasadit v prostředí PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span><span class="sxs-lookup"><span data-stu-id="f8129-152">For more information, see  [Deploy with PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span></span>

## <a name="deploy-with-cli"></a><span data-ttu-id="f8129-153">Nasazení pomocí rozhraní příkazového řádku</span><span class="sxs-lookup"><span data-stu-id="f8129-153">Deploy with CLI</span></span>
<span data-ttu-id="f8129-154">Následující ukázka Hello používá rozhraní příkazového řádku Azure (CLI).</span><span class="sxs-lookup"><span data-stu-id="f8129-154">hello following sample uses Azure command-line interface (CLI).</span></span> <span data-ttu-id="f8129-155">Vytvoří cluster a jeho účet závislého úložiště a kontejneru voláním šablony Resource Manageru:</span><span class="sxs-lookup"><span data-stu-id="f8129-155">It creates a cluster and its dependent storage account and container by calling a Resource Manager template:</span></span>

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"

<span data-ttu-id="f8129-156">Jste tooenter výzvami:</span><span class="sxs-lookup"><span data-stu-id="f8129-156">You are prompted tooenter:</span></span>
* <span data-ttu-id="f8129-157">Název clusteru Hello.</span><span class="sxs-lookup"><span data-stu-id="f8129-157">hello cluster name.</span></span>
* <span data-ttu-id="f8129-158">heslo uživatele Hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="f8129-158">hello cluster user password.</span></span> <span data-ttu-id="f8129-159">(výchozí uživatelské jméno hello **správce**.)</span><span class="sxs-lookup"><span data-stu-id="f8129-159">(hello default username is **admin**.)</span></span>
* <span data-ttu-id="f8129-160">heslo uživatele SSH Hello.</span><span class="sxs-lookup"><span data-stu-id="f8129-160">hello SSH user password.</span></span> <span data-ttu-id="f8129-161">(uživatelské jméno SSH výchozí hello **sshuser**.)</span><span class="sxs-lookup"><span data-stu-id="f8129-161">(hello default SSH username is **sshuser**.)</span></span>

<span data-ttu-id="f8129-162">Hello následující kód obsahuje vložené parametry:</span><span class="sxs-lookup"><span data-stu-id="f8129-162">hello following code provides inline parameters:</span></span>

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

## <a name="deploy-with-hello-rest-api"></a><span data-ttu-id="f8129-163">Nasazení s hello REST API</span><span class="sxs-lookup"><span data-stu-id="f8129-163">Deploy with hello REST API</span></span>
<span data-ttu-id="f8129-164">V tématu [nasadit s hello REST API](../azure-resource-manager/resource-group-template-deploy-rest.md).</span><span class="sxs-lookup"><span data-stu-id="f8129-164">See [Deploy with hello REST API](../azure-resource-manager/resource-group-template-deploy-rest.md).</span></span>

## <a name="deploy-with-visual-studio"></a><span data-ttu-id="f8129-165">Nasazení s využitím sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8129-165">Deploy with Visual Studio</span></span>
 <span data-ttu-id="f8129-166">Pomocí sady Visual Studio toocreate projekt skupiny prostředků a nasadit tooAzure hello uživatelském rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f8129-166">Use Visual Studio toocreate a resource group project and deploy it tooAzure through hello user interface.</span></span> <span data-ttu-id="f8129-167">Vyberete typ hello tooinclude prostředky ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="f8129-167">You select hello type of resources tooinclude in your project.</span></span> <span data-ttu-id="f8129-168">Tyto prostředky se automaticky přidají toohello šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="f8129-168">Those resources are automatically added toohello Resource Manager template.</span></span> <span data-ttu-id="f8129-169">projekt Hello také poskytuje šablony hello toodeploy skript prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f8129-169">hello project also provides a PowerShell script toodeploy hello template.</span></span>

<span data-ttu-id="f8129-170">Úvod toousing Visual Studio se skupinami prostředků, najdete v části [vytvoření a nasazení skupin prostředků Azure pomocí sady Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="f8129-170">For an introduction toousing Visual Studio with resource groups, see [Creating and deploying Azure resource groups through Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="f8129-171">Řešení potíží</span><span class="sxs-lookup"><span data-stu-id="f8129-171">Troubleshoot</span></span>

<span data-ttu-id="f8129-172">Pokud narazíte na problémy s vytvářením clusterů HDInsight, podívejte se na [požadavky na řízení přístupu](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="f8129-172">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8129-173">Další kroky</span><span class="sxs-lookup"><span data-stu-id="f8129-173">Next steps</span></span>
<span data-ttu-id="f8129-174">V tomto článku jste se naučili několik způsobů toocreate clusteru služby HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f8129-174">In this article, you have learned several ways toocreate an HDInsight cluster.</span></span> <span data-ttu-id="f8129-175">toolearn více, najdete v části hello následující články:</span><span class="sxs-lookup"><span data-stu-id="f8129-175">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="f8129-176">Příklad nasazení prostředků prostřednictvím klientské knihovny hello .NET, naleznete v části [nasadit prostředky pomocí knihovny .NET a šablonu](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f8129-176">For an example of deploying resources through hello .NET client library, see [Deploy resources by using .NET libraries and a template](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="f8129-177">Podrobný příklad nasazení aplikace naleznete v tématu [zřídit a nasadit mikroslužeb předvídatelné v Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).</span><span class="sxs-lookup"><span data-stu-id="f8129-177">For an in-depth example of deploying an application, see [Provision and deploy microservices predictably in Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).</span></span>
* <span data-ttu-id="f8129-178">Pokyny k nasazení vašeho prostředí toodifferent řešení najdete v tématu [vývojová a testovací prostředí v Microsoft Azure](../solution-dev-test-environments.md).</span><span class="sxs-lookup"><span data-stu-id="f8129-178">For guidance on deploying your solution toodifferent environments, see [Development and test environments in Microsoft Azure](../solution-dev-test-environments.md).</span></span>
* <span data-ttu-id="f8129-179">toolearn o hello části hello šablony Azure Resource Manageru, najdete v části [vytváření šablon](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f8129-179">toolearn about hello sections of hello Azure Resource Manager template, see [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="f8129-180">Seznam funkcí hello v šablonu Azure Resource Manager můžete použít, najdete v části [funkce šablon](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="f8129-180">For a list of hello functions you can use in an Azure Resource Manager template, see [Template functions](../azure-resource-manager/resource-group-template-functions.md).</span></span>

## <a name="appendix-resource-manager-template-toocreate-a-hadoop-cluster"></a><span data-ttu-id="f8129-181">Dodatek: Resource Manager šablony toocreate clusteru Hadoop</span><span class="sxs-lookup"><span data-stu-id="f8129-181">Appendix: Resource Manager template toocreate a Hadoop cluster</span></span>
<span data-ttu-id="f8129-182">Hello následující šablony Azure Resource Manager vytvoří cluster systémem Linux Hadoop se účet závislého úložiště Azure hello.</span><span class="sxs-lookup"><span data-stu-id="f8129-182">hello following Azure Resource Manager template creates a Linux-based Hadoop cluster with hello dependent Azure storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="f8129-183">Tato ukázka obsahuje informace o konfiguraci pro metaúložiště Hive a metaúložiště Oozie.</span><span class="sxs-lookup"><span data-stu-id="f8129-183">This sample includes configuration information for Hive metastore and Oozie metastore.</span></span> <span data-ttu-id="f8129-184">Odebrání oddílu hello nebo nakonfigurujte hello části před použitím šablony hello.</span><span class="sxs-lookup"><span data-stu-id="f8129-184">Remove hello section or configure hello section before using hello template.</span></span>
>
>

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "hello name of hello HDInsight cluster toocreate."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used toosubmit jobs toohello cluster and toolog into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "sshUserName": {
        "type": "string",
        "defaultValue": "sshuser",
        "metadata": {
            "description": "These credentials can be used tooremotely access hello cluster."
        }
        },
        "sshPassword": {
        "type": "securestring",
        "metadata": {
            "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
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
            "description": "hello location where all azure resources will be deployed."
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
            "description": "hello type of hello HDInsight cluster toocreate."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "hello number of nodes in hello HDInsight cluster."
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

## <a name="appendix-resource-manager-template-toocreate-a-spark-cluster"></a><span data-ttu-id="f8129-185">Dodatek: Resource Manager šablony toocreate clusteru Spark</span><span class="sxs-lookup"><span data-stu-id="f8129-185">Appendix: Resource Manager template toocreate a Spark cluster</span></span>

<span data-ttu-id="f8129-186">Tato část obsahuje šablony Resource Manageru, které můžete použít toocreate clusteru HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="f8129-186">This section provides a Resource Manager template that you can use toocreate an HDInsight Spark cluster.</span></span> <span data-ttu-id="f8129-187">Tato šablona zahrnuje konfigurace pro `spark-defaults` a `spark-thrift-sparkconf` (pro clustery Spark 1.6) a `spark2-defaults` a `spark2-thrift-sparkconf` (pro clustery Spark 2).</span><span class="sxs-lookup"><span data-stu-id="f8129-187">This template includes configurations for `spark-defaults` and `spark-thrift-sparkconf` (for Spark 1.6 clusters) and `spark2-defaults` and `spark2-thrift-sparkconf` (for Spark 2 clusters).</span></span> <span data-ttu-id="f8129-188">Kromě toho toothis, HDInsight vypočítá a nastaví konfigurace, jako `spark.executor.instances`, `spark.executor.memory`, a `spark.executor.cores` na základě velikosti clusteru hello.</span><span class="sxs-lookup"><span data-stu-id="f8129-188">In addition toothis, HDInsight calculates and sets configurations such as `spark.executor.instances`, `spark.executor.memory`, and `spark.executor.cores` based on hello cluster size.</span></span> 

<span data-ttu-id="f8129-189">Pokud nastavíte všechny jeden parametr v části v rámci samotné hello šablony, HDInsight nepodporuje výpočtu a nastavit další parametry hello hello stejné části.</span><span class="sxs-lookup"><span data-stu-id="f8129-189">If you set any one parameter in a section as part of hello template itself, HDInsight does not calculate and set hello other parameters of hello same section.</span></span> <span data-ttu-id="f8129-190">Například parametr `spark.executor.instances` v hello `spark-defaults` konfigurace.</span><span class="sxs-lookup"><span data-stu-id="f8129-190">For example, parameter `spark.executor.instances` is in hello  `spark-defaults` configuration.</span></span> <span data-ttu-id="f8129-191">Pokud nastavíte parametr jiné (například `spark.yarn.exector.memoryOverhead`) v hello `spark-defaults` konfigurace, HDInsight nepodporuje výpočtu a nastavit hello `spark.executor.instances` také parametr.</span><span class="sxs-lookup"><span data-stu-id="f8129-191">If you set another parameter (for example, `spark.yarn.exector.memoryOverhead`) in hello `spark-defaults` configuration, HDInsight does not calculate and set hello `spark.executor.instances` parameter as well.</span></span>

    {
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "parameters": {
        "clusterName": {
            "type": "string",
            "metadata": {
                "description": "hello name of hello HDInsight cluster toocreate."
            }
        },
        "clusterLoginUserName": {
            "type": "string",
            "defaultValue": "admin",
            "metadata": {
                "description": "These credentials can be used toosubmit jobs toohello cluster and toolog into cluster dashboards."
            }
        },
        "clusterLoginPassword": {
            "type": "securestring",
            "metadata": {
                "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus",
            "metadata": {
                "description": "hello location where all azure resources will be deployed."
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
                "description": "hello number of nodes in hello HDInsight cluster."
            }
        },
        "clusterKind": {
            "type": "string",
            "defaultValue": "SPARK",
            "metadata": {
                "description": "hello type of hello HDInsight cluster toocreate."
            }
        },
        "sshUserName": {
            "type": "string",
            "defaultValue": "sshuser",
            "metadata": {
                "description": "These credentials can be used tooremotely access hello cluster."
            }
        },
        "sshPassword": {
            "type": "securestring",
            "metadata": {
                "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
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
