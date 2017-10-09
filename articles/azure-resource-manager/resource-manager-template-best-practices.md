---
title: "aaaBest postupy pro vytváření šablon Resource Manageru | Microsoft Docs"
description: "Pokyny pro zjednodušit vaše šablony Azure Resource Manager."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 31b10deb-0183-47ce-a5ba-6d0ff2ae8ab3
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: tomfitz
ms.openlocfilehash: ec9bbe218c4f2c6a92ca44b5e9c9c71029e22151
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a><span data-ttu-id="cab38-103">Osvědčené postupy pro vytváření šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="cab38-103">Best practices for creating Azure Resource Manager templates</span></span>
<span data-ttu-id="cab38-104">Tyto pokyny vám pomůžou vytvořit šablony Azure Resource Manager, které jsou toouse spolehlivé a snadné.</span><span class="sxs-lookup"><span data-stu-id="cab38-104">These guidelines can help you create Azure Resource Manager templates that are reliable and easy toouse.</span></span> <span data-ttu-id="cab38-105">Hello pokyny jsou pouze návrhy.</span><span class="sxs-lookup"><span data-stu-id="cab38-105">hello guidelines are only suggestions.</span></span> <span data-ttu-id="cab38-106">Nejsou požadavky, pokud není uvedeno jinak.</span><span class="sxs-lookup"><span data-stu-id="cab38-106">They are not requirements, except where noted.</span></span> <span data-ttu-id="cab38-107">Váš scénář může vyžadovat varianta mezi hello následující přístupy nebo příklady.</span><span class="sxs-lookup"><span data-stu-id="cab38-107">Your scenario might require a variation of one of hello following approaches or examples.</span></span>

## <a name="resource-names"></a><span data-ttu-id="cab38-108">Názvy prostředků</span><span class="sxs-lookup"><span data-stu-id="cab38-108">Resource names</span></span>
<span data-ttu-id="cab38-109">Obecně platí pracujete s tři typy názvy prostředků ve službě Správce prostředků:</span><span class="sxs-lookup"><span data-stu-id="cab38-109">Generally, you work with three types of resource names in Resource Manager:</span></span>

