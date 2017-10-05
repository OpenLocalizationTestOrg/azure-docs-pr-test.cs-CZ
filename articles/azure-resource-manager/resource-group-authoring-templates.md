---
title: "Struktura šablony Azure Resource Manager a syntaxe | Microsoft Docs"
description: "Popisuje strukturu a vlastnosti šablon Azure Resource Manager pomocí deklarativní syntaxe JSON."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/14/2017
ms.author: tomfitz
ms.openlocfilehash: dc9b64062d7f68c83aa090eec96744819a5ca423
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a><span data-ttu-id="7282c-103">Pochopit strukturu a syntaxe šablon Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7282c-103">Understand the structure and syntax of Azure Resource Manager templates</span></span>
<span data-ttu-id="7282c-104">Toto téma popisuje strukturu šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7282c-104">This topic describes the structure of an Azure Resource Manager template.</span></span> <span data-ttu-id="7282c-105">Představuje různé části šablony a vlastnosti, které jsou k dispozici v těchto částech.</span><span class="sxs-lookup"><span data-stu-id="7282c-105">It presents the different sections of a template and the properties that are available in those sections.</span></span> <span data-ttu-id="7282c-106">Šablona se skládá z JSON a výrazy, které můžete použít k vytvoření hodnot pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="7282c-106">The template consists of JSON and expressions that you can use to construct values for your deployment.</span></span> <span data-ttu-id="7282c-107">Podrobný kurz k vytvoření šablony, najdete v části [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="7282c-107">For a step-by-step tutorial on creating a template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>

## <a name="template-format"></a><span data-ttu-id="7282c-108">Formát šablony</span><span class="sxs-lookup"><span data-stu-id="7282c-108">Template format</span></span>
<span data-ttu-id="7282c-109">Ve své nejjednodušší struktuře šablonu obsahuje následující prvky:</span><span class="sxs-lookup"><span data-stu-id="7282c-109">In its simplest structure, a template contains the following elements:</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "resources": [  ],
    "outputs": {  }
}
```

| <span data-ttu-id="7282c-110">Název elementu</span><span class="sxs-lookup"><span data-stu-id="7282c-110">Element name</span></span> | <span data-ttu-id="7282c-111">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7282c-111">Required</span></span> | <span data-ttu-id="7282c-112">Popis</span><span class="sxs-lookup"><span data-stu-id="7282c-112">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="7282c-113">$schema</span><span class="sxs-lookup"><span data-stu-id="7282c-113">$schema</span></span> |<span data-ttu-id="7282c-114">Ano</span><span class="sxs-lookup"><span data-stu-id="7282c-114">Yes</span></span> |<span data-ttu-id="7282c-115">Umístění souboru schématu JSON, který popisuje verzi jazyka šablony.</span><span class="sxs-lookup"><span data-stu-id="7282c-115">Location of the JSON schema file that describes the version of the template language.</span></span> <span data-ttu-id="7282c-116">Použijte adresu URL v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="7282c-116">Use the URL shown in the preceding example.</span></span> |
| <span data-ttu-id="7282c-117">contentVersion</span><span class="sxs-lookup"><span data-stu-id="7282c-117">contentVersion</span></span> |<span data-ttu-id="7282c-118">Ano</span><span class="sxs-lookup"><span data-stu-id="7282c-118">Yes</span></span> |<span data-ttu-id="7282c-119">Verze šablony (jako je například 1.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="7282c-119">Version of the template (such as 1.0.0.0).</span></span> <span data-ttu-id="7282c-120">Můžete zadat jakoukoli hodnotu pro tento element.</span><span class="sxs-lookup"><span data-stu-id="7282c-120">You can provide any value for this element.</span></span> <span data-ttu-id="7282c-121">Při nasazení prostředků pomocí šablony, tuto hodnotu lze zajistit, aby se používal správnou šablonu.</span><span class="sxs-lookup"><span data-stu-id="7282c-121">When deploying resources using the template, this value can be used to make sure that the right template is being used.</span></span> |
| <span data-ttu-id="7282c-122">Parametry</span><span class="sxs-lookup"><span data-stu-id="7282c-122">parameters</span></span> |<span data-ttu-id="7282c-123">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-123">No</span></span> |<span data-ttu-id="7282c-124">Hodnoty, které jsou k dispozici při nasazení pro přizpůsobení nasazení prostředků.</span><span class="sxs-lookup"><span data-stu-id="7282c-124">Values that are provided when deployment is executed to customize resource deployment.</span></span> |
| <span data-ttu-id="7282c-125">proměnné</span><span class="sxs-lookup"><span data-stu-id="7282c-125">variables</span></span> |<span data-ttu-id="7282c-126">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-126">No</span></span> |<span data-ttu-id="7282c-127">Hodnoty, které se používá jako fragmenty JSON v šabloně, které zjednodušují výrazy jazyka šablony.</span><span class="sxs-lookup"><span data-stu-id="7282c-127">Values that are used as JSON fragments in the template to simplify template language expressions.</span></span> |
| <span data-ttu-id="7282c-128">Prostředky</span><span class="sxs-lookup"><span data-stu-id="7282c-128">resources</span></span> |<span data-ttu-id="7282c-129">Ano</span><span class="sxs-lookup"><span data-stu-id="7282c-129">Yes</span></span> |<span data-ttu-id="7282c-130">Typy prostředků, které jsou nasazené nebo aktualizovány v skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="7282c-130">Resource types that are deployed or updated in a resource group.</span></span> |
| <span data-ttu-id="7282c-131">Výstupy</span><span class="sxs-lookup"><span data-stu-id="7282c-131">outputs</span></span> |<span data-ttu-id="7282c-132">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-132">No</span></span> |<span data-ttu-id="7282c-133">Hodnoty, které se vrátí po nasazení.</span><span class="sxs-lookup"><span data-stu-id="7282c-133">Values that are returned after deployment.</span></span> |

<span data-ttu-id="7282c-134">Každý prvek obsahuje vlastnosti, které můžete zadat.</span><span class="sxs-lookup"><span data-stu-id="7282c-134">Each element contains properties you can set.</span></span> <span data-ttu-id="7282c-135">Následující příklad obsahuje úplnou syntaxí šablony:</span><span class="sxs-lookup"><span data-stu-id="7282c-135">The following example contains the full syntax for a template:</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  
        "<parameter-name>" : {
            "type" : "<type-of-parameter-value>",
            "defaultValue": "<default-value-of-parameter>",
            "allowedValues": [ "<array-of-allowed-values>" ],
            "minValue": <minimum-value-for-int>,
            "maxValue": <maximum-value-for-int>,
            "minLength": <minimum-length-for-string-or-array>,
            "maxLength": <maximum-length-for-string-or-array-parameters>,
            "metadata": {
                "description": "<description-of-the parameter>" 
            }
        }
    },
    "variables": {  
        "<variable-name>": "<variable-value>",
        "<variable-name>": { 
            <variable-complex-type-value> 
        }
    },
    "resources": [
      {
          "condition": "<boolean-value-whether-to-deploy>",
          "apiVersion": "<api-version-of-resource>",
          "type": "<resource-provider-namespace/resource-type-name>",
          "name": "<name-of-the-resource>",
          "location": "<location-of-resource>",
          "tags": {
              "<tag-name1>": "<tag-value1>",
              "<tag-name2>": "<tag-value2>"
          },
          "comments": "<your-reference-notes>",
          "copy": {
              "name": "<name-of-copy-loop>",
              "count": "<number-of-iterations>",
              "mode": "<serial-or-parallel>",
              "batchSize": "<number-to-deploy-serially>"
          },
          "dependsOn": [
              "<array-of-related-resource-names>"
          ],
          "properties": {
              "<settings-for-the-resource>",
              "copy": [
                  {
                      "name": ,
                      "count": ,
                      "input": {}
                  }
              ]
          },
          "resources": [
              "<array-of-child-resources>"
          ]
      }
    ],
    "outputs": {
        "<outputName>" : {
            "type" : "<type-of-output-value>",
            "value": "<output-value-expression>"
        }
    }
}
```

