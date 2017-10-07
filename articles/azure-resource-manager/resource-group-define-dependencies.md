---
title: "aaaSet pořadím nasazení pro prostředky Azure | Microsoft Docs"
description: "Popisuje, jak se tooset jeden prostředek jako závislé na jiný prostředek během nasazení tooensure prostředky nasadí ve správném pořadí hello."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 34ebaf1e-480c-4b4d-9bf6-251bd3f8f2cf
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: tomfitz
ms.openlocfilehash: 2f658f4c85236966c46b34a65aafb8426c92806c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="define-hello-order-for-deploying-resources-in-azure-resource-manager-templates"></a><span data-ttu-id="b934e-103">Definování hello pořadí pro nasazení prostředků v šablonách Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b934e-103">Define hello order for deploying resources in Azure Resource Manager templates</span></span>
<span data-ttu-id="b934e-104">Pro daný prostředek může být další prostředky, které musí existovat před nasazením hello prostředků.</span><span class="sxs-lookup"><span data-stu-id="b934e-104">For a given resource, there can be other resources that must exist before hello resource is deployed.</span></span> <span data-ttu-id="b934e-105">Například SQL server, musí existovat před pokusem o toodeploy databázi SQL.</span><span class="sxs-lookup"><span data-stu-id="b934e-105">For example, a SQL server must exist before attempting toodeploy a SQL database.</span></span> <span data-ttu-id="b934e-106">Můžete definovat tuto relaci označením jeden prostředek jako závislé na hello jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="b934e-106">You define this relationship by marking one resource as dependent on hello other resource.</span></span> <span data-ttu-id="b934e-107">Definování závislostí s hello **dependsOn** element, nebo pomocí hello **odkaz** funkce.</span><span class="sxs-lookup"><span data-stu-id="b934e-107">You define a dependency with hello **dependsOn** element, or by using hello **reference** function.</span></span> 

<span data-ttu-id="b934e-108">Správce prostředků vyhodnotí hello závislosti mezi prostředky a nasadí je v pořadí podle jejich závislé.</span><span class="sxs-lookup"><span data-stu-id="b934e-108">Resource Manager evaluates hello dependencies between resources, and deploys them in their dependent order.</span></span> <span data-ttu-id="b934e-109">Pokud nejsou na sobě navzájem závislé prostředky, Resource Manager je nasadí současně.</span><span class="sxs-lookup"><span data-stu-id="b934e-109">When resources are not dependent on each other, Resource Manager deploys them in parallel.</span></span> <span data-ttu-id="b934e-110">Potřebujete jenom toodefine závislosti pro prostředky, které jsou nasazeny v hello stejné šablony.</span><span class="sxs-lookup"><span data-stu-id="b934e-110">You only need toodefine dependencies for resources that are deployed in hello same template.</span></span> 

## <a name="dependson"></a><span data-ttu-id="b934e-111">dependsOn</span><span class="sxs-lookup"><span data-stu-id="b934e-111">dependsOn</span></span>
<span data-ttu-id="b934e-112">V rámci vaší šablony hello dependsOn prvek vám umožní toodefine jeden prostředek jako závisí na jeden nebo více prostředků.</span><span class="sxs-lookup"><span data-stu-id="b934e-112">Within your template, hello dependsOn element enables you toodefine one resource as a dependent on one or more resources.</span></span> <span data-ttu-id="b934e-113">Jeho hodnota může být čárkami oddělený seznam názvy prostředků.</span><span class="sxs-lookup"><span data-stu-id="b934e-113">Its value can be a comma-separated list of resource names.</span></span> 

