---
title: "aaaAzure Resource Manager strukturu šablony a syntaxe | Microsoft Docs"
description: "Popisuje hello strukturu a vlastnosti šablon Azure Resource Manager pomocí deklarativní syntaxe JSON."
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
ms.openlocfilehash: b0709852f8777c91cc1704d6bca16257a017d515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-hello-structure-and-syntax-of-azure-resource-manager-templates"></a><span data-ttu-id="6ca12-103">Pochopit strukturu hello a syntaxe šablon Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="6ca12-103">Understand hello structure and syntax of Azure Resource Manager templates</span></span>
<span data-ttu-id="6ca12-104">Toto téma popisuje strukturu hello šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="6ca12-104">This topic describes hello structure of an Azure Resource Manager template.</span></span> <span data-ttu-id="6ca12-105">Představuje různé části šablony a hello vlastnosti, které jsou k dispozici v těchto částech hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-105">It presents hello different sections of a template and hello properties that are available in those sections.</span></span> <span data-ttu-id="6ca12-106">Šablona Hello se skládá z JSON a výrazy, můžete použít hodnoty tooconstruct pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="6ca12-106">hello template consists of JSON and expressions that you can use tooconstruct values for your deployment.</span></span> <span data-ttu-id="6ca12-107">Podrobný kurz k vytvoření šablony, najdete v části [vytvoření vaší první šablony Azure Resource Manager](resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="6ca12-107">For a step-by-step tutorial on creating a template, see [Create your first Azure Resource Manager template](resource-manager-create-first-template.md).</span></span>

## <a name="template-format"></a><span data-ttu-id="6ca12-108">Formát šablony</span><span class="sxs-lookup"><span data-stu-id="6ca12-108">Template format</span></span>
<span data-ttu-id="6ca12-109">Ve své nejjednodušší struktura obsahuje šablonu hello následující prvky:</span><span class="sxs-lookup"><span data-stu-id="6ca12-109">In its simplest structure, a template contains hello following elements:</span></span>

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

