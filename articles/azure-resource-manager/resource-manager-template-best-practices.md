---
title: "Osvědčené postupy pro vytváření šablon Resource Manager | Microsoft Docs"
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
ms.openlocfilehash: a23301ba88279af3f7bf4d353ae808e9eeb0900d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a><span data-ttu-id="aa0c0-103">Osvědčené postupy pro vytváření šablon Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="aa0c0-103">Best practices for creating Azure Resource Manager templates</span></span>
<span data-ttu-id="aa0c0-104">Tyto pokyny vám pomůžou vytvořit šablony Azure Resource Manager, které jsou spolehlivé a snadno se používá.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-104">These guidelines can help you create Azure Resource Manager templates that are reliable and easy to use.</span></span> <span data-ttu-id="aa0c0-105">Pokyny jsou pouze návrhy.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-105">The guidelines are only suggestions.</span></span> <span data-ttu-id="aa0c0-106">Nejsou požadavky, pokud není uvedeno jinak.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-106">They are not requirements, except where noted.</span></span> <span data-ttu-id="aa0c0-107">Váš scénář může vyžadovat varianta jednu z následujících přístupy nebo příklady.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-107">Your scenario might require a variation of one of the following approaches or examples.</span></span>

## <a name="resource-names"></a><span data-ttu-id="aa0c0-108">Názvy prostředků</span><span class="sxs-lookup"><span data-stu-id="aa0c0-108">Resource names</span></span>
<span data-ttu-id="aa0c0-109">Obecně platí pracujete s tři typy názvy prostředků ve službě Správce prostředků:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-109">Generally, you work with three types of resource names in Resource Manager:</span></span>

