---
title: "Nastavení pořadí nasazení pro prostředky Azure | Microsoft Docs"
description: "Popisuje, jak nastavit jeden prostředek jako závislé na jiný prostředek během nasazení, aby se prostředky nasadí ve správném pořadí."
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
ms.openlocfilehash: 3d6a46116ae9d7d940bc10dfa832540f42c0af7e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="define-the-order-for-deploying-resources-in-azure-resource-manager-templates"></a><span data-ttu-id="af6be-103">Definovat pořadí pro nasazení prostředků v šablonách Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="af6be-103">Define the order for deploying resources in Azure Resource Manager templates</span></span>
<span data-ttu-id="af6be-104">Pro daný prostředek může být další prostředky, které musí existovat před nasazením prostředku.</span><span class="sxs-lookup"><span data-stu-id="af6be-104">For a given resource, there can be other resources that must exist before the resource is deployed.</span></span> <span data-ttu-id="af6be-105">Například SQL server, musí existovat před pokusem o nasazení databáze SQL.</span><span class="sxs-lookup"><span data-stu-id="af6be-105">For example, a SQL server must exist before attempting to deploy a SQL database.</span></span> <span data-ttu-id="af6be-106">Tento vztah definujete označením jeden prostředek jako závislý na jiných prostředku.</span><span class="sxs-lookup"><span data-stu-id="af6be-106">You define this relationship by marking one resource as dependent on the other resource.</span></span> <span data-ttu-id="af6be-107">Můžete definovat závislosti s **dependsOn** element, nebo pomocí **odkaz** funkce.</span><span class="sxs-lookup"><span data-stu-id="af6be-107">You define a dependency with the **dependsOn** element, or by using the **reference** function.</span></span> 

<span data-ttu-id="af6be-108">Správce prostředků vyhodnotí závislosti mezi prostředky a nasadí je v pořadí podle jejich závislé.</span><span class="sxs-lookup"><span data-stu-id="af6be-108">Resource Manager evaluates the dependencies between resources, and deploys them in their dependent order.</span></span> <span data-ttu-id="af6be-109">Pokud nejsou na sobě navzájem závislé prostředky, Resource Manager je nasadí současně.</span><span class="sxs-lookup"><span data-stu-id="af6be-109">When resources are not dependent on each other, Resource Manager deploys them in parallel.</span></span> <span data-ttu-id="af6be-110">Stačí definování závislostí u prostředků, které jsou nasazeny do stejné šablony.</span><span class="sxs-lookup"><span data-stu-id="af6be-110">You only need to define dependencies for resources that are deployed in the same template.</span></span> 

## <a name="dependson"></a><span data-ttu-id="af6be-111">dependsOn</span><span class="sxs-lookup"><span data-stu-id="af6be-111">dependsOn</span></span>
<span data-ttu-id="af6be-112">V rámci vaší šablony dependsOn element umožňuje definovat jeden prostředek jako závisí na jeden nebo více prostředků.</span><span class="sxs-lookup"><span data-stu-id="af6be-112">Within your template, the dependsOn element enables you to define one resource as a dependent on one or more resources.</span></span> <span data-ttu-id="af6be-113">Jeho hodnota může být čárkami oddělený seznam názvy prostředků.</span><span class="sxs-lookup"><span data-stu-id="af6be-113">Its value can be a comma-separated list of resource names.</span></span> 

<span data-ttu-id="af6be-114">Následující příklad ukazuje škálovací sadu virtuálních počítačů, které závisí na Vyrovnávání zatížení, virtuální sítě a smyčku, která vytváří více účtů úložiště.</span><span class="sxs-lookup"><span data-stu-id="af6be-114">The following example shows a virtual machine scale set that depends on a load balancer, virtual network, and a loop that creates multiple storage accounts.</span></span> <span data-ttu-id="af6be-115">Tyto další prostředky nejsou vidět v následujícím příkladu, ale jejich by bylo potřeba existují na jiném místě v šabloně.</span><span class="sxs-lookup"><span data-stu-id="af6be-115">These other resources are not shown in the following example, but they would need to exist elsewhere in the template.</span></span>

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