| <span data-ttu-id="6ca12-110">Název elementu</span><span class="sxs-lookup"><span data-stu-id="6ca12-110">Element name</span></span> | <span data-ttu-id="6ca12-111">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="6ca12-111">Required</span></span> | <span data-ttu-id="6ca12-112">Popis</span><span class="sxs-lookup"><span data-stu-id="6ca12-112">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="6ca12-113">$schema</span><span class="sxs-lookup"><span data-stu-id="6ca12-113">$schema</span></span> |<span data-ttu-id="6ca12-114">Ano</span><span class="sxs-lookup"><span data-stu-id="6ca12-114">Yes</span></span> |<span data-ttu-id="6ca12-115">Umístění souboru schématu JSON hello, který popisuje hello verze jazyka šablony hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-115">Location of hello JSON schema file that describes hello version of hello template language.</span></span> <span data-ttu-id="6ca12-116">Použijte adresu URL hello uvedené v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-116">Use hello URL shown in hello preceding example.</span></span> |
| <span data-ttu-id="6ca12-117">contentVersion</span><span class="sxs-lookup"><span data-stu-id="6ca12-117">contentVersion</span></span> |<span data-ttu-id="6ca12-118">Ano</span><span class="sxs-lookup"><span data-stu-id="6ca12-118">Yes</span></span> |<span data-ttu-id="6ca12-119">Verze šablony hello (například 1.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="6ca12-119">Version of hello template (such as 1.0.0.0).</span></span> <span data-ttu-id="6ca12-120">Můžete zadat jakoukoli hodnotu pro tento element.</span><span class="sxs-lookup"><span data-stu-id="6ca12-120">You can provide any value for this element.</span></span> <span data-ttu-id="6ca12-121">Při nasazení prostředků pomocí šablony hello, tato hodnota může být použité toomake se, že šablona správné hello používá.</span><span class="sxs-lookup"><span data-stu-id="6ca12-121">When deploying resources using hello template, this value can be used toomake sure that hello right template is being used.</span></span> |
| <span data-ttu-id="6ca12-122">parameters</span><span class="sxs-lookup"><span data-stu-id="6ca12-122">parameters</span></span> |<span data-ttu-id="6ca12-123">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-123">No</span></span> |<span data-ttu-id="6ca12-124">Hodnoty, které jsou k dispozici po nasazení provést nasazení toocustomize prostředků.</span><span class="sxs-lookup"><span data-stu-id="6ca12-124">Values that are provided when deployment is executed toocustomize resource deployment.</span></span> |
| <span data-ttu-id="6ca12-125">proměnné</span><span class="sxs-lookup"><span data-stu-id="6ca12-125">variables</span></span> |<span data-ttu-id="6ca12-126">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-126">No</span></span> |<span data-ttu-id="6ca12-127">Hodnoty, které jsou použity jako fragmenty JSON ve výrazech jazyka šablony toosimplify šablony hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-127">Values that are used as JSON fragments in hello template toosimplify template language expressions.</span></span> |
| <span data-ttu-id="6ca12-128">Prostředky</span><span class="sxs-lookup"><span data-stu-id="6ca12-128">resources</span></span> |<span data-ttu-id="6ca12-129">Ano</span><span class="sxs-lookup"><span data-stu-id="6ca12-129">Yes</span></span> |<span data-ttu-id="6ca12-130">Typy prostředků, které jsou nasazené nebo aktualizovány v skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="6ca12-130">Resource types that are deployed or updated in a resource group.</span></span> |
| <span data-ttu-id="6ca12-131">Výstupy</span><span class="sxs-lookup"><span data-stu-id="6ca12-131">outputs</span></span> |<span data-ttu-id="6ca12-132">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-132">No</span></span> |<span data-ttu-id="6ca12-133">Hodnoty, které se vrátí po nasazení.</span><span class="sxs-lookup"><span data-stu-id="6ca12-133">Values that are returned after deployment.</span></span> |

<span data-ttu-id="6ca12-134">Každý prvek obsahuje vlastnosti, které můžete zadat.</span><span class="sxs-lookup"><span data-stu-id="6ca12-134">Each element contains properties you can set.</span></span> <span data-ttu-id="6ca12-135">Následující ukázka Hello obsahuje hello úplnou syntaxí šablony:</span><span class="sxs-lookup"><span data-stu-id="6ca12-135">hello following example contains hello full syntax for a template:</span></span>

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
                "description": "<description-of-hello parameter>" 
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

<span data-ttu-id="6ca12-136">Jsme zkontrolujte hello části šablony hello podrobněji později v tomto tématu.</span><span class="sxs-lookup"><span data-stu-id="6ca12-136">We examine hello sections of hello template in greater detail later in this topic.</span></span>

## <a name="expressions-and-functions"></a><span data-ttu-id="6ca12-137">Výrazy a funkce</span><span class="sxs-lookup"><span data-stu-id="6ca12-137">Expressions and functions</span></span>
<span data-ttu-id="6ca12-138">Základní syntaxe Hello hello šablony je JSON.</span><span class="sxs-lookup"><span data-stu-id="6ca12-138">hello basic syntax of hello template is JSON.</span></span> <span data-ttu-id="6ca12-139">Výrazy a funkce však rozšířit hodnoty na JSON hello, které jsou k dispozici v rámci šablony hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-139">However, expressions and functions extend hello JSON values available within hello template.</span></span>  <span data-ttu-id="6ca12-140">Výrazy jsou zapsané v JSON textové literály jejichž první a poslední znaky jsou hello závorky: `[` a `]`, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="6ca12-140">Expressions are written within JSON string literals whose first and last characters are hello brackets: `[` and `]`, respectively.</span></span> <span data-ttu-id="6ca12-141">Hello hodnotu výrazu hello vyhodnotí při nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-141">hello value of hello expression is evaluated when hello template is deployed.</span></span> <span data-ttu-id="6ca12-142">Při zápisu jako řetězcový literál, může být výsledkem vyhodnocení výrazu hello hello jiného typu formátu JSON, jako je například pole nebo celé číslo, v závislosti na skutečný výraz hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-142">While written as a string literal, hello result of evaluating hello expression can be of a different JSON type, such as an array or integer, depending on hello actual expression.</span></span>  <span data-ttu-id="6ca12-143">řetězcový literál začínat závorky toohave `[`, ale je interpretován jako výraz, můžete přidat řetězec navíc závorky toostart hello s `[[`.</span><span class="sxs-lookup"><span data-stu-id="6ca12-143">toohave a literal string start with a bracket `[`, but not have it interpreted as an expression, add an extra bracket toostart hello string with `[[`.</span></span>

<span data-ttu-id="6ca12-144">Výrazy se obvykle používá s operacemi tooperform funkce pro konfiguraci nasazení hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-144">Typically, you use expressions with functions tooperform operations for configuring hello deployment.</span></span> <span data-ttu-id="6ca12-145">Jenom jako v jazyce JavaScript, volání funkce jsou formátovány jako `functionName(arg1,arg2,arg3)`.</span><span class="sxs-lookup"><span data-stu-id="6ca12-145">Just like in JavaScript, function calls are formatted as `functionName(arg1,arg2,arg3)`.</span></span> <span data-ttu-id="6ca12-146">Vlastnosti odkazovat pomocí operátorů dot a [index] hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-146">You reference properties by using hello dot and [index] operators.</span></span>

<span data-ttu-id="6ca12-147">Hello následující příklad ukazuje, jak toouse několik funkcí při vytváření hodnoty:</span><span class="sxs-lookup"><span data-stu-id="6ca12-147">hello following example shows how toouse several functions when constructing values:</span></span>

```json
"variables": {
    "location": "[resourceGroup().location]",
    "usernameAndPassword": "[concat(parameters('username'), ':', parameters('password'))]",
    "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
}
```

<span data-ttu-id="6ca12-148">Hello úplný seznam funkcí šablony najdete v tématu [funkce šablon Azure Resource Manager](resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="6ca12-148">For hello full list of template functions, see [Azure Resource Manager template functions](resource-group-template-functions.md).</span></span> 

## <a name="parameters"></a><span data-ttu-id="6ca12-149">Parametry</span><span class="sxs-lookup"><span data-stu-id="6ca12-149">Parameters</span></span>
<span data-ttu-id="6ca12-150">V části Parametry hello hello šablony zadejte hodnoty, které můžete zadat při nasazování hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="6ca12-150">In hello parameters section of hello template, you specify which values you can input when deploying hello resources.</span></span> <span data-ttu-id="6ca12-151">Tyto hodnoty parametrů povolit nasazení hello toocustomize zadáním hodnoty, které jsou přizpůsobené pro konkrétní prostředí (například vývoj, testování a provozním).</span><span class="sxs-lookup"><span data-stu-id="6ca12-151">These parameter values enable you toocustomize hello deployment by providing values that are tailored for a particular environment (such as dev, test, and production).</span></span> <span data-ttu-id="6ca12-152">Nemáte tooprovide parametry v šabloně, ale bez parametrů šablony vždy nasazení hello stejné prostředky s hello stejné názvy a umístění a vlastností.</span><span class="sxs-lookup"><span data-stu-id="6ca12-152">You do not have tooprovide parameters in your template, but without parameters your template would always deploy hello same resources with hello same names, locations, and properties.</span></span>

<span data-ttu-id="6ca12-153">Definujte parametry s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="6ca12-153">You define parameters with hello following structure:</span></span>

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
            "description": "<description-of-hello parameter>" 
        }
    }
}
```

| <span data-ttu-id="6ca12-154">Název elementu</span><span class="sxs-lookup"><span data-stu-id="6ca12-154">Element name</span></span> | <span data-ttu-id="6ca12-155">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="6ca12-155">Required</span></span> | <span data-ttu-id="6ca12-156">Popis</span><span class="sxs-lookup"><span data-stu-id="6ca12-156">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="6ca12-157">Název parametru</span><span class="sxs-lookup"><span data-stu-id="6ca12-157">parameterName</span></span> |<span data-ttu-id="6ca12-158">Ano</span><span class="sxs-lookup"><span data-stu-id="6ca12-158">Yes</span></span> |<span data-ttu-id="6ca12-159">Název parametru hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-159">Name of hello parameter.</span></span> <span data-ttu-id="6ca12-160">Musí být platný identifikátor jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6ca12-160">Must be a valid JavaScript identifier.</span></span> |
| <span data-ttu-id="6ca12-161">type</span><span class="sxs-lookup"><span data-stu-id="6ca12-161">type</span></span> |<span data-ttu-id="6ca12-162">Ano</span><span class="sxs-lookup"><span data-stu-id="6ca12-162">Yes</span></span> |<span data-ttu-id="6ca12-163">Typ hodnoty parametru hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-163">Type of hello parameter value.</span></span> <span data-ttu-id="6ca12-164">Zobrazit hello seznam povolených typů za touto tabulkou.</span><span class="sxs-lookup"><span data-stu-id="6ca12-164">See hello list of allowed types after this table.</span></span> |
| <span data-ttu-id="6ca12-165">Výchozí hodnota</span><span class="sxs-lookup"><span data-stu-id="6ca12-165">defaultValue</span></span> |<span data-ttu-id="6ca12-166">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-166">No</span></span> |<span data-ttu-id="6ca12-167">Výchozí hodnota pro parametr hello, pokud není zadána žádná hodnota pro parametr hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-167">Default value for hello parameter, if no value is provided for hello parameter.</span></span> |
| <span data-ttu-id="6ca12-168">allowedValues</span><span class="sxs-lookup"><span data-stu-id="6ca12-168">allowedValues</span></span> |<span data-ttu-id="6ca12-169">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-169">No</span></span> |<span data-ttu-id="6ca12-170">Pole povolených hodnot pro jistotu, že je zadaná hodnota pravé hello toomake parametr hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-170">Array of allowed values for hello parameter toomake sure that hello right value is provided.</span></span> |
| <span data-ttu-id="6ca12-171">MinValue</span><span class="sxs-lookup"><span data-stu-id="6ca12-171">minValue</span></span> |<span data-ttu-id="6ca12-172">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-172">No</span></span> |<span data-ttu-id="6ca12-173">Hello minimální hodnotu pro parametry typ int, tato hodnota je (včetně).</span><span class="sxs-lookup"><span data-stu-id="6ca12-173">hello minimum value for int type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="6ca12-174">MaxValue</span><span class="sxs-lookup"><span data-stu-id="6ca12-174">maxValue</span></span> |<span data-ttu-id="6ca12-175">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-175">No</span></span> |<span data-ttu-id="6ca12-176">Hello maximální hodnotu pro parametry typ int, tato hodnota je (včetně).</span><span class="sxs-lookup"><span data-stu-id="6ca12-176">hello maximum value for int type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="6ca12-177">minLength</span><span class="sxs-lookup"><span data-stu-id="6ca12-177">minLength</span></span> |<span data-ttu-id="6ca12-178">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-178">No</span></span> |<span data-ttu-id="6ca12-179">Minimální délka Hello string, secureString a parametry typu pole, tato hodnota je (včetně).</span><span class="sxs-lookup"><span data-stu-id="6ca12-179">hello minimum length for string, secureString, and array type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="6ca12-180">Hodnota maxLength</span><span class="sxs-lookup"><span data-stu-id="6ca12-180">maxLength</span></span> |<span data-ttu-id="6ca12-181">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-181">No</span></span> |<span data-ttu-id="6ca12-182">Maximální délka Hello string, secureString a parametry typu pole, tato hodnota je (včetně).</span><span class="sxs-lookup"><span data-stu-id="6ca12-182">hello maximum length for string, secureString, and array type parameters, this value is inclusive.</span></span> |
| <span data-ttu-id="6ca12-183">description</span><span class="sxs-lookup"><span data-stu-id="6ca12-183">description</span></span> |<span data-ttu-id="6ca12-184">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-184">No</span></span> |<span data-ttu-id="6ca12-185">Zobrazí popis hello parametr, který je toousers prostřednictvím portálu hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-185">Description of hello parameter that is displayed toousers through hello portal.</span></span> |

<span data-ttu-id="6ca12-186">Hello povolené typy a hodnoty jsou:</span><span class="sxs-lookup"><span data-stu-id="6ca12-186">hello allowed types and values are:</span></span>

* <span data-ttu-id="6ca12-187">**řetězec**</span><span class="sxs-lookup"><span data-stu-id="6ca12-187">**string**</span></span>
* <span data-ttu-id="6ca12-188">**secureString**</span><span class="sxs-lookup"><span data-stu-id="6ca12-188">**secureString**</span></span>
* <span data-ttu-id="6ca12-189">**celá čísla**</span><span class="sxs-lookup"><span data-stu-id="6ca12-189">**int**</span></span>
* <span data-ttu-id="6ca12-190">**BOOL**</span><span class="sxs-lookup"><span data-stu-id="6ca12-190">**bool**</span></span>
* <span data-ttu-id="6ca12-191">**objekt**</span><span class="sxs-lookup"><span data-stu-id="6ca12-191">**object**</span></span> 
* <span data-ttu-id="6ca12-192">**secureObject**</span><span class="sxs-lookup"><span data-stu-id="6ca12-192">**secureObject**</span></span>
* <span data-ttu-id="6ca12-193">**pole**</span><span class="sxs-lookup"><span data-stu-id="6ca12-193">**array**</span></span>

<span data-ttu-id="6ca12-194">toospecify parametr jako volitelná, zadejte hodnotu defaultValue (může být prázdný řetězec).</span><span class="sxs-lookup"><span data-stu-id="6ca12-194">toospecify a parameter as optional, provide a defaultValue (can be an empty string).</span></span> 

<span data-ttu-id="6ca12-195">Pokud zadáte název parametru v šabloně odpovídající parametr v hello příkaz toodeploy hello šablony, je potenciální nejednoznačnosti hello hodnoty, které zadáte.</span><span class="sxs-lookup"><span data-stu-id="6ca12-195">If you specify a parameter name in your template that matches a parameter in hello command toodeploy hello template, there is potential ambiguity about hello values you provide.</span></span> <span data-ttu-id="6ca12-196">Správce prostředků řeší tento nedorozuměním přidáním hello operátory **FromTemplate** toohello parametr šablony.</span><span class="sxs-lookup"><span data-stu-id="6ca12-196">Resource Manager resolves this confusion by adding hello postfix **FromTemplate** toohello template parameter.</span></span> <span data-ttu-id="6ca12-197">Například, pokud zahrnete parametr s názvem **ResourceGroupName** v šabloně, je v konfliktu s hello **ResourceGroupName** parametr v hello [ Nové AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) rutiny.</span><span class="sxs-lookup"><span data-stu-id="6ca12-197">For example, if you include a parameter named **ResourceGroupName** in your template, it conflicts with hello **ResourceGroupName** parameter in hello [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet.</span></span> <span data-ttu-id="6ca12-198">Během nasazení se výzvami tooprovide hodnotu **ResourceGroupNameFromTemplate**.</span><span class="sxs-lookup"><span data-stu-id="6ca12-198">During deployment, you are prompted tooprovide a value for **ResourceGroupNameFromTemplate**.</span></span> <span data-ttu-id="6ca12-199">Obecně platí neměli byste toto nedorozuměním není pojmenováním parametry s hello stejný název jako parametry použité pro operace nasazení.</span><span class="sxs-lookup"><span data-stu-id="6ca12-199">In general, you should avoid this confusion by not naming parameters with hello same name as parameters used for deployment operations.</span></span>

> [!NOTE]
> <span data-ttu-id="6ca12-200">Všechna hesla, klíče a jiné tajné měli používat hello **secureString** typu.</span><span class="sxs-lookup"><span data-stu-id="6ca12-200">All passwords, keys, and other secrets should use hello **secureString** type.</span></span> <span data-ttu-id="6ca12-201">Pokud předáte citlivá data v objektu JSON, použít hello **secureObject** typu.</span><span class="sxs-lookup"><span data-stu-id="6ca12-201">If you pass sensitive data in a JSON object, use hello **secureObject** type.</span></span> <span data-ttu-id="6ca12-202">Parametry šablony s secureString nebo secureObject typů nelze přečíst po nasazení prostředků.</span><span class="sxs-lookup"><span data-stu-id="6ca12-202">Template parameters with secureString or secureObject types cannot be read after resource deployment.</span></span> 
> 
> <span data-ttu-id="6ca12-203">Hello následující položku v historii nasazení hello příkladu hello hodnotu pro řetězce a objekt, ale ne pro secureString a secureObject.</span><span class="sxs-lookup"><span data-stu-id="6ca12-203">For example, hello following entry in hello deployment history shows hello value for a string and object but not for secureString and secureObject.</span></span>
>
> ![Zobrazit hodnoty nasazení](./media/resource-group-authoring-templates/show-parameters.png)  
>

<span data-ttu-id="6ca12-205">Následující příklad ukazuje, jak Hello toodefine parametry:</span><span class="sxs-lookup"><span data-stu-id="6ca12-205">hello following example shows how toodefine parameters:</span></span>

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

<span data-ttu-id="6ca12-206">Jak tooinput hello parametr hodnoty během nasazení, naleznete v části [nasazení aplikace pomocí šablony Azure Resource Manageru](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="6ca12-206">For how tooinput hello parameter values during deployment, see [Deploy an application with Azure Resource Manager template](resource-group-template-deploy.md).</span></span> 

## <a name="variables"></a><span data-ttu-id="6ca12-207">Proměnné</span><span class="sxs-lookup"><span data-stu-id="6ca12-207">Variables</span></span>
<span data-ttu-id="6ca12-208">V části proměnných hello vytvořit hodnoty, které lze použít v celé vaší šablony.</span><span class="sxs-lookup"><span data-stu-id="6ca12-208">In hello variables section, you construct values that can be used throughout your template.</span></span> <span data-ttu-id="6ca12-209">Není nutné toodefine proměnné, ale jejich často zjednodušit vaše šablony snížením složité výrazy.</span><span class="sxs-lookup"><span data-stu-id="6ca12-209">You do not need toodefine variables, but they often simplify your template by reducing complex expressions.</span></span>

<span data-ttu-id="6ca12-210">Můžete zadat proměnné určené s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="6ca12-210">You define variables with hello following structure:</span></span>

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    }
}
```