* <span data-ttu-id="aa0c0-110">Názvy prostředků, které musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-110">Resource names that must be unique.</span></span>
* <span data-ttu-id="aa0c0-111">Názvy prostředků, které nemusí být jedinečný, ale můžete rozhodnout pro poskytnutí název, který vám pomůže určit prostředek na základě kontextu.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-111">Resource names that are not required to be unique, but you choose to provide a name that can help you identify a resource based on context.</span></span>
* <span data-ttu-id="aa0c0-112">Názvy prostředků, které mohou být obecný.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-112">Resource names that can be generic.</span></span>

 <span data-ttu-id="aa0c0-113">Informace o omezení přístupu názvem prostředků najdete v tématu [doporučená zásady vytváření názvů pro prostředky Azure](../guidance/guidance-naming-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-113">For information about resource name restrictions, see [Recommended naming conventions for Azure resources](../guidance/guidance-naming-conventions.md).</span></span>

### <a name="unique-resource-names"></a><span data-ttu-id="aa0c0-114">Názvy jedinečný prostředků</span><span class="sxs-lookup"><span data-stu-id="aa0c0-114">Unique resource names</span></span>
<span data-ttu-id="aa0c0-115">Je nutné zadat název jedinečný prostředek pro jakýkoli typ prostředku, který má přístup koncový bod data.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-115">You must provide a unique resource name for any resource type that has a data access endpoint.</span></span> <span data-ttu-id="aa0c0-116">Některé běžné typy prostředků, které vyžadují jedinečný název patří:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-116">Some common resource types that require a unique name include:</span></span>

* <span data-ttu-id="aa0c0-117">Úložiště Azure<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="aa0c0-117">Azure Storage<sup>1</sup></span></span> 
* <span data-ttu-id="aa0c0-118">Funkce Web Apps ve službě Azure App Service</span><span class="sxs-lookup"><span data-stu-id="aa0c0-118">Web Apps feature of Azure App Service</span></span>
* <span data-ttu-id="aa0c0-119">SQL Server</span><span class="sxs-lookup"><span data-stu-id="aa0c0-119">SQL Server</span></span>
* <span data-ttu-id="aa0c0-120">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="aa0c0-120">Azure Key Vault</span></span>
* <span data-ttu-id="aa0c0-121">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="aa0c0-121">Azure Redis Cache</span></span>
* <span data-ttu-id="aa0c0-122">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="aa0c0-122">Azure Batch</span></span>
* <span data-ttu-id="aa0c0-123">Azure Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="aa0c0-123">Azure Traffic Manager</span></span>
* <span data-ttu-id="aa0c0-124">Azure Search</span><span class="sxs-lookup"><span data-stu-id="aa0c0-124">Azure Search</span></span>
* <span data-ttu-id="aa0c0-125">Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="aa0c0-125">Azure HDInsight</span></span>

<span data-ttu-id="aa0c0-126"><sup>1</sup> názvy účtů úložiště musí být také malými písmeny, 24 znaků nebo méně, a nemusí být všechny pomlčky.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-126"><sup>1</sup> Storage account names also must be lowercase, 24 characters or less, and not have any hyphens.</span></span>

<span data-ttu-id="aa0c0-127">Pokud zadáte parametr pro název prostředku, musí se při nasazení prostředku zadejte jedinečný název.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-127">If you provide a parameter for a resource name, you must provide a unique name when you deploy the resource.</span></span> <span data-ttu-id="aa0c0-128">Volitelně můžete vytvořit proměnné, která se používá [uniqueString()](resource-group-template-functions-string.md#uniquestring) funkce při generování názvu.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-128">Optionally, you can create a variable that uses the [uniqueString()](resource-group-template-functions-string.md#uniquestring) function to generate a name.</span></span> 

<span data-ttu-id="aa0c0-129">Můžete také chtít přidat předponu nebo příponu k **uniqueString** výsledek.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-129">You also might want to add a prefix or suffix to the **uniqueString** result.</span></span> <span data-ttu-id="aa0c0-130">Úprava jedinečný název můžete další snadno identifikovat typ prostředku z názvu.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-130">Modifying the unique name can help you more easily identify the resource type from the name.</span></span> <span data-ttu-id="aa0c0-131">Můžete například vygenerovat jedinečný název pro účet úložiště pomocí následující proměnnou:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-131">For example, you can generate a unique name for a storage account by using the following variable:</span></span>

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a><span data-ttu-id="aa0c0-132">Názvy prostředků pro identifikaci</span><span class="sxs-lookup"><span data-stu-id="aa0c0-132">Resource names for identification</span></span>
<span data-ttu-id="aa0c0-133">Některé typy prostředků, které můžete chtít názvu, ale jejich názvy nemusí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-133">Some resource types you might want to name, but their names do not have to be unique.</span></span> <span data-ttu-id="aa0c0-134">Pro tyto typy prostředků můžete zadat název, který identifikuje prostředek kontextu a typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-134">For these resource types, you can provide a name that identifies both the resource context and the resource type.</span></span> <span data-ttu-id="aa0c0-135">Zadejte popisný název, který pomáhá identifikovat prostředku v seznamu prostředků.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-135">Provide a descriptive name that helps you identify the resource in a list of resources.</span></span> <span data-ttu-id="aa0c0-136">Pokud budete muset použít jiný název prostředku pro jiné nasazení, můžete použít parametr pro název:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-136">If you need to use a different resource name for different deployments, you can use a parameter for the name:</span></span>

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "The name of the VM to create."
        }
    }
}
```

<span data-ttu-id="aa0c0-137">Pokud není nutné předávat název během nasazení, můžete použít proměnné:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-137">If you do not need to pass in a name during deployment, you can use a variable:</span></span> 

```json
"variables": {
    "vmName": "demoLinuxVM"
}
```

<span data-ttu-id="aa0c0-138">Můžete taky použít hodnotu pevně:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-138">You also can use a hard-coded value:</span></span>

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "demoLinuxVM",
  ...
}
```

### <a name="generic-resource-names"></a><span data-ttu-id="aa0c0-139">Obecný zdroj názvy</span><span class="sxs-lookup"><span data-stu-id="aa0c0-139">Generic resource names</span></span>
<span data-ttu-id="aa0c0-140">Pro typy prostředků, které většinou přistupovat prostřednictvím jiného prostředku můžete použít obecný název, který je pevně zakódovaná v šabloně.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-140">For resource types that you mostly access through a different resource, you can use a generic name that is hard-coded in the template.</span></span> <span data-ttu-id="aa0c0-141">Například můžete nastavit standardní, obecný název pravidla brány firewall na serveru SQL server:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-141">For example, you can set a standard, generic name for firewall rules on a SQL server:</span></span>

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="parameters"></a><span data-ttu-id="aa0c0-142">Parametry</span><span class="sxs-lookup"><span data-stu-id="aa0c0-142">Parameters</span></span>
<span data-ttu-id="aa0c0-143">Při práci s parametry, může být užitečné následující informace:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-143">The following information can be helpful when you work with parameters:</span></span>