<span data-ttu-id="7282c-136">Jsme zkontrolujte v částech šablony podrobněji později v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="7282c-136">We examine the sections of the template in greater detail later in this topic.</span></span>

## <a name="expressions-and-functions"></a><span data-ttu-id="7282c-137">Výrazy a funkce</span><span class="sxs-lookup"><span data-stu-id="7282c-137">Expressions and functions</span></span>
<span data-ttu-id="7282c-138">Základní syntaxe šablony je JSON.</span><span class="sxs-lookup"><span data-stu-id="7282c-138">The basic syntax of the template is JSON.</span></span> <span data-ttu-id="7282c-139">K dispozici v rámci šablony JSON hodnoty rozšířit však výrazy a funkce.</span><span class="sxs-lookup"><span data-stu-id="7282c-139">However, expressions and functions extend the JSON values available within the template.</span></span>  <span data-ttu-id="7282c-140">Výrazy jsou zapsané v JSON textové literály jejichž první a poslední znaky jsou hranaté závorky: `[` a `]`, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="7282c-140">Expressions are written within JSON string literals whose first and last characters are the brackets: `[` and `]`, respectively.</span></span> <span data-ttu-id="7282c-141">Hodnota výrazu vyhodnotí při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="7282c-141">The value of the expression is evaluated when the template is deployed.</span></span> <span data-ttu-id="7282c-142">Při zápisu jako řetězcový literál, může být výsledkem vyhodnocení výrazu jiného typu formátu JSON, jako je například pole nebo celé číslo, v závislosti na skutečný výraz.</span><span class="sxs-lookup"><span data-stu-id="7282c-142">While written as a string literal, the result of evaluating the expression can be of a different JSON type, such as an array or integer, depending on the actual expression.</span></span>  <span data-ttu-id="7282c-143">Tak, aby měl řetězcový literál začínat závorky `[`, ale je interpretován jako výraz, můžete přidat další znak pravé závorky zahájíte řetězec s `[[`.</span><span class="sxs-lookup"><span data-stu-id="7282c-143">To have a literal string start with a bracket `[`, but not have it interpreted as an expression, add an extra bracket to start the string with `[[`.</span></span>

<span data-ttu-id="7282c-144">Obvykle používají výrazy s funkcí k provádění operací pro konfiguraci nasazení.</span><span class="sxs-lookup"><span data-stu-id="7282c-144">Typically, you use expressions with functions to perform operations for configuring the deployment.</span></span> <span data-ttu-id="7282c-145">Jenom jako v jazyce JavaScript, volání funkce jsou formátovány jako `functionName(arg1,arg2,arg3)`.</span><span class="sxs-lookup"><span data-stu-id="7282c-145">Just like in JavaScript, function calls are formatted as `functionName(arg1,arg2,arg3)`.</span></span> <span data-ttu-id="7282c-146">Vlastnosti odkazovat pomocí operátorů dot a [index].</span><span class="sxs-lookup"><span data-stu-id="7282c-146">You reference properties by using the dot and [index] operators.</span></span>

<span data-ttu-id="7282c-147">Následující příklad ukazuje, jak použít několik funkcí, při vytváření hodnoty:</span><span class="sxs-lookup"><span data-stu-id="7282c-147">The following example shows how to use several functions when constructing values:</span></span>

```json
"variables": {
    "location": "[resourceGroup().location]",
    "usernameAndPassword": "[concat(parameters('username'), ':', parameters('password'))]",
    "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
}
```