<span data-ttu-id="b934e-114">Hello následující příklad ukazuje škálovací sadu virtuálních počítačů, které závisí na Vyrovnávání zatížení, virtuální sítě a smyčku, která vytváří více účtů úložiště.</span><span class="sxs-lookup"><span data-stu-id="b934e-114">hello following example shows a virtual machine scale set that depends on a load balancer, virtual network, and a loop that creates multiple storage accounts.</span></span> <span data-ttu-id="b934e-115">Tyto další prostředky nejsou zobrazeny v hello následující příklad, ale budou potřebovat tooexist jinde v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="b934e-115">These other resources are not shown in hello following example, but they would need tooexist elsewhere in hello template.</span></span>

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "[variables('namingInfix')]",
  "location": "[variables('location')]",
  "apiVersion": "2016-03-30",
  "tags": {
    "displayName": "VMScaleSet"
  },
  "dependsOn": [
    "[variables('loadBalancerName')]",
    "[variables('virtualNetworkName')]",
    "storageLoop",
  ],
  ...
}
```

<span data-ttu-id="b934e-116">V předchozím příkladu hello, je zahrnuta závislost na hello prostředky, které jsou vytvořené pomocí kopírovací smyčkou s názvem **storageLoop**.</span><span class="sxs-lookup"><span data-stu-id="b934e-116">In hello preceding example, a dependency is included on hello resources that are created through a copy loop named **storageLoop**.</span></span> <span data-ttu-id="b934e-117">Příklad, naleznete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="b934e-117">For an example, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

<span data-ttu-id="b934e-118">Při definování závislostí, můžete použít hello prostředků zprostředkovatele oboru názvů a prostředek typu tooavoid nejednoznačnosti.</span><span class="sxs-lookup"><span data-stu-id="b934e-118">When defining dependencies, you can include hello resource provider namespace and resource type tooavoid ambiguity.</span></span> <span data-ttu-id="b934e-119">Například tooclarify, které nástroj pro vyrovnávání zatížení a virtuální síť, která může mít hello stejné názvy jako jiné prostředky, hello použijte následující formát:</span><span class="sxs-lookup"><span data-stu-id="b934e-119">For example, tooclarify a load balancer and virtual network that may have hello same names as other resources, use hello following format:</span></span>

```json
"dependsOn": [
  "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
  "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
]
``` 

<span data-ttu-id="b934e-120">Může být nakloněné toouse dependsOn toomap vztahy mezi prostředky, je důležité toounderstand proč vaše změny.</span><span class="sxs-lookup"><span data-stu-id="b934e-120">While you may be inclined toouse dependsOn toomap relationships between your resources, it's important toounderstand why you're doing it.</span></span> <span data-ttu-id="b934e-121">Pro příklad, jak vzájemně propojeny prostředky, toodocument dependsOn není hello správný přístup.</span><span class="sxs-lookup"><span data-stu-id="b934e-121">For example, toodocument how resources are interconnected, dependsOn is not hello right approach.</span></span> <span data-ttu-id="b934e-122">Nemůže zadat dotaz, které prostředky byly definovány v elementu dependsOn hello po nasazení.</span><span class="sxs-lookup"><span data-stu-id="b934e-122">You cannot query which resources were defined in hello dependsOn element after deployment.</span></span> <span data-ttu-id="b934e-123">Pomocí dependsOn můžete případně ovlivnit času nasazení protože Resource Manager není nasazen v paralelní dva prostředky, které jsou závislé.</span><span class="sxs-lookup"><span data-stu-id="b934e-123">By using dependsOn, you potentially impact deployment time because Resource Manager does not deploy in parallel two resources that have a dependency.</span></span> <span data-ttu-id="b934e-124">toodocument vztahy mezi prostředky, použijte [propojování prostředků](/rest/api/resources/resourcelinks).</span><span class="sxs-lookup"><span data-stu-id="b934e-124">toodocument relationships between resources, instead use [resource linking](/rest/api/resources/resourcelinks).</span></span>

## <a name="child-resources"></a><span data-ttu-id="b934e-125">Podřízené prostředky</span><span class="sxs-lookup"><span data-stu-id="b934e-125">Child resources</span></span>
<span data-ttu-id="b934e-126">Vlastnost Hello prostředky vám umožní toospecify podřízené prostředky, které jsou prostředků související toohello definovaný.</span><span class="sxs-lookup"><span data-stu-id="b934e-126">hello resources property allows you toospecify child resources that are related toohello resource being defined.</span></span> <span data-ttu-id="b934e-127">Podřízené prostředky se dají jenom definované pěti úrovněmi.</span><span class="sxs-lookup"><span data-stu-id="b934e-127">Child resources can only be defined five levels deep.</span></span> <span data-ttu-id="b934e-128">Je důležité vytvořit toonote, který není implicitní závislost mezi podřízených prostředků a hello nadřazený prostředek.</span><span class="sxs-lookup"><span data-stu-id="b934e-128">It is important toonote that an implicit dependency is not created between a child resource and hello parent resource.</span></span> <span data-ttu-id="b934e-129">Pokud třeba hello podřízených prostředků toobe nenasadí po hello nadřazený prostředek, musí explicitně stavu této závislosti s vlastností dependsOn hello.</span><span class="sxs-lookup"><span data-stu-id="b934e-129">If you need hello child resource toobe deployed after hello parent resource, you must explicitly state that dependency with hello dependsOn property.</span></span> 

<span data-ttu-id="b934e-130">Každý nadřazený prostředek akceptuje pouze určité typy prostředků jako podřízené prostředky.</span><span class="sxs-lookup"><span data-stu-id="b934e-130">Each parent resource accepts only certain resource types as child resources.</span></span> <span data-ttu-id="b934e-131">Hello přijata typy prostředků jsou určené v hello [schéma šablony](https://github.com/Azure/azure-resource-manager-schemas) hello nadřazené prostředku.</span><span class="sxs-lookup"><span data-stu-id="b934e-131">hello accepted resource types are specified in hello [template schema](https://github.com/Azure/azure-resource-manager-schemas) of hello parent resource.</span></span> <span data-ttu-id="b934e-132">Hello název typu prostředku podřízené obsahuje hello název typu prostředku nadřazené hello, jako například **Microsoft.Web/sites/config** a **Microsoft.Web/sites/extensions** jsou obě podřízené prostředky hello  **Microsoft.Web/sites**.</span><span class="sxs-lookup"><span data-stu-id="b934e-132">hello name of child resource type includes hello name of hello parent resource type, such as **Microsoft.Web/sites/config** and **Microsoft.Web/sites/extensions** are both child resources of hello **Microsoft.Web/sites**.</span></span>

<span data-ttu-id="b934e-133">Hello následující příklad ukazuje systému SQL server a databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="b934e-133">hello following example shows a SQL server and SQL database.</span></span> <span data-ttu-id="b934e-134">Všimněte si, že je definován explicitní závislosti mezi hello SQL database a SQL server, i když je databáze hello podřízený hello server.</span><span class="sxs-lookup"><span data-stu-id="b934e-134">Notice that an explicit dependency is defined between hello SQL database and SQL server, even though hello database is a child of hello server.</span></span>

```json
"resources": [
  {
    "name": "[variables('sqlserverName')]",
    "type": "Microsoft.Sql/servers",
    "location": "[resourceGroup().location]",
    "tags": {
      "displayName": "SqlServer"
    },
    "apiVersion": "2014-04-01-preview",
    "properties": {
      "administratorLogin": "[parameters('administratorLogin')]",
      "administratorLoginPassword": "[parameters('administratorLoginPassword')]"
    },
    "resources": [
      {
        "name": "[parameters('databaseName')]",
        "type": "databases",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "Database"
        },
        "apiVersion": "2014-04-01-preview",
        "dependsOn": [
          "[variables('sqlserverName')]"
        ],
        "properties": {
          "edition": "[parameters('edition')]",
          "collation": "[parameters('collation')]",
          "maxSizeBytes": "[parameters('maxSizeBytes')]",
          "requestedServiceObjectiveName": "[parameters('requestedServiceObjectiveName')]"
        }
      }
    ]
  }
]
```

## <a name="reference-function"></a><span data-ttu-id="b934e-135">Reference – funkce</span><span class="sxs-lookup"><span data-stu-id="b934e-135">reference function</span></span>
<span data-ttu-id="b934e-136">Hello [odkazu funkci](resource-group-template-functions-resource.md#reference) umožňuje výrazu tooderive svou hodnotu z jiných dvojice název a hodnota JSON nebo modul runtime prostředky.</span><span class="sxs-lookup"><span data-stu-id="b934e-136">hello [reference function](resource-group-template-functions-resource.md#reference) enables an expression tooderive its value from other JSON name and value pairs or runtime resources.</span></span> <span data-ttu-id="b934e-137">Odkaz na výrazy implicitně deklarovat, že jeden prostředek závisí na jiném.</span><span class="sxs-lookup"><span data-stu-id="b934e-137">Reference expressions implicitly declare that one resource depends on another.</span></span> <span data-ttu-id="b934e-138">Obecný formát Hello je:</span><span class="sxs-lookup"><span data-stu-id="b934e-138">hello general format is:</span></span>

```json
reference('resourceName').propertyPath
```

<span data-ttu-id="b934e-139">V následujícím příkladu hello koncový bod CDN explicitně závisí na hello profil CDN a implicitně závisí na webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="b934e-139">In hello following example, a CDN endpoint explicitly depends on hello CDN profile, and implicitly depends on a web app.</span></span>

```json
{
    "name": "[variables('endpointName')]",
    "type": "endpoints",
    "location": "[resourceGroup().location]",
    "apiVersion": "2016-04-02",
    "dependsOn": [
            "[variables('profileName')]"
    ],
    "properties": {
        "originHostHeader": "[reference(variables('webAppName')).hostNames[0]]",
        ...
    }
```

<span data-ttu-id="b934e-140">Můžete použít tento element nebo hello dependsOn element toospecify závislosti, ale nepotřebujete toouse i pro hello stejné závislý prostředek.</span><span class="sxs-lookup"><span data-stu-id="b934e-140">You can use either this element or hello dependsOn element toospecify dependencies, but you do not need toouse both for hello same dependent resource.</span></span> <span data-ttu-id="b934e-141">Kdykoli je to možné, použijte implicitní odkaz tooavoid přidání nepotřebné závislostí.</span><span class="sxs-lookup"><span data-stu-id="b934e-141">Whenever possible, use an implicit reference tooavoid adding an unnecessary dependency.</span></span>

<span data-ttu-id="b934e-142">Další, najdete v části toolearn [odkazu funkci](resource-group-template-functions-resource.md#reference).</span><span class="sxs-lookup"><span data-stu-id="b934e-142">toolearn more, see [reference function](resource-group-template-functions-resource.md#reference).</span></span>

## <a name="recommendations-for-setting-dependencies"></a><span data-ttu-id="b934e-143">Doporučení pro nastavení závislostí</span><span class="sxs-lookup"><span data-stu-id="b934e-143">Recommendations for setting dependencies</span></span>

<span data-ttu-id="b934e-144">Při rozhodování, jakou tooset závislosti, použijte následující pokyny hello:</span><span class="sxs-lookup"><span data-stu-id="b934e-144">When deciding what dependencies tooset, use hello following guidelines:</span></span>

* <span data-ttu-id="b934e-145">Nastavte jako několik závislosti míře.</span><span class="sxs-lookup"><span data-stu-id="b934e-145">Set as few dependencies as possible.</span></span>
* <span data-ttu-id="b934e-146">Nastavte podřízených prostředků jako závislý na prostředku jeho nadřazený.</span><span class="sxs-lookup"><span data-stu-id="b934e-146">Set a child resource as dependent on its parent resource.</span></span>
* <span data-ttu-id="b934e-147">Použití hello **odkaz** funkce tooset implicitní závislosti mezi prostředky, které je třeba tooshare vlastnost.</span><span class="sxs-lookup"><span data-stu-id="b934e-147">Use hello **reference** function tooset implicit dependencies between resources that need tooshare a property.</span></span> <span data-ttu-id="b934e-148">Nepřidávejte explicitní závislosti (**dependsOn**) Pokud jste již definována implicitní závislostí.</span><span class="sxs-lookup"><span data-stu-id="b934e-148">Do not add an explicit dependency (**dependsOn**) when you have already defined an implicit dependency.</span></span> <span data-ttu-id="b934e-149">Tento přístup snižuje riziko hello použití nepotřebné závislosti.</span><span class="sxs-lookup"><span data-stu-id="b934e-149">This approach reduces hello risk of having unnecessary dependencies.</span></span> 
* <span data-ttu-id="b934e-150">Nastaví závislost, pokud prostředek nemůže být **vytvořit** bez funkce z jiného prostředku.</span><span class="sxs-lookup"><span data-stu-id="b934e-150">Set a dependency when a resource cannot be **created** without functionality from another resource.</span></span> <span data-ttu-id="b934e-151">Nenastavujte závislost, pokud hello prostředků komunikovat po nasazení.</span><span class="sxs-lookup"><span data-stu-id="b934e-151">Do not set a dependency if hello resources only interact after deployment.</span></span>
* <span data-ttu-id="b934e-152">Umožňují závislosti cascade bez nastavení je explicitně.</span><span class="sxs-lookup"><span data-stu-id="b934e-152">Let dependencies cascade without setting them explicitly.</span></span> <span data-ttu-id="b934e-153">Například virtuálního počítače závisí na rozhraní virtuální sítě a hello virtuální síťové rozhraní závisí na virtuální síť a veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="b934e-153">For example, your virtual machine depends on a virtual network interface, and hello virtual network interface depends on a virtual network and public IP addresses.</span></span> <span data-ttu-id="b934e-154">Proto hello virtuální počítač je nasazené po všech tří prostředky, ale nenastavíte explicitně hello virtuálního počítače jako závislé na všechny tři zdroje.</span><span class="sxs-lookup"><span data-stu-id="b934e-154">Therefore, hello virtual machine is deployed after all three resources, but do not explicitly set hello virtual machine as dependent on all three resources.</span></span> <span data-ttu-id="b934e-155">Tento postup vysvětluje pořadí závislostí hello a umožňuje jednodušší šablony toochange hello později.</span><span class="sxs-lookup"><span data-stu-id="b934e-155">This approach clarifies hello dependency order and makes it easier toochange hello template later.</span></span>
* <span data-ttu-id="b934e-156">Pokud hodnota se dá určit před nasazením, zkuste nasazení prostředků hello bez závislosti.</span><span class="sxs-lookup"><span data-stu-id="b934e-156">If a value can be determined before deployment, try deploying hello resource without a dependency.</span></span> <span data-ttu-id="b934e-157">Například pokud hodnota konfigurace potřebuje hello název jiného prostředku, nemusí závislost.</span><span class="sxs-lookup"><span data-stu-id="b934e-157">For example, if a configuration value needs hello name of another resource, you might not need a dependency.</span></span> <span data-ttu-id="b934e-158">V tomto návodu nefunguje vždy vzhledem k tomu, že některé prostředky, ověřte existenci hello hello jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="b934e-158">This guidance does not always work because some resources verify hello existence of hello other resource.</span></span> <span data-ttu-id="b934e-159">Pokud narazíte na chyby, přidejte závislosti.</span><span class="sxs-lookup"><span data-stu-id="b934e-159">If you receive an error, add a dependency.</span></span> 

<span data-ttu-id="b934e-160">Správce prostředků identifikuje cyklické závislosti během ověřování šablony.</span><span class="sxs-lookup"><span data-stu-id="b934e-160">Resource Manager identifies circular dependencies during template validation.</span></span> <span data-ttu-id="b934e-161">Pokud se zobrazí chyba oznamující, že existuje cyklická závislost, nevyhodnotí toosee vaší šablony, pokud nejsou potřebné žádné závislosti a lze odebrat.</span><span class="sxs-lookup"><span data-stu-id="b934e-161">If you receive an error stating that a circular dependency exists, evaluate your template toosee if any dependencies are not needed and can be removed.</span></span> <span data-ttu-id="b934e-162">Pokud odebrání závislostí nefunguje, se můžete vyhnout cyklické závislosti přesunutím některé operace nasazení do podřízené prostředky, které jsou nasazeny po hello prostředky, které mají hello cyklická závislost.</span><span class="sxs-lookup"><span data-stu-id="b934e-162">If removing dependencies does not work, you can avoid circular dependencies by moving some deployment operations into child resources that are deployed after hello resources that have hello circular dependency.</span></span> <span data-ttu-id="b934e-163">Předpokládejme například, nasazujete dva virtuální počítače, ale je nutné nastavit na každé z nich najdete toohello jiné vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="b934e-163">For example, suppose you are deploying two virtual machines but you must set properties on each one that refer toohello other.</span></span> <span data-ttu-id="b934e-164">Můžete je nasadit v hello následující pořadí:</span><span class="sxs-lookup"><span data-stu-id="b934e-164">You can deploy them in hello following order:</span></span>

1. <span data-ttu-id="b934e-165">vm1</span><span class="sxs-lookup"><span data-stu-id="b934e-165">vm1</span></span>
2. <span data-ttu-id="b934e-166">virtuálního počítače 2</span><span class="sxs-lookup"><span data-stu-id="b934e-166">vm2</span></span>
3. <span data-ttu-id="b934e-167">Rozšíření na vm1 závisí na vm1 a virtuálního počítače 2.</span><span class="sxs-lookup"><span data-stu-id="b934e-167">Extension on vm1 depends on vm1 and vm2.</span></span> <span data-ttu-id="b934e-168">rozšíření Hello nastaví hodnoty vm1, který získá z virtuálního počítače 2.</span><span class="sxs-lookup"><span data-stu-id="b934e-168">hello extension sets values on vm1 that it gets from vm2.</span></span>
4. <span data-ttu-id="b934e-169">Rozšíření na virtuálního počítače 2 závisí na vm1 a virtuálního počítače 2.</span><span class="sxs-lookup"><span data-stu-id="b934e-169">Extension on vm2 depends on vm1 and vm2.</span></span> <span data-ttu-id="b934e-170">rozšíření Hello nastaví hodnoty virtuálního počítače 2, který získá ze vm1.</span><span class="sxs-lookup"><span data-stu-id="b934e-170">hello extension sets values on vm2 that it gets from vm1.</span></span>

<span data-ttu-id="b934e-171">Informace o vyhodnocování hello pořadí nasazení a řešení chyb při závislostí najdete v tématu [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="b934e-171">For information about assessing hello deployment order and resolving dependency errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b934e-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b934e-172">Next steps</span></span>
* <span data-ttu-id="b934e-173">toolearn o řešení potíží s závislosti při nasazení, najdete v části [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="b934e-173">toolearn about troubleshooting dependencies during deployment, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="b934e-174">toolearn o vytváření šablon Azure Resource Manageru, najdete v části [vytváření šablon](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b934e-174">toolearn about creating Azure Resource Manager templates, see [Authoring templates](resource-group-authoring-templates.md).</span></span> 
* <span data-ttu-id="b934e-175">Seznam dostupných funkcí hello v šabloně, naleznete v části [funkce šablon](resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="b934e-175">For a list of hello available functions in a template, see [Template functions](resource-group-template-functions.md).</span></span>