<span data-ttu-id="6ca12-211">Následující příklad ukazuje, jak Hello toodefine proměnné, která se vytvářejí na základě dvě hodnoty parametrů:</span><span class="sxs-lookup"><span data-stu-id="6ca12-211">hello following example shows how toodefine a variable that is constructed from two parameter values:</span></span>

```json
"variables": {
    "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
}
```

<span data-ttu-id="6ca12-212">Hello další příklad ukazuje, proměnné, která je komplexního typu JSON a proměnné, které se vytvářejí na základě jiné proměnné:</span><span class="sxs-lookup"><span data-stu-id="6ca12-212">hello next example shows a variable that is a complex JSON type, and variables that are constructed from other variables:</span></span>

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

## <a name="resources"></a><span data-ttu-id="6ca12-213">Zdroje</span><span class="sxs-lookup"><span data-stu-id="6ca12-213">Resources</span></span>
<span data-ttu-id="6ca12-214">V části prostředky hello definujete hello prostředky, které jsou nasazené a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="6ca12-214">In hello resources section, you define hello resources that are deployed or updated.</span></span> <span data-ttu-id="6ca12-215">V této části můžete získat složité, protože musíte pochopit hello typy, které nasazujete tooprovide hello správné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6ca12-215">This section can get complicated because you must understand hello types you are deploying tooprovide hello right values.</span></span> <span data-ttu-id="6ca12-216">Hello konkrétní prostředky hodnoty (apiVersion, typ a vlastnosti), je nutné, aby tooset naleznete v části [definování zdrojů v šablonách Azure Resource Manager](/azure/templates/).</span><span class="sxs-lookup"><span data-stu-id="6ca12-216">For hello resource-specific values (apiVersion, type, and properties) that you need tooset, see [Define resources in Azure Resource Manager templates](/azure/templates/).</span></span> 