* <span data-ttu-id="cab38-110">Názvy prostředků, které musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="cab38-110">Resource names that must be unique.</span></span>
* <span data-ttu-id="cab38-111">Názvy prostředků, které nejsou nezbytné toobe jedinečné, ale zvolíte tooprovide název, který vám může pomoct identifikovat prostředků na základě kontextu.</span><span class="sxs-lookup"><span data-stu-id="cab38-111">Resource names that are not required toobe unique, but you choose tooprovide a name that can help you identify a resource based on context.</span></span>
* <span data-ttu-id="cab38-112">Názvy prostředků, které mohou být obecný.</span><span class="sxs-lookup"><span data-stu-id="cab38-112">Resource names that can be generic.</span></span>

 <span data-ttu-id="cab38-113">Informace o omezení přístupu názvem prostředků najdete v tématu [doporučená zásady vytváření názvů pro prostředky Azure](../guidance/guidance-naming-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="cab38-113">For information about resource name restrictions, see [Recommended naming conventions for Azure resources](../guidance/guidance-naming-conventions.md).</span></span>

### <a name="unique-resource-names"></a><span data-ttu-id="cab38-114">Názvy jedinečný prostředků</span><span class="sxs-lookup"><span data-stu-id="cab38-114">Unique resource names</span></span>
<span data-ttu-id="cab38-115">Je nutné zadat název jedinečný prostředek pro jakýkoli typ prostředku, který má přístup koncový bod data.</span><span class="sxs-lookup"><span data-stu-id="cab38-115">You must provide a unique resource name for any resource type that has a data access endpoint.</span></span> <span data-ttu-id="cab38-116">Některé běžné typy prostředků, které vyžadují jedinečný název patří:</span><span class="sxs-lookup"><span data-stu-id="cab38-116">Some common resource types that require a unique name include:</span></span>

* <span data-ttu-id="cab38-117">Úložiště Azure<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="cab38-117">Azure Storage<sup>1</sup></span></span> 
* <span data-ttu-id="cab38-118">Funkce Web Apps ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="cab38-118">Web Apps feature of Azure App Service</span></span>
* <span data-ttu-id="cab38-119">SQL Server</span><span class="sxs-lookup"><span data-stu-id="cab38-119">SQL Server</span></span>
* <span data-ttu-id="cab38-120">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="cab38-120">Azure Key Vault</span></span>
* <span data-ttu-id="cab38-121">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="cab38-121">Azure Redis Cache</span></span>
* <span data-ttu-id="cab38-122">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="cab38-122">Azure Batch</span></span>
* <span data-ttu-id="cab38-123">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="cab38-123">Azure Traffic Manager</span></span>
* <span data-ttu-id="cab38-124">Azure Search</span><span class="sxs-lookup"><span data-stu-id="cab38-124">Azure Search</span></span>
* <span data-ttu-id="cab38-125">Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="cab38-125">Azure HDInsight</span></span>

<span data-ttu-id="cab38-126"><sup>1</sup> názvy účtů úložiště musí být také malými písmeny, 24 znaků nebo méně, a nemusí být všechny pomlčky.</span><span class="sxs-lookup"><span data-stu-id="cab38-126"><sup>1</sup> Storage account names also must be lowercase, 24 characters or less, and not have any hyphens.</span></span>

<span data-ttu-id="cab38-127">Pokud zadáte parametr pro název prostředku, musí se při nasazení prostředků hello zadejte jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="cab38-127">If you provide a parameter for a resource name, you must provide a unique name when you deploy hello resource.</span></span> <span data-ttu-id="cab38-128">Volitelně můžete vytvořit proměnné, která používá hello [uniqueString()](resource-group-template-functions-string.md#uniquestring) toogenerate název funkce.</span><span class="sxs-lookup"><span data-stu-id="cab38-128">Optionally, you can create a variable that uses hello [uniqueString()](resource-group-template-functions-string.md#uniquestring) function toogenerate a name.</span></span> 

<span data-ttu-id="cab38-129">Také může má tooadd předponu nebo příponu toohello **uniqueString** výsledek.</span><span class="sxs-lookup"><span data-stu-id="cab38-129">You also might want tooadd a prefix or suffix toohello **uniqueString** result.</span></span> <span data-ttu-id="cab38-130">Změny hello jedinečný název můžete další snadno identifikovat typ prostředku hello z názvu hello.</span><span class="sxs-lookup"><span data-stu-id="cab38-130">Modifying hello unique name can help you more easily identify hello resource type from hello name.</span></span> <span data-ttu-id="cab38-131">Můžete například vygenerovat jedinečný název pro účet úložiště pomocí hello následující proměnné:</span><span class="sxs-lookup"><span data-stu-id="cab38-131">For example, you can generate a unique name for a storage account by using hello following variable:</span></span>

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a><span data-ttu-id="cab38-132">Názvy prostředků pro identifikaci</span><span class="sxs-lookup"><span data-stu-id="cab38-132">Resource names for identification</span></span>
<span data-ttu-id="cab38-133">Některé typy prostředků můžete chtít tooname, avšak jejich názvy nemáte toobe jedinečný.</span><span class="sxs-lookup"><span data-stu-id="cab38-133">Some resource types you might want tooname, but their names do not have toobe unique.</span></span> <span data-ttu-id="cab38-134">Pro tyto typy prostředků můžete zadat název, který identifikuje prostředek kontextu hello a typ prostředku hello.</span><span class="sxs-lookup"><span data-stu-id="cab38-134">For these resource types, you can provide a name that identifies both hello resource context and hello resource type.</span></span> <span data-ttu-id="cab38-135">Zadejte popisný název, který pomáhá identifikovat hello prostředku v seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="cab38-135">Provide a descriptive name that helps you identify hello resource in a list of resources.</span></span> <span data-ttu-id="cab38-136">Pokud potřebujete toouse název jiného prostředku pro jiné nasazení, můžete použít parametr pro název hello:</span><span class="sxs-lookup"><span data-stu-id="cab38-136">If you need toouse a different resource name for different deployments, you can use a parameter for hello name:</span></span>

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "hello name of hello VM toocreate."
        }
    }
}
```

<span data-ttu-id="cab38-137">Pokud nepotřebujete toopass v názvu během nasazení, můžete použít proměnné:</span><span class="sxs-lookup"><span data-stu-id="cab38-137">If you do not need toopass in a name during deployment, you can use a variable:</span></span> 

```json
"variables": {
    "vmName": "demoLinuxVM"
}
```

<span data-ttu-id="cab38-138">Můžete taky použít hodnotu pevně:</span><span class="sxs-lookup"><span data-stu-id="cab38-138">You also can use a hard-coded value:</span></span>

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "demoLinuxVM",
  ...
}
```

### <a name="generic-resource-names"></a><span data-ttu-id="cab38-139">Obecný zdroj názvy</span><span class="sxs-lookup"><span data-stu-id="cab38-139">Generic resource names</span></span>
<span data-ttu-id="cab38-140">Pro typy prostředků, které většinou přistupovat prostřednictvím jiného prostředku můžete použít obecný název, který je pevně zakódovaná v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="cab38-140">For resource types that you mostly access through a different resource, you can use a generic name that is hard-coded in hello template.</span></span> <span data-ttu-id="cab38-141">Například můžete nastavit standardní, obecný název pravidla brány firewall na serveru SQL server:</span><span class="sxs-lookup"><span data-stu-id="cab38-141">For example, you can set a standard, generic name for firewall rules on a SQL server:</span></span>

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="parameters"></a><span data-ttu-id="cab38-142">Parametry</span><span class="sxs-lookup"><span data-stu-id="cab38-142">Parameters</span></span>
<span data-ttu-id="cab38-143">Hello následující informace mohou být užitečné při práci s parametry:</span><span class="sxs-lookup"><span data-stu-id="cab38-143">hello following information can be helpful when you work with parameters:</span></span>