* <span data-ttu-id="aa0c0-144">Minimalizujte využití parametrů.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-144">Minimize your use of parameters.</span></span> <span data-ttu-id="aa0c0-145">Pokud je to možné, použijte proměnnou nebo literálovou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-145">Whenever possible, use a variable or a literal value.</span></span> <span data-ttu-id="aa0c0-146">Použijte parametry jenom pro tyto scénáře:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-146">Use parameters only for these scenarios:</span></span>
   
   * <span data-ttu-id="aa0c0-147">Nastavení, které chcete použít variace podle prostředí (SKU, velikost, kapacity).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-147">Settings that you want to use variations of according to environment (SKU, size, capacity).</span></span>
   * <span data-ttu-id="aa0c0-148">Názvy prostředků, které chcete určit pro snazší identifikaci.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-148">Resource names that you want to specify for easy identification.</span></span>
   * <span data-ttu-id="aa0c0-149">Hodnoty, které používáte k provedení dalších úloh (například správce uživatelské jméno).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-149">Values that you use frequently to complete other tasks (such as an admin user name).</span></span>
   * <span data-ttu-id="aa0c0-150">Tajné klíče (jako jsou hesla).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-150">Secrets (such as passwords).</span></span>
   * <span data-ttu-id="aa0c0-151">Číslo nebo pole hodnot, který má použít při vytváření více instancí typu prostředku.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-151">The number or array of values to use when you create multiple instances of a resource type.</span></span>
* <span data-ttu-id="aa0c0-152">Použijte formát camelCase pro názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-152">Use camel case for parameter names.</span></span>
* <span data-ttu-id="aa0c0-153">Zadejte popis všechny parametry v metadatech:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-153">Provide a description of every parameter in the metadata:</span></span>

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "The type of the new storage account created to store the VM disks."
           }
       }
   }
   ```

* <span data-ttu-id="aa0c0-154">Definujte výchozí hodnoty pro parametry (s výjimkou hesla a klíče SSH):</span><span class="sxs-lookup"><span data-stu-id="aa0c0-154">Define default values for parameters (except for passwords and SSH keys):</span></span>
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "The type of the new storage account created to store the VM disks."
            }
        }
   }
   ```