<span data-ttu-id="6ca12-217">Definování zdrojů s hello strukturu:</span><span class="sxs-lookup"><span data-stu-id="6ca12-217">You define resources with hello following structure:</span></span>

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

| <span data-ttu-id="6ca12-218">Název elementu</span><span class="sxs-lookup"><span data-stu-id="6ca12-218">Element name</span></span> | <span data-ttu-id="6ca12-219">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="6ca12-219">Required</span></span> | <span data-ttu-id="6ca12-220">Popis</span><span class="sxs-lookup"><span data-stu-id="6ca12-220">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="6ca12-221">Podmínka</span><span class="sxs-lookup"><span data-stu-id="6ca12-221">condition</span></span> | <span data-ttu-id="6ca12-222">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-222">No</span></span> | <span data-ttu-id="6ca12-223">Logická hodnota, která určuje, jestli je nasazené hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="6ca12-223">Boolean value that indicates whether hello resource is deployed.</span></span> |
| <span data-ttu-id="6ca12-224">apiVersion</span><span class="sxs-lookup"><span data-stu-id="6ca12-224">apiVersion</span></span> |<span data-ttu-id="6ca12-225">Ano</span><span class="sxs-lookup"><span data-stu-id="6ca12-225">Yes</span></span> |<span data-ttu-id="6ca12-226">Verze toouse hello REST API pro vytváření prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-226">Version of hello REST API toouse for creating hello resource.</span></span> |
| <span data-ttu-id="6ca12-227">type</span><span class="sxs-lookup"><span data-stu-id="6ca12-227">type</span></span> |<span data-ttu-id="6ca12-228">Ano</span><span class="sxs-lookup"><span data-stu-id="6ca12-228">Yes</span></span> |<span data-ttu-id="6ca12-229">Typ prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-229">Type of hello resource.</span></span> <span data-ttu-id="6ca12-230">Tato hodnota je kombinací hello obor názvů zprostředkovatele prostředků hello a typ prostředku hello (například **Microsoft.Storage/storageAccounts**).</span><span class="sxs-lookup"><span data-stu-id="6ca12-230">This value is a combination of hello namespace of hello resource provider and hello resource type (such as **Microsoft.Storage/storageAccounts**).</span></span> |
| <span data-ttu-id="6ca12-231">jméno</span><span class="sxs-lookup"><span data-stu-id="6ca12-231">name</span></span> |<span data-ttu-id="6ca12-232">Ano</span><span class="sxs-lookup"><span data-stu-id="6ca12-232">Yes</span></span> |<span data-ttu-id="6ca12-233">Název prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-233">Name of hello resource.</span></span> <span data-ttu-id="6ca12-234">Hello název musí splňovat omezení součást URI definované v RFC3986.</span><span class="sxs-lookup"><span data-stu-id="6ca12-234">hello name must follow URI component restrictions defined in RFC3986.</span></span> <span data-ttu-id="6ca12-235">Kromě toho služby Azure, které zveřejňují hello prostředků název toooutside strany ověřit název toomake hello, že není pokusu o toospoof jiné identity.</span><span class="sxs-lookup"><span data-stu-id="6ca12-235">In addition, Azure services that expose hello resource name toooutside parties validate hello name toomake sure it is not an attempt toospoof another identity.</span></span> |
| <span data-ttu-id="6ca12-236">location</span><span class="sxs-lookup"><span data-stu-id="6ca12-236">location</span></span> |<span data-ttu-id="6ca12-237">Je to různé.</span><span class="sxs-lookup"><span data-stu-id="6ca12-237">Varies</span></span> |<span data-ttu-id="6ca12-238">Podporovaná geo umístění hello zadaný prostředek.</span><span class="sxs-lookup"><span data-stu-id="6ca12-238">Supported geo-locations of hello provided resource.</span></span> <span data-ttu-id="6ca12-239">Můžete vybrat libovolný hello dostupných umístění, ale obvykle má smysl toopick ten, který je zavřít tooyour uživatele.</span><span class="sxs-lookup"><span data-stu-id="6ca12-239">You can select any of hello available locations, but typically it makes sense toopick one that is close tooyour users.</span></span> <span data-ttu-id="6ca12-240">Obvykle se také má smysl tooplace prostředky, které vzájemně spolupracovat v hello stejné oblasti.</span><span class="sxs-lookup"><span data-stu-id="6ca12-240">Usually, it also makes sense tooplace resources that interact with each other in hello same region.</span></span> <span data-ttu-id="6ca12-241">Většina typů prostředků vyžadují umístění, ale některé typy (například přiřazení role) nevyžadují umístění.</span><span class="sxs-lookup"><span data-stu-id="6ca12-241">Most resource types require a location, but some types (such as a role assignment) do not require a location.</span></span> <span data-ttu-id="6ca12-242">V tématu [nastavit umístění prostředku v šablonách Azure Resource Manager](resource-manager-template-location.md).</span><span class="sxs-lookup"><span data-stu-id="6ca12-242">See [Set resource location in Azure Resource Manager templates](resource-manager-template-location.md).</span></span> |
| <span data-ttu-id="6ca12-243">tags</span><span class="sxs-lookup"><span data-stu-id="6ca12-243">tags</span></span> |<span data-ttu-id="6ca12-244">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-244">No</span></span> |<span data-ttu-id="6ca12-245">Značky, které jsou přidružené hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="6ca12-245">Tags that are associated with hello resource.</span></span> <span data-ttu-id="6ca12-246">V tématu [označit prostředky v šablonách Azure Resource Manager](resource-manager-template-tags.md).</span><span class="sxs-lookup"><span data-stu-id="6ca12-246">See [Tag resources in Azure Resource Manager templates](resource-manager-template-tags.md).</span></span> |
| <span data-ttu-id="6ca12-247">Komentáře</span><span class="sxs-lookup"><span data-stu-id="6ca12-247">comments</span></span> |<span data-ttu-id="6ca12-248">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-248">No</span></span> |<span data-ttu-id="6ca12-249">Poznámky pro dokumentaci hello prostředky ve vaší šabloně</span><span class="sxs-lookup"><span data-stu-id="6ca12-249">Your notes for documenting hello resources in your template</span></span> |
| <span data-ttu-id="6ca12-250">Kopírování</span><span class="sxs-lookup"><span data-stu-id="6ca12-250">copy</span></span> |<span data-ttu-id="6ca12-251">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-251">No</span></span> |<span data-ttu-id="6ca12-252">V případě potřeby více než jednu instanci hello počet toocreate prostředky.</span><span class="sxs-lookup"><span data-stu-id="6ca12-252">If more than one instance is needed, hello number of resources toocreate.</span></span> <span data-ttu-id="6ca12-253">paralelní je výchozí režim Hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-253">hello default mode is parallel.</span></span> <span data-ttu-id="6ca12-254">Zadejte sériové režim, když mají všechny nebo z nich hello toodeploy prostředky v hello stejnou dobu.</span><span class="sxs-lookup"><span data-stu-id="6ca12-254">Specify serial mode when you do not want all or hello resources toodeploy at hello same time.</span></span> <span data-ttu-id="6ca12-255">Další informace najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="6ca12-255">For more information, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span> |
| <span data-ttu-id="6ca12-256">dependsOn</span><span class="sxs-lookup"><span data-stu-id="6ca12-256">dependsOn</span></span> |<span data-ttu-id="6ca12-257">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-257">No</span></span> |<span data-ttu-id="6ca12-258">Prostředky, které musí být nasazené, než je nasazený tento prostředek.</span><span class="sxs-lookup"><span data-stu-id="6ca12-258">Resources that must be deployed before this resource is deployed.</span></span> <span data-ttu-id="6ca12-259">Správce prostředků vyhodnotí hello závislosti mezi prostředky a nasadí je ve správném pořadí hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-259">Resource Manager evaluates hello dependencies between resources and deploys them in hello correct order.</span></span> <span data-ttu-id="6ca12-260">Pokud nejsou na sobě navzájem závislé prostředky, jsou nasazeny současně.</span><span class="sxs-lookup"><span data-stu-id="6ca12-260">When resources are not dependent on each other, they are deployed in parallel.</span></span> <span data-ttu-id="6ca12-261">Hello hodnota může být čárkami oddělený seznam prostředek názvy nebo jedinečné identifikátory prostředků.</span><span class="sxs-lookup"><span data-stu-id="6ca12-261">hello value can be a comma-separated list of a resource names or resource unique identifiers.</span></span> <span data-ttu-id="6ca12-262">Zobrazit seznam pouze těch prostředků, které jsou nasazeny v této šabloně.</span><span class="sxs-lookup"><span data-stu-id="6ca12-262">Only list resources that are deployed in this template.</span></span> <span data-ttu-id="6ca12-263">Prostředky, které nejsou v této šabloně definovány již musí existovat.</span><span class="sxs-lookup"><span data-stu-id="6ca12-263">Resources that are not defined in this template must already exist.</span></span> <span data-ttu-id="6ca12-264">Vyhněte se přidání nepotřebné závislostí, jak mohou zpomalit nasazení a vytvoření cyklické závislosti.</span><span class="sxs-lookup"><span data-stu-id="6ca12-264">Avoid adding unnecessary dependencies as they can slow your deployment and create circular dependencies.</span></span> <span data-ttu-id="6ca12-265">Pokyny v závislosti na nastavení najdete v tématu [definování závislostí v šablonách Azure Resource Manager](resource-group-define-dependencies.md).</span><span class="sxs-lookup"><span data-stu-id="6ca12-265">For guidance on setting dependencies, see [Defining dependencies in Azure Resource Manager templates](resource-group-define-dependencies.md).</span></span> |
| <span data-ttu-id="6ca12-266">properties</span><span class="sxs-lookup"><span data-stu-id="6ca12-266">properties</span></span> |<span data-ttu-id="6ca12-267">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-267">No</span></span> |<span data-ttu-id="6ca12-268">Nastavení konfigurace specifických prostředků.</span><span class="sxs-lookup"><span data-stu-id="6ca12-268">Resource-specific configuration settings.</span></span> <span data-ttu-id="6ca12-269">Hello hodnoty vlastností hello jsou hello stejné jako hello hodnoty, které zadáte v textu žádosti hello hello REST API operaci (metoda PUT) toocreate hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="6ca12-269">hello values for hello properties are hello same as hello values you provide in hello request body for hello REST API operation (PUT method) toocreate hello resource.</span></span> <span data-ttu-id="6ca12-270">Můžete také určit toocreate pole kopie více instancí vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="6ca12-270">You can also specify a copy array toocreate multiple instances of a property.</span></span> <span data-ttu-id="6ca12-271">Další informace najdete v tématu [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="6ca12-271">For more information, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span> |
| <span data-ttu-id="6ca12-272">Prostředky</span><span class="sxs-lookup"><span data-stu-id="6ca12-272">resources</span></span> |<span data-ttu-id="6ca12-273">Ne</span><span class="sxs-lookup"><span data-stu-id="6ca12-273">No</span></span> |<span data-ttu-id="6ca12-274">Podřízené prostředky, které jsou závislé na prostředku hello definovaný.</span><span class="sxs-lookup"><span data-stu-id="6ca12-274">Child resources that depend on hello resource being defined.</span></span> <span data-ttu-id="6ca12-275">Zadejte pouze typy prostředků, které jsou povoleny ve schématu hello hello nadřazené prostředku.</span><span class="sxs-lookup"><span data-stu-id="6ca12-275">Only provide resource types that are permitted by hello schema of hello parent resource.</span></span> <span data-ttu-id="6ca12-276">Hello plně kvalifikovaný typ prostředku podřízené hello obsahuje hello nadřazený typ prostředku, jako například **Microsoft.Web/sites/extensions**.</span><span class="sxs-lookup"><span data-stu-id="6ca12-276">hello fully qualified type of hello child resource includes hello parent resource type, such as **Microsoft.Web/sites/extensions**.</span></span> <span data-ttu-id="6ca12-277">Závislost na nadřazený prostředek hello není implicitní.</span><span class="sxs-lookup"><span data-stu-id="6ca12-277">Dependency on hello parent resource is not implied.</span></span> <span data-ttu-id="6ca12-278">Je nutné explicitně zadat tuto závislost.</span><span class="sxs-lookup"><span data-stu-id="6ca12-278">You must explicitly define that dependency.</span></span> |