<span data-ttu-id="af6be-116">V předchozím příkladu je zahrnuta závislost na prostředcích, které jsou vytvořené pomocí kopírovací smyčkou s názvem **storageLoop**.</span><span class="sxs-lookup"><span data-stu-id="af6be-116">In the preceding example, a dependency is included on the resources that are created through a copy loop named **storageLoop**.</span></span> <span data-ttu-id="af6be-117">Příklad, naleznete v části [vytvořit více instancí prostředků ve službě Správce prostředků Azure](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="af6be-117">For an example, see [Create multiple instances of resources in Azure Resource Manager](resource-group-create-multiple.md).</span></span>

<span data-ttu-id="af6be-118">Při definování závislostí, můžete použít obor názvů zprostředkovatele prostředků a typ prostředku, aby se zabránilo nejednoznačnosti.</span><span class="sxs-lookup"><span data-stu-id="af6be-118">When defining dependencies, you can include the resource provider namespace and resource type to avoid ambiguity.</span></span> <span data-ttu-id="af6be-119">O vysvětlení, nástroj pro vyrovnávání zatížení a virtuální síť, která může mít stejné názvy jako jiné prostředky, například použijte následující formát:</span><span class="sxs-lookup"><span data-stu-id="af6be-119">For example, to clarify a load balancer and virtual network that may have the same names as other resources, use the following format:</span></span>

```json
"dependsOn": [
  "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
  "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
]
``` 

<span data-ttu-id="af6be-120">Když jste sklon nesmí být používat dependsOn mapovat vztahy mezi prostředky, je důležité pochopit, proč vaše změny.</span><span class="sxs-lookup"><span data-stu-id="af6be-120">While you may be inclined to use dependsOn to map relationships between your resources, it's important to understand why you're doing it.</span></span> <span data-ttu-id="af6be-121">Například k dokumentu, způsobu vzájemného propojení prostředků, dependsOn není správný přístup.</span><span class="sxs-lookup"><span data-stu-id="af6be-121">For example, to document how resources are interconnected, dependsOn is not the right approach.</span></span> <span data-ttu-id="af6be-122">Nemůže zadat dotaz, které prostředky byly definovány v elementu dependsOn po nasazení.</span><span class="sxs-lookup"><span data-stu-id="af6be-122">You cannot query which resources were defined in the dependsOn element after deployment.</span></span> <span data-ttu-id="af6be-123">Pomocí dependsOn můžete případně ovlivnit času nasazení protože Resource Manager není nasazen v paralelní dva prostředky, které jsou závislé.</span><span class="sxs-lookup"><span data-stu-id="af6be-123">By using dependsOn, you potentially impact deployment time because Resource Manager does not deploy in parallel two resources that have a dependency.</span></span> <span data-ttu-id="af6be-124">K dokumentu vztahy mezi prostředky, místo toho použít [propojování prostředků](/rest/api/resources/resourcelinks).</span><span class="sxs-lookup"><span data-stu-id="af6be-124">To document relationships between resources, instead use [resource linking](/rest/api/resources/resourcelinks).</span></span>

## <a name="child-resources"></a><span data-ttu-id="af6be-125">Podřízené prostředky</span><span class="sxs-lookup"><span data-stu-id="af6be-125">Child resources</span></span>
<span data-ttu-id="af6be-126">Vlastnost prostředků můžete zadat podřízené prostředky, které se vztahují k prostředku definovaný.</span><span class="sxs-lookup"><span data-stu-id="af6be-126">The resources property allows you to specify child resources that are related to the resource being defined.</span></span> <span data-ttu-id="af6be-127">Podřízené prostředky se dají jenom definované pěti úrovněmi.</span><span class="sxs-lookup"><span data-stu-id="af6be-127">Child resources can only be defined five levels deep.</span></span> <span data-ttu-id="af6be-128">Je důležité si uvědomit, že nebyl vytvořen implicitní závislost mezi podřízených prostředků a nadřazený prostředek.</span><span class="sxs-lookup"><span data-stu-id="af6be-128">It is important to note that an implicit dependency is not created between a child resource and the parent resource.</span></span> <span data-ttu-id="af6be-129">Pokud potřebujete podřízených prostředků k nasazení po nadřazeném prostředku, musí explicitně stavu této závislosti s vlastností dependsOn.</span><span class="sxs-lookup"><span data-stu-id="af6be-129">If you need the child resource to be deployed after the parent resource, you must explicitly state that dependency with the dependsOn property.</span></span> 

<span data-ttu-id="af6be-130">Každý nadřazený prostředek akceptuje pouze určité typy prostředků jako podřízené prostředky.</span><span class="sxs-lookup"><span data-stu-id="af6be-130">Each parent resource accepts only certain resource types as child resources.</span></span> <span data-ttu-id="af6be-131">Typy prostředků přijala jsou určené v [schéma šablony](https://github.com/Azure/azure-resource-manager-schemas) nadřazené prostředku.</span><span class="sxs-lookup"><span data-stu-id="af6be-131">The accepted resource types are specified in the [template schema](https://github.com/Azure/azure-resource-manager-schemas) of the parent resource.</span></span> <span data-ttu-id="af6be-132">Název typu prostředku podřízené obsahuje název nadřazené typ prostředku, jako například **Microsoft.Web/sites/config** a **Microsoft.Web/sites/extensions** jsou obě podřízené prostředky **Microsoft.Web/sites**.</span><span class="sxs-lookup"><span data-stu-id="af6be-132">The name of child resource type includes the name of the parent resource type, such as **Microsoft.Web/sites/config** and **Microsoft.Web/sites/extensions** are both child resources of the **Microsoft.Web/sites**.</span></span>

<span data-ttu-id="af6be-133">Následující příklad ukazuje systému SQL server a SQL database.</span><span class="sxs-lookup"><span data-stu-id="af6be-133">The following example shows a SQL server and SQL database.</span></span> <span data-ttu-id="af6be-134">Všimněte si, že je definován explicitní závislosti mezi SQL database a SQL server, i v případě, že databáze je podřízený server.</span><span class="sxs-lookup"><span data-stu-id="af6be-134">Notice that an explicit dependency is defined between the SQL database and SQL server, even though the database is a child of the server.</span></span>

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

## <a name="reference-function"></a><span data-ttu-id="af6be-135">Reference – funkce</span><span class="sxs-lookup"><span data-stu-id="af6be-135">reference function</span></span>
<span data-ttu-id="af6be-136">[Odkazu funkci](resource-group-template-functions-resource.md#reference) umožňuje výrazu odvození svou hodnotu z dalších dvojice název a hodnota JSON nebo modul runtime prostředky.</span><span class="sxs-lookup"><span data-stu-id="af6be-136">The [reference function](resource-group-template-functions-resource.md#reference) enables an expression to derive its value from other JSON name and value pairs or runtime resources.</span></span> <span data-ttu-id="af6be-137">Odkaz na výrazy implicitně deklarovat, že jeden prostředek závisí na jiném.</span><span class="sxs-lookup"><span data-stu-id="af6be-137">Reference expressions implicitly declare that one resource depends on another.</span></span> <span data-ttu-id="af6be-138">Obecný formát je:</span><span class="sxs-lookup"><span data-stu-id="af6be-138">The general format is:</span></span>

```json
reference('resourceName').propertyPath
```

<span data-ttu-id="af6be-139">V následujícím příkladu koncový bod CDN explicitně závisí na profil CDN a implicitně závisí na webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="af6be-139">In the following example, a CDN endpoint explicitly depends on the CDN profile, and implicitly depends on a web app.</span></span>

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

<span data-ttu-id="af6be-140">Tento element nebo dependsOn element můžete použít k určení závislostí, ale není potřeba použít pro stejné závislý prostředek.</span><span class="sxs-lookup"><span data-stu-id="af6be-140">You can use either this element or the dependsOn element to specify dependencies, but you do not need to use both for the same dependent resource.</span></span> <span data-ttu-id="af6be-141">Kdykoli je to možné, vyhněte se přidávání nepotřebné závislostí pomocí implicitní odkaz.</span><span class="sxs-lookup"><span data-stu-id="af6be-141">Whenever possible, use an implicit reference to avoid adding an unnecessary dependency.</span></span>

<span data-ttu-id="af6be-142">Další informace najdete v tématu [odkazu funkci](resource-group-template-functions-resource.md#reference).</span><span class="sxs-lookup"><span data-stu-id="af6be-142">To learn more, see [reference function](resource-group-template-functions-resource.md#reference).</span></span>

## <a name="recommendations-for-setting-dependencies"></a><span data-ttu-id="af6be-143">Doporučení pro nastavení závislostí</span><span class="sxs-lookup"><span data-stu-id="af6be-143">Recommendations for setting dependencies</span></span>

<span data-ttu-id="af6be-144">Při rozhodování, jakou závislosti nastavit, použijte následující pokyny:</span><span class="sxs-lookup"><span data-stu-id="af6be-144">When deciding what dependencies to set, use the following guidelines:</span></span>

* <span data-ttu-id="af6be-145">Nastavte jako několik závislosti míře.</span><span class="sxs-lookup"><span data-stu-id="af6be-145">Set as few dependencies as possible.</span></span>
* <span data-ttu-id="af6be-146">Nastavte podřízených prostředků jako závislý na prostředku jeho nadřazený.</span><span class="sxs-lookup"><span data-stu-id="af6be-146">Set a child resource as dependent on its parent resource.</span></span>
* <span data-ttu-id="af6be-147">Použití **odkaz** funkce nastavit implicitní závislosti mezi prostředky, které je potřeba sdílet vlastnost.</span><span class="sxs-lookup"><span data-stu-id="af6be-147">Use the **reference** function to set implicit dependencies between resources that need to share a property.</span></span> <span data-ttu-id="af6be-148">Nepřidávejte explicitní závislosti (**dependsOn**) Pokud jste již definována implicitní závislostí.</span><span class="sxs-lookup"><span data-stu-id="af6be-148">Do not add an explicit dependency (**dependsOn**) when you have already defined an implicit dependency.</span></span> <span data-ttu-id="af6be-149">Tento přístup snižuje riziko použití nepotřebné závislosti.</span><span class="sxs-lookup"><span data-stu-id="af6be-149">This approach reduces the risk of having unnecessary dependencies.</span></span> 
* <span data-ttu-id="af6be-150">Nastaví závislost, pokud prostředek nemůže být **vytvořit** bez funkce z jiného prostředku.</span><span class="sxs-lookup"><span data-stu-id="af6be-150">Set a dependency when a resource cannot be **created** without functionality from another resource.</span></span> <span data-ttu-id="af6be-151">Nenastavujte závislost, pokud prostředky komunikovat po nasazení.</span><span class="sxs-lookup"><span data-stu-id="af6be-151">Do not set a dependency if the resources only interact after deployment.</span></span>
* <span data-ttu-id="af6be-152">Umožňují závislosti cascade bez nastavení je explicitně.</span><span class="sxs-lookup"><span data-stu-id="af6be-152">Let dependencies cascade without setting them explicitly.</span></span> <span data-ttu-id="af6be-153">Například virtuálního počítače závisí na rozhraní virtuální sítě a virtuální síťové rozhraní závisí na virtuální síť a veřejné IP adresy.</span><span class="sxs-lookup"><span data-stu-id="af6be-153">For example, your virtual machine depends on a virtual network interface, and the virtual network interface depends on a virtual network and public IP addresses.</span></span> <span data-ttu-id="af6be-154">Proto tento virtuální počítač je nasazené po všech tří prostředky, ale nenastavíte explicitně virtuální počítač jako závislé na prostředcích všechny tři.</span><span class="sxs-lookup"><span data-stu-id="af6be-154">Therefore, the virtual machine is deployed after all three resources, but do not explicitly set the virtual machine as dependent on all three resources.</span></span> <span data-ttu-id="af6be-155">Tento postup vysvětluje, pak pořadí závislostí a usnadňuje později změnit šablonu.</span><span class="sxs-lookup"><span data-stu-id="af6be-155">This approach clarifies the dependency order and makes it easier to change the template later.</span></span>
* <span data-ttu-id="af6be-156">Pokud hodnota se dá určit před nasazením, zkuste nasazení prostředků bez závislosti.</span><span class="sxs-lookup"><span data-stu-id="af6be-156">If a value can be determined before deployment, try deploying the resource without a dependency.</span></span> <span data-ttu-id="af6be-157">Například pokud hodnota konfigurace potřebuje název jiného prostředku, nemusí závislost.</span><span class="sxs-lookup"><span data-stu-id="af6be-157">For example, if a configuration value needs the name of another resource, you might not need a dependency.</span></span> <span data-ttu-id="af6be-158">V tomto návodu nefunguje vždy vzhledem k tomu, že některé prostředky, ověřte existenci jiný prostředek.</span><span class="sxs-lookup"><span data-stu-id="af6be-158">This guidance does not always work because some resources verify the existence of the other resource.</span></span> <span data-ttu-id="af6be-159">Pokud narazíte na chyby, přidejte závislosti.</span><span class="sxs-lookup"><span data-stu-id="af6be-159">If you receive an error, add a dependency.</span></span> 

<span data-ttu-id="af6be-160">Správce prostředků identifikuje cyklické závislosti během ověřování šablony.</span><span class="sxs-lookup"><span data-stu-id="af6be-160">Resource Manager identifies circular dependencies during template validation.</span></span> <span data-ttu-id="af6be-161">Pokud se zobrazí chyba oznamující, že existuje cyklická závislost, vyhodnoťte šablony zda všechny závislosti nejsou potřebné, a může být odebrán.</span><span class="sxs-lookup"><span data-stu-id="af6be-161">If you receive an error stating that a circular dependency exists, evaluate your template to see if any dependencies are not needed and can be removed.</span></span> <span data-ttu-id="af6be-162">Pokud odebrání závislostí nefunguje, se můžete vyhnout cyklické závislosti přesunutím některé operace nasazení do podřízené prostředky, které jsou nasazeny po prostředky, které mají cyklickou závislost.</span><span class="sxs-lookup"><span data-stu-id="af6be-162">If removing dependencies does not work, you can avoid circular dependencies by moving some deployment operations into child resources that are deployed after the resources that have the circular dependency.</span></span> <span data-ttu-id="af6be-163">Předpokládejme například, kterou nasazujete dva virtuální počítače, ale je nutné nastavit vlastnosti u každé z nich odkazovat na druhý.</span><span class="sxs-lookup"><span data-stu-id="af6be-163">For example, suppose you are deploying two virtual machines but you must set properties on each one that refer to the other.</span></span> <span data-ttu-id="af6be-164">Je můžete nasadit v následujícím pořadí:</span><span class="sxs-lookup"><span data-stu-id="af6be-164">You can deploy them in the following order:</span></span>

1. <span data-ttu-id="af6be-165">vm1</span><span class="sxs-lookup"><span data-stu-id="af6be-165">vm1</span></span>
2. <span data-ttu-id="af6be-166">virtuálního počítače 2</span><span class="sxs-lookup"><span data-stu-id="af6be-166">vm2</span></span>
3. <span data-ttu-id="af6be-167">Rozšíření na vm1 závisí na vm1 a virtuálního počítače 2.</span><span class="sxs-lookup"><span data-stu-id="af6be-167">Extension on vm1 depends on vm1 and vm2.</span></span> <span data-ttu-id="af6be-168">Rozšíření nastaví hodnoty vm1, který získá z virtuálního počítače 2.</span><span class="sxs-lookup"><span data-stu-id="af6be-168">The extension sets values on vm1 that it gets from vm2.</span></span>
4. <span data-ttu-id="af6be-169">Rozšíření na virtuálního počítače 2 závisí na vm1 a virtuálního počítače 2.</span><span class="sxs-lookup"><span data-stu-id="af6be-169">Extension on vm2 depends on vm1 and vm2.</span></span> <span data-ttu-id="af6be-170">Rozšíření virtuálního počítače 2, který získá ze vm1 nastaví hodnoty.</span><span class="sxs-lookup"><span data-stu-id="af6be-170">The extension sets values on vm2 that it gets from vm1.</span></span>

<span data-ttu-id="af6be-171">Informace o vyhodnocování pořadí nasazení a řešení chyb při závislostí najdete v tématu [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="af6be-171">For information about assessing the deployment order and resolving dependency errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="af6be-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="af6be-172">Next steps</span></span>
* <span data-ttu-id="af6be-173">Další informace o řešení potíží s závislosti při nasazení najdete v tématu [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="af6be-173">To learn about troubleshooting dependencies during deployment, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="af6be-174">Další informace o vytváření šablon Azure Resource Manageru, najdete v části [vytváření šablon](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="af6be-174">To learn about creating Azure Resource Manager templates, see [Authoring templates](resource-group-authoring-templates.md).</span></span> 
* <span data-ttu-id="af6be-175">Seznam dostupných funkcí v šabloně najdete v tématu [funkce šablon](resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="af6be-175">For a list of the available functions in a template, see [Template functions](resource-group-template-functions.md).</span></span>

