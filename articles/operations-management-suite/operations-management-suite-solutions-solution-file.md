---
title: "Vytváření řešení pro správu v Operations Management Suite (OMS) | Microsoft Docs"
description: "Řešení pro správu rozšířit funkce služby Operations Management Suite (OMS) tím, že poskytuje scénářů zabalené správy, které zákazníci mohou přidat do jejich pracovním prostorem OMS.  Tento článek poskytuje podrobné informace o tom, jak můžete vytvořit řešení správy, který se má použít ve svém vlastním prostředí nebo k dispozici pro vaše zákazníky."
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
ms.openlocfilehash: ee3462c13101d18921dc488b08c79e1e4e02ff3a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="creating-a-management-solution-file-in-operations-management-suite-oms-preview"></a><span data-ttu-id="1631e-104">Vytvoření souboru řešení pro správu v Operations Management Suite (OMS) (Preview)</span><span class="sxs-lookup"><span data-stu-id="1631e-104">Creating a management solution file in Operations Management Suite (OMS) (Preview)</span></span>
> [!NOTE]
> <span data-ttu-id="1631e-105">Toto je předběžná dokumentace pro vytváření řešení pro správu v OMS, které jsou aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="1631e-105">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="1631e-106">Žádné schéma popsané níže se mohou změnit.</span><span class="sxs-lookup"><span data-stu-id="1631e-106">Any schema described below is subject to change.</span></span>  