<span data-ttu-id="7282c-148">Úplný seznam funkcí šablony najdete v tématu [funkce šablon Azure Resource Manager](resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="7282c-148">For the full list of template functions, see [Azure Resource Manager template functions](resource-group-template-functions.md).</span></span> 

## <a name="parameters"></a><span data-ttu-id="7282c-149">Parametry</span><span class="sxs-lookup"><span data-stu-id="7282c-149">Parameters</span></span>
<span data-ttu-id="7282c-150">V sekci parametrů šablony zadejte hodnoty, které můžete zadat při nasazování prostředky.</span><span class="sxs-lookup"><span data-stu-id="7282c-150">In the parameters section of the template, you specify which values you can input when deploying the resources.</span></span> <span data-ttu-id="7282c-151">Tyto hodnoty parametrů umožňují přizpůsobit nasazení zadáním hodnoty, které jsou přizpůsobené pro konkrétní prostředí (například vývoj, testování a provozním).</span><span class="sxs-lookup"><span data-stu-id="7282c-151">These parameter values enable you to customize the deployment by providing values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="7282c-152">Není nutné zadat parametry v šabloně, ale bez parametrů šablony vždy nasazení stejné prostředky se stejnými názvy, umístění a vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7282c-152">You do not have to provide parameters in your template, but without parameters your template would always deploy the same resources with the same names, locations, and properties.</span></span>

<span data-ttu-id="7282c-153">Můžete definovat parametry s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="7282c-153">You define parameters with the following structure:</span></span>

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
            "description": "<description-of-the parameter>" 
        }
    }
}
```

| <span data-ttu-id="7282c-154">Název elementu</span><span class="sxs-lookup"><span data-stu-id="7282c-154">Element name</span></span> | <span data-ttu-id="7282c-155">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7282c-155">Required</span></span> | <span data-ttu-id="7282c-156">Popis</span><span class="sxs-lookup"><span data-stu-id="7282c-156">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="7282c-157">Název parametru</span><span class="sxs-lookup"><span data-stu-id="7282c-157">parameterName</span></span> |<span data-ttu-id="7282c-158">Ano</span><span class="sxs-lookup"><span data-stu-id="7282c-158">Yes</span></span> |<span data-ttu-id="7282c-159">Název parametru.</span><span class="sxs-lookup"><span data-stu-id="7282c-159">Name of the parameter.</span></span> <span data-ttu-id="7282c-160">Musí být platný identifikátor jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7282c-160">Must be a valid JavaScript identifier.</span></span> |
| <span data-ttu-id="7282c-161">type</span><span class="sxs-lookup"><span data-stu-id="7282c-161">type</span></span> |<span data-ttu-id="7282c-162">Ano</span><span class="sxs-lookup"><span data-stu-id="7282c-162">Yes</span></span> |<span data-ttu-id="7282c-163">Typ hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="7282c-163">Type of the parameter value.</span></span> <span data-ttu-id="7282c-164">Zobrazit seznam povolených typů za touto tabulkou.</span><span class="sxs-lookup"><span data-stu-id="7282c-164">See the list of allowed types after this table.</span></span> |
| <span data-ttu-id="7282c-165">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="7282c-165">defaultValue</span></span> |<span data-ttu-id="7282c-166">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-166">No</span></span> |<span data-ttu-id="7282c-167">Výchozí hodnota pro parametr, pokud není zadána žádná hodnota pro parametr.</span><span class="sxs-lookup"><span data-stu-id="7282c-167">Default value for the parameter, if no value is provided for the parameter.</span></span> |
| <span data-ttu-id="7282c-168">allowedValues</span><span class="sxs-lookup"><span data-stu-id="7282c-168">allowedValues</span></span> |<span data-ttu-id="7282c-169">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-169">No</span></span> |<span data-ttu-id="7282c-170">Povolené hodnoty pro parametr a ujistěte se, že je zadaná hodnota pravé pole.</span><span class="sxs-lookup"><span data-stu-id="7282c-170">Array of allowed values for the parameter to make sure that the right value is provided.</span></span> |
| <span data-ttu-id="7282c-171">MinValue</span><span class="sxs-lookup"><span data-stu-id="7282c-171">minValue</span></span> |<span data-ttu-id="7282c-172">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-172">No</span></span> |<span data-ttu-id="7282c-173">Minimální hodnota pro parametry typu int, tato hodnota je (včetně).</span><span class="sxs-lookup"><span data-stu-id="7282c-173">The minimum value for int type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="7282c-174">MaxValue</span><span class="sxs-lookup"><span data-stu-id="7282c-174">maxValue</span></span> |<span data-ttu-id="7282c-175">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-175">No</span></span> |<span data-ttu-id="7282c-176">Maximální hodnota parametry typu int, tato hodnota je (včetně).</span><span class="sxs-lookup"><span data-stu-id="7282c-176">The maximum value for int type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="7282c-177">minLength</span><span class="sxs-lookup"><span data-stu-id="7282c-177">minLength</span></span> |<span data-ttu-id="7282c-178">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-178">No</span></span> |<span data-ttu-id="7282c-179">Minimální délka řetězce, secureString a parametry typu pole, tato hodnota je (včetně).</span><span class="sxs-lookup"><span data-stu-id="7282c-179">The minimum length for string, secureString, and array type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="7282c-180">Hodnota maxLength</span><span class="sxs-lookup"><span data-stu-id="7282c-180">maxLength</span></span> |<span data-ttu-id="7282c-181">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-181">No</span></span> |<span data-ttu-id="7282c-182">Maximální délka řetězce, secureString a parametry typu pole, tato hodnota je (včetně).</span><span class="sxs-lookup"><span data-stu-id="7282c-182">The maximum length for string, secureString, and array type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="7282c-183">Popis</span><span class="sxs-lookup"><span data-stu-id="7282c-183">description</span></span> |<span data-ttu-id="7282c-184">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-184">No</span></span> |<span data-ttu-id="7282c-185">Popis parametru, který se zobrazí uživatelům prostřednictvím portálu.</span><span class="sxs-lookup"><span data-stu-id="7282c-185">Description of the parameter that is displayed to users through the portal.</span></span> |

<span data-ttu-id="7282c-186">Povolené typy a hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="7282c-186">The allowed types and values are:</span></span>

* <span data-ttu-id="7282c-187">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="7282c-187">**string**</span></span>
* <span data-ttu-id="7282c-188">**secureString**</span><span class="sxs-lookup"><span data-stu-id="7282c-188">**secureString**</span></span>
* <span data-ttu-id="7282c-189">**celá čísla**</span><span class="sxs-lookup"><span data-stu-id="7282c-189">**int**</span></span>
* <span data-ttu-id="7282c-190">**BOOL**</span><span class="sxs-lookup"><span data-stu-id="7282c-190">**bool**</span></span>
* <span data-ttu-id="7282c-191">**objekt**</span><span class="sxs-lookup"><span data-stu-id="7282c-191">**object**</span></span> 
* <span data-ttu-id="7282c-192">**secureObject**</span><span class="sxs-lookup"><span data-stu-id="7282c-192">**secureObject**</span></span>
* <span data-ttu-id="7282c-193">**pole**</span><span class="sxs-lookup"><span data-stu-id="7282c-193">**array**</span></span>

<span data-ttu-id="7282c-194">Pro zadání parametru, jako je volitelné, zadejte hodnotu defaultValue (může být prázdný řetězec).</span><span class="sxs-lookup"><span data-stu-id="7282c-194">To specify a parameter as optional, provide a defaultValue (can be an empty string).</span></span> 

<span data-ttu-id="7282c-195">Pokud zadáte název parametru v šabloně, který odpovídá parametru v příkazu k nasazení šablony, je potenciální nejednoznačnosti hodnoty, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="7282c-195">If you specify a parameter name in your template that matches a parameter in the command to deploy the template, there is potential ambiguity about the values you provide.</span></span> <span data-ttu-id="7282c-196">Správce prostředků řeší tento nedorozuměním přidáním operátory **FromTemplate** pro parametr šablony.</span><span class="sxs-lookup"><span data-stu-id="7282c-196">Resource Manager resolves this confusion by adding the postfix **FromTemplate** to the template parameter.</span></span> <span data-ttu-id="7282c-197">Například, pokud zahrnete parametr s názvem **ResourceGroupName** v šabloně, je v konfliktu s **ResourceGroupName** parametr ve [New-AzureRmResourceGroupDeployment ](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) rutiny.</span><span class="sxs-lookup"><span data-stu-id="7282c-197">For example, if you include a parameter named **ResourceGroupName** in your template, it conflicts with the **ResourceGroupName** parameter in the [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet.</span></span> <span data-ttu-id="7282c-198">Během nasazení, budete vyzváni, zadejte hodnotu pro **ResourceGroupNameFromTemplate**.</span><span class="sxs-lookup"><span data-stu-id="7282c-198">During deployment, you are prompted to provide a value for **ResourceGroupNameFromTemplate**.</span></span> <span data-ttu-id="7282c-199">Obecně platí neměli byste toto nedorozuměním není pojmenováním parametry se stejným názvem jako parametry použité pro operace nasazení.</span><span class="sxs-lookup"><span data-stu-id="7282c-199">In general, you should avoid this confusion by not naming parameters with the same name as parameters used for deployment operations.</span></span>

> [!NOTE]
> <span data-ttu-id="7282c-200">Všechna hesla, klíče a jiné tajné klíče by měl používat **secureString** typu.</span><span class="sxs-lookup"><span data-stu-id="7282c-200">All passwords, keys, and other secrets should use the **secureString** type.</span></span> <span data-ttu-id="7282c-201">Pokud předáte citlivá data v objektu JSON, použít **secureObject** typu.</span><span class="sxs-lookup"><span data-stu-id="7282c-201">If you pass sensitive data in a JSON object, use the **secureObject** type.</span></span> <span data-ttu-id="7282c-202">Parametry šablony s secureString nebo secureObject typů nelze přečíst po nasazení prostředků.</span><span class="sxs-lookup"><span data-stu-id="7282c-202">Template parameters with secureString or secureObject types cannot be read after resource deployment.</span></span> 
> 
> <span data-ttu-id="7282c-203">Například následující položku v historii nasazení zobrazuje hodnotu pro řetězce a objekt, ale ne pro secureString a secureObject.</span><span class="sxs-lookup"><span data-stu-id="7282c-203">For example, the following entry in the deployment history shows the value for a string and object but not for secureString and secureObject.</span></span>
>
> ![Zobrazit hodnoty nasazení](./media/resource-group-authoring-templates/show-parameters.png)  
>

<span data-ttu-id="7282c-205">Následující příklad ukazuje, jak definovat parametry:</span><span class="sxs-lookup"><span data-stu-id="7282c-205">The following example shows how to define parameters:</span></span>

```json
"parameters": {
    "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
    },
    "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
    },
    "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
    },
    "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
    }
}
```

<span data-ttu-id="7282c-206">Pro vstupní hodnoty parametrů během nasazování naleznete v tématu [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="7282c-206">For how to input the parameter values during deployment, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span> 

## <a name="variables"></a><span data-ttu-id="7282c-207">Proměnné</span><span class="sxs-lookup"><span data-stu-id="7282c-207">Variables</span></span>
<span data-ttu-id="7282c-208">V sekci proměnných můžete vytvořit hodnoty, které lze použít v celé vaší šablony.</span><span class="sxs-lookup"><span data-stu-id="7282c-208">In the variables section, you construct values that can be used throughout your template.</span></span> <span data-ttu-id="7282c-209">Není potřeba definovat proměnné, ale jejich často zjednodušit vaše šablony snížením složité výrazy.</span><span class="sxs-lookup"><span data-stu-id="7282c-209">You do not need to define variables, but they often simplify your template by reducing complex expressions.</span></span>

<span data-ttu-id="7282c-210">Můžete definovat proměnné s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="7282c-210">You define variables with the following structure:</span></span>

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    }
}
```

