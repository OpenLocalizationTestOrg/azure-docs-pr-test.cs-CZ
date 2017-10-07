---
title: "řešení pro správu aaaCreating v Operations Management Suite (OMS) | Microsoft Docs"
description: "Řešení pro správu rozšíření hello funkce služby Operations Management Suite (OMS) tím, že poskytuje scénářů správy zabalené, aby zákazníci můžete přidat pracovní prostor OMS tootheir.  Tento článek obsahuje informace o tom, jak můžete vytvořit toobe řešení správy použít ve svém vlastním prostředí nebo provedené dostupné tooyour zákazníků."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: 1915e204-ba7e-431b-9718-9eb6b4213ad8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/30/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f408df1b21f519fd1eb2cbeb19cca18f6c4161f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="creating-a-management-solution-file-in-operations-management-suite-oms-preview"></a><span data-ttu-id="48519-104">Vytvoření souboru řešení pro správu v Operations Management Suite (OMS) (Preview)</span><span class="sxs-lookup"><span data-stu-id="48519-104">Creating a management solution file in Operations Management Suite (OMS) (Preview)</span></span>
> [!NOTE]
> <span data-ttu-id="48519-105">Toto je předběžná dokumentace pro vytváření řešení pro správu v OMS, které jsou aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="48519-105">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="48519-106">Žádné schéma níže popsané je toochange subjektu.</span><span class="sxs-lookup"><span data-stu-id="48519-106">Any schema described below is subject toochange.</span></span>  