<span data-ttu-id="6ca12-279">Hello prostředků obsahuje pole toodeploy prostředky hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-279">hello resources section contains an array of hello resources toodeploy.</span></span> <span data-ttu-id="6ca12-280">V rámci každého prostředku můžete také definovat pole podřízené prostředky.</span><span class="sxs-lookup"><span data-stu-id="6ca12-280">Within each resource, you can also define an array of child resources.</span></span> <span data-ttu-id="6ca12-281">Proto vaše oddílu prostředků může mít struktura jako:</span><span class="sxs-lookup"><span data-stu-id="6ca12-281">Therefore, your resources section could have a structure like:</span></span>

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

<span data-ttu-id="6ca12-282">Další informace o definování podřízené prostředky najdete v tématu [nastavte název a typ pro podřízený prostředek v šabloně Resource Manager](resource-manager-template-child-resource.md).</span><span class="sxs-lookup"><span data-stu-id="6ca12-282">For more information about defining child resources, see [Set name and type for child resource in Resource Manager template](resource-manager-template-child-resource.md).</span></span>

<span data-ttu-id="6ca12-283">Hello **podmínku** element určuje, jestli je nasazené hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="6ca12-283">hello **condition** element specifies whether hello resource is deployed.</span></span> <span data-ttu-id="6ca12-284">Hello hodnota pro tento element překládá tootrue, nebo hodnotu NEPRAVDA.</span><span class="sxs-lookup"><span data-stu-id="6ca12-284">hello value for this element resolves tootrue or false.</span></span> <span data-ttu-id="6ca12-285">Například toospecify zda nový účet úložiště je nasazená, použijte:</span><span class="sxs-lookup"><span data-stu-id="6ca12-285">For example, toospecify whether a new storage account is deployed, use:</span></span>

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