* <span data-ttu-id="cab38-144">Minimalizujte využití parametrů.</span><span class="sxs-lookup"><span data-stu-id="cab38-144">Minimize your use of parameters.</span></span> <span data-ttu-id="cab38-145">Pokud je to možné, použijte proměnnou nebo literálovou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="cab38-145">Whenever possible, use a variable or a literal value.</span></span> <span data-ttu-id="cab38-146">Použijte parametry jenom pro tyto scénáře:</span><span class="sxs-lookup"><span data-stu-id="cab38-146">Use parameters only for these scenarios:</span></span>
   
   * <span data-ttu-id="cab38-147">Nastavení, které chcete toouse variace podle tooenvironment (SKU, velikost, kapacity).</span><span class="sxs-lookup"><span data-stu-id="cab38-147">Settings that you want toouse variations of according tooenvironment (SKU, size, capacity).</span></span>
   * <span data-ttu-id="cab38-148">Názvy prostředků, který má toospecify pro snazší identifikaci.</span><span class="sxs-lookup"><span data-stu-id="cab38-148">Resource names that you want toospecify for easy identification.</span></span>
   * <span data-ttu-id="cab38-149">Hodnoty, že používáte často toocomplete další úkoly (například správce uživatelské jméno).</span><span class="sxs-lookup"><span data-stu-id="cab38-149">Values that you use frequently toocomplete other tasks (such as an admin user name).</span></span>
   * <span data-ttu-id="cab38-150">Tajné klíče (jako jsou hesla).</span><span class="sxs-lookup"><span data-stu-id="cab38-150">Secrets (such as passwords).</span></span>
   * <span data-ttu-id="cab38-151">číslo Hello nebo pole hodnoty toouse při vytváření více instancí typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="cab38-151">hello number or array of values toouse when you create multiple instances of a resource type.</span></span>
* <span data-ttu-id="cab38-152">Použijte formát camelCase pro názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="cab38-152">Use camel case for parameter names.</span></span>
* <span data-ttu-id="cab38-153">Zadejte popis všechny parametry v metadatech hello:</span><span class="sxs-lookup"><span data-stu-id="cab38-153">Provide a description of every parameter in hello metadata:</span></span>

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "hello type of hello new storage account created toostore hello VM disks."
           }
       }
   }
   ```

* <span data-ttu-id="cab38-154">Definujte výchozí hodnoty pro parametry (s výjimkou hesla a klíče SSH):</span><span class="sxs-lookup"><span data-stu-id="cab38-154">Define default values for parameters (except for passwords and SSH keys):</span></span>
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "hello type of hello new storage account created toostore hello VM disks."
            }
        }
   }
   ```

* <span data-ttu-id="cab38-155">Použití **SecureString** pro všechny hesla a tajné klíče:</span><span class="sxs-lookup"><span data-stu-id="cab38-155">Use **SecureString** for all passwords and secrets:</span></span> 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "hello value of hello secret toostore in hello vault."
           }
       }
   }
   ```

* <span data-ttu-id="cab38-156">Pokud je to možné, nepoužívejte parametr toospecify umístění.</span><span class="sxs-lookup"><span data-stu-id="cab38-156">Whenever possible, don't use a parameter toospecify location.</span></span> <span data-ttu-id="cab38-157">Místo toho použijte hello **umístění** vlastnost hello skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="cab38-157">Instead, use hello **location** property of hello resource group.</span></span> <span data-ttu-id="cab38-158">Pomocí hello **resourceGroup () .location** výraz pro všechny prostředky, prostředky v šabloně hello nasadí hello stejné umístění jako skupina prostředků hello:</span><span class="sxs-lookup"><span data-stu-id="cab38-158">By using hello **resourceGroup().location** expression for all your resources, resources in hello template are deployed in hello same location as hello resource group:</span></span>
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         ...
     }
   ]
   ```
   
   <span data-ttu-id="cab38-159">Pokud typ prostředku je podporován pouze omezený počet umístění, můžete chtít toospecify platné umístění přímo v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="cab38-159">If a resource type is supported in only a limited number of locations, you might want toospecify a valid location directly in hello template.</span></span> <span data-ttu-id="cab38-160">Pokud musíte použít **umístění** parametr, že hodnota parametru co nejvíce sdílet s prostředky, které jsou pravděpodobně toobe v hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="cab38-160">If you must use a **location** parameter, share that parameter value as much as possible with resources that are likely toobe in hello same location.</span></span> <span data-ttu-id="cab38-161">Tím se minimalizují hello počet oznámení, která jsou uživatelé vyzváni tooprovide informace o umístění.</span><span class="sxs-lookup"><span data-stu-id="cab38-161">This minimizes hello number of times users are asked tooprovide location information.</span></span>
* <span data-ttu-id="cab38-162">Vyhněte se použití parametr nebo proměnná pro hello verze rozhraní API pro typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="cab38-162">Avoid using a parameter or variable for hello API version for a resource type.</span></span> <span data-ttu-id="cab38-163">Vlastnosti prostředku a hodnoty se může lišit podle čísla verze.</span><span class="sxs-lookup"><span data-stu-id="cab38-163">Resource properties and values can vary by version number.</span></span> <span data-ttu-id="cab38-164">IntelliSense v editoru kódu nemůže určit správné schéma hello verze rozhraní API hello nastavena tooa parametr nebo proměnná.</span><span class="sxs-lookup"><span data-stu-id="cab38-164">IntelliSense in a code editor cannot determine hello correct schema when hello API version is set tooa parameter or variable.</span></span> <span data-ttu-id="cab38-165">Místo toho pevně hello verze rozhraní API v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="cab38-165">Instead, hard-code hello API version in hello template.</span></span>