<span data-ttu-id="48519-107">Řešení pro správu v Operations Management Suite (OMS) jsou implementované jako [šablony Resource Manageru](../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="48519-107">Management solutions in Operations Management Suite (OMS) are implemented as [Resource Manager templates](../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>  <span data-ttu-id="48519-108">hlavní úloh Hello dozvědět, jak je řešení pro správu tooauthor učení jak příliš[vytvořit šablonu](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="48519-108">hello main task in learning how tooauthor management solutions is learning how too[author a template](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>  <span data-ttu-id="48519-109">Tento článek obsahuje jedinečné Podrobnosti šablony použité pro řešení a jak tooconfigure typické řešení prostředky.</span><span class="sxs-lookup"><span data-stu-id="48519-109">This article provides unique details of templates used for solutions and how tooconfigure typical solution resources.</span></span>


## <a name="tools"></a><span data-ttu-id="48519-110">Nástroje</span><span class="sxs-lookup"><span data-stu-id="48519-110">Tools</span></span>

<span data-ttu-id="48519-111">Všechny toowork textového editoru můžete používat se soubory řešení, ale doporučujeme, abyste využití hello funkce poskytované v sadě Visual Studio nebo Visual Studio Code, jak je popsáno v následujících článcích hello.</span><span class="sxs-lookup"><span data-stu-id="48519-111">You can use any text editor toowork with solution files, but we recommend leveraging hello features provided in Visual Studio or Visual Studio Code as described in hello following articles.</span></span>

- [<span data-ttu-id="48519-112">Vytvoření a nasazení skupin prostředků Azure pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48519-112">Creating and deploying Azure resource groups through Visual Studio</span></span>](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
- [<span data-ttu-id="48519-113">Práce s šablony Azure Resource Manageru v sadě Visual Studio kódu</span><span class="sxs-lookup"><span data-stu-id="48519-113">Working with Azure Resource Manager Templates in Visual Studio Code</span></span>](../azure-resource-manager/resource-manager-vs-code.md)




## <a name="structure"></a><span data-ttu-id="48519-114">Struktura</span><span class="sxs-lookup"><span data-stu-id="48519-114">Structure</span></span>
<span data-ttu-id="48519-115">Základní struktura Hello soubor řešení správy je hello stejné jako [šablony Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md#template-format) tedy následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="48519-115">hello basic structure of a management solution file is hello same as a [Resource Manager Template](../azure-resource-manager/resource-group-authoring-templates.md#template-format) which is as follows.</span></span>  <span data-ttu-id="48519-116">Každý z níže uvedených částech hello popisuje elementy hello nejvyšší úrovně a a jejich obsah v řešení.</span><span class="sxs-lookup"><span data-stu-id="48519-116">Each of hello sections below describes hello top level elements and and their contents in a solution.</span></span>  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a><span data-ttu-id="48519-117">Parametry</span><span class="sxs-lookup"><span data-stu-id="48519-117">Parameters</span></span>
<span data-ttu-id="48519-118">[Parametry](../azure-resource-manager/resource-group-authoring-templates.md#parameters) jsou hodnoty, které požadujete od hello uživatele při instalaci hello řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="48519-118">[Parameters](../azure-resource-manager/resource-group-authoring-templates.md#parameters) are values that you require from hello user when they install hello management solution.</span></span>  <span data-ttu-id="48519-119">Jsou standardní parametry, které budou mít všechna řešení a podle potřeby můžete přidat další parametry pro vaše konkrétní řešení.</span><span class="sxs-lookup"><span data-stu-id="48519-119">There are standard parameters that all solutions will have, and you can add additional parameters as required for your particular solution.</span></span>  <span data-ttu-id="48519-120">Jak budou uživatelé zadali hodnoty parametrů při instalaci řešení bude záviset na konkrétní parametr hello a jak se instaluje hello řešení.</span><span class="sxs-lookup"><span data-stu-id="48519-120">How users will provide parameter values when they install your solution will depend on hello particular parameter and how hello solution is being installed.</span></span>

<span data-ttu-id="48519-121">Když uživatel nainstaluje řešení pro správu prostřednictvím hello [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) nebo [šablony Azure QuickStart](operations-management-suite-solutions.md#finding-and-installing-management-solutions) jsou výzvami tooselect [OMS pracovní prostor a účet Automation. ](operations-management-suite-solutions.md#oms-workspace-and-automation-account).</span><span class="sxs-lookup"><span data-stu-id="48519-121">When a user installs your management solution through hello [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) or [Azure QuickStart templates](operations-management-suite-solutions.md#finding-and-installing-management-solutions) they are prompted tooselect an [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account).</span></span>  <span data-ttu-id="48519-122">Toto jsou hodnoty používané toopopulate hello každé standardní parametry hello.</span><span class="sxs-lookup"><span data-stu-id="48519-122">These are used toopopulate hello values of each of hello standard parameters.</span></span>  <span data-ttu-id="48519-123">Hello uživatel nebude vyzván k toodirectly zadejte hodnoty pro parametry hello standardní, ale jsou výzvami tooprovide hodnoty pro žádné další parametry.</span><span class="sxs-lookup"><span data-stu-id="48519-123">hello user is not prompted toodirectly provide values for hello standard parameters, but they are prompted tooprovide values for any additional parameters.</span></span>

<span data-ttu-id="48519-124">Když uživatel hello nainstaluje řešení [jinou metodu](operations-management-suite-solutions.md#finding-and-installing-management-solutions), musí zadat hodnotu pro všechny standardní parametry a všech dalších parametrů.</span><span class="sxs-lookup"><span data-stu-id="48519-124">When hello user installs your solution [another method](operations-management-suite-solutions.md#finding-and-installing-management-solutions), they must provide a value for all standard parameters and all additional parameters.</span></span>

<span data-ttu-id="48519-125">Ukázka parametrů jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="48519-125">A sample parameter is shown below.</span></span>  

    "startTime": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

<span data-ttu-id="48519-126">Hello následující tabulka popisuje atributy hello parametru.</span><span class="sxs-lookup"><span data-stu-id="48519-126">hello following table describes hello attributes of a parameter.</span></span>

| <span data-ttu-id="48519-127">Atribut</span><span class="sxs-lookup"><span data-stu-id="48519-127">Attribute</span></span> | <span data-ttu-id="48519-128">Popis</span><span class="sxs-lookup"><span data-stu-id="48519-128">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="48519-129">type</span><span class="sxs-lookup"><span data-stu-id="48519-129">type</span></span> |<span data-ttu-id="48519-130">Datový typ pro parametr hello.</span><span class="sxs-lookup"><span data-stu-id="48519-130">Data type for hello parameter.</span></span> <span data-ttu-id="48519-131">Hello vstupního ovládacího prvku zobrazuje pro uživatele hello závisí na typu dat hello.</span><span class="sxs-lookup"><span data-stu-id="48519-131">hello input control displayed for hello user depends on hello data type.</span></span><br><br><span data-ttu-id="48519-132">BOOL – rozevíracího seznamu</span><span class="sxs-lookup"><span data-stu-id="48519-132">bool - Drop down box</span></span><br><span data-ttu-id="48519-133">String – textové pole</span><span class="sxs-lookup"><span data-stu-id="48519-133">string - Text box</span></span><br><span data-ttu-id="48519-134">int – textové pole</span><span class="sxs-lookup"><span data-stu-id="48519-134">int - Text box</span></span><br><span data-ttu-id="48519-135">SecureString - pole pro heslo</span><span class="sxs-lookup"><span data-stu-id="48519-135">securestring - Password field</span></span><br> |
| <span data-ttu-id="48519-136">category</span><span class="sxs-lookup"><span data-stu-id="48519-136">category</span></span> |<span data-ttu-id="48519-137">Volitelné kategorie pro parametr hello.</span><span class="sxs-lookup"><span data-stu-id="48519-137">Optional category for hello parameter.</span></span>  <span data-ttu-id="48519-138">Parametry v hello stejné kategorii jsou seskupeny dohromady.</span><span class="sxs-lookup"><span data-stu-id="48519-138">Parameters in hello same category are grouped together.</span></span> |
| <span data-ttu-id="48519-139">Ovládací prvek</span><span class="sxs-lookup"><span data-stu-id="48519-139">control</span></span> |<span data-ttu-id="48519-140">Další funkce pro parametry řetězce.</span><span class="sxs-lookup"><span data-stu-id="48519-140">Additional functionality for string parameters.</span></span><br><br><span data-ttu-id="48519-141">Zobrazí se datum a čas - datum a čas řízení.</span><span class="sxs-lookup"><span data-stu-id="48519-141">datetime - Datetime control is displayed.</span></span><br><span data-ttu-id="48519-142">identifikátor GUID – hodnota identifikátoru Guid je generován automaticky, a parametr hello se nezobrazí.</span><span class="sxs-lookup"><span data-stu-id="48519-142">guid - Guid value is automatically generated, and hello parameter is not displayed.</span></span> |
| <span data-ttu-id="48519-143">description</span><span class="sxs-lookup"><span data-stu-id="48519-143">description</span></span> |<span data-ttu-id="48519-144">Volitelný popis pro parametr hello.</span><span class="sxs-lookup"><span data-stu-id="48519-144">Optional description for hello parameter.</span></span>  <span data-ttu-id="48519-145">Zobrazí informace o bublinách další toohello parametr.</span><span class="sxs-lookup"><span data-stu-id="48519-145">Displayed in an information balloon next toohello parameter.</span></span> |

### <a name="standard-parameters"></a><span data-ttu-id="48519-146">Standardní parametry</span><span class="sxs-lookup"><span data-stu-id="48519-146">Standard parameters</span></span>
<span data-ttu-id="48519-147">Hello následující tabulka uvádí hello standardní parametry pro všechna řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="48519-147">hello following table lists hello standard parameters for all management solutions.</span></span>  <span data-ttu-id="48519-148">Tyto hodnoty jsou naplněny pro uživatele hello místo dotaz na ně při řešení z Azure Marketplace nebo rychlý start šablon hello.</span><span class="sxs-lookup"><span data-stu-id="48519-148">These values are populated for hello user instead of prompting for them when your solution is installed from hello Azure Marketplace or Quickstart templates.</span></span>  <span data-ttu-id="48519-149">Pokud řešení hello se instaluje s jinou metodu, musí uživatel Hello zadat hodnoty pro ně.</span><span class="sxs-lookup"><span data-stu-id="48519-149">hello user must provide values for them if hello solution is installed with another method.</span></span>

> [!NOTE]
> <span data-ttu-id="48519-150">Hello uživatelské rozhraní v šablonách Azure Marketplace a rychlý start hello očekává hello názvy parametrů v tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="48519-150">hello user interface in hello Azure Marketplace and Quickstart templates is expecting hello parameter names in hello table.</span></span>  <span data-ttu-id="48519-151">Pokud používáte jiný parametr názvy pak hello uživateli zobrazí výzva pro ně, a nebude se automaticky vyplní.</span><span class="sxs-lookup"><span data-stu-id="48519-151">If you use different parameter names then hello user will be prompted for them, and they will not be automatically populated.</span></span>
>
>

| <span data-ttu-id="48519-152">Parametr</span><span class="sxs-lookup"><span data-stu-id="48519-152">Parameter</span></span> | <span data-ttu-id="48519-153">Typ</span><span class="sxs-lookup"><span data-stu-id="48519-153">Type</span></span> | <span data-ttu-id="48519-154">Popis</span><span class="sxs-lookup"><span data-stu-id="48519-154">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="48519-155">název účtu</span><span class="sxs-lookup"><span data-stu-id="48519-155">accountName</span></span> |<span data-ttu-id="48519-156">Řetězec</span><span class="sxs-lookup"><span data-stu-id="48519-156">string</span></span> |<span data-ttu-id="48519-157">Název účtu Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="48519-157">Azure Automation account name.</span></span> |
| <span data-ttu-id="48519-158">pricingTier</span><span class="sxs-lookup"><span data-stu-id="48519-158">pricingTier</span></span> |<span data-ttu-id="48519-159">Řetězec</span><span class="sxs-lookup"><span data-stu-id="48519-159">string</span></span> |<span data-ttu-id="48519-160">Cenová úroveň pracovní prostor analýzy protokolů a účet Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="48519-160">Pricing tier of both Log Analytics workspace and Azure Automation account.</span></span> |
| <span data-ttu-id="48519-161">regionId</span><span class="sxs-lookup"><span data-stu-id="48519-161">regionId</span></span> |<span data-ttu-id="48519-162">Řetězec</span><span class="sxs-lookup"><span data-stu-id="48519-162">string</span></span> |<span data-ttu-id="48519-163">Oblast hello účet Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="48519-163">Region of hello Azure Automation account.</span></span> |
| <span data-ttu-id="48519-164">Název řešení SolutionName</span><span class="sxs-lookup"><span data-stu-id="48519-164">solutionName</span></span> |<span data-ttu-id="48519-165">Řetězec</span><span class="sxs-lookup"><span data-stu-id="48519-165">string</span></span> |<span data-ttu-id="48519-166">Název řešení hello.</span><span class="sxs-lookup"><span data-stu-id="48519-166">Name of hello solution.</span></span>  <span data-ttu-id="48519-167">Pokud nasazujete řešení prostřednictvím šablony rychlý start, pak byste měli definovat název řešení solutionName jako parametr, můžete definovat místo nutnosti toospecify hello uživatele, jeden řetězec.</span><span class="sxs-lookup"><span data-stu-id="48519-167">If you are deploying your solution through Quickstart templates, then you should define solutionName as a parameter so you can define a string instead requiring hello user toospecify one.</span></span> |
| <span data-ttu-id="48519-168">workspaceName</span><span class="sxs-lookup"><span data-stu-id="48519-168">workspaceName</span></span> |<span data-ttu-id="48519-169">Řetězec</span><span class="sxs-lookup"><span data-stu-id="48519-169">string</span></span> |<span data-ttu-id="48519-170">Název pracovního prostoru analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="48519-170">Log Analytics workspace name.</span></span> |
| <span data-ttu-id="48519-171">workspaceRegionId</span><span class="sxs-lookup"><span data-stu-id="48519-171">workspaceRegionId</span></span> |<span data-ttu-id="48519-172">Řetězec</span><span class="sxs-lookup"><span data-stu-id="48519-172">string</span></span> |<span data-ttu-id="48519-173">Oblast pracovního prostoru analýzy protokolů hello.</span><span class="sxs-lookup"><span data-stu-id="48519-173">Region of hello Log Analytics workspace.</span></span> |


<span data-ttu-id="48519-174">Následuje hello struktura hello standardní parametry, které můžete zkopírovat a vložit do souboru řešení.</span><span class="sxs-lookup"><span data-stu-id="48519-174">Following is hello structure of hello standard parameters that you can copy and paste into your solution file.</span></span>  

    "parameters": {
        "workspaceName": {
            "type": "string",
            "metadata": {
                "description": "A valid Log Analytics workspace name"
            }
        },
        "accountName": {
               "type": "string",
               "metadata": {
                   "description": "A valid Azure Automation account name"
               }
        },
        "workspaceRegionId": {
               "type": "string",
               "metadata": {
                   "description": "Region of hello Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of hello Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        }
    }


<span data-ttu-id="48519-175">Odkazovat tooparameter hodnoty v další prvky hello řešení se syntaxí hello **parametry (název parametru)**.</span><span class="sxs-lookup"><span data-stu-id="48519-175">You refer tooparameter values in other elements of hello solution with hello syntax **parameters('parameter name')**.</span></span>  <span data-ttu-id="48519-176">Například tooaccess hello název pracovního prostoru, byste použili **parameters('workspaceName')**</span><span class="sxs-lookup"><span data-stu-id="48519-176">For example, tooaccess hello workspace name, you would use **parameters('workspaceName')**</span></span>

## <a name="variables"></a><span data-ttu-id="48519-177">Proměnné</span><span class="sxs-lookup"><span data-stu-id="48519-177">Variables</span></span>
<span data-ttu-id="48519-178">[Proměnné](../azure-resource-manager/resource-group-authoring-templates.md#variables) jsou hodnoty, které budete používat v hello zbytek hello řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="48519-178">[Variables](../azure-resource-manager/resource-group-authoring-templates.md#variables) are values that you will use in hello rest of hello management solution.</span></span>  <span data-ttu-id="48519-179">Tyto hodnoty nejsou zveřejněné toohello uživatel, který instaluje hello řešení.</span><span class="sxs-lookup"><span data-stu-id="48519-179">These values are not exposed toohello user installing hello solution.</span></span>  <span data-ttu-id="48519-180">Jsou to určený tooprovide hello Autor na jednom místě, kde můžete spravovat hodnoty, které se dají použít více než jednou v celé řešení hello.</span><span class="sxs-lookup"><span data-stu-id="48519-180">They are intended tooprovide hello author with a single location where they can manage values that may be used multiple times throughout hello solution.</span></span> <span data-ttu-id="48519-181">Řešení konkrétních tooyour hodnoty měli umístit do proměnné jako názvem na rozdíl od toohard kódování je v hello **prostředky** elementu.</span><span class="sxs-lookup"><span data-stu-id="48519-181">You should put any values specific tooyour solution in variables as opposed toohard coding them in hello **resources** element.</span></span>  <span data-ttu-id="48519-182">To usnadňuje srozumitelnější hello kódu a umožňuje vám tooeasily změnit tyto hodnoty v novějších verzích.</span><span class="sxs-lookup"><span data-stu-id="48519-182">This makes hello code more readable and allows you tooeasily change these values in later versions.</span></span>

<span data-ttu-id="48519-183">Následuje příklad **proměnné** element s typické parametry použité v řešení.</span><span class="sxs-lookup"><span data-stu-id="48519-183">Following is an example of a **variables** element with typical parameters used in solutions.</span></span>

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

<span data-ttu-id="48519-184">Najdete hodnoty toovariable prostřednictvím řešení hello se syntaxí hello **proměnné ('název proměnné')**.</span><span class="sxs-lookup"><span data-stu-id="48519-184">You refer toovariable values through hello solution with hello syntax **variables('variable name')**.</span></span>  <span data-ttu-id="48519-185">Například tooaccess hello název řešení SolutionName proměnné, byste použili **variables('SolutionName')**.</span><span class="sxs-lookup"><span data-stu-id="48519-185">For example, tooaccess hello SolutionName variable, you would use **variables('SolutionName')**.</span></span>

<span data-ttu-id="48519-186">Můžete také definovat komplexní proměnné tohoto několik sad hodnot.</span><span class="sxs-lookup"><span data-stu-id="48519-186">You can also define complex variables that multiple sets of values.</span></span>  <span data-ttu-id="48519-187">Tyto jsou zvláště užitečné při řešení pro správu, které definujete více vlastností pro různé typy prostředků.</span><span class="sxs-lookup"><span data-stu-id="48519-187">These are particularly useful in management solutions where you are defining multiple properties for different types of resources.</span></span>  <span data-ttu-id="48519-188">Například může změnit strukturu proměnné hello řešení uvedené výše toohello následující.</span><span class="sxs-lookup"><span data-stu-id="48519-188">For example, you could restructure hello solution variables shown above toohello following.</span></span>

    "variables": {
        "Solution": {
          "Version": "1.1",
          "Publisher": "Contoso",
          "Name": "My Solution"
        },
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

<span data-ttu-id="48519-189">V takovém případě najdete hodnoty toovariable prostřednictvím řešení hello se syntaxí hello **variables('variable name').property**.</span><span class="sxs-lookup"><span data-stu-id="48519-189">In this case, you refer toovariable values through hello solution with hello syntax **variables('variable name').property**.</span></span>  <span data-ttu-id="48519-190">Například tooaccess hello řešení název proměnné, byste použili **variables('Solution'). Název**.</span><span class="sxs-lookup"><span data-stu-id="48519-190">For example, tooaccess hello Solution Name variable, you would use **variables('Solution').Name**.</span></span>

## <a name="resources"></a><span data-ttu-id="48519-191">Zdroje</span><span class="sxs-lookup"><span data-stu-id="48519-191">Resources</span></span>
<span data-ttu-id="48519-192">[Prostředky](../azure-resource-manager/resource-group-authoring-templates.md#resources) definovat hello různé prostředky, které nainstaluje a nakonfiguruje vaše řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="48519-192">[Resources](../azure-resource-manager/resource-group-authoring-templates.md#resources) define hello different resources that your management solution will install and configure.</span></span>  <span data-ttu-id="48519-193">Bude jím hello největší a těch nejsložitějších část hello šablony.</span><span class="sxs-lookup"><span data-stu-id="48519-193">This will be hello largest and most complex portion of hello template.</span></span>  <span data-ttu-id="48519-194">Můžete získat hello strukturu a úplný popis elementů prostředků v [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md#resources).</span><span class="sxs-lookup"><span data-stu-id="48519-194">You can get hello structure and complete description of resource elements in [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md#resources).</span></span>  <span data-ttu-id="48519-195">Různé prostředky, které se obvykle definují, jsou popsané v další články v této dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="48519-195">Different resources that you will typically define are detailed in other articles in this documentation.</span></span> 


### <a name="dependencies"></a><span data-ttu-id="48519-196">Závislosti</span><span class="sxs-lookup"><span data-stu-id="48519-196">Dependencies</span></span>
<span data-ttu-id="48519-197">Hello **dependsOn** určuje elementy [závislostí](../azure-resource-manager/resource-group-define-dependencies.md) na jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="48519-197">hello **dependsOn** elements specifies a [dependency](../azure-resource-manager/resource-group-define-dependencies.md) on another resource.</span></span>  <span data-ttu-id="48519-198">Při instalaci hello řešení prostředku se nevytvoří, dokud všechny jeho závislé součásti byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="48519-198">When hello solution is installed, a resource is not created until all of its dependencies have been created.</span></span>  <span data-ttu-id="48519-199">Například může být vaše řešení [spuštění sady runbook](operations-management-suite-solutions-resources-automation.md#runbooks) při instalaci pomocí [úlohy prostředků](operations-management-suite-solutions-resources-automation.md#automation-jobs).</span><span class="sxs-lookup"><span data-stu-id="48519-199">For example, your solution might [start a runbook](operations-management-suite-solutions-resources-automation.md#runbooks) when it's installed using a [job resource](operations-management-suite-solutions-resources-automation.md#automation-jobs).</span></span>  <span data-ttu-id="48519-200">prostředek úlohy Hello by být závislá na hello runbook prostředků toomake se, že dané sady runbook hello je vytvořen, než bude vytvořena úloha hello.</span><span class="sxs-lookup"><span data-stu-id="48519-200">hello job resource would be dependent on hello runbook resource toomake sure that hello runbook is created before hello job is created.</span></span>

### <a name="oms-workspace-and-automation-account"></a><span data-ttu-id="48519-201">Pracovní prostor OMS a účet Automation.</span><span class="sxs-lookup"><span data-stu-id="48519-201">OMS workspace and Automation account</span></span>
<span data-ttu-id="48519-202">Vyžaduje řešení pro správu [pracovním prostorem OMS](../log-analytics/log-analytics-manage-access.md) toocontain zobrazení a [účet Automation](../automation/automation-security-overview.md#automation-account-overview) toocontain sady runbook a související prostředky.</span><span class="sxs-lookup"><span data-stu-id="48519-202">Management solutions require an [OMS workspace](../log-analytics/log-analytics-manage-access.md) toocontain views and an [Automation account](../automation/automation-security-overview.md#automation-account-overview) toocontain runbooks and related resources.</span></span>  <span data-ttu-id="48519-203">Toto musí být k dispozici před hello prostředky v řešení hello vytvářejí a nesmí být definována v hello řešení sám sebe.</span><span class="sxs-lookup"><span data-stu-id="48519-203">These must be available before hello resources in hello solution are created and should not be defined in hello solution itself.</span></span>  <span data-ttu-id="48519-204">Hello uživatele bude [zadejte prostoru a účet](operations-management-suite-solutions.md#oms-workspace-and-automation-account) při jejich nasazování svého řešení, ale jako autor hello byste měli zvážit následující body hello.</span><span class="sxs-lookup"><span data-stu-id="48519-204">hello user will [specify a workspace and account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) when they deploy your solution, but as hello author you should consider hello following points.</span></span>

## <a name="solution-resource"></a><span data-ttu-id="48519-205">Řešení prostředků</span><span class="sxs-lookup"><span data-stu-id="48519-205">Solution resource</span></span>
<span data-ttu-id="48519-206">Každé řešení vyžaduje záznam prostředků v hello **prostředky** element, který definuje hello řešení sám sebe.</span><span class="sxs-lookup"><span data-stu-id="48519-206">Each solution requires a resource entry in hello **resources** element that defines hello solution itself.</span></span>  <span data-ttu-id="48519-207">To bude mít typ **Microsoft.OperationsManagement/solutions** a mít hello strukturu.</span><span class="sxs-lookup"><span data-stu-id="48519-207">This will have a type of **Microsoft.OperationsManagement/solutions** and have hello following structure.</span></span> <span data-ttu-id="48519-208">To zahrnuje [standardní parametry](#parameters) a [proměnné](#variables) , které jsou obvykle používanými toodefine vlastnosti hello řešení.</span><span class="sxs-lookup"><span data-stu-id="48519-208">This includes [standard parameters](#parameters) and [variables](#variables) that are typically used toodefine properties of hello solution.</span></span>


    {
      "name": "[concat(variables('Solution').Name, '[' ,parameters('workspacename'), ']')]",
      "location": "[parameters('workspaceRegionId')]",
      "tags": { },
      "type": "Microsoft.OperationsManagement/solutions",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
        <list-of-resources>
      ],
      "properties": {
        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
        "referencedResources": [
            <list-of-referenced-resources>
        ],
        "containedResources": [
            <list-of-contained-resources>
        ]
      },
      "plan": {
        "name": "[concat(variables('Solution').Name, '[' ,parameters('workspaceName'), ']')]",
        "Version": "[variables('Solution').Version]",
        "product": "[variables('ProductName')]",
        "publisher": "[variables('Solution').Publisher]",
        "promotionCode": ""
      }
    }




### <a name="dependencies"></a><span data-ttu-id="48519-209">Závislosti</span><span class="sxs-lookup"><span data-stu-id="48519-209">Dependencies</span></span>
<span data-ttu-id="48519-210">musí mít Hello řešení prostředků [závislostí](../azure-resource-manager/resource-group-define-dependencies.md) na každý jiný prostředek v řešení hello vzhledem k tomu, že potřebují tooexist před vytvořením hello řešení.</span><span class="sxs-lookup"><span data-stu-id="48519-210">hello solution resource must have a [dependency](../azure-resource-manager/resource-group-define-dependencies.md) on every other resource in hello solution since they need tooexist before hello solution can be created.</span></span>  <span data-ttu-id="48519-211">To uděláte tak, že přidáte položku pro všechny prostředky v hello **dependsOn** elementu.</span><span class="sxs-lookup"><span data-stu-id="48519-211">You do this by adding an entry for each resource in hello **dependsOn** element.</span></span>

### <a name="properties"></a><span data-ttu-id="48519-212">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="48519-212">Properties</span></span>
<span data-ttu-id="48519-213">Hello řešení prostředků má hello vlastnosti v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="48519-213">hello solution resource has hello properties in hello following table.</span></span>  <span data-ttu-id="48519-214">To zahrnuje hello prostředky odkazovat a obsažených hello řešení, která definuje, jak se spravuje hello prostředků po instalaci hello řešení.</span><span class="sxs-lookup"><span data-stu-id="48519-214">This includes hello resources referenced and contained by hello solution which defines how hello resource is managed after hello solution is installed.</span></span>  <span data-ttu-id="48519-215">Všechny prostředky v řešení hello by měl být uvedený v buď hello **referencedResources** nebo hello **containedResources** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="48519-215">Each resource in hello solution should be listed in either hello **referencedResources** or hello **containedResources** property.</span></span>

| <span data-ttu-id="48519-216">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="48519-216">Property</span></span> | <span data-ttu-id="48519-217">Popis</span><span class="sxs-lookup"><span data-stu-id="48519-217">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="48519-218">workspaceResourceId</span><span class="sxs-lookup"><span data-stu-id="48519-218">workspaceResourceId</span></span> |<span data-ttu-id="48519-219">ID pracovního prostoru analýzy protokolů hello v podobě hello  *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<název pracovního prostoru\>*.</span><span class="sxs-lookup"><span data-stu-id="48519-219">ID of hello Log Analytics workspace in hello form *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Workspace Name\>*.</span></span> |
| <span data-ttu-id="48519-220">referencedResources</span><span class="sxs-lookup"><span data-stu-id="48519-220">referencedResources</span></span> |<span data-ttu-id="48519-221">Seznam prostředků v hello řešení, které by se neměly odebírat, když dojde k odebrání hello řešení.</span><span class="sxs-lookup"><span data-stu-id="48519-221">List of resources in hello solution that should not be removed when hello solution is removed.</span></span> |
| <span data-ttu-id="48519-222">containedResources</span><span class="sxs-lookup"><span data-stu-id="48519-222">containedResources</span></span> |<span data-ttu-id="48519-223">Seznam prostředků v hello řešení, které byste měli odebrat, když dojde k odebrání hello řešení.</span><span class="sxs-lookup"><span data-stu-id="48519-223">List of resources in hello solution that should be removed when hello solution is removed.</span></span> |

<span data-ttu-id="48519-224">výše uvedený příklad Hello je řešení s sady runbook, plán a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="48519-224">hello example  above is for a solution with a runbook, a schedule, and view.</span></span>  <span data-ttu-id="48519-225">plán Hello a sady runbook jsou *odkazované* v hello **vlastnosti** element tak nejsou odebrány při odebrání hello řešení.</span><span class="sxs-lookup"><span data-stu-id="48519-225">hello schedule and runbook are *referenced* in hello  **properties**  element so they are not removed when hello solution is removed.</span></span>  <span data-ttu-id="48519-226">zobrazení Hello *obsažené* proto je odebrána, když dojde k odebrání hello řešení.</span><span class="sxs-lookup"><span data-stu-id="48519-226">hello view is *contained* so it is removed when hello solution is removed.</span></span>

### <a name="plan"></a><span data-ttu-id="48519-227">Plánování</span><span class="sxs-lookup"><span data-stu-id="48519-227">Plan</span></span>
<span data-ttu-id="48519-228">Hello **plán** entity hello řešení prostředku má hello vlastnosti v hello následující tabulka.</span><span class="sxs-lookup"><span data-stu-id="48519-228">hello **plan** entity of hello solution resource has hello properties in hello following table.</span></span>

| <span data-ttu-id="48519-229">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="48519-229">Property</span></span> | <span data-ttu-id="48519-230">Popis</span><span class="sxs-lookup"><span data-stu-id="48519-230">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="48519-231">jméno</span><span class="sxs-lookup"><span data-stu-id="48519-231">name</span></span> |<span data-ttu-id="48519-232">Název řešení hello.</span><span class="sxs-lookup"><span data-stu-id="48519-232">Name of hello solution.</span></span> |
| <span data-ttu-id="48519-233">Verze</span><span class="sxs-lookup"><span data-stu-id="48519-233">version</span></span> |<span data-ttu-id="48519-234">Verze hello řešení, počítáno od autora hello.</span><span class="sxs-lookup"><span data-stu-id="48519-234">Version of hello solution as determined by hello author.</span></span> |
| <span data-ttu-id="48519-235">Produktu</span><span class="sxs-lookup"><span data-stu-id="48519-235">product</span></span> |<span data-ttu-id="48519-236">Jedinečné řetězce tooidentify hello řešení.</span><span class="sxs-lookup"><span data-stu-id="48519-236">Unique string tooidentify hello solution.</span></span> |
| <span data-ttu-id="48519-237">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="48519-237">publisher</span></span> |<span data-ttu-id="48519-238">Vydavatel hello řešení.</span><span class="sxs-lookup"><span data-stu-id="48519-238">Publisher of hello solution.</span></span> |



## <a name="sample"></a><span data-ttu-id="48519-239">Ukázka</span><span class="sxs-lookup"><span data-stu-id="48519-239">Sample</span></span>
<span data-ttu-id="48519-240">Ukázky soubory řešení s prostředek řešení můžete zobrazit v hello následující umístění.</span><span class="sxs-lookup"><span data-stu-id="48519-240">You can view samples of solution files with a solution resource at hello following locations.</span></span>

- [<span data-ttu-id="48519-241">Prostředky služby Automation</span><span class="sxs-lookup"><span data-stu-id="48519-241">Automation resources</span></span>](operations-management-suite-solutions-resources-automation.md#sample)
- [<span data-ttu-id="48519-242">Hledání a výstraha prostředky</span><span class="sxs-lookup"><span data-stu-id="48519-242">Search and alert resources</span></span>](operations-management-suite-solutions-resources-searches-alerts.md#sample)


## <a name="next-steps"></a><span data-ttu-id="48519-243">Další kroky</span><span class="sxs-lookup"><span data-stu-id="48519-243">Next steps</span></span>
* <span data-ttu-id="48519-244">[Přidat uložená hledání a výstrahy](operations-management-suite-solutions-resources-searches-alerts.md) tooyour řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="48519-244">[Add saved searches and alerts](operations-management-suite-solutions-resources-searches-alerts.md) tooyour management solution.</span></span>
* <span data-ttu-id="48519-245">[Přidání zobrazení](operations-management-suite-solutions-resources-views.md) tooyour řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="48519-245">[Add views](operations-management-suite-solutions-resources-views.md) tooyour management solution.</span></span>
* <span data-ttu-id="48519-246">[Přidat sady runbook a dalším prostředkům Automation](operations-management-suite-solutions-resources-automation.md) tooyour řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="48519-246">[Add runbooks and other Automation resources](operations-management-suite-solutions-resources-automation.md) tooyour management solution.</span></span>
* <span data-ttu-id="48519-247">Další podrobnosti o hello [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="48519-247">Learn hello details of [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="48519-248">Hledání [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates) ukázky různých šablonách Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="48519-248">Search [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates) for samples of different Resource Manager templates.</span></span>