<span data-ttu-id="6ca12-286">Příklad použití nový nebo existující prostředek, naleznete v části [nový nebo stávající šablonu podmínku](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span><span class="sxs-lookup"><span data-stu-id="6ca12-286">For an example of using a new or existing resource, see [New or existing condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).</span></span>

<span data-ttu-id="6ca12-287">toospecify zda virtuální počítač nasazený pomocí hesla nebo klíče SSH, definovat dvě verze hello virtuálního počítače šablony a použít **podmínku** toodifferentiate využití.</span><span class="sxs-lookup"><span data-stu-id="6ca12-287">toospecify whether a virtual machine is deployed with a password or SSH key, define two versions of hello virtual machine in your template and use **condition** toodifferentiate usage.</span></span> <span data-ttu-id="6ca12-288">Předání parametru, který určuje, které toodeploy scénář.</span><span class="sxs-lookup"><span data-stu-id="6ca12-288">Pass a parameter that specifies which scenario toodeploy.</span></span>

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

<span data-ttu-id="6ca12-289">Příklad použití hesla nebo SSH klíče toodeploy virtuálního počítače, naleznete v části [uživatelské jméno nebo SSH podmínku šablony](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span><span class="sxs-lookup"><span data-stu-id="6ca12-289">For an example of using a password or SSH key toodeploy virtual machine, see [Username or SSH condition template](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).</span></span>

## <a name="outputs"></a><span data-ttu-id="6ca12-290">Výstupy</span><span class="sxs-lookup"><span data-stu-id="6ca12-290">Outputs</span></span>
<span data-ttu-id="6ca12-291">V části výstupy hello zadejte hodnoty, které jsou vráceny z nasazení.</span><span class="sxs-lookup"><span data-stu-id="6ca12-291">In hello Outputs section, you specify values that are returned from deployment.</span></span> <span data-ttu-id="6ca12-292">Například může vrátit hello URI tooaccess nasazené prostředků.</span><span class="sxs-lookup"><span data-stu-id="6ca12-292">For example, you could return hello URI tooaccess a deployed resource.</span></span>

<span data-ttu-id="6ca12-293">Hello následující příklad ukazuje hello Struktura definice výstup:</span><span class="sxs-lookup"><span data-stu-id="6ca12-293">hello following example shows hello structure of an output definition:</span></span>

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| <span data-ttu-id="6ca12-294">Název elementu</span><span class="sxs-lookup"><span data-stu-id="6ca12-294">Element name</span></span> | <span data-ttu-id="6ca12-295">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="6ca12-295">Required</span></span> | <span data-ttu-id="6ca12-296">Popis</span><span class="sxs-lookup"><span data-stu-id="6ca12-296">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="6ca12-297">outputName</span><span class="sxs-lookup"><span data-stu-id="6ca12-297">outputName</span></span> |<span data-ttu-id="6ca12-298">Ano</span><span class="sxs-lookup"><span data-stu-id="6ca12-298">Yes</span></span> |<span data-ttu-id="6ca12-299">Název hodnoty výstup hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-299">Name of hello output value.</span></span> <span data-ttu-id="6ca12-300">Musí být platný identifikátor jazyka JavaScript.</span><span class="sxs-lookup"><span data-stu-id="6ca12-300">Must be a valid JavaScript identifier.</span></span> |
| <span data-ttu-id="6ca12-301">type</span><span class="sxs-lookup"><span data-stu-id="6ca12-301">type</span></span> |<span data-ttu-id="6ca12-302">Ano</span><span class="sxs-lookup"><span data-stu-id="6ca12-302">Yes</span></span> |<span data-ttu-id="6ca12-303">Typ hodnoty výstup hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-303">Type of hello output value.</span></span> <span data-ttu-id="6ca12-304">Výstupní hodnoty podporovat hello stejné typy jako vstupní parametry šablony.</span><span class="sxs-lookup"><span data-stu-id="6ca12-304">Output values support hello same types as template input parameters.</span></span> |
| <span data-ttu-id="6ca12-305">hodnota</span><span class="sxs-lookup"><span data-stu-id="6ca12-305">value</span></span> |<span data-ttu-id="6ca12-306">Ano</span><span class="sxs-lookup"><span data-stu-id="6ca12-306">Yes</span></span> |<span data-ttu-id="6ca12-307">Výraz jazyka šablony, který se vyhodnotí a vrátí jako výstupní hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6ca12-307">Template language expression that is evaluated and returned as output value.</span></span> |

<span data-ttu-id="6ca12-308">Hello následující příklad ukazuje hodnotu, která je vrácena v části výstupy hello.</span><span class="sxs-lookup"><span data-stu-id="6ca12-308">hello following example shows a value that is returned in hello Outputs section.</span></span>

```json
"outputs": {
    "siteUri" : {
        "type" : "string",
        "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
    }
}
```

<span data-ttu-id="6ca12-309">Další informace o práci s výstupem najdete v tématu [sdílení stavu v šablonách Azure Resource Manager](best-practices-resource-manager-state.md).</span><span class="sxs-lookup"><span data-stu-id="6ca12-309">For more information about working with output, see [Sharing state in Azure Resource Manager templates](best-practices-resource-manager-state.md).</span></span>

## <a name="template-limits"></a><span data-ttu-id="6ca12-310">Omezení šablony</span><span class="sxs-lookup"><span data-stu-id="6ca12-310">Template limits</span></span>

<span data-ttu-id="6ca12-311">Omezit velikost hello vaše šablona too1 MB a každý parametr souboru too64 KB.</span><span class="sxs-lookup"><span data-stu-id="6ca12-311">Limit hello size of your template too1 MB, and each parameter file too64 KB.</span></span> <span data-ttu-id="6ca12-312">omezení 1 MB Hello platí toohello konečného stavu hello šablony po rozšířila s definic iterativní prostředků a hodnoty pro parametry a proměnné.</span><span class="sxs-lookup"><span data-stu-id="6ca12-312">hello 1-MB limit applies toohello final state of hello template after it has been expanded with iterative resource definitions, and values for variables and parameters.</span></span> 

<span data-ttu-id="6ca12-313">Také jste omezeni na:</span><span class="sxs-lookup"><span data-stu-id="6ca12-313">You are also limited to:</span></span>

* <span data-ttu-id="6ca12-314">256 parametry</span><span class="sxs-lookup"><span data-stu-id="6ca12-314">256 parameters</span></span>
* <span data-ttu-id="6ca12-315">256 proměnné</span><span class="sxs-lookup"><span data-stu-id="6ca12-315">256 variables</span></span>
* <span data-ttu-id="6ca12-316">800 prostředky (včetně počet kopií)</span><span class="sxs-lookup"><span data-stu-id="6ca12-316">800 resources (including copy count)</span></span>
* <span data-ttu-id="6ca12-317">64 výstupní hodnoty</span><span class="sxs-lookup"><span data-stu-id="6ca12-317">64 output values</span></span>
* <span data-ttu-id="6ca12-318">24,576 znaků výraz šablony</span><span class="sxs-lookup"><span data-stu-id="6ca12-318">24,576 characters in a template expression</span></span>

<span data-ttu-id="6ca12-319">Některá omezení šablony můžete překročit pomocí vnořené šablony.</span><span class="sxs-lookup"><span data-stu-id="6ca12-319">You can exceed some template limits by using a nested template.</span></span> <span data-ttu-id="6ca12-320">Další informace najdete v tématu [použití propojených šablon při nasazování prostředků Azure](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="6ca12-320">For more information, see [Using linked templates when deploying Azure resources](resource-group-linked-templates.md).</span></span> <span data-ttu-id="6ca12-321">číslo hello tooreduce parametry, proměnné nebo výstupů, můžete sloučit několik hodnot do objektu.</span><span class="sxs-lookup"><span data-stu-id="6ca12-321">tooreduce hello number of parameters, variables, or outputs, you can combine several values into an object.</span></span> <span data-ttu-id="6ca12-322">Další informace najdete v tématu [objektů jako parametry](resource-manager-objects-as-parameters.md).</span><span class="sxs-lookup"><span data-stu-id="6ca12-322">For more information, see [Objects as parameters](resource-manager-objects-as-parameters.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ca12-323">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6ca12-323">Next steps</span></span>
* <span data-ttu-id="6ca12-324">tooview dokončení šablon pro mnoho různých typů řešení, najdete v části hello [šablon Azure rychlý Start](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="6ca12-324">tooview complete templates for many different types of solutions, see hello [Azure Quickstart Templates](https://azure.microsoft.com/documentation/templates/).</span></span>
* <span data-ttu-id="6ca12-325">Podrobnosti o funkcích hello můžete použít z v rámci šablon najdete v tématu [funkce šablon Azure Resource Manager](resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="6ca12-325">For details about hello functions you can use from within a template, see [Azure Resource Manager Template Functions](resource-group-template-functions.md).</span></span>
* <span data-ttu-id="6ca12-326">toocombine více šablony během nasazení, viz [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="6ca12-326">toocombine multiple templates during deployment, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>
* <span data-ttu-id="6ca12-327">Může být nutné toouse prostředky, které existují v jiné skupině prostředků.</span><span class="sxs-lookup"><span data-stu-id="6ca12-327">You may need toouse resources that exist within a different resource group.</span></span> <span data-ttu-id="6ca12-328">Tento scénář je běžný, při práci s účty úložiště a virtuální sítě, které jsou sdíleny více skupin prostředků.</span><span class="sxs-lookup"><span data-stu-id="6ca12-328">This scenario is common when working with storage accounts or virtual networks that are shared across multiple resource groups.</span></span> <span data-ttu-id="6ca12-329">Další informace najdete v tématu hello [resourceId funkce](resource-group-template-functions-resource.md#resourceid).</span><span class="sxs-lookup"><span data-stu-id="6ca12-329">For more information, see hello [resourceId function](resource-group-template-functions-resource.md#resourceid).</span></span>