<span data-ttu-id="7282c-211">Následující příklad ukazuje, jak k definování proměnné, který je sestavený ze dvou hodnot parametrů:</span><span class="sxs-lookup"><span data-stu-id="7282c-211">The following example shows how to define a variable that is constructed from two parameter values:</span></span>

```json
"variables": {
    "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
}
```

<span data-ttu-id="7282c-212">Další příklad ukazuje, proměnné, která je komplexního typu JSON a proměnné, které se vytvářejí na základě jiné proměnné:</span><span class="sxs-lookup"><span data-stu-id="7282c-212">The next example shows a variable that is a complex JSON type, and variables that are constructed from other variables:</span></span>

```json
"parameters": {
    "environmentName": {
        "type": "string",
        "allowedValues": [
          "test",
          "prod"
        ]
    }
},
"variables": {
    "environmentSettings": {
        "test": {
            "instancesSize": "Small",
            "instancesCount": 1
        },
        "prod": {
            "instancesSize": "Large",
            "instancesCount": 4
        }
    },
    "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
    "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
    "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
}
```

## <a name="resources"></a><span data-ttu-id="7282c-213">Zdroje</span><span class="sxs-lookup"><span data-stu-id="7282c-213">Resources</span></span>
<span data-ttu-id="7282c-214">V části prostředky definujete prostředky, které jsou nasazené a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="7282c-214">In the resources section, you define the resources that are deployed or updated.</span></span> <span data-ttu-id="7282c-215">V této části můžete získat složité, protože je potřeba pochopit, typy, které nasazujete zadejte správné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="7282c-215">This section can get complicated because you must understand the types you are deploying to provide the right values.</span></span> <span data-ttu-id="7282c-216">Pro konkrétní prostředky hodnoty (apiVersion, typ a vlastnosti), které je nutné nastavit, najdete v části [definování zdrojů v šablonách Azure Resource Manager](/azure/templates/).</span><span class="sxs-lookup"><span data-stu-id="7282c-216">For the resource-specific values (apiVersion, type, and properties) that you need to set, see [Define resources in Azure Resource Manager templates](/azure/templates/).</span></span> 

