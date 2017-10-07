---
title: "aaaManage prostředky s hello rozhraní příkazového řádku Azure | Microsoft Docs"
description: "Použít toomanage hello rozhraní příkazového řádku Azure (CLI) Azure prostředků a skupin"
editor: 
manager: timlt
documentationcenter: 
author: tfitzmac
services: azure-resource-manager
ms.assetid: bb0af466-4f65-4559-ac3a-43985fa096ff
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: tomfitz
ms.openlocfilehash: 3df70e123d14d3bbf2648c71970bac1db4afc025
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-cli-toomanage-azure-resources-and-resource-groups"></a><span data-ttu-id="6e21e-103">Použití Azure CLI toomanage hello Azure skupiny prostředků a prostředky</span><span class="sxs-lookup"><span data-stu-id="6e21e-103">Use hello Azure CLI toomanage Azure resources and resource groups</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6e21e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6e21e-104">Portal</span></span>](resource-group-portal.md) 
> * [<span data-ttu-id="6e21e-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6e21e-105">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="6e21e-106">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6e21e-106">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="6e21e-107">REST API</span><span class="sxs-lookup"><span data-stu-id="6e21e-107">REST API</span></span>](resource-manager-rest-api.md)
> 
> 

<span data-ttu-id="6e21e-108">Hello rozhraní příkazového řádku Azure (Azure CLI) je jedním z několika nástrojů můžete použít toodeploy a spravovat prostředky pomocí Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="6e21e-108">hello Azure Command-Line Interface (Azure CLI) is one of several tools you can use toodeploy and manage resources with Resource Manager.</span></span> <span data-ttu-id="6e21e-109">Tento článek představuje běžné způsoby toomanage Azure prostředků a skupin prostředků pomocí hello rozhraní příkazového řádku Azure v režimu Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6e21e-109">This article introduces common ways toomanage Azure resources and resource groups by using hello Azure CLI in Resource Manager mode.</span></span> <span data-ttu-id="6e21e-110">Informace o použití prostředků toodeploy hello rozhraní příkazového řádku najdete v tématu [nasazení prostředků pomocí šablony Resource Manageru a rozhraní příkazového řádku Azure](resource-group-template-deploy-cli.md).</span><span class="sxs-lookup"><span data-stu-id="6e21e-110">For information about using hello CLI toodeploy resources, see [Deploy resources with Resource Manager templates and Azure CLI](resource-group-template-deploy-cli.md).</span></span> <span data-ttu-id="6e21e-111">Informace o prostředků Azure a Resource Manager, najdete v článku hello [přehled Azure Resource Manageru](resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6e21e-111">For background about Azure resources and Resource Manager, visit hello [Azure Resource Manager Overview](resource-group-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="6e21e-112">toomanage Azure prostředky s hello příkazového řádku Azure CLI, je třeba příliš[nainstalovat hello rozhraní příkazového řádku Azure](../cli-install-nodejs.md), a [přihlásit tooAzure](../xplat-cli-connect.md) pomocí hello `azure login` příkaz.</span><span class="sxs-lookup"><span data-stu-id="6e21e-112">toomanage Azure resources with hello Azure CLI, you need too[install hello Azure CLI](../cli-install-nodejs.md), and [log in tooAzure](../xplat-cli-connect.md) by using hello `azure login` command.</span></span> <span data-ttu-id="6e21e-113">Ujistěte se, hello rozhraní příkazového řádku je v režimu Resource Manager (Spustit `azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="6e21e-113">Make sure hello CLI is in Resource Manager mode (run `azure config mode arm`).</span></span> <span data-ttu-id="6e21e-114">Pokud jste provádí tyto věci, jste toogo připraven.</span><span class="sxs-lookup"><span data-stu-id="6e21e-114">If you've done these things, you're ready toogo.</span></span>
> 
> 

## <a name="get-resource-groups-and-resources"></a><span data-ttu-id="6e21e-115">Skupiny prostředků a prostředky</span><span class="sxs-lookup"><span data-stu-id="6e21e-115">Get resource groups and resources</span></span>
### <a name="resource-groups"></a><span data-ttu-id="6e21e-116">Skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="6e21e-116">Resource groups</span></span>
<span data-ttu-id="6e21e-117">Tento příkaz Spustit tooget seznam všech skupin prostředků v předplatného a jejich umístění.</span><span class="sxs-lookup"><span data-stu-id="6e21e-117">tooget a list of all resource groups in your subscription and their locations, run this command.</span></span>

    azure group list


### <a name="resources"></a><span data-ttu-id="6e21e-118">Zdroje</span><span class="sxs-lookup"><span data-stu-id="6e21e-118">Resources</span></span>
 <span data-ttu-id="6e21e-119">Název všechny prostředky ve skupině, například ten, který se toolist *testRG*, použijte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="6e21e-119">toolist all resources in a group, such as one with name *testRG*, use hello following command:</span></span>

    azure resource list testRG

<span data-ttu-id="6e21e-120">tooview jednotlivé zdroje v rámci skupiny hello, jako je například virtuální počítač s názvem *MyUbuntuVM*, použijte příkaz jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="6e21e-120">tooview an individual resource within hello group, such as a VM named *MyUbuntuVM*, use a command like hello following:</span></span>

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

<span data-ttu-id="6e21e-121">Všimněte si hello **Microsoft.Compute/virtualMachines** parametr.</span><span class="sxs-lookup"><span data-stu-id="6e21e-121">Notice hello **Microsoft.Compute/virtualMachines** parameter.</span></span> <span data-ttu-id="6e21e-122">Tento parametr určuje typ hello hello prostředku, který požadujete informace na.</span><span class="sxs-lookup"><span data-stu-id="6e21e-122">This parameter indicates hello type of hello resource you are requesting information on.</span></span>

> [!NOTE]
> <span data-ttu-id="6e21e-123">Při použití hello **prostředků azure** příkazy než hello **seznamu** příkaz, je nutné zadat hello rozhraní API verze hello prostředku s hello **-o** parametr.</span><span class="sxs-lookup"><span data-stu-id="6e21e-123">When using hello **azure resource** commands other than hello **list** command, you must specify hello API version of hello resource with hello **-o** parameter.</span></span> <span data-ttu-id="6e21e-124">Pokud si nejste jisti o hello verze rozhraní API, projděte si soubor šablony hello a najít hello apiVersion pole pro prostředek hello.</span><span class="sxs-lookup"><span data-stu-id="6e21e-124">If you're unsure about hello API version, consult hello template file and find hello apiVersion field for hello resource.</span></span> <span data-ttu-id="6e21e-125">Další informace o verzích rozhraní API ve službě Správce prostředků najdete v tématu [zprostředkovatelé prostředků a typy](resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="6e21e-125">For more about API versions in Resource Manager, see [Resource providers and types](resource-manager-supported-services.md).</span></span>
> 
> 

<span data-ttu-id="6e21e-126">Při zobrazení podrobností na prostředek, je často užitečné toouse hello `--json` parametr.</span><span class="sxs-lookup"><span data-stu-id="6e21e-126">When viewing details on a resource, it is often useful toouse hello `--json` parameter.</span></span> <span data-ttu-id="6e21e-127">Tento parametr umožňuje hello výstup čitelnější, protože některé hodnoty jsou vnořené struktury nebo kolekce.</span><span class="sxs-lookup"><span data-stu-id="6e21e-127">This parameter makes hello output more readable, because some values are nested structures, or collections.</span></span> <span data-ttu-id="6e21e-128">Hello následující příklad ukazuje vracejících hello výsledky hello **zobrazit** příkaz jako dokument JSON.</span><span class="sxs-lookup"><span data-stu-id="6e21e-128">hello following example demonstrates returning hello results of hello **show** command as a JSON document.</span></span>

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

> [!NOTE]
> <span data-ttu-id="6e21e-129">Pokud chcete uložit toofile data JSON hello pomocí hello &gt; znak toodirect hello výstupní tooa soubor.</span><span class="sxs-lookup"><span data-stu-id="6e21e-129">If you want, save hello JSON data toofile by using hello &gt; character toodirect hello output tooa file.</span></span> <span data-ttu-id="6e21e-130">Například:</span><span class="sxs-lookup"><span data-stu-id="6e21e-130">For example:</span></span>
> 
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`
> 
> 

### <a name="tags"></a><span data-ttu-id="6e21e-131">Značky</span><span class="sxs-lookup"><span data-stu-id="6e21e-131">Tags</span></span>
[!INCLUDE [resource-manager-tag-resources-cli](../../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a><span data-ttu-id="6e21e-132">Správa prostředků</span><span class="sxs-lookup"><span data-stu-id="6e21e-132">Manage resources</span></span>
<span data-ttu-id="6e21e-133">tooadd prostředků, jako je například úložiště účet tooa skupinu prostředků, spusťte příkaz podobný:</span><span class="sxs-lookup"><span data-stu-id="6e21e-133">tooadd a resource such as a storage account tooa resource group, run a command similar to:</span></span>

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"

<span data-ttu-id="6e21e-134">Kromě toho toospecifying hello verze rozhraní API hello prostředku s hello **-o** parametr, použijte hello **-p** řetězec formátu JSON toopass parametrů s jakékoli požadované nebo další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6e21e-134">In addition toospecifying hello API version of hello resource with hello **-o** parameter, use hello **-p** parameter toopass a JSON-formatted string with any required or additional properties.</span></span>

<span data-ttu-id="6e21e-135">toodelete na existující prostředek, jako je například prostředek virtuálního počítače, použijte příkaz jako hello následující:</span><span class="sxs-lookup"><span data-stu-id="6e21e-135">toodelete an existing resource such as a virtual machine resource, use a command like hello following:</span></span>

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

<span data-ttu-id="6e21e-136">toomove existující skupiny prostředků tooanother prostředků nebo předplatného, použijte hello **přesunutí prostředku azure** příkaz.</span><span class="sxs-lookup"><span data-stu-id="6e21e-136">toomove existing resources tooanother resource group or subscription, use hello **azure resource move** command.</span></span> <span data-ttu-id="6e21e-137">Následující příklad ukazuje, jak Hello toomove Redis Cache tooa novou skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6e21e-137">hello following example shows how toomove a Redis Cache tooa new resource group.</span></span> <span data-ttu-id="6e21e-138">V hello **-i** parametr zadejte čárkami oddělený seznam id prostředku hello toomove.</span><span class="sxs-lookup"><span data-stu-id="6e21e-138">In hello **-i** parameter, provide a comma-separated list of hello resource id's toomove.</span></span>

    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-tooresources"></a><span data-ttu-id="6e21e-139">Řízení přístupu tooresources</span><span class="sxs-lookup"><span data-stu-id="6e21e-139">Control access tooresources</span></span>
<span data-ttu-id="6e21e-140">Můžete použít rozhraní příkazového řádku Azure toocreate hello a spravovat zásady toocontrol přístup tooAzure prostředky.</span><span class="sxs-lookup"><span data-stu-id="6e21e-140">You can use hello Azure CLI toocreate and manage policies toocontrol access tooAzure resources.</span></span> <span data-ttu-id="6e21e-141">Pro informace o definice zásady a přiřazení zásad tooresources viz [používat prostředky toomanage zásad a kontrolujte přístup](resource-manager-policy.md).</span><span class="sxs-lookup"><span data-stu-id="6e21e-141">For background about policy definitions and assigning policies tooresources, see [Use policy toomanage resources and control access](resource-manager-policy.md).</span></span>

<span data-ttu-id="6e21e-142">Například hello následující zásady toodeny definovat všechny požadavky, které není umístění západní USA nebo Sever střední USA a uložte ho policy.json soubor definice zásady toohello:</span><span class="sxs-lookup"><span data-stu-id="6e21e-142">For example, define hello following policy toodeny all requests where location is not West US or North Central US, and save it toohello policy definition file policy.json:</span></span>

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

<span data-ttu-id="6e21e-143">Spusťte hello **vytvoření definice zásady** příkaz:</span><span class="sxs-lookup"><span data-stu-id="6e21e-143">Then run hello **policy definition create** command:</span></span>

    azure policy definition create MyPolicy -p c:\temp\policy.json

<span data-ttu-id="6e21e-144">Tento příkaz zobrazí výstup podobný toohello následující:</span><span class="sxs-lookup"><span data-stu-id="6e21e-144">This command shows output similar toohello following:</span></span>

    + <span data-ttu-id="6e21e-145">Vytvořením definice zásady MyPolicy data: PolicyName: MyPolicy data: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy</span><span class="sxs-lookup"><span data-stu-id="6e21e-145">Creating policy definition MyPolicy data:    PolicyName:             MyPolicy data:    PolicyDefinitionId:     /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/MyPolicy</span></span>

    <span data-ttu-id="6e21e-146">data: Třída PolicyType: vlastní data: DisplayName: není definovaná data: Popis: není definovaná data: PolicyRule: pole = umístění, v = [westus, northcentralus], účinek = Odepřít</span><span class="sxs-lookup"><span data-stu-id="6e21e-146">data:    PolicyType:             Custom data:    DisplayName:            undefined data:    Description:            undefined data:    PolicyRule:             field=location, in=[westus, northcentralus], effect=deny</span></span>

 <span data-ttu-id="6e21e-147">tooassign zásadu v oboru hello chcete, použijte hello **PolicyDefinitionId** vrácená z předchozí příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="6e21e-147">tooassign a policy at hello scope you want, use hello **PolicyDefinitionId** returned from hello previous command.</span></span> <span data-ttu-id="6e21e-148">V následujícím příkladu hello tento obor je hello předplatné, ale můžete určit obor skupin tooresource nebo jednotlivé prostředky:</span><span class="sxs-lookup"><span data-stu-id="6e21e-148">In hello following example, this scope is hello subscription, but you can scope tooresource groups or individual resources:</span></span>

    <span data-ttu-id="6e21e-149">Vytvoření přiřazení zásady Azure MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /</span><span class="sxs-lookup"><span data-stu-id="6e21e-149">azure policy assignment create MyPolicyAssignment -p /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/########-####-####-####-############/</span></span>

<span data-ttu-id="6e21e-150">Můžete získat, změnit nebo odebrat definice zásady pomocí hello **Zobrazit definice zásady**, **definici sady zásad**, a **odstranit definice zásady** příkazy.</span><span class="sxs-lookup"><span data-stu-id="6e21e-150">You can get, change, or remove policy definitions by using hello **policy definition show**, **policy definition set**, and **policy definition delete** commands.</span></span>

<span data-ttu-id="6e21e-151">Podobně můžete získat, změnit nebo odebrat přiřazení zásad pomocí hello **zobrazit přiřazení zásad**, **sady přiřazení zásad**, a **odstranit přiřazení zásady** příkazy .</span><span class="sxs-lookup"><span data-stu-id="6e21e-151">Similarly, you can get, change, or remove policy assignments by using hello **policy assignment show**, **policy assignment set**, and **policy assignment delete** commands.</span></span>

## <a name="export-a-resource-group-as-a-template"></a><span data-ttu-id="6e21e-152">Export skupiny prostředků jako šablonu</span><span class="sxs-lookup"><span data-stu-id="6e21e-152">Export a resource group as a template</span></span>
<span data-ttu-id="6e21e-153">Pro existující skupinu prostředků můžete zobrazit hello šablony Resource Manageru pro skupinu prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="6e21e-153">For an existing resource group, you can view hello Resource Manager template for hello resource group.</span></span> <span data-ttu-id="6e21e-154">Export šablony hello nabízí dvě výhody:</span><span class="sxs-lookup"><span data-stu-id="6e21e-154">Exporting hello template offers two benefits:</span></span>

1. <span data-ttu-id="6e21e-155">Snadno můžete automatizovat budoucí nasazení řešení hello vzhledem k tomu, že všechny infrastruktury hello je definována v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="6e21e-155">You can easily automate future deployments of hello solution because all hello infrastructure is defined in hello template.</span></span>
2. <span data-ttu-id="6e21e-156">Můžete seznámit se syntaxí šablony prohlížením hello formátu JSON, který představuje vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="6e21e-156">You can become familiar with template syntax by looking at hello JSON that represents your solution.</span></span>

<span data-ttu-id="6e21e-157">Pomocí hello rozhraní příkazového řádku Azure, můžete buď vyexportovat šablonu, která představuje hello aktuální stav vaší skupiny prostředků, nebo stáhnout hello šablonu, která byla použita pro konkrétní nasazení.</span><span class="sxs-lookup"><span data-stu-id="6e21e-157">Using hello Azure CLI, you can either export a template that represents hello current state of your resource group, or download hello template that was used for a particular deployment.</span></span>

* <span data-ttu-id="6e21e-158">**Export hello šablony pro skupinu prostředků** – to je užitečné, když jste provedli změny skupiny prostředků tooa a potřebovat tooretrieve hello reprezentace JSON v jejím aktuálním stavu.</span><span class="sxs-lookup"><span data-stu-id="6e21e-158">**Export hello template for a resource group** - This is helpful when you made changes tooa resource group, and need tooretrieve hello JSON representation of its current state.</span></span> <span data-ttu-id="6e21e-159">Vygenerované šablony hello však obsahuje pouze minimální počet parametrů a žádné proměnné.</span><span class="sxs-lookup"><span data-stu-id="6e21e-159">However, hello generated template contains only a minimal number of parameters and no variables.</span></span> <span data-ttu-id="6e21e-160">Většina hodnot hello v šabloně hello jsou pevně.</span><span class="sxs-lookup"><span data-stu-id="6e21e-160">Most of hello values in hello template are hard-coded.</span></span> <span data-ttu-id="6e21e-161">Před nasazením hello vygenerované šablony, můžete tooconvert více hodnot hello do parametrů, můžete přizpůsobit hello nasazení pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="6e21e-161">Before deploying hello generated template, you may wish tooconvert more of hello values into parameters so you can customize hello deployment for different environments.</span></span>
  
    <span data-ttu-id="6e21e-162">tooexport hello šablony pro prostředek skupiny tooa místní adresář, spusťte hello `azure group export` příkaz, jak ukazuje následující příklad hello.</span><span class="sxs-lookup"><span data-stu-id="6e21e-162">tooexport hello template for a resource group tooa local directory, run hello `azure group export` command as shown in hello following example.</span></span> <span data-ttu-id="6e21e-163">(Nahraďte do místního adresáře, které jsou vhodné pro vaše prostředí operačního systému.)</span><span class="sxs-lookup"><span data-stu-id="6e21e-163">(Substitute a local directory appropriate for your operating system environment.)</span></span>
  
        azure group export testRG ~/azure/templates/
* <span data-ttu-id="6e21e-164">**Stáhnout hello šablonu pro konkrétní nasazení** – to je užitečné, když potřebujete hello tooview skutečné se šablonu, která byla toodeploy využité prostředky.</span><span class="sxs-lookup"><span data-stu-id="6e21e-164">**Download hello template for a particular deployment** -- This is helpful when you need tooview hello actual template that was used toodeploy resources.</span></span> <span data-ttu-id="6e21e-165">Šablona Hello zahrnuje všechny parametry a proměnné definované pro původní nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="6e21e-165">hello template includes all parameters and variables defined for hello original deployment.</span></span> <span data-ttu-id="6e21e-166">Pokud někdo ve vaší organizaci provedené změny skupiny prostředků toohello mimo hello definice v šabloně hello, ale tato šablona nepředstavuje hello aktuální stav skupiny prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="6e21e-166">However, if someone in your organization made changes toohello resource group outside of hello definition in hello template, this template doesn't represent hello current state of hello resource group.</span></span>
  
    <span data-ttu-id="6e21e-167">toodownload hello Šablona používaná pro konkrétní nasazení tooa místní adresář, spusťte hello `azure group deployment template download` příkaz.</span><span class="sxs-lookup"><span data-stu-id="6e21e-167">toodownload hello template used for a particular deployment tooa local directory, run hello `azure group deployment template download` command.</span></span> <span data-ttu-id="6e21e-168">Například:</span><span class="sxs-lookup"><span data-stu-id="6e21e-168">For example:</span></span>
  
        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/

> [!NOTE]
> <span data-ttu-id="6e21e-169">Export šablony je ve verzi preview, a ne všechny typy prostředků v současné době podporují export šablony.</span><span class="sxs-lookup"><span data-stu-id="6e21e-169">Template export is in preview, and not all resource types currently support exporting a template.</span></span> <span data-ttu-id="6e21e-170">Při pokusu o tooexport šablonu, může se zobrazit chyba s oznámením, že některé prostředky nebyly exportovat.</span><span class="sxs-lookup"><span data-stu-id="6e21e-170">When attempting tooexport a template, you may see an error that states some resources were not exported.</span></span> <span data-ttu-id="6e21e-171">V případě potřeby ručně definovat tyto prostředky ve vaší šabloně po stáhnout.</span><span class="sxs-lookup"><span data-stu-id="6e21e-171">If needed, manually define these resources in your template after downloading it.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="6e21e-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6e21e-172">Next steps</span></span>
* <span data-ttu-id="6e21e-173">Podrobnosti o tooget operace nasazení a řešení chyby nasazení s hello rozhraní příkazového řádku Azure najdete v tématu [zobrazit operace nasazení](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="6e21e-173">tooget details of deployment operations and troubleshoot deployment errors with hello Azure CLI, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="6e21e-174">Pokud chcete toouse hello rozhraní příkazového řádku tooset aplikace nebo skriptu tooaccess prostředky, najdete v části [použití Azure CLI toocreate objekt služby prostředků tooaccess](resource-group-authenticate-service-principal-cli.md).</span><span class="sxs-lookup"><span data-stu-id="6e21e-174">If you want toouse hello CLI tooset up an application or script tooaccess resources, see [Use Azure CLI toocreate a service principal tooaccess resources](resource-group-authenticate-service-principal-cli.md).</span></span>
* <span data-ttu-id="6e21e-175">Pokyny k použití Resource Manager tooeffectively podniky můžou spravovat předplatná najdete v tématu [Azure enterprise vygenerované uživatelské rozhraní – zásady správného řízení doporučený předplatné](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="6e21e-175">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

