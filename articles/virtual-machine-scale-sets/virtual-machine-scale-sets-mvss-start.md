---
title: "Další informace o šablonách sady škálování virtuálního počítače | Microsoft Docs"
description: "Naučte se vytvořit šablonu sady minimální přijatelná škálování pro sady škálování virtuálního počítače"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: negat
ms.openlocfilehash: 65f02c4675eb752dcc82e9a1d1c7f6c2c193fc32
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="learn-about-virtual-machine-scale-set-templates"></a><span data-ttu-id="d2bc2-103">Další informace o šablonách sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="d2bc2-103">Learn about virtual machine scale set templates</span></span>
<span data-ttu-id="d2bc2-104">[Šablony Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) nabízí skvělou možnost pro nasazení skupin souvisejících prostředků.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-104">[Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) are a great way to deploy groups of related resources.</span></span> <span data-ttu-id="d2bc2-105">Tato řada kurz ukazuje, jak vytvořit šablonu sady minimální přijatelná škálování a postup úpravy této šablony, aby vyhovovala různé scénáře.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-105">This tutorial series shows how to create a minimum viable scale set template and how to modify this template to suit various scenarios.</span></span> <span data-ttu-id="d2bc2-106">Všechny příklady pocházejí z tohoto [úložiště GitHub](https://github.com/gatneil/mvss).</span><span class="sxs-lookup"><span data-stu-id="d2bc2-106">All examples come from this [GitHub repository](https://github.com/gatneil/mvss).</span></span> 

<span data-ttu-id="d2bc2-107">Tato šablona má být jednoduché.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-107">This template is intended to be simple.</span></span> <span data-ttu-id="d2bc2-108">Podrobnější příklady škálování nastavení šablony najdete [úložiště GitHub šablony Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates) a vyhledejte složky, které obsahují řetězec `vmss`.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-108">For more complete examples of scale set templates, see the [Azure Quickstart Templates GitHub repository](https://github.com/Azure/azure-quickstart-templates) and search for folders that contain the string `vmss`.</span></span>

<span data-ttu-id="d2bc2-109">Pokud jste již obeznámeni s vytváření šablon, můžete přeskočit k části "Další kroky" a zjistit, jak upravit tuto šablonu.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-109">If you are already familiar with creating templates, you can skip to the "Next steps" section to see how to modify this template.</span></span>

## <a name="review-the-template"></a><span data-ttu-id="d2bc2-110">Zkontrolujte šablonu</span><span class="sxs-lookup"><span data-stu-id="d2bc2-110">Review the template</span></span>

<span data-ttu-id="d2bc2-111">Ke kontrole šablony sady naše minimální přijatelná škálování, použijte Githubu [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="d2bc2-111">Use GitHub to review our minimum viable scale set template, [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).</span></span>

<span data-ttu-id="d2bc2-112">V tomto kurzu jsme zkontrolujte diff (`git diff master minimum-viable-scale-set`) k vytvoření minimální přijatelná měřítka nastavit šablonu část podle část.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-112">In this tutorial, we examine the diff (`git diff master minimum-viable-scale-set`) to create the minimum viable scale set template piece by piece.</span></span>

## <a name="define-schema-and-contentversion"></a><span data-ttu-id="d2bc2-113">Zadejte $schema a contentVersion</span><span class="sxs-lookup"><span data-stu-id="d2bc2-113">Define $schema and contentVersion</span></span>
<span data-ttu-id="d2bc2-114">Nejprve definujeme `$schema` a `contentVersion` v šabloně.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-114">First, we define `$schema` and `contentVersion` in the template.</span></span> <span data-ttu-id="d2bc2-115">`$schema` Element definuje verze jazyka šablony a používá se pro Visual Studio zvýraznění syntaxe a podobné funkce ověřování.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-115">The `$schema` element defines the version of the template language and is used for Visual Studio syntax highlighting and similar validation features.</span></span> <span data-ttu-id="d2bc2-116">`contentVersion` Element nepoužívá Azure.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-116">The `contentVersion` element is not used by Azure.</span></span> <span data-ttu-id="d2bc2-117">Místo toho pomáhá udržovat přehled o verzi šablony.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-117">Instead, it helps you keep track of the template version.</span></span>

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```
## <a name="define-parameters"></a><span data-ttu-id="d2bc2-118">Definujte parametry</span><span class="sxs-lookup"><span data-stu-id="d2bc2-118">Define parameters</span></span>
<span data-ttu-id="d2bc2-119">V dalším kroku jsme definovali dva parametry `adminUsername` a `adminPassword`.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-119">Next, we define two parameters, `adminUsername` and `adminPassword`.</span></span> <span data-ttu-id="d2bc2-120">Parametry jsou hodnoty, které zadáte v době nasazení.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-120">Parameters are values you specify at the time of deployment.</span></span> <span data-ttu-id="d2bc2-121">`adminUsername` Parametr je jednoduše `string` typu, ale protože `adminPassword` je tajný klíč, jsme poskytněte typu `securestring`.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-121">The `adminUsername` parameter is simply a `string` type, but because `adminPassword` is a secret, we give it type `securestring`.</span></span> <span data-ttu-id="d2bc2-122">Později tyto parametry se předávají do konfigurace sady škálování.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-122">Later, these parameters are passed into the scale set configuration.</span></span>

```json
  "parameters": {
    "adminUsername": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    }
  },
```
## <a name="define-variables"></a><span data-ttu-id="d2bc2-123">Definování proměnných</span><span class="sxs-lookup"><span data-stu-id="d2bc2-123">Define variables</span></span>
<span data-ttu-id="d2bc2-124">Správce prostředků šablony vám také umožní zadat proměnné určené k použít později v šabloně.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-124">Resource Manager templates also let you define variables to be used later in the template.</span></span> <span data-ttu-id="d2bc2-125">Naše ukázka nepoužívá všechny proměnné, takže jsme jste prázdné objekt JSON.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-125">Our example doesn't use any variables, so we've left the JSON object empty.</span></span>

```json
  "variables": {},
```

## <a name="define-resources"></a><span data-ttu-id="d2bc2-126">Definování zdrojů</span><span class="sxs-lookup"><span data-stu-id="d2bc2-126">Define resources</span></span>
<span data-ttu-id="d2bc2-127">Dále je oddílu prostředků šablony.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-127">Next is the resources section of the template.</span></span> <span data-ttu-id="d2bc2-128">Tady můžete definovat, co Opravdu chcete nasadit.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-128">Here, you define what you actually want to deploy.</span></span> <span data-ttu-id="d2bc2-129">Na rozdíl od `parameters` a `variables` (které jsou objekty JSON), `resources` je JSON seznam objektů JSON.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-129">Unlike `parameters` and `variables` (which are JSON objects), `resources` is a JSON list of JSON objects.</span></span>

```json
   "resources": [
```

<span data-ttu-id="d2bc2-130">Všechny zdroje vyžadují `type`, `name`, `apiVersion`, a `location` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-130">All resources require `type`, `name`, `apiVersion`, and `location` properties.</span></span> <span data-ttu-id="d2bc2-131">První prostředek v tomto příkladu má typ `Microsft.Network/virtualNetwork`, název `myVnet`a apiVersion `2016-03-30`.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-131">This example's first resource has type `Microsft.Network/virtualNetwork`, name `myVnet`, and apiVersion `2016-03-30`.</span></span> <span data-ttu-id="d2bc2-132">(Nejnovější verzi rozhraní API pro typ prostředku, najdete v tématu [dokumentace k Azure REST API](https://docs.microsoft.com/rest/api/).)</span><span class="sxs-lookup"><span data-stu-id="d2bc2-132">(To find the latest API version for a resource type, see the [Azure REST API documentation](https://docs.microsoft.com/rest/api/).)</span></span>

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2016-12-01",
```

## <a name="specify-location"></a><span data-ttu-id="d2bc2-133">Zadejte umístění</span><span class="sxs-lookup"><span data-stu-id="d2bc2-133">Specify location</span></span>
<span data-ttu-id="d2bc2-134">Pokud chcete zadat umístění pro virtuální síť, používáme [funkce šablony Resource Manageru](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="d2bc2-134">To specify the location for the virtual network, we use a [Resource Manager template function](../azure-resource-manager/resource-group-template-functions.md).</span></span> <span data-ttu-id="d2bc2-135">Tato funkce musí být uzavřena do uvozovek a hranaté závorky takto: `"[<template-function>]"`.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-135">This function must be enclosed in quotes and square brackets like this: `"[<template-function>]"`.</span></span> <span data-ttu-id="d2bc2-136">V tomto případě používáme `resourceGroup` funkce.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-136">In this case, we use the `resourceGroup` function.</span></span> <span data-ttu-id="d2bc2-137">Přebírá v bez argumentů a vrátí objekt JSON s metadata o skupině prostředků, které toto nasazení je nasazován na.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-137">It takes in no arguments and returns a JSON object with metadata about the resource group this deployment is being deployed to.</span></span> <span data-ttu-id="d2bc2-138">Skupina prostředků je nastaven uživatelem v době nasazení.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-138">The resource group is set by the user at the time of deployment.</span></span> <span data-ttu-id="d2bc2-139">Jsme pak index do tohoto objektu JSON s `.location` získat umístění z objektu JSON.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-139">We then index into this JSON object with `.location` to get the location from the JSON object.</span></span>

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a><span data-ttu-id="d2bc2-140">Zadejte vlastnosti virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="d2bc2-140">Specify virtual network properties</span></span>
<span data-ttu-id="d2bc2-141">Každý prostředek, správce prostředků má svou vlastní `properties` v sekci konfigurace, které jsou specifické pro daný prostředek.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-141">Each Resource Manager resource has its own `properties` section for configurations specific to the resource.</span></span> <span data-ttu-id="d2bc2-142">V takovém případě jsme zadat, že virtuální síť musí mít jednu podsíť pomocí rozsah privátních IP adres `10.0.0.0/16`.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-142">In this case, we specify that the virtual network should have one subnet using the private IP address range `10.0.0.0/16`.</span></span> <span data-ttu-id="d2bc2-143">Sada škálování je vždy obsažené v jedné podsíti.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-143">A scale set is always contained within one subnet.</span></span> <span data-ttu-id="d2bc2-144">Ho nemůže span podsítě.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-144">It cannot span subnets.</span></span>

```json
       "properties": {
         "addressSpace": {
           "addressPrefixes": [
             "10.0.0.0/16"
           ]
         },
         "subnets": [
           {
             "name": "mySubnet",
             "properties": {
               "addressPrefix": "10.0.0.0/16"
             }
           }
         ]
       }
     },
```

## <a name="add-dependson-list"></a><span data-ttu-id="d2bc2-145">Přidat seznam dependsOn</span><span class="sxs-lookup"><span data-stu-id="d2bc2-145">Add dependsOn list</span></span>
<span data-ttu-id="d2bc2-146">Kromě požadovaných `type`, `name`, `apiVersion`, a `location` vlastnosti každého prostředku může mít volitelný `dependsOn` seznamu řetězců.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-146">In addition to the required `type`, `name`, `apiVersion`, and `location` properties, each resource can have an optional `dependsOn` list of strings.</span></span> <span data-ttu-id="d2bc2-147">Tento seznam určuje, které další prostředky z tohoto nasazení musí dokončit před nasazením tohoto prostředku.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-147">This list specifies which other resources from this deployment must finish before deploying this resource.</span></span>

<span data-ttu-id="d2bc2-148">V seznamu, virtuální sítě z předchozího příkladu je v tomto případě pouze jeden element.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-148">In this case, there is only one element in the list, the virtual network from the previous example.</span></span> <span data-ttu-id="d2bc2-149">Tuto závislost určíme, protože sada měřítka musí síti existovat před vytvořením všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-149">We specify this dependency because the scale set needs the network to exist before creating any VMs.</span></span> <span data-ttu-id="d2bc2-150">Tímto způsobem, byly sadou škálování může poskytnout tyto virtuální počítače privátních IP adres z rozsahu IP adres dříve zadán ve vlastnostech síťového.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-150">This way, the scale set can give these VMs private IP addresses from the IP address range previously specified in the network properties.</span></span> <span data-ttu-id="d2bc2-151">Formát každý řetězec v seznamu dependsOn je `<type>/<name>`.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-151">The format of each string in the dependsOn list is `<type>/<name>`.</span></span> <span data-ttu-id="d2bc2-152">Použijte stejný `type` a `name` použít dříve v definici prostředků virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-152">Use the same `type` and `name` used previously in the virtual network resource definition.</span></span>

```json
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "apiVersion": "2016-04-30-preview",
       "location": "[resourceGroup().location]",
       "dependsOn": [
         "Microsoft.Network/virtualNetworks/myVnet"
       ],
```
## <a name="specify-scale-set-properties"></a><span data-ttu-id="d2bc2-153">Zadejte vlastnosti sady škálování</span><span class="sxs-lookup"><span data-stu-id="d2bc2-153">Specify scale set properties</span></span>
<span data-ttu-id="d2bc2-154">Sady škálování mít mnoho vlastností pro přizpůsobení virtuálních počítačů v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-154">Scale sets have many properties for customizing the VMs in the scale set.</span></span> <span data-ttu-id="d2bc2-155">Úplný seznam těchto vlastností najdete v tématu [škálovací sady dokumentace k REST API](https://docs.microsoft.com/en-us/rest/api/virtualmachinescalesets/create-or-update-a-set).</span><span class="sxs-lookup"><span data-stu-id="d2bc2-155">For a full list of these properties, see the [scale set REST API documentation](https://docs.microsoft.com/en-us/rest/api/virtualmachinescalesets/create-or-update-a-set).</span></span> <span data-ttu-id="d2bc2-156">V tomto kurzu jsme nastaví jen několik běžně používané vlastností.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-156">For this tutorial, we will set only a few commonly used properties.</span></span>
### <a name="supply-vm-size-and-capacity"></a><span data-ttu-id="d2bc2-157">Zadejte velikost virtuálního počítače a kapacity</span><span class="sxs-lookup"><span data-stu-id="d2bc2-157">Supply VM size and capacity</span></span>
<span data-ttu-id="d2bc2-158">Měřítko nastavit musí vědět, jakou velikost virtuálního počítače k vytvoření ("název sku") a kolik tyto virtuální počítače na vytvoření ("kapacita sku").</span><span class="sxs-lookup"><span data-stu-id="d2bc2-158">The scale set needs to know what size of VM to create ("sku name") and how many such VMs to create ("sku capacity").</span></span> <span data-ttu-id="d2bc2-159">Informace, které velikosti virtuálních počítačů jsou k dispozici, najdete v tématu [velikosti virtuálních počítačů dokumentaci](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).</span><span class="sxs-lookup"><span data-stu-id="d2bc2-159">To see which VM sizes are available, see the [VM Sizes documentation](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).</span></span>

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a><span data-ttu-id="d2bc2-160">Vyberte typ aktualizace</span><span class="sxs-lookup"><span data-stu-id="d2bc2-160">Choose type of updates</span></span>
<span data-ttu-id="d2bc2-161">Také sad škálování je potřeba vědět, jak pro zpracování aktualizací v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-161">The scale set also needs to know how to handle updates on the scale set.</span></span> <span data-ttu-id="d2bc2-162">V současné době existují dvě možnosti, `Manual` a `Automatic`.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-162">Currently, there are two options, `Manual` and `Automatic`.</span></span> <span data-ttu-id="d2bc2-163">Další informace o rozdílech mezi nimi, najdete v dokumentaci na [jak upgradovat škálovací sadu](./virtual-machine-scale-sets-upgrade-scale-set.md).</span><span class="sxs-lookup"><span data-stu-id="d2bc2-163">For more information on the differences between the two, see the documentation on [how to upgrade a scale set](./virtual-machine-scale-sets-upgrade-scale-set.md).</span></span>

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a><span data-ttu-id="d2bc2-164">Vyberte operační systém virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="d2bc2-164">Choose VM operating system</span></span>
<span data-ttu-id="d2bc2-165">Měřítko nastavit musí vědět, jaké operační systém se umístí na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-165">The scale set needs to know what operating system to put on the VMs.</span></span> <span data-ttu-id="d2bc2-166">Zde vytvoříme virtuální počítače s bitovou kopii plně opravou 16.04 LTS Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-166">Here, we create the VMs with a fully patched Ubuntu 16.04-LTS image.</span></span>

```json
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
               "publisher": "Canonical",
               "offer": "UbuntuServer",
               "sku": "16.04-LTS",
               "version": "latest"
             }
           },
```

### <a name="specify-computernameprefix"></a><span data-ttu-id="d2bc2-167">Zadejte computerNamePrefix</span><span class="sxs-lookup"><span data-stu-id="d2bc2-167">Specify computerNamePrefix</span></span>
<span data-ttu-id="d2bc2-168">Sada škálování nasadí víc virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-168">The scale set deploys multiple VMs.</span></span> <span data-ttu-id="d2bc2-169">Místo zadání každý název virtuálního počítače, určíme `computerNamePrefix`.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-169">Instead of specifying each VM name, we specify `computerNamePrefix`.</span></span> <span data-ttu-id="d2bc2-170">Škálovací sadu připojí index k předponu pro každý virtuální počítač, aby názvy virtuálních počítačů formuláře `<computerNamePrefix>_<auto-generated-index>`.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-170">The scale set appends an index to the prefix for each VM, so VM names have the form `<computerNamePrefix>_<auto-generated-index>`.</span></span>

<span data-ttu-id="d2bc2-171">V následující fragment kódu použijeme parametry z před nastavit správce uživatelské jméno a heslo pro všechny virtuální počítače v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-171">In the following snippet, we use the parameters from before to set the administrator username and password for all VMs in the scale set.</span></span> <span data-ttu-id="d2bc2-172">Jsme to provést pomocí `parameters` funkce šablony.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-172">We do this with the `parameters` template function.</span></span> <span data-ttu-id="d2bc2-173">Tato funkce přebírá řetězec, který určuje, které parametr, který bude odkazovat na a výstupy hodnota tohoto parametru.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-173">This function takes in a string that specifies which parameter to refer to and outputs the value for that parameter.</span></span>

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a><span data-ttu-id="d2bc2-174">Zadejte konfigurace sítě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="d2bc2-174">Specify VM network configuration</span></span>
<span data-ttu-id="d2bc2-175">Nakonec je potřeba zadat příslušnou konfiguraci sítě pro virtuální počítače ve škálovací sadě.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-175">Finally, we need to specify the network configuration for the VMs in the scale set.</span></span> <span data-ttu-id="d2bc2-176">V takovém případě musíme pouze zadejte ID podsítě vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-176">In this case, we only need to specify the ID of the subnet created earlier.</span></span> <span data-ttu-id="d2bc2-177">Tato hodnota informuje měřítka nastavit uvést rozhraní sítě v této podsíti.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-177">This tells the scale set to put the network interfaces in this subnet.</span></span>

<span data-ttu-id="d2bc2-178">Můžete získat ID virtuální sítě obsahující podsíť pomocí `resourceId` funkce šablony.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-178">You can get the ID of the virtual network containing the subnet by using the `resourceId` template function.</span></span> <span data-ttu-id="d2bc2-179">Tato funkce přebírá typu a název prostředku a vrátí plně kvalifikovaný identifikátor prostředku.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-179">This function takes in the type and name of a resource and returns the fully qualified identifier of that resource.</span></span> <span data-ttu-id="d2bc2-180">Toto ID má tvar:`/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`</span><span class="sxs-lookup"><span data-stu-id="d2bc2-180">This ID has the form: `/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`</span></span>

<span data-ttu-id="d2bc2-181">Však identifikátor virtuální sítě není dostatečná.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-181">However, the identifier of the virtual network is not enough.</span></span> <span data-ttu-id="d2bc2-182">Musíte zadat konkrétní podsíti, aby měřítka nastavit virtuální počítače musí být v.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-182">You must specify the specific subnet that the scale set VMs should be in.</span></span> <span data-ttu-id="d2bc2-183">K tomuto účelu řetězení `/subnets/mySubnet` ID virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-183">To do this, concatenate `/subnets/mySubnet` to the ID of the virtual network.</span></span> <span data-ttu-id="d2bc2-184">Výsledkem je plně kvalifikovaný ID podsítě.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-184">The result is the fully qualified ID of the subnet.</span></span> <span data-ttu-id="d2bc2-185">Proveďte toto zřetězení s `concat` funkce, která přebírá v řadě řetězce a vrátí jejich zřetězení.</span><span class="sxs-lookup"><span data-stu-id="d2bc2-185">Do this concatenation with the `concat` function, which takes in a series of strings and returns their concatenation.</span></span>

```json
           "networkProfile": {
             "networkInterfaceConfigurations": [
               {
                 "name": "myNic",
                 "properties": {
                   "primary": "true",
                   "ipConfigurations": [
                     {
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
                           "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
                         }
                       }
                     }
                   ]
                 }
               }
             ]
           }
         }
       }
     }
   ]
}

```

## <a name="next-steps"></a><span data-ttu-id="d2bc2-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d2bc2-186">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