<span data-ttu-id="7282c-217">Můžete definovat prostředky s následující strukturou:</span><span class="sxs-lookup"><span data-stu-id="7282c-217">You define resources with the following structure:</span></span>

```json
"resources": [
  {
      "condition": "<boolean-value-whether-to-deploy>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": "<number-of-iterations>",
          "mode": "<serial-or-parallel>",
          "batchSize": "<number-to-deploy-serially>"
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| <span data-ttu-id="7282c-218">Název elementu</span><span class="sxs-lookup"><span data-stu-id="7282c-218">Element name</span></span> | <span data-ttu-id="7282c-219">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7282c-219">Required</span></span> | <span data-ttu-id="7282c-220">Popis</span><span class="sxs-lookup"><span data-stu-id="7282c-220">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="7282c-221">Podmínka</span><span class="sxs-lookup"><span data-stu-id="7282c-221">condition</span></span> | <span data-ttu-id="7282c-222">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-222">No</span></span> | <span data-ttu-id="7282c-223">Logická hodnota, která určuje, jestli je nasazené prostředku.</span><span class="sxs-lookup"><span data-stu-id="7282c-223">Boolean value that indicates whether the resource is deployed.</span></span> |
| <span data-ttu-id="7282c-224">apiVersion</span><span class="sxs-lookup"><span data-stu-id="7282c-224">apiVersion</span></span> |<span data-ttu-id="7282c-225">Ano</span><span class="sxs-lookup"><span data-stu-id="7282c-225">Yes</span></span> |<span data-ttu-id="7282c-226">Verze rozhraní REST API pro vytvoření prostředku.</span><span class="sxs-lookup"><span data-stu-id="7282c-226">Version of the REST API to use for creating the resource.</span></span> |
| <span data-ttu-id="7282c-227">type</span><span class="sxs-lookup"><span data-stu-id="7282c-227">type</span></span> |<span data-ttu-id="7282c-228">Ano</span><span class="sxs-lookup"><span data-stu-id="7282c-228">Yes</span></span> |<span data-ttu-id="7282c-229">Typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="7282c-229">Type of the resource.</span></span> <span data-ttu-id="7282c-230">Tato hodnota je kombinací obor názvů zprostředkovatele prostředků a typ prostředku (například **Microsoft.Storage/storageAccounts**).</span><span class="sxs-lookup"><span data-stu-id="7282c-230">This value is a combination of the namespace of the resource provider and the resource type (such as **Microsoft.Storage/storageAccounts**).</span></span> |
| <span data-ttu-id="7282c-231">jméno</span><span class="sxs-lookup"><span data-stu-id="7282c-231">name</span></span> |<span data-ttu-id="7282c-232">Ano</span><span class="sxs-lookup"><span data-stu-id="7282c-232">Yes</span></span> |<span data-ttu-id="7282c-233">Název prostředku.</span><span class="sxs-lookup"><span data-stu-id="7282c-233">Name of the resource.</span></span> <span data-ttu-id="7282c-234">Název musí splňovat omezení součást URI definované v RFC3986.</span><span class="sxs-lookup"><span data-stu-id="7282c-234">The name must follow URI component restrictions defined in RFC3986.</span></span> <span data-ttu-id="7282c-235">Kromě toho služby Azure, které zveřejňují názvu prostředku třetí stranou ověřit název, který má Ujistěte se, zda není pokus o zfalšovat jiné identity.</span><span class="sxs-lookup"><span data-stu-id="7282c-235">In addition, Azure services that expose the resource name to outside parties validate the name to make sure it is not an attempt to spoof another identity.</span></span> |
| <span data-ttu-id="7282c-236">location</span><span class="sxs-lookup"><span data-stu-id="7282c-236">location</span></span> |<span data-ttu-id="7282c-237">Je to různé.</span><span class="sxs-lookup"><span data-stu-id="7282c-237">Varies</span></span> |<span data-ttu-id="7282c-238">Podporované geografické umístění zadaného prostředku.</span><span class="sxs-lookup"><span data-stu-id="7282c-238">Supported geo-locations of the provided resource.</span></span> <span data-ttu-id="7282c-239">Můžete vybrat některý z dostupných umístění, ale obvykle má smysl vyberte ten, který je blízko vaši uživatelé.</span><span class="sxs-lookup"><span data-stu-id="7282c-239">You can select any of the available locations, but typically it makes sense to pick one that is close to your users.</span></span> <span data-ttu-id="7282c-240">Obvykle také má smysl umístit prostředky, které vzájemně spolupracovat ve stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="7282c-240">Usually, it also makes sense to place resources that interact with each other in the same region.</span></span> <span data-ttu-id="7282c-241">Většina typů prostředků vyžadují umístění, ale některé typy (například přiřazení role) nevyžadují umístění.</span><span class="sxs-lookup"><span data-stu-id="7282c-241">Most resource types require a location, but some types (such as a role assignment) do not require a location.</span></span> <span data-ttu-id="7282c-242">V tématu [nastavit umístění prostředku v šablonách Azure Resource Manager](resource-manager-template-location.md).</span><span class="sxs-lookup"><span data-stu-id="7282c-242">See [Set resource location in Azure Resource Manager templates](resource-manager-template-location.md).</span></span> |
| <span data-ttu-id="7282c-243">tags</span><span class="sxs-lookup"><span data-stu-id="7282c-243">tags</span></span> |<span data-ttu-id="7282c-244">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-244">No</span></span> |<span data-ttu-id="7282c-245">Značky, které jsou přidružené k prostředku.</span><span class="sxs-lookup"><span data-stu-id="7282c-245">Tags that are associated with the resource.</span></span> <span data-ttu-id="7282c-246">V tématu [označit prostředky v šablonách Azure Resource Manager](resource-manager-template-tags.md).</span><span class="sxs-lookup"><span data-stu-id="7282c-246">See [Tag resources in Azure Resource Manager templates](resource-manager-template-tags.md).</span></span> |
| <span data-ttu-id="7282c-247">Komentáře</span><span class="sxs-lookup"><span data-stu-id="7282c-247">comments</span></span> |<span data-ttu-id="7282c-248">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-248">No</span></span> |<span data-ttu-id="7282c-249">Poznámky pro dokumentaci prostředky ve vaší šabloně</span><span class="sxs-lookup"><span data-stu-id="7282c-249">Your notes for documenting the resources in your template</span></span> |
| <span data-ttu-id="7282c-250">Kopírování</span><span class="sxs-lookup"><span data-stu-id="7282c-250">copy</span></span> |<span data-ttu-id="7282c-251">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-251">No</span></span> |<span data-ttu-id="7282c-252">V případě potřeby více než jednu instanci počet zdrojů pro vytvoření.</span><span class="sxs-lookup"><span data-stu-id="7282c-252">If more than one instance is needed, the number of resources to create.</span></span> <span data-ttu-id="7282c-253">Paralelní je výchozí režim.</span><span class="sxs-lookup"><span data-stu-id="7282c-253">The default mode is parallel.</span></span> <span data-ttu-id="7282c-254">Zadejte sériové režim, když nechcete, aby všechny nebo prostředky do nasazení ve stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="7282c-254">Specify serial mode when you do not want all or the resources to deploy at the same time.</span></span> <span data-ttu-id="7282c-255">Další informace najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="7282c-255">For more information, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span> |
| <span data-ttu-id="7282c-256">dependsOn</span><span class="sxs-lookup"><span data-stu-id="7282c-256">dependsOn</span></span> |<span data-ttu-id="7282c-257">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-257">No</span></span> |<span data-ttu-id="7282c-258">Prostředky, které musí být nasazené, než je nasazený tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="7282c-258">Resources that must be deployed before this resource is deployed.</span></span> <span data-ttu-id="7282c-259">Správce prostředků vyhodnotí závislosti mezi prostředky a nasadí je ve správném pořadí.</span><span class="sxs-lookup"><span data-stu-id="7282c-259">Resource Manager evaluates the dependencies between resources and deploys them in the correct order.</span></span> <span data-ttu-id="7282c-260">Pokud nejsou na sobě navzájem závislé prostředky, jsou nasazeny současně.</span><span class="sxs-lookup"><span data-stu-id="7282c-260">When resources are not dependent on each other, they are deployed in parallel.</span></span> <span data-ttu-id="7282c-261">Hodnota může být čárkami oddělený seznam prostředek názvy nebo jedinečné identifikátory prostředků.</span><span class="sxs-lookup"><span data-stu-id="7282c-261">The value can be a comma-separated list of a resource names or resource unique identifiers.</span></span> <span data-ttu-id="7282c-262">Zobrazit seznam pouze těch prostředků, které jsou nasazeny v této šabloně.</span><span class="sxs-lookup"><span data-stu-id="7282c-262">Only list resources that are deployed in this template.</span></span> <span data-ttu-id="7282c-263">Prostředky, které nejsou v této šabloně definovány již musí existovat.</span><span class="sxs-lookup"><span data-stu-id="7282c-263">Resources that are not defined in this template must already exist.</span></span> <span data-ttu-id="7282c-264">Vyhněte se přidání nepotřebné závislostí, jak mohou zpomalit nasazení a vytvoření cyklické závislosti.</span><span class="sxs-lookup"><span data-stu-id="7282c-264">Avoid adding unnecessary dependencies as they can slow your deployment and create circular dependencies.</span></span> <span data-ttu-id="7282c-265">Pokyny v závislosti na nastavení najdete v tématu [definování závislostí v šablonách Azure Resource Manager](resource-group-define-dependencies.md).</span><span class="sxs-lookup"><span data-stu-id="7282c-265">For guidance on setting dependencies, see [Defining dependencies in Azure Resource Manager templates](resource-group-define-dependencies.md).</span></span> |
| <span data-ttu-id="7282c-266">properties</span><span class="sxs-lookup"><span data-stu-id="7282c-266">properties</span></span> |<span data-ttu-id="7282c-267">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-267">No</span></span> |<span data-ttu-id="7282c-268">Nastavení konfigurace specifických prostředků.</span><span class="sxs-lookup"><span data-stu-id="7282c-268">Resource-specific configuration settings.</span></span> <span data-ttu-id="7282c-269">Hodnoty pro vlastnosti jsou stejné jako hodnoty, které zadáte v textu požadavku REST API operaci (metoda PUT) k vytvoření prostředku.</span><span class="sxs-lookup"><span data-stu-id="7282c-269">The values for the properties are the same as the values you provide in the request body for the REST API operation (PUT method) to create the resource.</span></span> <span data-ttu-id="7282c-270">Můžete také zadat pole kopie vytvořit více instancí vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="7282c-270">You can also specify a copy array to create multiple instances of a property.</span></span> <span data-ttu-id="7282c-271">Další informace najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="7282c-271">For more information, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span> |
| <span data-ttu-id="7282c-272">Prostředky</span><span class="sxs-lookup"><span data-stu-id="7282c-272">resources</span></span> |<span data-ttu-id="7282c-273">Ne</span><span class="sxs-lookup"><span data-stu-id="7282c-273">No</span></span> |<span data-ttu-id="7282c-274">Podřízené prostředky, které jsou závislé na prostředku definovaný.</span><span class="sxs-lookup"><span data-stu-id="7282c-274">Child resources that depend on the resource being defined.</span></span> <span data-ttu-id="7282c-275">Zadejte pouze typy prostředků, které jsou povoleny schématem nadřazený prostředek.</span><span class="sxs-lookup"><span data-stu-id="7282c-275">Only provide resource types that are permitted by the schema of the parent resource.</span></span> <span data-ttu-id="7282c-276">Plně kvalifikovaný typ prostředku podřízené obsahuje nadřazený typ prostředku, jako například **Microsoft.Web/sites/extensions**.</span><span class="sxs-lookup"><span data-stu-id="7282c-276">The fully qualified type of the child resource includes the parent resource type, such as **Microsoft.Web/sites/extensions**.</span></span> <span data-ttu-id="7282c-277">Závislost na nadřazeném prostředku není implicitní.</span><span class="sxs-lookup"><span data-stu-id="7282c-277">Dependency on the parent resource is not implied.</span></span> <span data-ttu-id="7282c-278">Je nutné explicitně zadat tuto závislost.</span><span class="sxs-lookup"><span data-stu-id="7282c-278">You must explicitly define that dependency.</span></span> |

<span data-ttu-id="7282c-279">V části prostředky obsahuje pole prostředky pro nasazení.</span><span class="sxs-lookup"><span data-stu-id="7282c-279">The resources section contains an array of the resources to deploy.</span></span> <span data-ttu-id="7282c-280">V rámci každého prostředku můžete také definovat pole podřízené prostředky.</span><span class="sxs-lookup"><span data-stu-id="7282c-280">Within each resource, you can also define an array of child resources.</span></span> <span data-ttu-id="7282c-281">Proto vaše oddílu prostředků může mít struktura jako:</span><span class="sxs-lookup"><span data-stu-id="7282c-281">Therefore, your resources section could have a structure like:</span></span>

```json
"resources": [
  {
      "name": "resourceA",
  },
  {
      "name": "resourceB",
      "resources": [
        {
            "name": "firstChildResourceB",
        },
        {   
            "name": "secondChildResourceB",
        }
      ]
  },
  {
      "name": "resourceC",
  }
]
```      

<span data-ttu-id="7282c-282">Další informace o definování podřízené prostředky najdete v tématu [nastavte název a typ pro podřízený prostředek v šabloně Resource Manager](resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="7282c-282">For more information about defining child resources, see [Set name and type for child resource in Resource Manager template](resource-manager-template-child-resource.md).</span></span>

<span data-ttu-id="7282c-283">**Podmínku** element určuje, zda je nasazení na prostředek.</span><span class="sxs-lookup"><span data-stu-id="7282c-283">The **condition** element specifies whether the resource is deployed.</span></span> <span data-ttu-id="7282c-284">Hodnota pro tento element překládá true nebo false.</span><span class="sxs-lookup"><span data-stu-id="7282c-284">The value for this element resolves to true or false.</span></span> <span data-ttu-id="7282c-285">Například k určení, jestli je nasazené nový účet úložiště, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="7282c-285">For example, to specify whether a new storage account is deployed, use:</span></span>

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

<span data-ttu-id="7282c-286">Příklad použití nový nebo existující prostředek, naleznete v části [nový nebo stávající šablonu podmínku](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span><span class="sxs-lookup"><span data-stu-id="7282c-286">For an example of using a new or existing resource, see [New or existing condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span></span>

<span data-ttu-id="7282c-287">Chcete-li určit, zda je virtuální počítač nasadit pomocí hesla nebo klíče SSH, definovat dvě verze virtuálního počítače šablony a použít **podmínku** k odlišení využití.</span><span class="sxs-lookup"><span data-stu-id="7282c-287">To specify whether a virtual machine is deployed with a password or SSH key, define two versions of the virtual machine in your template and use **condition** to differentiate usage.</span></span> <span data-ttu-id="7282c-288">Předání parametru, který určuje scénář, který chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="7282c-288">Pass a parameter that specifies which scenario to deploy.</span></span>

```json
{
    "condition": "[equals(parameters('passwordOrSshKey'),'password')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'password')]",
    "properties": {
        "osProfile": {
            "computerName": "[variables('vmName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
        },
        ...
    },
    ...
},
{
    "condition": "[equals(parameters('passwordOrSshKey'),'sshKey')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'ssh')]",
    "properties": {
        "osProfile": {
            "linuxConfiguration": {
                "disablePasswordAuthentication": "true",
                "ssh": {
                    "publicKeys": [
                        {
                            "path": "[variables('sshKeyPath')]",
                            "keyData": "[parameters('adminSshKey')]"
                        }
                    ]
                }
            }
        },
        ...
    },
    ...
}
``` 

<span data-ttu-id="7282c-289">Příklad použití heslo nebo klíč SSH pro virtuální počítač nejde nasadit, naleznete v části [uživatelské jméno nebo SSH podmínku šablony](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span><span class="sxs-lookup"><span data-stu-id="7282c-289">For an example of using a password or SSH key to deploy virtual machine, see [Username or SSH condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span></span>

## <a name="outputs"></a><span data-ttu-id="7282c-290">Výstupy</span><span class="sxs-lookup"><span data-stu-id="7282c-290">Outputs</span></span>
<span data-ttu-id="7282c-291">V části výstupy zadejte hodnoty, které jsou vráceny z nasazení.</span><span class="sxs-lookup"><span data-stu-id="7282c-291">In the Outputs section, you specify values that are returned from deployment.</span></span> <span data-ttu-id="7282c-292">Například může vrátit identifikátor URI pro přístup k prostředkům nasazené.</span><span class="sxs-lookup"><span data-stu-id="7282c-292">For example, you could return the URI to access a deployed resource.</span></span>

<span data-ttu-id="7282c-293">Následující příklad ukazuje strukturu definici výstup:</span><span class="sxs-lookup"><span data-stu-id="7282c-293">The following example shows the structure of an output definition:</span></span>

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| <span data-ttu-id="7282c-294">Název elementu</span><span class="sxs-lookup"><span data-stu-id="7282c-294">Element name</span></span> | <span data-ttu-id="7282c-295">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="7282c-295">Required</span></span> | <span data-ttu-id="7282c-296">Popis</span><span class="sxs-lookup"><span data-stu-id="7282c-296">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="7282c-297">outputName</span><span class="sxs-lookup"><span data-stu-id="7282c-297">outputName</span></span> |<span data-ttu-id="7282c-298">Ano</span><span class="sxs-lookup"><span data-stu-id="7282c-298">Yes</span></span> |<span data-ttu-id="7282c-299">Název výstupní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7282c-299">Name of the output value.</span></span> <span data-ttu-id="7282c-300">Musí být platný identifikátor jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7282c-300">Must be a valid JavaScript identifier.</span></span> |
| <span data-ttu-id="7282c-301">type</span><span class="sxs-lookup"><span data-stu-id="7282c-301">type</span></span> |<span data-ttu-id="7282c-302">Ano</span><span class="sxs-lookup"><span data-stu-id="7282c-302">Yes</span></span> |<span data-ttu-id="7282c-303">Typ hodnoty výstup.</span><span class="sxs-lookup"><span data-stu-id="7282c-303">Type of the output value.</span></span> <span data-ttu-id="7282c-304">Výstupní hodnoty podporují stejné typy jako vstupní parametry šablony.</span><span class="sxs-lookup"><span data-stu-id="7282c-304">Output values support the same types as template input parameters.</span></span> |
| <span data-ttu-id="7282c-305">hodnota</span><span class="sxs-lookup"><span data-stu-id="7282c-305">value</span></span> |<span data-ttu-id="7282c-306">Ano</span><span class="sxs-lookup"><span data-stu-id="7282c-306">Yes</span></span> |<span data-ttu-id="7282c-307">Výraz jazyka šablony, který se vyhodnotí a vrátí jako výstupní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="7282c-307">Template language expression that is evaluated and returned as output value.</span></span> |

<span data-ttu-id="7282c-308">Následující příklad ukazuje, hodnotu, která je vrácena v části výstupy.</span><span class="sxs-lookup"><span data-stu-id="7282c-308">The following example shows a value that is returned in the Outputs section.</span></span>

```json
"outputs": {
    "siteUri" : {
        "type" : "string",
        "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
    }
}
```

<span data-ttu-id="7282c-309">Další informace o práci s výstupem najdete v tématu [sdílení stavu v šablonách Azure Resource Manager](best-practices-resource-manager-state.md).</span><span class="sxs-lookup"><span data-stu-id="7282c-309">For more information about working with output, see [Sharing state in Azure Resource Manager templates](best-practices-resource-manager-state.md).</span></span>

## <a name="template-limits"></a><span data-ttu-id="7282c-310">Omezení šablony</span><span class="sxs-lookup"><span data-stu-id="7282c-310">Template limits</span></span>

<span data-ttu-id="7282c-311">Omezení velikosti vaší šablony 1 MB a každý soubor parametrů na 64 KB.</span><span class="sxs-lookup"><span data-stu-id="7282c-311">Limit the size of your template to 1 MB, and each parameter file to 64 KB.</span></span> <span data-ttu-id="7282c-312">Omezení 1 MB platí pro konečného stavu šablony po rozšířila s definic iterativní prostředků a hodnoty pro parametry a proměnné.</span><span class="sxs-lookup"><span data-stu-id="7282c-312">The 1-MB limit applies to the final state of the template after it has been expanded with iterative resource definitions, and values for variables and parameters.</span></span> 

<span data-ttu-id="7282c-313">Také jste omezeni na:</span><span class="sxs-lookup"><span data-stu-id="7282c-313">You are also limited to:</span></span>

* <span data-ttu-id="7282c-314">256 parametry</span><span class="sxs-lookup"><span data-stu-id="7282c-314">256 parameters</span></span>
* <span data-ttu-id="7282c-315">256 proměnné</span><span class="sxs-lookup"><span data-stu-id="7282c-315">256 variables</span></span>
* <span data-ttu-id="7282c-316">800 prostředky (včetně počet kopií)</span><span class="sxs-lookup"><span data-stu-id="7282c-316">800 resources (including copy count)</span></span>
* <span data-ttu-id="7282c-317">64 výstupní hodnoty</span><span class="sxs-lookup"><span data-stu-id="7282c-317">64 output values</span></span>
* <span data-ttu-id="7282c-318">24,576 znaků výraz šablony</span><span class="sxs-lookup"><span data-stu-id="7282c-318">24,576 characters in a template expression</span></span>

<span data-ttu-id="7282c-319">Některá omezení šablony můžete překročit pomocí vnořené šablony.</span><span class="sxs-lookup"><span data-stu-id="7282c-319">You can exceed some template limits by using a nested template.</span></span> <span data-ttu-id="7282c-320">Další informace najdete v tématu [použití propojených šablon při nasazování prostředků Azure](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="7282c-320">For more information, see [Using linked templates when deploying Azure resources](resource-group-linked-templates.md).</span></span> <span data-ttu-id="7282c-321">Abyste snížili počet parametrů, proměnné nebo výstupů, můžete sloučit několik hodnot do objektu.</span><span class="sxs-lookup"><span data-stu-id="7282c-321">To reduce the number of parameters, variables, or outputs, you can combine several values into an object.</span></span> <span data-ttu-id="7282c-322">Další informace najdete v tématu [objektů jako parametry](resource-manager-objects-as-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="7282c-322">For more information, see [Objects as parameters](resource-manager-objects-as-parameters.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7282c-323">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7282c-323">Next steps</span></span>
* <span data-ttu-id="7282c-324">Hotové šablony pro mnoho různých typů řešení najdete na stránce [Šablony Azure pro rychlý start](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="7282c-324">To view complete templates for many different types of solutions, see the [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates/).</span></span>
* <span data-ttu-id="7282c-325">Podrobnosti o funkcích, které můžete použít z v rámci šablon najdete v tématu [funkce šablon Azure Resource Manager](resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="7282c-325">For details about the functions you can use from within a template, see [Azure Resource Manager Template Functions](resource-group-template-functions.md).</span></span>
* <span data-ttu-id="7282c-326">Pokud chcete kombinovat několik šablon během nasazení, přečtěte si téma [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="7282c-326">To combine multiple templates during deployment, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="7282c-327">Musíte používat prostředky, které existují v jiné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="7282c-327">You may need to use resources that exist within a different resource group.</span></span> <span data-ttu-id="7282c-328">Tento scénář je běžný, při práci s účty úložiště a virtuální sítě, které jsou sdíleny více skupin prostředků.</span><span class="sxs-lookup"><span data-stu-id="7282c-328">This scenario is common when working with storage accounts or virtual networks that are shared across multiple resource groups.</span></span> <span data-ttu-id="7282c-329">Další informace najdete v tématu [resourceId funkce](resource-group-template-functions-resource.md#resourceid).</span><span class="sxs-lookup"><span data-stu-id="7282c-329">For more information, see the [resourceId function](resource-group-template-functions-resource.md#resourceid).</span></span>