<span data-ttu-id="1631e-107">Řešení pro správu v Operations Management Suite (OMS) jsou implementované jako [šablony Resource Manageru](../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="1631e-107">Management solutions in Operations Management Suite (OMS) are implemented as [Resource Manager templates](../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>  <span data-ttu-id="1631e-108">Hlavní úloha naučit vytvářet řešení pro správu je učení postup [vytvořit šablonu](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1631e-108">The main task in learning how to author management solutions is learning how to [author a template](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>  <span data-ttu-id="1631e-109">Tento článek obsahuje jedinečné Podrobnosti šablony použité pro řešení a jak nakonfigurovat prostředky typické řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-109">This article provides unique details of templates used for solutions and how to configure typical solution resources.</span></span>


## <a name="tools"></a><span data-ttu-id="1631e-110">Nástroje</span><span class="sxs-lookup"><span data-stu-id="1631e-110">Tools</span></span>

<span data-ttu-id="1631e-111">Libovolného textového editoru můžete pracovat se soubory řešení, ale doporučujeme využívat funkce uvedené v sadě Visual Studio nebo Visual Studio Code, jak je popsáno v následujících článcích.</span><span class="sxs-lookup"><span data-stu-id="1631e-111">You can use any text editor to work with solution files, but we recommend leveraging the features provided in Visual Studio or Visual Studio Code as described in the following articles.</span></span>

- [<span data-ttu-id="1631e-112">Vytvoření a nasazení skupin prostředků Azure pomocí sady Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1631e-112">Creating and deploying Azure resource groups through Visual Studio</span></span>](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md)
- [<span data-ttu-id="1631e-113">Práce s šablony Azure Resource Manageru v sadě Visual Studio kódu</span><span class="sxs-lookup"><span data-stu-id="1631e-113">Working with Azure Resource Manager Templates in Visual Studio Code</span></span>](../azure-resource-manager/resource-manager-vs-code.md)




## <a name="structure"></a><span data-ttu-id="1631e-114">Struktura</span><span class="sxs-lookup"><span data-stu-id="1631e-114">Structure</span></span>
<span data-ttu-id="1631e-115">Základní struktura soubor řešení správy je stejné jako [šablony Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md#template-format) tedy následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="1631e-115">The basic structure of a management solution file is the same as a [Resource Manager Template](../azure-resource-manager/resource-group-authoring-templates.md#template-format) which is as follows.</span></span>  <span data-ttu-id="1631e-116">Každý z níže uvedených částech popisuje elementy nejvyšší úrovně a a jejich obsah v řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-116">Each of the sections below describes the top level elements and and their contents in a solution.</span></span>  

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "1.0",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

## <a name="parameters"></a><span data-ttu-id="1631e-117">Parametry</span><span class="sxs-lookup"><span data-stu-id="1631e-117">Parameters</span></span>
<span data-ttu-id="1631e-118">[Parametry](../azure-resource-manager/resource-group-authoring-templates.md#parameters) jsou hodnoty, které požadujete od uživatele při instalaci řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="1631e-118">[Parameters](../azure-resource-manager/resource-group-authoring-templates.md#parameters) are values that you require from the user when they install the management solution.</span></span>  <span data-ttu-id="1631e-119">Jsou standardní parametry, které budou mít všechna řešení a podle potřeby můžete přidat další parametry pro vaše konkrétní řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-119">There are standard parameters that all solutions will have, and you can add additional parameters as required for your particular solution.</span></span>  <span data-ttu-id="1631e-120">Jak budou uživatelé zadali hodnoty parametrů při instalaci řešení bude záviset na konkrétní parametr a jak se instaluje řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-120">How users will provide parameter values when they install your solution will depend on the particular parameter and how the solution is being installed.</span></span>

<span data-ttu-id="1631e-121">Když uživatel nainstaluje řešení pro správu prostřednictvím [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) nebo [šablony Azure QuickStart](operations-management-suite-solutions.md#finding-and-installing-management-solutions) se výzva k výběru [pracovním prostorem OMS a automatizace účtu](operations-management-suite-solutions.md#oms-workspace-and-automation-account).</span><span class="sxs-lookup"><span data-stu-id="1631e-121">When a user installs your management solution through the [Azure Marketplace](operations-management-suite-solutions.md#finding-and-installing-management-solutions) or [Azure QuickStart templates](operations-management-suite-solutions.md#finding-and-installing-management-solutions) they are prompted to select an [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account).</span></span>  <span data-ttu-id="1631e-122">Ty se používají k naplnění hodnoty jednotlivých standardní parametry.</span><span class="sxs-lookup"><span data-stu-id="1631e-122">These are used to populate the values of each of the standard parameters.</span></span>  <span data-ttu-id="1631e-123">Uživatel nebude vyzván k přímo zadat hodnoty pro standardní parametry, ale bude vyzván k zadání hodnot pro žádné další parametry.</span><span class="sxs-lookup"><span data-stu-id="1631e-123">The user is not prompted to directly provide values for the standard parameters, but they are prompted to provide values for any additional parameters.</span></span>

<span data-ttu-id="1631e-124">Když uživatel nainstaluje řešení [jinou metodu](operations-management-suite-solutions.md#finding-and-installing-management-solutions), musí zadat hodnotu pro všechny standardní parametry a všech dalších parametrů.</span><span class="sxs-lookup"><span data-stu-id="1631e-124">When the user installs your solution [another method](operations-management-suite-solutions.md#finding-and-installing-management-solutions), they must provide a value for all standard parameters and all additional parameters.</span></span>

<span data-ttu-id="1631e-125">Ukázka parametrů jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="1631e-125">A sample parameter is shown below.</span></span>  

    "startTime": {
        "type": "string",
        "metadata": {
            "description": "Enter time for starting VMs by resource group.",
            "control": "datetime",
            "category": "Schedule"
        }

<span data-ttu-id="1631e-126">Následující tabulka popisuje atributy parametru.</span><span class="sxs-lookup"><span data-stu-id="1631e-126">The following table describes the attributes of a parameter.</span></span>

| <span data-ttu-id="1631e-127">Atribut</span><span class="sxs-lookup"><span data-stu-id="1631e-127">Attribute</span></span> | <span data-ttu-id="1631e-128">Popis</span><span class="sxs-lookup"><span data-stu-id="1631e-128">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="1631e-129">type</span><span class="sxs-lookup"><span data-stu-id="1631e-129">type</span></span> |<span data-ttu-id="1631e-130">Datový typ pro parametr.</span><span class="sxs-lookup"><span data-stu-id="1631e-130">Data type for the parameter.</span></span> <span data-ttu-id="1631e-131">Vstupní ovládací prvek zobrazí pro uživatele, závisí na typu dat.</span><span class="sxs-lookup"><span data-stu-id="1631e-131">The input control displayed for the user depends on the data type.</span></span><br><br><span data-ttu-id="1631e-132">BOOL – rozevíracího seznamu</span><span class="sxs-lookup"><span data-stu-id="1631e-132">bool - Drop down box</span></span><br><span data-ttu-id="1631e-133">String – textové pole</span><span class="sxs-lookup"><span data-stu-id="1631e-133">string - Text box</span></span><br><span data-ttu-id="1631e-134">int – textové pole</span><span class="sxs-lookup"><span data-stu-id="1631e-134">int - Text box</span></span><br><span data-ttu-id="1631e-135">SecureString - pole pro heslo</span><span class="sxs-lookup"><span data-stu-id="1631e-135">securestring - Password field</span></span><br> |
| <span data-ttu-id="1631e-136">category</span><span class="sxs-lookup"><span data-stu-id="1631e-136">category</span></span> |<span data-ttu-id="1631e-137">Volitelné kategorie pro parametr.</span><span class="sxs-lookup"><span data-stu-id="1631e-137">Optional category for the parameter.</span></span>  <span data-ttu-id="1631e-138">Parametry ve stejné kategorii jsou seskupeny dohromady.</span><span class="sxs-lookup"><span data-stu-id="1631e-138">Parameters in the same category are grouped together.</span></span> |
| <span data-ttu-id="1631e-139">Ovládací prvek</span><span class="sxs-lookup"><span data-stu-id="1631e-139">control</span></span> |<span data-ttu-id="1631e-140">Další funkce pro parametry řetězce.</span><span class="sxs-lookup"><span data-stu-id="1631e-140">Additional functionality for string parameters.</span></span><br><br><span data-ttu-id="1631e-141">Zobrazí se datum a čas - datum a čas řízení.</span><span class="sxs-lookup"><span data-stu-id="1631e-141">datetime - Datetime control is displayed.</span></span><br><span data-ttu-id="1631e-142">identifikátor GUID – hodnota identifikátoru Guid je generován automaticky, a parametr nezobrazuje.</span><span class="sxs-lookup"><span data-stu-id="1631e-142">guid - Guid value is automatically generated, and the parameter is not displayed.</span></span> |
| <span data-ttu-id="1631e-143">Popis</span><span class="sxs-lookup"><span data-stu-id="1631e-143">description</span></span> |<span data-ttu-id="1631e-144">Volitelný popis pro parametr.</span><span class="sxs-lookup"><span data-stu-id="1631e-144">Optional description for the parameter.</span></span>  <span data-ttu-id="1631e-145">Zobrazí v bublinách informace vedle parametru.</span><span class="sxs-lookup"><span data-stu-id="1631e-145">Displayed in an information balloon next to the parameter.</span></span> |

### <a name="standard-parameters"></a><span data-ttu-id="1631e-146">Standardní parametry</span><span class="sxs-lookup"><span data-stu-id="1631e-146">Standard parameters</span></span>
<span data-ttu-id="1631e-147">Následující tabulka uvádí standardní parametry pro všechna řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="1631e-147">The following table lists the standard parameters for all management solutions.</span></span>  <span data-ttu-id="1631e-148">Tyto hodnoty jsou naplněny pro uživatele místo dotaz na ně při řešení z Azure Marketplace nebo šablony rychlý start.</span><span class="sxs-lookup"><span data-stu-id="1631e-148">These values are populated for the user instead of prompting for them when your solution is installed from the Azure Marketplace or Quickstart templates.</span></span>  <span data-ttu-id="1631e-149">Uživatel musí poskytnout hodnoty pro ně, pokud řešení se instaluje s jinou metodu.</span><span class="sxs-lookup"><span data-stu-id="1631e-149">The user must provide values for them if the solution is installed with another method.</span></span>

> [!NOTE]
> <span data-ttu-id="1631e-150">Uživatelské rozhraní v Azure Marketplace a šablony rychlý start očekává názvy parametrů v tabulce.</span><span class="sxs-lookup"><span data-stu-id="1631e-150">The user interface in the Azure Marketplace and Quickstart templates is expecting the parameter names in the table.</span></span>  <span data-ttu-id="1631e-151">Pokud používáte jiný parametr názvy pak uživatele vyzváni k jejich a nebude se automaticky vyplní.</span><span class="sxs-lookup"><span data-stu-id="1631e-151">If you use different parameter names then the user will be prompted for them, and they will not be automatically populated.</span></span>
>
>

| <span data-ttu-id="1631e-152">Parametr</span><span class="sxs-lookup"><span data-stu-id="1631e-152">Parameter</span></span> | <span data-ttu-id="1631e-153">Typ</span><span class="sxs-lookup"><span data-stu-id="1631e-153">Type</span></span> | <span data-ttu-id="1631e-154">Popis</span><span class="sxs-lookup"><span data-stu-id="1631e-154">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="1631e-155">název účtu</span><span class="sxs-lookup"><span data-stu-id="1631e-155">accountName</span></span> |<span data-ttu-id="1631e-156">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1631e-156">string</span></span> |<span data-ttu-id="1631e-157">Název účtu Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="1631e-157">Azure Automation account name.</span></span> |
| <span data-ttu-id="1631e-158">pricingTier</span><span class="sxs-lookup"><span data-stu-id="1631e-158">pricingTier</span></span> |<span data-ttu-id="1631e-159">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1631e-159">string</span></span> |<span data-ttu-id="1631e-160">Cenová úroveň pracovní prostor analýzy protokolů a účet Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="1631e-160">Pricing tier of both Log Analytics workspace and Azure Automation account.</span></span> |
| <span data-ttu-id="1631e-161">regionId</span><span class="sxs-lookup"><span data-stu-id="1631e-161">regionId</span></span> |<span data-ttu-id="1631e-162">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1631e-162">string</span></span> |<span data-ttu-id="1631e-163">Oblast účet Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="1631e-163">Region of the Azure Automation account.</span></span> |
| <span data-ttu-id="1631e-164">Název řešení SolutionName</span><span class="sxs-lookup"><span data-stu-id="1631e-164">solutionName</span></span> |<span data-ttu-id="1631e-165">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1631e-165">string</span></span> |<span data-ttu-id="1631e-166">Název řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-166">Name of the solution.</span></span>  <span data-ttu-id="1631e-167">Pokud nasazujete řešení prostřednictvím šablony rychlý start, pak byste měli definovat název řešení solutionName jako parametr, můžete definovat místo nutnosti uživateli zadat jeden řetězec.</span><span class="sxs-lookup"><span data-stu-id="1631e-167">If you are deploying your solution through Quickstart templates, then you should define solutionName as a parameter so you can define a string instead requiring the user to specify one.</span></span> |
| <span data-ttu-id="1631e-168">workspaceName</span><span class="sxs-lookup"><span data-stu-id="1631e-168">workspaceName</span></span> |<span data-ttu-id="1631e-169">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1631e-169">string</span></span> |<span data-ttu-id="1631e-170">Název pracovního prostoru analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="1631e-170">Log Analytics workspace name.</span></span> |
| <span data-ttu-id="1631e-171">workspaceRegionId</span><span class="sxs-lookup"><span data-stu-id="1631e-171">workspaceRegionId</span></span> |<span data-ttu-id="1631e-172">Řetězec</span><span class="sxs-lookup"><span data-stu-id="1631e-172">string</span></span> |<span data-ttu-id="1631e-173">Oblast pracovního prostoru analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="1631e-173">Region of the Log Analytics workspace.</span></span> |


<span data-ttu-id="1631e-174">Následuje strukturu standardní parametry, které můžete zkopírovat a vložit do souboru řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-174">Following is the structure of the standard parameters that you can copy and paste into your solution file.</span></span>  

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
                   "description": "Region of the Log Analytics workspace"
            }
        },
        "regionId": {
            "type": "string",
            "metadata": {
                "description": "Region of the Azure Automation account"
            }
        },
        "pricingTier": {
            "type": "string",
            "metadata": {
                "description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
        }
    }


<span data-ttu-id="1631e-175">Odkazujete na hodnoty parametrů v další prvky řešení se syntaxí **parametry (název parametru)**.</span><span class="sxs-lookup"><span data-stu-id="1631e-175">You refer to parameter values in other elements of the solution with the syntax **parameters('parameter name')**.</span></span>  <span data-ttu-id="1631e-176">Například pokud chcete získat přístup k název pracovního prostoru, použijte **parameters('workspaceName')**</span><span class="sxs-lookup"><span data-stu-id="1631e-176">For example, to access the workspace name, you would use **parameters('workspaceName')**</span></span>

## <a name="variables"></a><span data-ttu-id="1631e-177">Proměnné</span><span class="sxs-lookup"><span data-stu-id="1631e-177">Variables</span></span>
<span data-ttu-id="1631e-178">[Proměnné](../azure-resource-manager/resource-group-authoring-templates.md#variables) jsou hodnoty, které budete používat ve zbývající části řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="1631e-178">[Variables](../azure-resource-manager/resource-group-authoring-templates.md#variables) are values that you will use in the rest of the management solution.</span></span>  <span data-ttu-id="1631e-179">Tyto hodnoty nejsou zveřejněné má uživatel instalující řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-179">These values are not exposed to the user installing the solution.</span></span>  <span data-ttu-id="1631e-180">Ty jsou určené Autor poskytnout na jednom místě, kde můžete spravovat hodnoty, které se dají použít více než jednou v celé řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-180">They are intended to provide the author with a single location where they can manage values that may be used multiple times throughout the solution.</span></span> <span data-ttu-id="1631e-181">Byste měli umístit všechny hodnoty specifické pro vaše řešení v proměnné a pevné kódování v **prostředky** elementu.</span><span class="sxs-lookup"><span data-stu-id="1631e-181">You should put any values specific to your solution in variables as opposed to hard coding them in the **resources** element.</span></span>  <span data-ttu-id="1631e-182">To usnadňuje kód srozumitelnější a umožňuje snadno změnit tyto hodnoty v novějších verzích.</span><span class="sxs-lookup"><span data-stu-id="1631e-182">This makes the code more readable and allows you to easily change these values in later versions.</span></span>

<span data-ttu-id="1631e-183">Následuje příklad **proměnné** element s typické parametry použité v řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-183">Following is an example of a **variables** element with typical parameters used in solutions.</span></span>

    "variables": {
        "SolutionVersion": "1.1",
        "SolutionPublisher": "Contoso",
        "SolutionName": "My Solution",
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

<span data-ttu-id="1631e-184">Odkazujete na hodnoty proměnné prostřednictvím řešení se syntaxí **proměnné ('název proměnné')**.</span><span class="sxs-lookup"><span data-stu-id="1631e-184">You refer to variable values through the solution with the syntax **variables('variable name')**.</span></span>  <span data-ttu-id="1631e-185">Například pokud chcete přístup k proměnné Název řešení SolutionName, použijte **variables('SolutionName')**.</span><span class="sxs-lookup"><span data-stu-id="1631e-185">For example, to access the SolutionName variable, you would use **variables('SolutionName')**.</span></span>

<span data-ttu-id="1631e-186">Můžete také definovat komplexní proměnné tohoto několik sad hodnot.</span><span class="sxs-lookup"><span data-stu-id="1631e-186">You can also define complex variables that multiple sets of values.</span></span>  <span data-ttu-id="1631e-187">Tyto jsou zvláště užitečné při řešení pro správu, které definujete více vlastností pro různé typy prostředků.</span><span class="sxs-lookup"><span data-stu-id="1631e-187">These are particularly useful in management solutions where you are defining multiple properties for different types of resources.</span></span>  <span data-ttu-id="1631e-188">Například může změnit strukturu proměnné řešení uvedené výše takto.</span><span class="sxs-lookup"><span data-stu-id="1631e-188">For example, you could restructure the solution variables shown above to the following.</span></span>

    "variables": {
        "Solution": {
          "Version": "1.1",
          "Publisher": "Contoso",
          "Name": "My Solution"
        },
        "LogAnalyticsApiVersion": "2015-11-01-preview",
        "AutomationApiVersion": "2015-10-31"
    },

<span data-ttu-id="1631e-189">V takovém případě je odkazovat na hodnoty proměnné prostřednictvím řešení se syntaxí **variables('variable name').property**.</span><span class="sxs-lookup"><span data-stu-id="1631e-189">In this case, you refer to variable values through the solution with the syntax **variables('variable name').property**.</span></span>  <span data-ttu-id="1631e-190">Například pokud chcete přístup k proměnné Název řešení, použijte **variables('Solution'). Název**.</span><span class="sxs-lookup"><span data-stu-id="1631e-190">For example, to access the Solution Name variable, you would use **variables('Solution').Name**.</span></span>

## <a name="resources"></a><span data-ttu-id="1631e-191">Zdroje</span><span class="sxs-lookup"><span data-stu-id="1631e-191">Resources</span></span>
<span data-ttu-id="1631e-192">[Prostředky](../azure-resource-manager/resource-group-authoring-templates.md#resources) definovat různé prostředky, které nainstaluje a nakonfiguruje vaše řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="1631e-192">[Resources](../azure-resource-manager/resource-group-authoring-templates.md#resources) define the different resources that your management solution will install and configure.</span></span>  <span data-ttu-id="1631e-193">To bude největší a těch nejsložitějších část šablony.</span><span class="sxs-lookup"><span data-stu-id="1631e-193">This will be the largest and most complex portion of the template.</span></span>  <span data-ttu-id="1631e-194">Můžete získat strukturu a úplný popis elementů prostředků v [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md#resources).</span><span class="sxs-lookup"><span data-stu-id="1631e-194">You can get the structure and complete description of resource elements in [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md#resources).</span></span>  <span data-ttu-id="1631e-195">Různé prostředky, které se obvykle definují, jsou popsané v další články v této dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="1631e-195">Different resources that you will typically define are detailed in other articles in this documentation.</span></span> 


### <a name="dependencies"></a><span data-ttu-id="1631e-196">Závislosti</span><span class="sxs-lookup"><span data-stu-id="1631e-196">Dependencies</span></span>
<span data-ttu-id="1631e-197">**DependsOn** určuje elementy [závislostí](../azure-resource-manager/resource-group-define-dependencies.md) na jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="1631e-197">The **dependsOn** elements specifies a [dependency](../azure-resource-manager/resource-group-define-dependencies.md) on another resource.</span></span>  <span data-ttu-id="1631e-198">Při instalaci řešení prostředku se nevytvoří, dokud všechny jeho závislé součásti byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="1631e-198">When the solution is installed, a resource is not created until all of its dependencies have been created.</span></span>  <span data-ttu-id="1631e-199">Například může být vaše řešení [spuštění sady runbook](operations-management-suite-solutions-resources-automation.md#runbooks) při instalaci pomocí [úlohy prostředků](operations-management-suite-solutions-resources-automation.md#automation-jobs).</span><span class="sxs-lookup"><span data-stu-id="1631e-199">For example, your solution might [start a runbook](operations-management-suite-solutions-resources-automation.md#runbooks) when it's installed using a [job resource](operations-management-suite-solutions-resources-automation.md#automation-jobs).</span></span>  <span data-ttu-id="1631e-200">Prostředek úlohy by být závislý na prostředku sady runbook, abyste měli jistotu, že je sada runbook vytvořena předtím, než se vytvoří úloha.</span><span class="sxs-lookup"><span data-stu-id="1631e-200">The job resource would be dependent on the runbook resource to make sure that the runbook is created before the job is created.</span></span>

### <a name="oms-workspace-and-automation-account"></a><span data-ttu-id="1631e-201">Pracovní prostor OMS a účet Automation.</span><span class="sxs-lookup"><span data-stu-id="1631e-201">OMS workspace and Automation account</span></span>
<span data-ttu-id="1631e-202">Vyžaduje řešení pro správu [pracovním prostorem OMS](../log-analytics/log-analytics-manage-access.md) tak, aby obsahovala zobrazení a [účet Automation](../automation/automation-security-overview.md#automation-account-overview) tak, aby obsahovala sady runbook a související prostředky.</span><span class="sxs-lookup"><span data-stu-id="1631e-202">Management solutions require an [OMS workspace](../log-analytics/log-analytics-manage-access.md) to contain views and an [Automation account](../automation/automation-security-overview.md#automation-account-overview) to contain runbooks and related resources.</span></span>  <span data-ttu-id="1631e-203">Musí mít k dispozici před prostředky v řešení jsou vytvořeny a nesmí být definována v řešení sám sebe.</span><span class="sxs-lookup"><span data-stu-id="1631e-203">These must be available before the resources in the solution are created and should not be defined in the solution itself.</span></span>  <span data-ttu-id="1631e-204">Uživatel bude [zadejte prostoru a účet](operations-management-suite-solutions.md#oms-workspace-and-automation-account) při jejich nasazování svého řešení, ale jako autor byste měli zvážit následující body.</span><span class="sxs-lookup"><span data-stu-id="1631e-204">The user will [specify a workspace and account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) when they deploy your solution, but as the author you should consider the following points.</span></span>

## <a name="solution-resource"></a><span data-ttu-id="1631e-205">Řešení prostředků</span><span class="sxs-lookup"><span data-stu-id="1631e-205">Solution resource</span></span>
<span data-ttu-id="1631e-206">Každé řešení vyžaduje záznam prostředků v **prostředky** element, který definuje řešení sám sebe.</span><span class="sxs-lookup"><span data-stu-id="1631e-206">Each solution requires a resource entry in the **resources** element that defines the solution itself.</span></span>  <span data-ttu-id="1631e-207">To bude mít typ **Microsoft.OperationsManagement/solutions** a mít následující strukturu.</span><span class="sxs-lookup"><span data-stu-id="1631e-207">This will have a type of **Microsoft.OperationsManagement/solutions** and have the following structure.</span></span> <span data-ttu-id="1631e-208">To zahrnuje [standardní parametry](#parameters) a [proměnné](#variables) , jsou obvykle používány k definování vlastností řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-208">This includes [standard parameters](#parameters) and [variables](#variables) that are typically used to define properties of the solution.</span></span>


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




### <a name="dependencies"></a><span data-ttu-id="1631e-209">Závislosti</span><span class="sxs-lookup"><span data-stu-id="1631e-209">Dependencies</span></span>
<span data-ttu-id="1631e-210">Musí mít prostředek řešení [závislostí](../azure-resource-manager/resource-group-define-dependencies.md) na každý jiný prostředek v řešení vzhledem k tomu, že potřebují existovat, aby bylo možné vytvořit řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-210">The solution resource must have a [dependency](../azure-resource-manager/resource-group-define-dependencies.md) on every other resource in the solution since they need to exist before the solution can be created.</span></span>  <span data-ttu-id="1631e-211">To uděláte tak, že přidáte položku pro každý zdroj v **dependsOn** elementu.</span><span class="sxs-lookup"><span data-stu-id="1631e-211">You do this by adding an entry for each resource in the **dependsOn** element.</span></span>

### <a name="properties"></a><span data-ttu-id="1631e-212">Vlastnosti</span><span class="sxs-lookup"><span data-stu-id="1631e-212">Properties</span></span>
<span data-ttu-id="1631e-213">Řešení prostředek má vlastnosti v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="1631e-213">The solution resource has the properties in the following table.</span></span>  <span data-ttu-id="1631e-214">To zahrnuje prostředky odkazuje a obsažený v řešení, která definuje, jak se spravuje prostředku po instalaci řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-214">This includes the resources referenced and contained by the solution which defines how the resource is managed after the solution is installed.</span></span>  <span data-ttu-id="1631e-215">Všechny prostředky v řešení by měl být uveden buď **referencedResources** nebo **containedResources** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="1631e-215">Each resource in the solution should be listed in either the **referencedResources** or the **containedResources** property.</span></span>

| <span data-ttu-id="1631e-216">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="1631e-216">Property</span></span> | <span data-ttu-id="1631e-217">Popis</span><span class="sxs-lookup"><span data-stu-id="1631e-217">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="1631e-218">workspaceResourceId</span><span class="sxs-lookup"><span data-stu-id="1631e-218">workspaceResourceId</span></span> |<span data-ttu-id="1631e-219">ID pracovního prostoru analýzy protokolů ve formě  *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<název pracovního prostoru\>*.</span><span class="sxs-lookup"><span data-stu-id="1631e-219">ID of the Log Analytics workspace in the form *<Resource Group ID>/providers/Microsoft.OperationalInsights/workspaces/\<Workspace Name\>*.</span></span> |
| <span data-ttu-id="1631e-220">referencedResources</span><span class="sxs-lookup"><span data-stu-id="1631e-220">referencedResources</span></span> |<span data-ttu-id="1631e-221">Seznam prostředků v řešení, které by se neměly odebírat, když dojde k odebrání řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-221">List of resources in the solution that should not be removed when the solution is removed.</span></span> |
| <span data-ttu-id="1631e-222">containedResources</span><span class="sxs-lookup"><span data-stu-id="1631e-222">containedResources</span></span> |<span data-ttu-id="1631e-223">Seznam prostředků v řešení, které byste měli odebrat, když dojde k odebrání řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-223">List of resources in the solution that should be removed when the solution is removed.</span></span> |

<span data-ttu-id="1631e-224">V předchozím příkladu je řešení s sady runbook, plán a zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1631e-224">The example  above is for a solution with a runbook, a schedule, and view.</span></span>  <span data-ttu-id="1631e-225">Plán a sady runbook jsou *odkazované* v **vlastnosti** element tak nejsou odebrány při odebrání řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-225">The schedule and runbook are *referenced* in the  **properties**  element so they are not removed when the solution is removed.</span></span>  <span data-ttu-id="1631e-226">Zobrazení je *obsažené* proto je odebrána, když dojde k odebrání řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-226">The view is *contained* so it is removed when the solution is removed.</span></span>

### <a name="plan"></a><span data-ttu-id="1631e-227">Plánování</span><span class="sxs-lookup"><span data-stu-id="1631e-227">Plan</span></span>
<span data-ttu-id="1631e-228">**Plán** entity řešení prostředku má vlastnosti v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="1631e-228">The **plan** entity of the solution resource has the properties in the following table.</span></span>

| <span data-ttu-id="1631e-229">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="1631e-229">Property</span></span> | <span data-ttu-id="1631e-230">Popis</span><span class="sxs-lookup"><span data-stu-id="1631e-230">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="1631e-231">jméno</span><span class="sxs-lookup"><span data-stu-id="1631e-231">name</span></span> |<span data-ttu-id="1631e-232">Název řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-232">Name of the solution.</span></span> |
| <span data-ttu-id="1631e-233">Verze</span><span class="sxs-lookup"><span data-stu-id="1631e-233">version</span></span> |<span data-ttu-id="1631e-234">Verze řešení, počítáno od autora.</span><span class="sxs-lookup"><span data-stu-id="1631e-234">Version of the solution as determined by the author.</span></span> |
| <span data-ttu-id="1631e-235">Produktu</span><span class="sxs-lookup"><span data-stu-id="1631e-235">product</span></span> |<span data-ttu-id="1631e-236">Jedinečný řetězec k identifikaci řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-236">Unique string to identify the solution.</span></span> |
| <span data-ttu-id="1631e-237">Vydavatele</span><span class="sxs-lookup"><span data-stu-id="1631e-237">publisher</span></span> |<span data-ttu-id="1631e-238">Vydavatel řešení.</span><span class="sxs-lookup"><span data-stu-id="1631e-238">Publisher of the solution.</span></span> |



## <a name="sample"></a><span data-ttu-id="1631e-239">Ukázka</span><span class="sxs-lookup"><span data-stu-id="1631e-239">Sample</span></span>
<span data-ttu-id="1631e-240">Můžete zobrazit ukázky soubory řešení s prostředek řešení v následujících umístěních.</span><span class="sxs-lookup"><span data-stu-id="1631e-240">You can view samples of solution files with a solution resource at the following locations.</span></span>

- [<span data-ttu-id="1631e-241">Prostředky služby Automation</span><span class="sxs-lookup"><span data-stu-id="1631e-241">Automation resources</span></span>](operations-management-suite-solutions-resources-automation.md#sample)
- [<span data-ttu-id="1631e-242">Hledání a výstraha prostředky</span><span class="sxs-lookup"><span data-stu-id="1631e-242">Search and alert resources</span></span>](operations-management-suite-solutions-resources-searches-alerts.md#sample)


## <a name="next-steps"></a><span data-ttu-id="1631e-243">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1631e-243">Next steps</span></span>
* <span data-ttu-id="1631e-244">[Přidat uložená hledání a výstrahy](operations-management-suite-solutions-resources-searches-alerts.md) do řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="1631e-244">[Add saved searches and alerts](operations-management-suite-solutions-resources-searches-alerts.md) to your management solution.</span></span>
* <span data-ttu-id="1631e-245">[Přidání zobrazení](operations-management-suite-solutions-resources-views.md) do řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="1631e-245">[Add views](operations-management-suite-solutions-resources-views.md) to your management solution.</span></span>
* <span data-ttu-id="1631e-246">[Přidat sady runbook a dalším prostředkům Automation](operations-management-suite-solutions-resources-automation.md) do řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="1631e-246">[Add runbooks and other Automation resources](operations-management-suite-solutions-resources-automation.md) to your management solution.</span></span>
* <span data-ttu-id="1631e-247">Další podrobnosti o [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="1631e-247">Learn the details of [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="1631e-248">Hledání [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates) ukázky různých šablonách Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="1631e-248">Search [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates) for samples of different Resource Manager templates.</span></span>