* <span data-ttu-id="aa0c0-155">Použití **SecureString** pro všechny hesla a tajné klíče:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-155">Use **SecureString** for all passwords and secrets:</span></span> 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "The value of the secret to store in the vault."
           }
       }
   }
   ```

* <span data-ttu-id="aa0c0-156">Pokud je to možné, nepoužívejte parametr k určení umístění.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-156">Whenever possible, don't use a parameter to specify location.</span></span> <span data-ttu-id="aa0c0-157">Místo toho použijte **umístění** vlastnost skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-157">Instead, use the **location** property of the resource group.</span></span> <span data-ttu-id="aa0c0-158">Pomocí **resourceGroup () .location** výraz pro všechny vaše prostředky, v prostředcích v šabloně nasazených ve stejném umístění jako pro skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-158">By using the **resourceGroup().location** expression for all your resources, resources in the template are deployed in the same location as the resource group:</span></span>
   
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
   
   <span data-ttu-id="aa0c0-159">Pokud typ prostředku je podporován pouze omezený počet umístění, můžete chtít zadat platné umístění přímo v šabloně.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-159">If a resource type is supported in only a limited number of locations, you might want to specify a valid location directly in the template.</span></span> <span data-ttu-id="aa0c0-160">Pokud musíte použít **umístění** parametr, že hodnota parametru co nejvíce sdílet s prostředky, které by mohly být ve stejném umístění.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-160">If you must use a **location** parameter, share that parameter value as much as possible with resources that are likely to be in the same location.</span></span> <span data-ttu-id="aa0c0-161">Tím se minimalizují počet, který uživatelé vyzváni k zadání informace o umístění.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-161">This minimizes the number of times users are asked to provide location information.</span></span>
* <span data-ttu-id="aa0c0-162">Vyhněte se použití parametr nebo proměnná pro verze rozhraní API pro typ prostředku.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-162">Avoid using a parameter or variable for the API version for a resource type.</span></span> <span data-ttu-id="aa0c0-163">Vlastnosti prostředku a hodnoty se může lišit podle čísla verze.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-163">Resource properties and values can vary by version number.</span></span> <span data-ttu-id="aa0c0-164">IntelliSense v editoru kódu nemůže určit správné schéma verze rozhraní API je nastavena na parametr nebo proměnná.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-164">IntelliSense in a code editor cannot determine the correct schema when the API version is set to a parameter or variable.</span></span> <span data-ttu-id="aa0c0-165">Místo toho pevný kódu rozhraní API verze v šabloně.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-165">Instead, hard-code the API version in the template.</span></span>

## <a name="variables"></a><span data-ttu-id="aa0c0-166">Proměnné</span><span class="sxs-lookup"><span data-stu-id="aa0c0-166">Variables</span></span>
<span data-ttu-id="aa0c0-167">Při práci s proměnnými, může být užitečné následující informace:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-167">The following information can be helpful when you work with variables:</span></span>

* <span data-ttu-id="aa0c0-168">Použití proměnných hodnot, které je nutné použít více než jednou v šabloně.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-168">Use variables for values that you need to use more than once in a template.</span></span> <span data-ttu-id="aa0c0-169">Pokud hodnota je použit pouze jednou, hodnotu pevně usnadňuje šablony čtení.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-169">If a value is used only once, a hard-coded value makes your template easier to read.</span></span>
* <span data-ttu-id="aa0c0-170">Nelze použít [odkaz](resource-group-template-functions-resource.md#reference) fungovat v **proměnné** část šablony.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-170">You cannot use the [reference](resource-group-template-functions-resource.md#reference) function in the **variables** section of the template.</span></span> <span data-ttu-id="aa0c0-171">**Odkaz** funkce odvozuje svou hodnotu z prostředku stav modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-171">The **reference** function derives its value from the resource's runtime state.</span></span> <span data-ttu-id="aa0c0-172">Proměnné jsou však vyřešeny během počáteční analýzy šablony.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-172">However, variables are resolved during the initial parsing of the template.</span></span> <span data-ttu-id="aa0c0-173">Konstrukce hodnoty kterém musí **odkaz** funkce přímo v **prostředky** nebo **výstupy** část šablony.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-173">Construct values that need the **reference** function directly in the **resources** or **outputs** section of the template.</span></span>
* <span data-ttu-id="aa0c0-174">Zahrnout proměnné pro názvy prostředků, které musí být jedinečný, jak je popsáno v [názvy prostředků](#resource-names).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-174">Include variables for resource names that must be unique, as described in [Resource names](#resource-names).</span></span>
* <span data-ttu-id="aa0c0-175">Proměnné můžete seskupovat do komplexních objektů.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-175">You can group variables into complex objects.</span></span> <span data-ttu-id="aa0c0-176">Použití **variable.subentry** formátu k odkazování hodnotu z komplexního objektu.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-176">Use the **variable.subentry** format to reference a value from a complex object.</span></span> <span data-ttu-id="aa0c0-177">Seskupování proměnných vám mohou pomoci sledovat související proměnné.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-177">Grouping variables can help you track related variables.</span></span> <span data-ttu-id="aa0c0-178">Také zlepšuje čitelnost šablony.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-178">It also improves readability of the template.</span></span> <span data-ttu-id="aa0c0-179">Tady je příklad:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-179">Here's an example:</span></span>
   
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
   > <span data-ttu-id="aa0c0-180">Komplexní objekt nemůže obsahovat výraz, který odkazuje na hodnotu z komplexního objektu.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-180">A complex object cannot contain an expression that references a value from a complex object.</span></span> <span data-ttu-id="aa0c0-181">Definujte proměnnou samostatné pro tento účel.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-181">Define a separate variable for this purpose.</span></span>
   > 
   > 
   
     <span data-ttu-id="aa0c0-182">Pokročilé příklady používání komplexních objektů jako proměnné najdete v tématu [sdílet stavu v šablonách Azure Resource Manager](best-practices-resource-manager-state.md).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-182">For advanced examples of using complex objects as variables, see [Share state in Azure Resource Manager templates](best-practices-resource-manager-state.md).</span></span>

## <a name="resources"></a><span data-ttu-id="aa0c0-183">Zdroje</span><span class="sxs-lookup"><span data-stu-id="aa0c0-183">Resources</span></span>
<span data-ttu-id="aa0c0-184">Při práci s prostředky, může být užitečné následující informace:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-184">The following information can be helpful when you work with resources:</span></span>

* <span data-ttu-id="aa0c0-185">Chcete-li jiné přispěvatele pochopit účel prostředku, zadejte **komentáře** pro každý zdroj v šabloně:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-185">To help other contributors understand the purpose of the resource, specify **comments** for each resource in the template:</span></span>
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used to store the VM disks.",
         ...
     }
   ]
   ```