## <a name="variables"></a><span data-ttu-id="cab38-166">Proměnné</span><span class="sxs-lookup"><span data-stu-id="cab38-166">Variables</span></span>
<span data-ttu-id="cab38-167">Hello následující informace mohou být užitečné při práci s proměnnými:</span><span class="sxs-lookup"><span data-stu-id="cab38-167">hello following information can be helpful when you work with variables:</span></span>

* <span data-ttu-id="cab38-168">Proměnné lze použijte pro hodnoty, je nutné toouse více než jednou v šabloně.</span><span class="sxs-lookup"><span data-stu-id="cab38-168">Use variables for values that you need toouse more than once in a template.</span></span> <span data-ttu-id="cab38-169">Pokud hodnota je použit pouze jednou, hodnotě pevně je snazší tooread vaší šablony.</span><span class="sxs-lookup"><span data-stu-id="cab38-169">If a value is used only once, a hard-coded value makes your template easier tooread.</span></span>
* <span data-ttu-id="cab38-170">Nemůžete použít hello [odkaz](resource-group-template-functions-resource.md#reference) funkce v hello **proměnné** části hello šablony.</span><span class="sxs-lookup"><span data-stu-id="cab38-170">You cannot use hello [reference](resource-group-template-functions-resource.md#reference) function in hello **variables** section of hello template.</span></span> <span data-ttu-id="cab38-171">Hello **odkaz** funkce odvozuje svou hodnotu z stav modulu runtime hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="cab38-171">hello **reference** function derives its value from hello resource's runtime state.</span></span> <span data-ttu-id="cab38-172">Proměnné jsou však vyřešen během počáteční analýzy hello šablony hello.</span><span class="sxs-lookup"><span data-stu-id="cab38-172">However, variables are resolved during hello initial parsing of hello template.</span></span> <span data-ttu-id="cab38-173">Vytvoření hodnot, které je třeba hello **odkaz** funkce přímo v hello **prostředky** nebo **výstupy** části hello šablony.</span><span class="sxs-lookup"><span data-stu-id="cab38-173">Construct values that need hello **reference** function directly in hello **resources** or **outputs** section of hello template.</span></span>
* <span data-ttu-id="cab38-174">Zahrnout proměnné pro názvy prostředků, které musí být jedinečný, jak je popsáno v [názvy prostředků](#resource-names).</span><span class="sxs-lookup"><span data-stu-id="cab38-174">Include variables for resource names that must be unique, as described in [Resource names](#resource-names).</span></span>
* <span data-ttu-id="cab38-175">Proměnné můžete seskupovat do komplexních objektů.</span><span class="sxs-lookup"><span data-stu-id="cab38-175">You can group variables into complex objects.</span></span> <span data-ttu-id="cab38-176">Použití hello **variable.subentry** formátu tooreference hodnotu komplexního objektu.</span><span class="sxs-lookup"><span data-stu-id="cab38-176">Use hello **variable.subentry** format tooreference a value from a complex object.</span></span> <span data-ttu-id="cab38-177">Seskupování proměnných vám mohou pomoci sledovat související proměnné.</span><span class="sxs-lookup"><span data-stu-id="cab38-177">Grouping variables can help you track related variables.</span></span> <span data-ttu-id="cab38-178">Také zlepšuje čitelnost hello šablony.</span><span class="sxs-lookup"><span data-stu-id="cab38-178">It also improves readability of hello template.</span></span> <span data-ttu-id="cab38-179">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="cab38-179">Here's an example:</span></span>
   
   ```json
   "variables": {
       "storage": {
           "name": "[concat(uniqueString(resourceGroup().id),'storage')]",
           "type": "Standard_LRS"
       }
   },
   "resources": [
     {
         "type": "Microsoft.Storage/storageAccounts",
         "name": "[variables('storage').name]",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "sku": {
             "name": "[variables('storage').type]"
         },
         ...
     }
   ]
   ```
   
   > [!NOTE]
   > <span data-ttu-id="cab38-180">Komplexní objekt nemůže obsahovat výraz, který odkazuje na hodnotu z komplexního objektu.</span><span class="sxs-lookup"><span data-stu-id="cab38-180">A complex object cannot contain an expression that references a value from a complex object.</span></span> <span data-ttu-id="cab38-181">Definujte proměnnou samostatné pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="cab38-181">Define a separate variable for this purpose.</span></span>
   > 
   > 
   
     <span data-ttu-id="cab38-182">Pokročilé příklady používání komplexních objektů jako proměnné najdete v tématu [sdílet stavu v šablonách Azure Resource Manager](best-practices-resource-manager-state.md).</span><span class="sxs-lookup"><span data-stu-id="cab38-182">For advanced examples of using complex objects as variables, see [Share state in Azure Resource Manager templates](best-practices-resource-manager-state.md).</span></span>

## <a name="resources"></a><span data-ttu-id="cab38-183">Zdroje</span><span class="sxs-lookup"><span data-stu-id="cab38-183">Resources</span></span>
<span data-ttu-id="cab38-184">Hello následující informace mohou být užitečné při práci s prostředky:</span><span class="sxs-lookup"><span data-stu-id="cab38-184">hello following information can be helpful when you work with resources:</span></span>

* <span data-ttu-id="cab38-185">toohelp jiné přispěvatele pochopit účel hello hello prostředku, zadejte **komentáře** pro všechny prostředky v šabloně hello:</span><span class="sxs-lookup"><span data-stu-id="cab38-185">toohelp other contributors understand hello purpose of hello resource, specify **comments** for each resource in hello template:</span></span>
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used toostore hello VM disks.",
         ...
     }
   ]
   ```

* <span data-ttu-id="cab38-186">Můžete použít tooresources metadata tooadd značky.</span><span class="sxs-lookup"><span data-stu-id="cab38-186">You can use tags tooadd metadata tooresources.</span></span> <span data-ttu-id="cab38-187">Použijte tooadd informací o vašich prostředků.</span><span class="sxs-lookup"><span data-stu-id="cab38-187">Use metadata tooadd information about your resources.</span></span> <span data-ttu-id="cab38-188">Například můžete přidat metadata toorecord fakturačních údajů pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="cab38-188">For example, you can add metadata toorecord billing details for a resource.</span></span> <span data-ttu-id="cab38-189">Další informace najdete v tématu [pomocí značky tooorganize vašich prostředků Azure](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="cab38-189">For more information, see [Using tags tooorganize your Azure resources](resource-group-using-tags.md).</span></span>
* <span data-ttu-id="cab38-190">Pokud používáte *veřejný koncový bod* ve vaší šabloně (například Azure Blob storage veřejný koncový bod), *proveďte pevné kódování* hello oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="cab38-190">If you use a *public endpoint* in your template (such as an Azure Blob storage public endpoint), *do not hard-code* hello namespace.</span></span> <span data-ttu-id="cab38-191">Použití hello **odkaz** funkce toodynamically načíst hello oboru názvů.</span><span class="sxs-lookup"><span data-stu-id="cab38-191">Use hello **reference** function toodynamically retrieve hello namespace.</span></span> <span data-ttu-id="cab38-192">Tento přístup toodeploy hello šablony toodifferent veřejné obor názvů prostředí můžete použít beze změny ručně hello koncového bodu v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="cab38-192">You can use this approach toodeploy hello template toodifferent public namespace environments without manually changing hello endpoint in hello template.</span></span> <span data-ttu-id="cab38-193">Nastavit hello rozhraní API verze toohello stejnou verzi, kterou používáte pro účet úložiště hello v šabloně:</span><span class="sxs-lookup"><span data-stu-id="cab38-193">Set hello API version toohello same version that you are using for hello storage account in your template:</span></span>
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   <span data-ttu-id="cab38-194">Pokud účet úložiště hello je nasazena v hello stejné šablony, kterou vytváříte, není nutné obor názvů zprostředkovatele hello toospecify když odkazujete hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="cab38-194">If hello storage account is deployed in hello same template that you are creating, you do not need toospecify hello provider namespace when you reference hello resource.</span></span> <span data-ttu-id="cab38-195">Toto je hello zjednodušenou syntaxi:</span><span class="sxs-lookup"><span data-stu-id="cab38-195">This is hello simplified syntax:</span></span>
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   <span data-ttu-id="cab38-196">Pokud máte další hodnoty v šabloně, které jsou nakonfigurované toouse veřejné obor názvů, tyto hodnoty změnit tooreflect hello stejné **odkaz** funkce.</span><span class="sxs-lookup"><span data-stu-id="cab38-196">If you have other values in your template that are configured toouse a public namespace, change these values tooreflect hello same **reference** function.</span></span> <span data-ttu-id="cab38-197">Například můžete nastavit hello **storageUri** vlastnost diagnostického profilu virtuálního počítače hello:</span><span class="sxs-lookup"><span data-stu-id="cab38-197">For example, you can set hello **storageUri** property of hello virtual machine diagnostics profile:</span></span>
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   <span data-ttu-id="cab38-198">Můžete také použít stávající účet úložiště, který je v jiné skupině prostředků:</span><span class="sxs-lookup"><span data-stu-id="cab38-198">You also can reference an existing storage account that is in a different resource group:</span></span>

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* <span data-ttu-id="cab38-199">Přiřaďte veřejné IP adresy tooa virtuální počítač jenom v případě, že aplikace vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="cab38-199">Assign public IP addresses tooa virtual machine only when an application requires it.</span></span> <span data-ttu-id="cab38-200">tooconnect tooa virtuální počítač (VM) pro ladění, nebo pro správu nebo správu účely, použijte příchozích pravidel NAT, bránu virtuální sítě nebo jumpbox.</span><span class="sxs-lookup"><span data-stu-id="cab38-200">tooconnect tooa virtual machine (VM) for debugging, or for management or administrative purposes, use inbound NAT rules, a virtual network gateway, or a jumpbox.</span></span>
   
     <span data-ttu-id="cab38-201">Další informace o připojení toovirtual počítače najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="cab38-201">For more information about connecting toovirtual machines, see:</span></span>
   
   * [<span data-ttu-id="cab38-202">Spustit virtuální počítače pro architekturu N-vrstvá v Azure</span><span class="sxs-lookup"><span data-stu-id="cab38-202">Run VMs for an N-tier architecture in Azure</span></span>](../guidance/guidance-compute-n-tier-vm.md)
   * [<span data-ttu-id="cab38-203">Nastavit přístup WinRM pro virtuální počítače ve službě Správce prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="cab38-203">Set up WinRM access for VMs in Azure Resource Manager</span></span>](../virtual-machines/windows/winrm.md)
   * [<span data-ttu-id="cab38-204">Povolit tooyour externí přístup virtuálních počítačů pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="cab38-204">Allow external access tooyour VM by using hello Azure portal</span></span>](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [<span data-ttu-id="cab38-205">Povolit tooyour externí přístup virtuálních počítačů pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="cab38-205">Allow external access tooyour VM by using PowerShell</span></span>](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [<span data-ttu-id="cab38-206">Povolit externí přístup tooyour virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="cab38-206">Allow external access tooyour Linux VM by using Azure CLI</span></span>](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* <span data-ttu-id="cab38-207">Hello **domainNameLabel** vlastnost pro veřejné IP adresy musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="cab38-207">hello **domainNameLabel** property for public IP addresses must be unique.</span></span> <span data-ttu-id="cab38-208">Hello **domainNameLabel** hodnota musí být v rozmezí 3 až 63 znaků a postupujte podle pravidla hello určeného tento regulární výraz: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`.</span><span class="sxs-lookup"><span data-stu-id="cab38-208">hello **domainNameLabel** value must be between 3 and 63 characters long, and follow hello rules specified by this regular expression: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`.</span></span> <span data-ttu-id="cab38-209">Protože hello **uniqueString** funkce generuje řetězec, který je 13 znaků, hello **dnsPrefixString** parametr je omezená too50 znaků:</span><span class="sxs-lookup"><span data-stu-id="cab38-209">Because hello **uniqueString** function generates a string that is 13 characters long, hello **dnsPrefixString** parameter is limited too50 characters:</span></span>

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "hello DNS label for hello public IP address. It must be lowercase. It should match hello following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* <span data-ttu-id="cab38-210">Když přidáte rozšíření vlastních skriptů tooa a heslo, používat hello **commandToExecute** vlastnost hello **protectedSettings** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="cab38-210">When you add a password tooa custom script extension, use hello **commandToExecute** property in hello **protectedSettings** property:</span></span>
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > <span data-ttu-id="cab38-211">tooensure, které jsou tajné klíče šifrují, pokud jsou předávány jako parametry tooVMs a rozšíření, použijte hello **protectedSettings** vlastnost hello relevantní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="cab38-211">tooensure that secrets are encrypted when they are passed as parameters tooVMs and extensions, use hello **protectedSettings** property of hello relevant extensions.</span></span>
   > 
   > 

## <a name="outputs"></a><span data-ttu-id="cab38-212">Výstupy</span><span class="sxs-lookup"><span data-stu-id="cab38-212">Outputs</span></span>
<span data-ttu-id="cab38-213">Pokud chcete použít veřejné IP adresy toocreate šablony, patří **výstupy** oddíl, který vrátí podrobnosti hello IP adresy a hello plně kvalifikovaný název domény (FQDN).</span><span class="sxs-lookup"><span data-stu-id="cab38-213">If you use a template toocreate public IP addresses, include an **outputs** section that returns details of hello IP address and hello fully qualified domain name (FQDN).</span></span> <span data-ttu-id="cab38-214">Po nasazení můžete použít výstup hodnoty tooeasily načíst podrobnosti o veřejné IP adresy a plně kvalifikované názvy domén.</span><span class="sxs-lookup"><span data-stu-id="cab38-214">You can use output values tooeasily retrieve details about public IP addresses and FQDNs after deployment.</span></span> <span data-ttu-id="cab38-215">Když odkazujete hello prostředků, použijte hello rozhraní API verze, kterou jste použili toocreate ho:</span><span class="sxs-lookup"><span data-stu-id="cab38-215">When you reference hello resource, use hello API version that you used toocreate it:</span></span> 

```json
"outputs": {
    "fqdn": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').ipAddress]",
        "type": "string"
    }
}
```

## <a name="single-template-vs-nested-templates"></a><span data-ttu-id="cab38-216">Jediné šabloně vs. vnořené šablony</span><span class="sxs-lookup"><span data-stu-id="cab38-216">Single template vs. nested templates</span></span>
<span data-ttu-id="cab38-217">toodeploy řešení, můžete použít jednu šablonu nebo hlavní šablonu s více vnořených šablon.</span><span class="sxs-lookup"><span data-stu-id="cab38-217">toodeploy your solution, you can use either a single template or a main template with multiple nested templates.</span></span> <span data-ttu-id="cab38-218">Vnořené šablony jsou společné pro pokročilejší scénáře.</span><span class="sxs-lookup"><span data-stu-id="cab38-218">Nested templates are common for more advanced scenarios.</span></span> <span data-ttu-id="cab38-219">Pomocí šablony vnořené poskytuje, hello následující výhody:</span><span class="sxs-lookup"><span data-stu-id="cab38-219">Using a nested template gives you hello following advantages:</span></span>

* <span data-ttu-id="cab38-220">Lze rozčlenit řešení do cílové součásti.</span><span class="sxs-lookup"><span data-stu-id="cab38-220">You can break down a solution into targeted components.</span></span>
* <span data-ttu-id="cab38-221">Vnořené šablon s jinou hlavní šablony můžete znovu použít.</span><span class="sxs-lookup"><span data-stu-id="cab38-221">You can reuse nested templates with different main templates.</span></span>

<span data-ttu-id="cab38-222">Pokud si zvolíte toouse vnořené šablony, hello následující pokyny vám může pomoct standardizovat návrhu šablony.</span><span class="sxs-lookup"><span data-stu-id="cab38-222">If you choose toouse nested templates, hello following guidelines can help you standardize your template design.</span></span> <span data-ttu-id="cab38-223">Tyto pokyny jsou založené na [vzory pro navrhování šablon Azure Resource Manageru](best-practices-resource-manager-design-templates.md).</span><span class="sxs-lookup"><span data-stu-id="cab38-223">These guidelines are based on [patterns for designing Azure Resource Manager templates](best-practices-resource-manager-design-templates.md).</span></span> <span data-ttu-id="cab38-224">Doporučujeme, abyste k návrhu, který má hello následující šablony:</span><span class="sxs-lookup"><span data-stu-id="cab38-224">We recommend a design that has hello following templates:</span></span>

* <span data-ttu-id="cab38-225">**Hlavní šablonu** (azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="cab38-225">**Main template** (azuredeploy.json).</span></span> <span data-ttu-id="cab38-226">Používejte pro vstupní parametry hello.</span><span class="sxs-lookup"><span data-stu-id="cab38-226">Use for hello input parameters.</span></span>
* <span data-ttu-id="cab38-227">**Sdílené prostředky šablony**.</span><span class="sxs-lookup"><span data-stu-id="cab38-227">**Shared resources template**.</span></span> <span data-ttu-id="cab38-228">Použití toodeploy sdílené prostředky, které používají jiné prostředky (například virtuální sítě a dostupnost sady).</span><span class="sxs-lookup"><span data-stu-id="cab38-228">Use toodeploy shared resources that all other resources use (for example, virtual network and availability sets).</span></span> <span data-ttu-id="cab38-229">Použití hello **dependsOn** tooensure výrazu, tuto šablonu je nasadit před další šablony.</span><span class="sxs-lookup"><span data-stu-id="cab38-229">Use hello **dependsOn** expression tooensure that this template is deployed before other templates.</span></span>
* <span data-ttu-id="cab38-230">**Volitelné prostředků šablony**.</span><span class="sxs-lookup"><span data-stu-id="cab38-230">**Optional resources template**.</span></span> <span data-ttu-id="cab38-231">Použití tooconditionally nasadit prostředky v závislosti na parametr (například jumpbox).</span><span class="sxs-lookup"><span data-stu-id="cab38-231">Use tooconditionally deploy resources based on a parameter (for example, a jumpbox).</span></span>
* <span data-ttu-id="cab38-232">**Šablony prostředků člen**.</span><span class="sxs-lookup"><span data-stu-id="cab38-232">**Member resources template**.</span></span> <span data-ttu-id="cab38-233">Každý typ instance v aplikační vrstvě má svou vlastní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="cab38-233">Each instance type within an application tier has its own configuration.</span></span> <span data-ttu-id="cab38-234">V rámci vrstvy můžete definovat typy jiné instance.</span><span class="sxs-lookup"><span data-stu-id="cab38-234">Within a tier, you can define different instance types.</span></span> <span data-ttu-id="cab38-235">(Například hello první instance vytvoří cluster a další instance jsou přidány toohello existujícího clusteru.) Každý typ instance má svou vlastní šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="cab38-235">(For example, hello first instance creates a cluster, and additional instances are added toohello existing cluster.) Each instance type has its own deployment template.</span></span>
* <span data-ttu-id="cab38-236">**Skripty**.</span><span class="sxs-lookup"><span data-stu-id="cab38-236">**Scripts**.</span></span> <span data-ttu-id="cab38-237">Široce opakovaně použitelné skripty platí pro každý typ instance (například inicializovat a formát další disky).</span><span class="sxs-lookup"><span data-stu-id="cab38-237">Widely reusable scripts are applicable for each instance type (for example, initialize and format additional disks).</span></span> <span data-ttu-id="cab38-238">Vlastní skripty, které vytvoříte za účelem přizpůsobení konkrétní se liší, v závislosti na typu instance hello.</span><span class="sxs-lookup"><span data-stu-id="cab38-238">Custom scripts that you create for a specific customization purpose are different, based on hello instance type.</span></span>

![Vnořené šablony](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

<span data-ttu-id="cab38-240">Další informace najdete v tématu [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="cab38-240">For more information, see [Use linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="conditionally-link-toonested-templates"></a><span data-ttu-id="cab38-241">Podmíněná propojení toonested šablony</span><span class="sxs-lookup"><span data-stu-id="cab38-241">Conditionally link toonested templates</span></span>
<span data-ttu-id="cab38-242">Můžete použít parametr tooconditionally odkaz toonested šablonách.</span><span class="sxs-lookup"><span data-stu-id="cab38-242">You can use a parameter tooconditionally link toonested templates.</span></span> <span data-ttu-id="cab38-243">Parametr Hello stane součástí hello identifikátor URI pro šablonu hello:</span><span class="sxs-lookup"><span data-stu-id="cab38-243">hello parameter becomes part of hello URI for hello template:</span></span>

```json
"parameters": {
    "newOrExisting": {
        "type": "String",
        "allowedValues": [
            "new",
            "existing"
        ]
    }
},
"variables": {
    "templatelink": "[concat('https://raw.githubusercontent.com/Contoso/Templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
},
"resources": [
    {
        "apiVersion": "2015-01-01",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
            "mode": "incremental",
            "templateLink": {
                "uri": "[variables('templatelink')]",
                "contentVersion": "1.0.0.0"
            },
            "parameters": {
            }
        }
    }
]
```

## <a name="template-format"></a><span data-ttu-id="cab38-244">Formát šablony</span><span class="sxs-lookup"><span data-stu-id="cab38-244">Template format</span></span>
<span data-ttu-id="cab38-245">Je dobrým zvykem toopass šablony prostřednictvím validátor JSON.</span><span class="sxs-lookup"><span data-stu-id="cab38-245">It's a good practice toopass your template through a JSON validator.</span></span> <span data-ttu-id="cab38-246">Validátor můžete odebrat nadbytečné čárkami, kulaté závorky a hranaté závorky, které může způsobit chybu během nasazení.</span><span class="sxs-lookup"><span data-stu-id="cab38-246">A validator can help you remove extraneous commas, parentheses, and brackets that might cause an error during deployment.</span></span> <span data-ttu-id="cab38-247">Zkuste [JSONLint](http://jsonlint.com/) nebo linter balíček pro své oblíbené úpravou prostředí (Visual Studio Code, Atom, Sublime Text, Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="cab38-247">Try [JSONLint](http://jsonlint.com/) or a linter package for your favorite editing environment (Visual Studio Code, Atom, Sublime Text, Visual Studio).</span></span>

<span data-ttu-id="cab38-248">Je také vhodné tooformat vaše struktury JSON pro lepší čitelnost.</span><span class="sxs-lookup"><span data-stu-id="cab38-248">It's also a good idea tooformat your JSON for better readability.</span></span> <span data-ttu-id="cab38-249">Můžete vytvořit balíček formátovací modul JSON pro vaše místní editor.</span><span class="sxs-lookup"><span data-stu-id="cab38-249">You can use a JSON formatter package for your local editor.</span></span> <span data-ttu-id="cab38-250">V sadě Visual Studio, tooformat hello dokumentu, stiskněte klávesu **Ctrl + K, Ctrl + D**.</span><span class="sxs-lookup"><span data-stu-id="cab38-250">In Visual Studio, tooformat hello document, press **Ctrl+K, Ctrl+D**.</span></span> <span data-ttu-id="cab38-251">V sadě Visual Studio Code stiskněte **Alt + Shift + F**.</span><span class="sxs-lookup"><span data-stu-id="cab38-251">In Visual Studio Code, press **Alt+Shift+F**.</span></span> <span data-ttu-id="cab38-252">Pokud vaše místní editor nebude formátu hello dokumentu, můžete použít [online formátování](https://www.bing.com/search?q=json+formatter).</span><span class="sxs-lookup"><span data-stu-id="cab38-252">If your local editor doesn't format hello document, you can use an [online formatter](https://www.bing.com/search?q=json+formatter).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cab38-253">Další kroky</span><span class="sxs-lookup"><span data-stu-id="cab38-253">Next steps</span></span>
* <span data-ttu-id="cab38-254">Pokyny na architekturu řešení virtuálních počítačů najdete v tématu [spustit virtuální počítač s Windows v Azure](../guidance/guidance-compute-single-vm.md) a [spuštění virtuálního počítače s Linuxem v Azure](../guidance/guidance-compute-single-vm-linux.md).</span><span class="sxs-lookup"><span data-stu-id="cab38-254">For guidance on architecting your solution for virtual machines, see [Run a Windows VM in Azure](../guidance/guidance-compute-single-vm.md) and [Run a Linux VM in Azure](../guidance/guidance-compute-single-vm-linux.md).</span></span>
* <span data-ttu-id="cab38-255">Informace o nastavení účtu úložiště, najdete v části [kontrolní seznam výkonu a škálovatelnosti Azure Storage](../storage/common/storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="cab38-255">For guidance on setting up a storage account, see [Azure Storage performance and scalability checklist](../storage/common/storage-performance-checklist.md).</span></span>
* <span data-ttu-id="cab38-256">toolearn o tom, jak podniku můžete použít tooeffectively Resource Manager můžete spravovat odběry, najdete v části [Azure enterprise vygenerované uživatelské rozhraní: doporučený předplatné zásad správného řízení](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="cab38-256">toolearn about how an enterprise can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold: Prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