* <span data-ttu-id="aa0c0-186">Chcete-li přidat metadata k prostředkům můžete použít značky.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-186">You can use tags to add metadata to resources.</span></span> <span data-ttu-id="aa0c0-187">Metadata použijte k přidání informací o vašich prostředků.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-187">Use metadata to add information about your resources.</span></span> <span data-ttu-id="aa0c0-188">Například můžete přidat metadata k zaznamenání fakturačních údajů pro prostředek.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-188">For example, you can add metadata to record billing details for a resource.</span></span> <span data-ttu-id="aa0c0-189">Další informace najdete v tématu [použití značek k uspořádání prostředků Azure](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-189">For more information, see [Using tags to organize your Azure resources](resource-group-using-tags.md).</span></span>
* <span data-ttu-id="aa0c0-190">Pokud používáte *veřejný koncový bod* ve vaší šabloně (například Azure Blob storage veřejný koncový bod), *proveďte pevné kódování* obor názvů.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-190">If you use a *public endpoint* in your template (such as an Azure Blob storage public endpoint), *do not hard-code* the namespace.</span></span> <span data-ttu-id="aa0c0-191">Použití **odkaz** funkce, která se dynamicky načíst obor názvů.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-191">Use the **reference** function to dynamically retrieve the namespace.</span></span> <span data-ttu-id="aa0c0-192">Tento postup můžete použít k nasazení šablony do prostředí jiný obor názvů veřejné beze změny ručně koncového bodu v šabloně.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-192">You can use this approach to deploy the template to different public namespace environments without manually changing the endpoint in the template.</span></span> <span data-ttu-id="aa0c0-193">Verze rozhraní API se nastaví na stejnou verzi, kterou používáte pro účet úložiště v šabloně:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-193">Set the API version to the same version that you are using for the storage account in your template:</span></span>
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   <span data-ttu-id="aa0c0-194">Pokud účet úložiště je nasazen do stejné šablony, kterou vytváříte, není potřeba zadat obor názvů zprostředkovatele, když odkazujete na prostředek.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-194">If the storage account is deployed in the same template that you are creating, you do not need to specify the provider namespace when you reference the resource.</span></span> <span data-ttu-id="aa0c0-195">Toto je zjednodušenou syntaxi:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-195">This is the simplified syntax:</span></span>
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   <span data-ttu-id="aa0c0-196">Pokud máte jiné hodnoty v šabloně, které jsou nakonfigurovány pro použití veřejného obor názvů, změňte tyto hodnoty tak, aby odrážela stejné **odkaz** funkce.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-196">If you have other values in your template that are configured to use a public namespace, change these values to reflect the same **reference** function.</span></span> <span data-ttu-id="aa0c0-197">Například můžete nastavit **storageUri** vlastnost diagnostického profilu virtuálního počítače:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-197">For example, you can set the **storageUri** property of the virtual machine diagnostics profile:</span></span>
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   <span data-ttu-id="aa0c0-198">Můžete také použít stávající účet úložiště, který je v jiné skupině prostředků:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-198">You also can reference an existing storage account that is in a different resource group:</span></span>

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* <span data-ttu-id="aa0c0-199">Přiřaďte veřejné IP adresy k virtuálnímu počítači jenom v případě, že aplikace vyžaduje.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-199">Assign public IP addresses to a virtual machine only when an application requires it.</span></span> <span data-ttu-id="aa0c0-200">Pro připojení k virtuálnímu počítači (VM) pro ladění, nebo pro správu nebo správu účely, použijte příchozích pravidel NAT, bránu virtuální sítě nebo jumpbox.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-200">To connect to a virtual machine (VM) for debugging, or for management or administrative purposes, use inbound NAT rules, a virtual network gateway, or a jumpbox.</span></span>
   
     <span data-ttu-id="aa0c0-201">Další informace o připojení k virtuálním počítačům, najdete v tématu:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-201">For more information about connecting to virtual machines, see:</span></span>
   
   * [<span data-ttu-id="aa0c0-202">Spustit virtuální počítače pro architekturu N-vrstvá v Azure</span><span class="sxs-lookup"><span data-stu-id="aa0c0-202">Run VMs for an N-tier architecture in Azure</span></span>](../guidance/guidance-compute-n-tier-vm.md)
   * [<span data-ttu-id="aa0c0-203">Nastavit přístup WinRM pro virtuální počítače ve službě Správce prostředků Azure</span><span class="sxs-lookup"><span data-stu-id="aa0c0-203">Set up WinRM access for VMs in Azure Resource Manager</span></span>](../virtual-machines/windows/winrm.md)
   * [<span data-ttu-id="aa0c0-204">Externí přístup k virtuálnímu počítači povolit pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="aa0c0-204">Allow external access to your VM by using the Azure portal</span></span>](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [<span data-ttu-id="aa0c0-205">Povolit externí přístup k virtuálnímu počítači pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="aa0c0-205">Allow external access to your VM by using PowerShell</span></span>](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [<span data-ttu-id="aa0c0-206">Povolit externí přístup k virtuálním počítačům s Linuxem pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="aa0c0-206">Allow external access to your Linux VM by using Azure CLI</span></span>](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* <span data-ttu-id="aa0c0-207">**DomainNameLabel** vlastnost pro veřejné IP adresy musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-207">The **domainNameLabel** property for public IP addresses must be unique.</span></span> <span data-ttu-id="aa0c0-208">**DomainNameLabel** hodnota musí být v rozmezí 3 až 63 znaků a postupujte podle pravidla určeného tento regulární výraz: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-208">The **domainNameLabel** value must be between 3 and 63 characters long, and follow the rules specified by this regular expression: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`.</span></span> <span data-ttu-id="aa0c0-209">Protože **uniqueString** funkce generuje řetězec, který je 13 znaků, **dnsPrefixString** parametr je omezený na 50 znaků:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-209">Because the **uniqueString** function generates a string that is 13 characters long, the **dnsPrefixString** parameter is limited to 50 characters:</span></span>

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* <span data-ttu-id="aa0c0-210">Když přidáte heslo k rozšíření vlastních skriptů, použijte **commandToExecute** vlastnost **protectedSettings** vlastnost:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-210">When you add a password to a custom script extension, use the **commandToExecute** property in the **protectedSettings** property:</span></span>
   
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
   > <span data-ttu-id="aa0c0-211">K zajištění, že tajné klíče se šifrují, pokud jsou předávány jako parametry pro virtuální počítače a rozšíření, použijte **protectedSettings** vlastnost relevantní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-211">To ensure that secrets are encrypted when they are passed as parameters to VMs and extensions, use the **protectedSettings** property of the relevant extensions.</span></span>
   > 
   > 

## <a name="outputs"></a><span data-ttu-id="aa0c0-212">Výstupy</span><span class="sxs-lookup"><span data-stu-id="aa0c0-212">Outputs</span></span>
<span data-ttu-id="aa0c0-213">Pokud používáte šablonu pro vytvoření veřejné IP adresy, patří **výstupy** oddíl, který vrátí podrobnosti IP adresy a plně kvalifikovaný název domény (FQDN).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-213">If you use a template to create public IP addresses, include an **outputs** section that returns details of the IP address and the fully qualified domain name (FQDN).</span></span> <span data-ttu-id="aa0c0-214">Výstupní hodnoty můžete použít k snadnému načtení podrobností o veřejné IP adresy a plně kvalifikované názvy domény po nasazení.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-214">You can use output values to easily retrieve details about public IP addresses and FQDNs after deployment.</span></span> <span data-ttu-id="aa0c0-215">Když odkazujete na prostředek, použijte verzi rozhraní API, který jste použili k jeho vytvoření:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-215">When you reference the resource, use the API version that you used to create it:</span></span> 

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

## <a name="single-template-vs-nested-templates"></a><span data-ttu-id="aa0c0-216">Jediné šabloně vs. vnořené šablony</span><span class="sxs-lookup"><span data-stu-id="aa0c0-216">Single template vs. nested templates</span></span>
<span data-ttu-id="aa0c0-217">K nasazení řešení, můžete použít jednu šablonu nebo hlavní šablonu s více vnořených šablon.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-217">To deploy your solution, you can use either a single template or a main template with multiple nested templates.</span></span> <span data-ttu-id="aa0c0-218">Vnořené šablony jsou společné pro pokročilejší scénáře.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-218">Nested templates are common for more advanced scenarios.</span></span> <span data-ttu-id="aa0c0-219">Pomocí šablony vnořené nabízí následující výhody:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-219">Using a nested template gives you the following advantages:</span></span>

* <span data-ttu-id="aa0c0-220">Lze rozčlenit řešení do cílové součásti.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-220">You can break down a solution into targeted components.</span></span>
* <span data-ttu-id="aa0c0-221">Vnořené šablon s jinou hlavní šablony můžete znovu použít.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-221">You can reuse nested templates with different main templates.</span></span>

<span data-ttu-id="aa0c0-222">Pokud chcete použít vnořené šablony, můžete pomocí následujících pokynů standardizovat návrhu šablony.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-222">If you choose to use nested templates, the following guidelines can help you standardize your template design.</span></span> <span data-ttu-id="aa0c0-223">Tyto pokyny jsou založené na [vzory pro navrhování šablon Azure Resource Manageru](best-practices-resource-manager-design-templates.md).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-223">These guidelines are based on [patterns for designing Azure Resource Manager templates](best-practices-resource-manager-design-templates.md).</span></span> <span data-ttu-id="aa0c0-224">Doporučujeme, abyste k návrhu, který má následující šablony:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-224">We recommend a design that has the following templates:</span></span>

* <span data-ttu-id="aa0c0-225">**Hlavní šablonu** (azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-225">**Main template** (azuredeploy.json).</span></span> <span data-ttu-id="aa0c0-226">Používejte pro vstupní parametry.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-226">Use for the input parameters.</span></span>
* <span data-ttu-id="aa0c0-227">**Sdílené prostředky šablony**.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-227">**Shared resources template**.</span></span> <span data-ttu-id="aa0c0-228">Použít k nasazení sdílené prostředky, které používají jiné prostředky (například virtuální sítě a dostupnost sady).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-228">Use to deploy shared resources that all other resources use (for example, virtual network and availability sets).</span></span> <span data-ttu-id="aa0c0-229">Použití **dependsOn** výraz, který se ujistěte, že tato šablona nasazen před další šablony.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-229">Use the **dependsOn** expression to ensure that this template is deployed before other templates.</span></span>
* <span data-ttu-id="aa0c0-230">**Volitelné prostředků šablony**.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-230">**Optional resources template**.</span></span> <span data-ttu-id="aa0c0-231">Použijte k nasazení podmíněně prostředky v závislosti na parametr (například jumpbox).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-231">Use to conditionally deploy resources based on a parameter (for example, a jumpbox).</span></span>
* <span data-ttu-id="aa0c0-232">**Šablony prostředků člen**.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-232">**Member resources template**.</span></span> <span data-ttu-id="aa0c0-233">Každý typ instance v aplikační vrstvě má svou vlastní konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-233">Each instance type within an application tier has its own configuration.</span></span> <span data-ttu-id="aa0c0-234">V rámci vrstvy můžete definovat typy jiné instance.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-234">Within a tier, you can define different instance types.</span></span> <span data-ttu-id="aa0c0-235">(Například nejprve vytvoří cluster a další instance jsou přidány do existujícího clusteru.) Každý typ instance má svou vlastní šablonu nasazení.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-235">(For example, the first instance creates a cluster, and additional instances are added to the existing cluster.) Each instance type has its own deployment template.</span></span>
* <span data-ttu-id="aa0c0-236">**Skripty**.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-236">**Scripts**.</span></span> <span data-ttu-id="aa0c0-237">Široce opakovaně použitelné skripty platí pro každý typ instance (například inicializovat a formát další disky).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-237">Widely reusable scripts are applicable for each instance type (for example, initialize and format additional disks).</span></span> <span data-ttu-id="aa0c0-238">Vlastní skripty, které vytvoříte za účelem přizpůsobení konkrétní se liší, na základě typu instance.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-238">Custom scripts that you create for a specific customization purpose are different, based on the instance type.</span></span>

![Vnořené šablony](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

<span data-ttu-id="aa0c0-240">Další informace najdete v tématu [použití propojených šablon s Azure Resource Manager](resource-group-linked-templates.md).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-240">For more information, see [Use linked templates with Azure Resource Manager](resource-group-linked-templates.md).</span></span>

## <a name="conditionally-link-to-nested-templates"></a><span data-ttu-id="aa0c0-241">Podmíněná propojení vnořené šablon</span><span class="sxs-lookup"><span data-stu-id="aa0c0-241">Conditionally link to nested templates</span></span>
<span data-ttu-id="aa0c0-242">Podmíněná propojení vnořené šablon můžete parametr.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-242">You can use a parameter to conditionally link to nested templates.</span></span> <span data-ttu-id="aa0c0-243">Parametr stane součástí identifikátor URI pro šablonu:</span><span class="sxs-lookup"><span data-stu-id="aa0c0-243">The parameter becomes part of the URI for the template:</span></span>

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

## <a name="template-format"></a><span data-ttu-id="aa0c0-244">Formát šablony</span><span class="sxs-lookup"><span data-stu-id="aa0c0-244">Template format</span></span>
<span data-ttu-id="aa0c0-245">Je dobrým zvykem předat šablony prostřednictvím validátor JSON.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-245">It's a good practice to pass your template through a JSON validator.</span></span> <span data-ttu-id="aa0c0-246">Validátor můžete odebrat nadbytečné čárkami, kulaté závorky a hranaté závorky, které může způsobit chybu během nasazení.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-246">A validator can help you remove extraneous commas, parentheses, and brackets that might cause an error during deployment.</span></span> <span data-ttu-id="aa0c0-247">Zkuste [JSONLint](http://jsonlint.com/) nebo linter balíček pro své oblíbené úpravou prostředí (Visual Studio Code, Atom, Sublime Text, Visual Studio).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-247">Try [JSONLint](http://jsonlint.com/) or a linter package for your favorite editing environment (Visual Studio Code, Atom, Sublime Text, Visual Studio).</span></span>

<span data-ttu-id="aa0c0-248">Je také vhodné pro vaše struktury JSON pro lepší čitelnost.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-248">It's also a good idea to format your JSON for better readability.</span></span> <span data-ttu-id="aa0c0-249">Můžete vytvořit balíček formátovací modul JSON pro vaše místní editor.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-249">You can use a JSON formatter package for your local editor.</span></span> <span data-ttu-id="aa0c0-250">V sadě Visual Studio k formátování dokumentu, stiskněte **Ctrl + K, Ctrl + D**.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-250">In Visual Studio, to format the document, press **Ctrl+K, Ctrl+D**.</span></span> <span data-ttu-id="aa0c0-251">V sadě Visual Studio Code stiskněte **Alt + Shift + F**.</span><span class="sxs-lookup"><span data-stu-id="aa0c0-251">In Visual Studio Code, press **Alt+Shift+F**.</span></span> <span data-ttu-id="aa0c0-252">Pokud vaše místní editor nebude formátu dokumentu, můžete použít [online formátování](https://www.bing.com/search?q=json+formatter).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-252">If your local editor doesn't format the document, you can use an [online formatter](https://www.bing.com/search?q=json+formatter).</span></span>

## <a name="next-steps"></a><span data-ttu-id="aa0c0-253">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aa0c0-253">Next steps</span></span>
* <span data-ttu-id="aa0c0-254">Pokyny na architekturu řešení virtuálních počítačů najdete v tématu [spustit virtuální počítač s Windows v Azure](../guidance/guidance-compute-single-vm.md) a [spuštění virtuálního počítače s Linuxem v Azure](../guidance/guidance-compute-single-vm-linux.md).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-254">For guidance on architecting your solution for virtual machines, see [Run a Windows VM in Azure](../guidance/guidance-compute-single-vm.md) and [Run a Linux VM in Azure](../guidance/guidance-compute-single-vm-linux.md).</span></span>
* <span data-ttu-id="aa0c0-255">Informace o nastavení účtu úložiště, najdete v části [kontrolní seznam výkonu a škálovatelnosti Azure Storage](../storage/common/storage-performance-checklist.md).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-255">For guidance on setting up a storage account, see [Azure Storage performance and scalability checklist](../storage/common/storage-performance-checklist.md).</span></span>
* <span data-ttu-id="aa0c0-256">Další informace o tom, jak mohou organizace pomocí Správce prostředků efektivně spravovat odběry, najdete v části [Azure enterprise vygenerované uživatelské rozhraní: doporučený předplatné zásad správného řízení](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="aa0c0-256">To learn about how an enterprise can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold: Prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

