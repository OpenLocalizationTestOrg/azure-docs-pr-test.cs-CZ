---
title: "aaaLearn o škálování virtuálních počítačů nastavit šablony | Microsoft Docs"
description: "Další informace, že toocreate minimální přijatelná škálování nastavení šablony pro sady škálování virtuálního počítače"
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
ms.openlocfilehash: b7a1cf6c03b22585e16db9c071d45795c8ae75df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-virtual-machine-scale-set-templates"></a><span data-ttu-id="1eb82-103">Další informace o šablonách sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1eb82-103">Learn about virtual machine scale set templates</span></span>
<span data-ttu-id="1eb82-104">[Šablony Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) jsou skvělý způsob toodeploy skupiny související prostředky.</span><span class="sxs-lookup"><span data-stu-id="1eb82-104">[Azure Resource Manager templates](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) are a great way toodeploy groups of related resources.</span></span> <span data-ttu-id="1eb82-105">Tato řada kurz ukazuje, jak toocreate minimální přijatelná škálování nastavit šablonu a toomodify této šablony toosuit různé scénáře.</span><span class="sxs-lookup"><span data-stu-id="1eb82-105">This tutorial series shows how toocreate a minimum viable scale set template and how toomodify this template toosuit various scenarios.</span></span> <span data-ttu-id="1eb82-106">Všechny příklady pocházejí z tohoto [úložiště GitHub](https://github.com/gatneil/mvss).</span><span class="sxs-lookup"><span data-stu-id="1eb82-106">All examples come from this [GitHub repository](https://github.com/gatneil/mvss).</span></span> 

<span data-ttu-id="1eb82-107">Tato šablona je určený toobe jednoduché.</span><span class="sxs-lookup"><span data-stu-id="1eb82-107">This template is intended toobe simple.</span></span> <span data-ttu-id="1eb82-108">Podrobnější příklady škálování nastavení šablony najdete hello [úložiště GitHub šablony Azure rychlý Start](https://github.com/Azure/azure-quickstart-templates) a vyhledejte složky, které obsahují řetězec hello `vmss`.</span><span class="sxs-lookup"><span data-stu-id="1eb82-108">For more complete examples of scale set templates, see hello [Azure Quickstart Templates GitHub repository](https://github.com/Azure/azure-quickstart-templates) and search for folders that contain hello string `vmss`.</span></span>

<span data-ttu-id="1eb82-109">Pokud jste již obeznámeni s vytváření šablon, můžete přeskočit toohello toosee části "Další kroky" jak toomodify této šablony.</span><span class="sxs-lookup"><span data-stu-id="1eb82-109">If you are already familiar with creating templates, you can skip toohello "Next steps" section toosee how toomodify this template.</span></span>

## <a name="review-hello-template"></a><span data-ttu-id="1eb82-110">Šablona kontrolní hello</span><span class="sxs-lookup"><span data-stu-id="1eb82-110">Review hello template</span></span>

<span data-ttu-id="1eb82-111">Použít Githubu tooreview naše minimální přijatelná škálování nastavení šablony, [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="1eb82-111">Use GitHub tooreview our minimum viable scale set template, [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).</span></span>

<span data-ttu-id="1eb82-112">V tomto kurzu jsme zkontrolujte hello diff (`git diff master minimum-viable-scale-set`) sady toocreate hello minimální přijatelná škálování šablony část podle část.</span><span class="sxs-lookup"><span data-stu-id="1eb82-112">In this tutorial, we examine hello diff (`git diff master minimum-viable-scale-set`) toocreate hello minimum viable scale set template piece by piece.</span></span>

## <a name="define-schema-and-contentversion"></a><span data-ttu-id="1eb82-113">Zadejte $schema a contentVersion</span><span class="sxs-lookup"><span data-stu-id="1eb82-113">Define $schema and contentVersion</span></span>
<span data-ttu-id="1eb82-114">Nejprve definujeme `$schema` a `contentVersion` v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="1eb82-114">First, we define `$schema` and `contentVersion` in hello template.</span></span> <span data-ttu-id="1eb82-115">Hello `$schema` element definuje hello verze jazyka šablony hello a používá se pro Visual Studio zvýraznění syntaxe a podobné funkce ověřování.</span><span class="sxs-lookup"><span data-stu-id="1eb82-115">hello `$schema` element defines hello version of hello template language and is used for Visual Studio syntax highlighting and similar validation features.</span></span> <span data-ttu-id="1eb82-116">Hello `contentVersion` element nepoužívá Azure.</span><span class="sxs-lookup"><span data-stu-id="1eb82-116">hello `contentVersion` element is not used by Azure.</span></span> <span data-ttu-id="1eb82-117">Místo toho pomáhá udržovat přehled o verzi šablony hello.</span><span class="sxs-lookup"><span data-stu-id="1eb82-117">Instead, it helps you keep track of hello template version.</span></span>

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```
## <a name="define-parameters"></a><span data-ttu-id="1eb82-118">Definujte parametry</span><span class="sxs-lookup"><span data-stu-id="1eb82-118">Define parameters</span></span>
<span data-ttu-id="1eb82-119">V dalším kroku jsme definovali dva parametry `adminUsername` a `adminPassword`.</span><span class="sxs-lookup"><span data-stu-id="1eb82-119">Next, we define two parameters, `adminUsername` and `adminPassword`.</span></span> <span data-ttu-id="1eb82-120">Parametry jsou hodnoty, které zadáte během hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="1eb82-120">Parameters are values you specify at hello time of deployment.</span></span> <span data-ttu-id="1eb82-121">Hello `adminUsername` parametr je jednoduše `string` typu, ale protože `adminPassword` je tajný klíč, jsme poskytněte typu `securestring`.</span><span class="sxs-lookup"><span data-stu-id="1eb82-121">hello `adminUsername` parameter is simply a `string` type, but because `adminPassword` is a secret, we give it type `securestring`.</span></span> <span data-ttu-id="1eb82-122">Později tyto parametry se předávají do konfigurace sady škálování hello.</span><span class="sxs-lookup"><span data-stu-id="1eb82-122">Later, these parameters are passed into hello scale set configuration.</span></span>

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
## <a name="define-variables"></a><span data-ttu-id="1eb82-123">Definování proměnných</span><span class="sxs-lookup"><span data-stu-id="1eb82-123">Define variables</span></span>
<span data-ttu-id="1eb82-124">Šablony Resource Manageru vám také umožní definovat proměnné toobe použít později v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="1eb82-124">Resource Manager templates also let you define variables toobe used later in hello template.</span></span> <span data-ttu-id="1eb82-125">Naše ukázka nepoužívá všechny proměnné, takže jsme jste prázdné objekt JSON hello.</span><span class="sxs-lookup"><span data-stu-id="1eb82-125">Our example doesn't use any variables, so we've left hello JSON object empty.</span></span>

```json
  "variables": {},
```

## <a name="define-resources"></a><span data-ttu-id="1eb82-126">Definování zdrojů</span><span class="sxs-lookup"><span data-stu-id="1eb82-126">Define resources</span></span>
<span data-ttu-id="1eb82-127">Dále je oddílu prostředků hello hello šablony.</span><span class="sxs-lookup"><span data-stu-id="1eb82-127">Next is hello resources section of hello template.</span></span> <span data-ttu-id="1eb82-128">Zde, určit, co je ve skutečnosti toodeploy.</span><span class="sxs-lookup"><span data-stu-id="1eb82-128">Here, you define what you actually want toodeploy.</span></span> <span data-ttu-id="1eb82-129">Na rozdíl od `parameters` a `variables` (které jsou objekty JSON), `resources` je JSON seznam objektů JSON.</span><span class="sxs-lookup"><span data-stu-id="1eb82-129">Unlike `parameters` and `variables` (which are JSON objects), `resources` is a JSON list of JSON objects.</span></span>

```json
   "resources": [
```

<span data-ttu-id="1eb82-130">Všechny zdroje vyžadují `type`, `name`, `apiVersion`, a `location` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="1eb82-130">All resources require `type`, `name`, `apiVersion`, and `location` properties.</span></span> <span data-ttu-id="1eb82-131">První prostředek v tomto příkladu má typ `Microsft.Network/virtualNetwork`, název `myVnet`a apiVersion `2016-03-30`.</span><span class="sxs-lookup"><span data-stu-id="1eb82-131">This example's first resource has type `Microsft.Network/virtualNetwork`, name `myVnet`, and apiVersion `2016-03-30`.</span></span> <span data-ttu-id="1eb82-132">(toofind hello nejnovější verzi rozhraní API pro typ prostředku, najdete v části hello [dokumentace k Azure REST API](https://docs.microsoft.com/rest/api/).)</span><span class="sxs-lookup"><span data-stu-id="1eb82-132">(toofind hello latest API version for a resource type, see hello [Azure REST API documentation](https://docs.microsoft.com/rest/api/).)</span></span>

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2016-12-01",
```

## <a name="specify-location"></a><span data-ttu-id="1eb82-133">Zadejte umístění</span><span class="sxs-lookup"><span data-stu-id="1eb82-133">Specify location</span></span>
<span data-ttu-id="1eb82-134">toospecify hello umístění pro virtuální síť hello používáme [funkce šablony Resource Manageru](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="1eb82-134">toospecify hello location for hello virtual network, we use a [Resource Manager template function](../azure-resource-manager/resource-group-template-functions.md).</span></span> <span data-ttu-id="1eb82-135">Tato funkce musí být uzavřena do uvozovek a hranaté závorky takto: `"[<template-function>]"`.</span><span class="sxs-lookup"><span data-stu-id="1eb82-135">This function must be enclosed in quotes and square brackets like this: `"[<template-function>]"`.</span></span> <span data-ttu-id="1eb82-136">V tomto případě používáme hello `resourceGroup` funkce.</span><span class="sxs-lookup"><span data-stu-id="1eb82-136">In this case, we use hello `resourceGroup` function.</span></span> <span data-ttu-id="1eb82-137">Přebírá v bez argumentů a vrátí objekt JSON s metadata o hello skupinu prostředků, které toto nasazení je nasazován na.</span><span class="sxs-lookup"><span data-stu-id="1eb82-137">It takes in no arguments and returns a JSON object with metadata about hello resource group this deployment is being deployed to.</span></span> <span data-ttu-id="1eb82-138">Skupina prostředků Hello nastavena hello uživatelem v době hello nasazení.</span><span class="sxs-lookup"><span data-stu-id="1eb82-138">hello resource group is set by hello user at hello time of deployment.</span></span> <span data-ttu-id="1eb82-139">Jsme pak index do tohoto objektu JSON s `.location` tooget hello umístění z objektu JSON, hello.</span><span class="sxs-lookup"><span data-stu-id="1eb82-139">We then index into this JSON object with `.location` tooget hello location from hello JSON object.</span></span>

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a><span data-ttu-id="1eb82-140">Zadejte vlastnosti virtuální sítě</span><span class="sxs-lookup"><span data-stu-id="1eb82-140">Specify virtual network properties</span></span>
<span data-ttu-id="1eb82-141">Každý prostředek, správce prostředků má svou vlastní `properties` části pro konkrétní toohello prostředek konfigurace.</span><span class="sxs-lookup"><span data-stu-id="1eb82-141">Each Resource Manager resource has its own `properties` section for configurations specific toohello resource.</span></span> <span data-ttu-id="1eb82-142">V takovém případě určíme tuto hello virtuální síť musí mít jednu podsíť pomocí rozsah hello privátních IP adres `10.0.0.0/16`.</span><span class="sxs-lookup"><span data-stu-id="1eb82-142">In this case, we specify that hello virtual network should have one subnet using hello private IP address range `10.0.0.0/16`.</span></span> <span data-ttu-id="1eb82-143">Sada škálování je vždy obsažené v jedné podsíti.</span><span class="sxs-lookup"><span data-stu-id="1eb82-143">A scale set is always contained within one subnet.</span></span> <span data-ttu-id="1eb82-144">Ho nemůže span podsítě.</span><span class="sxs-lookup"><span data-stu-id="1eb82-144">It cannot span subnets.</span></span>

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

## <a name="add-dependson-list"></a><span data-ttu-id="1eb82-145">Přidat seznam dependsOn</span><span class="sxs-lookup"><span data-stu-id="1eb82-145">Add dependsOn list</span></span>
<span data-ttu-id="1eb82-146">Kromě toho toohello požaduje `type`, `name`, `apiVersion`, a `location` vlastnosti každého prostředku může mít volitelný `dependsOn` seznamu řetězců.</span><span class="sxs-lookup"><span data-stu-id="1eb82-146">In addition toohello required `type`, `name`, `apiVersion`, and `location` properties, each resource can have an optional `dependsOn` list of strings.</span></span> <span data-ttu-id="1eb82-147">Tento seznam určuje, které další prostředky z tohoto nasazení musí dokončit před nasazením tohoto prostředku.</span><span class="sxs-lookup"><span data-stu-id="1eb82-147">This list specifies which other resources from this deployment must finish before deploying this resource.</span></span>

<span data-ttu-id="1eb82-148">V seznamu hello hello virtuální sítě z předchozího příkladu hello je v tomto případě pouze jeden element.</span><span class="sxs-lookup"><span data-stu-id="1eb82-148">In this case, there is only one element in hello list, hello virtual network from hello previous example.</span></span> <span data-ttu-id="1eb82-149">Tuto závislost určíme, protože hello sady škálování vyžaduje hello sítě tooexist před vytvořením všechny virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="1eb82-149">We specify this dependency because hello scale set needs hello network tooexist before creating any VMs.</span></span> <span data-ttu-id="1eb82-150">Tímto způsobem hello škálovací sadu mohou poskytnout tyto virtuální počítače privátních IP adres z rozsahu IP adres hello dříve zadanou ve vlastnosti sítě hello.</span><span class="sxs-lookup"><span data-stu-id="1eb82-150">This way, hello scale set can give these VMs private IP addresses from hello IP address range previously specified in hello network properties.</span></span> <span data-ttu-id="1eb82-151">Formát Hello každý řetězec v seznamu dependsOn hello je `<type>/<name>`.</span><span class="sxs-lookup"><span data-stu-id="1eb82-151">hello format of each string in hello dependsOn list is `<type>/<name>`.</span></span> <span data-ttu-id="1eb82-152">Použití hello stejné `type` a `name` použít dříve v definici prostředků hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="1eb82-152">Use hello same `type` and `name` used previously in hello virtual network resource definition.</span></span>

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
## <a name="specify-scale-set-properties"></a><span data-ttu-id="1eb82-153">Zadejte vlastnosti sady škálování</span><span class="sxs-lookup"><span data-stu-id="1eb82-153">Specify scale set properties</span></span>
<span data-ttu-id="1eb82-154">Sady škálování mít mnoho vlastností pro přizpůsobení hello virtuální počítače ve škálovací sadě hello.</span><span class="sxs-lookup"><span data-stu-id="1eb82-154">Scale sets have many properties for customizing hello VMs in hello scale set.</span></span> <span data-ttu-id="1eb82-155">Úplný seznam těchto vlastností najdete v tématu hello [škálovací sady dokumentace k REST API](https://docs.microsoft.com/en-us/rest/api/virtualmachinescalesets/create-or-update-a-set).</span><span class="sxs-lookup"><span data-stu-id="1eb82-155">For a full list of these properties, see hello [scale set REST API documentation](https://docs.microsoft.com/en-us/rest/api/virtualmachinescalesets/create-or-update-a-set).</span></span> <span data-ttu-id="1eb82-156">V tomto kurzu jsme nastaví jen několik běžně používané vlastností.</span><span class="sxs-lookup"><span data-stu-id="1eb82-156">For this tutorial, we will set only a few commonly used properties.</span></span>
### <a name="supply-vm-size-and-capacity"></a><span data-ttu-id="1eb82-157">Zadejte velikost virtuálního počítače a kapacity</span><span class="sxs-lookup"><span data-stu-id="1eb82-157">Supply VM size and capacity</span></span>
<span data-ttu-id="1eb82-158">škálování Hello nastavit tooknow potřebám, jakou velikost virtuálního počítače toocreate ("název sku") a kolik takové toocreate virtuální počítače ("kapacita sku").</span><span class="sxs-lookup"><span data-stu-id="1eb82-158">hello scale set needs tooknow what size of VM toocreate ("sku name") and how many such VMs toocreate ("sku capacity").</span></span> <span data-ttu-id="1eb82-159">toosee které velikosti virtuálních počítačů jsou k dispozici, najdete v části hello [velikosti virtuálních počítačů dokumentaci](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).</span><span class="sxs-lookup"><span data-stu-id="1eb82-159">toosee which VM sizes are available, see hello [VM Sizes documentation](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).</span></span>

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a><span data-ttu-id="1eb82-160">Vyberte typ aktualizace</span><span class="sxs-lookup"><span data-stu-id="1eb82-160">Choose type of updates</span></span>
<span data-ttu-id="1eb82-161">také je nutné Hello škálovací sadu tooknow jak toohandle aktualizuje na sadě škálování hello.</span><span class="sxs-lookup"><span data-stu-id="1eb82-161">hello scale set also needs tooknow how toohandle updates on hello scale set.</span></span> <span data-ttu-id="1eb82-162">V současné době existují dvě možnosti, `Manual` a `Automatic`.</span><span class="sxs-lookup"><span data-stu-id="1eb82-162">Currently, there are two options, `Manual` and `Automatic`.</span></span> <span data-ttu-id="1eb82-163">Další informace o hello rozdíly mezi hello dva, naleznete v dokumentaci k hello na [jak tooupgrade škálování nastavit](./virtual-machine-scale-sets-upgrade-scale-set.md).</span><span class="sxs-lookup"><span data-stu-id="1eb82-163">For more information on hello differences between hello two, see hello documentation on [how tooupgrade a scale set](./virtual-machine-scale-sets-upgrade-scale-set.md).</span></span>

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a><span data-ttu-id="1eb82-164">Vyberte operační systém virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="1eb82-164">Choose VM operating system</span></span>
<span data-ttu-id="1eb82-165">Hello sady škálování potřebám tooknow tooput jaký operační systém na virtuálních počítačích hello.</span><span class="sxs-lookup"><span data-stu-id="1eb82-165">hello scale set needs tooknow what operating system tooput on hello VMs.</span></span> <span data-ttu-id="1eb82-166">Zde vytvoříme hello virtuální počítače s bitovou kopii plně opravou 16.04 LTS Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="1eb82-166">Here, we create hello VMs with a fully patched Ubuntu 16.04-LTS image.</span></span>

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

### <a name="specify-computernameprefix"></a><span data-ttu-id="1eb82-167">Zadejte computerNamePrefix</span><span class="sxs-lookup"><span data-stu-id="1eb82-167">Specify computerNamePrefix</span></span>
<span data-ttu-id="1eb82-168">Sada škálování Hello nasadí víc virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="1eb82-168">hello scale set deploys multiple VMs.</span></span> <span data-ttu-id="1eb82-169">Místo zadání každý název virtuálního počítače, určíme `computerNamePrefix`.</span><span class="sxs-lookup"><span data-stu-id="1eb82-169">Instead of specifying each VM name, we specify `computerNamePrefix`.</span></span> <span data-ttu-id="1eb82-170">Hello škálovací sadu připojí předponu toohello index pro každý virtuální počítač, aby názvy virtuálních počítačů hello formuláře `<computerNamePrefix>_<auto-generated-index>`.</span><span class="sxs-lookup"><span data-stu-id="1eb82-170">hello scale set appends an index toohello prefix for each VM, so VM names have hello form `<computerNamePrefix>_<auto-generated-index>`.</span></span>

<span data-ttu-id="1eb82-171">V hello následující fragment kódu použijeme hello parametry z před tooset hello jméno a heslo správce pro všechny virtuální počítače ve škálovací sadě hello.</span><span class="sxs-lookup"><span data-stu-id="1eb82-171">In hello following snippet, we use hello parameters from before tooset hello administrator username and password for all VMs in hello scale set.</span></span> <span data-ttu-id="1eb82-172">Provedeme to pomocí hello `parameters` funkce šablony.</span><span class="sxs-lookup"><span data-stu-id="1eb82-172">We do this with hello `parameters` template function.</span></span> <span data-ttu-id="1eb82-173">Tato funkce přebírá řetězec, který určuje, které tooand toorefer parametr výstupy hello hodnotu tohoto parametru.</span><span class="sxs-lookup"><span data-stu-id="1eb82-173">This function takes in a string that specifies which parameter toorefer tooand outputs hello value for that parameter.</span></span>

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a><span data-ttu-id="1eb82-174">Zadejte konfigurace sítě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="1eb82-174">Specify VM network configuration</span></span>
<span data-ttu-id="1eb82-175">Nakonec potřebujeme toospecify hello síťovou konfiguraci pro virtuální počítače hello v sadě škálování hello.</span><span class="sxs-lookup"><span data-stu-id="1eb82-175">Finally, we need toospecify hello network configuration for hello VMs in hello scale set.</span></span> <span data-ttu-id="1eb82-176">V takovém případě potřebujeme pouze toospecify hello ID podsítě hello vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="1eb82-176">In this case, we only need toospecify hello ID of hello subnet created earlier.</span></span> <span data-ttu-id="1eb82-177">Tato hodnota informuje, že hello sady škálování tooput hello síťových rozhraní v této podsíti.</span><span class="sxs-lookup"><span data-stu-id="1eb82-177">This tells hello scale set tooput hello network interfaces in this subnet.</span></span>

<span data-ttu-id="1eb82-178">Můžete získat ID hello hello virtuální sítě obsahující hello podsíť pomocí hello `resourceId` funkce šablony.</span><span class="sxs-lookup"><span data-stu-id="1eb82-178">You can get hello ID of hello virtual network containing hello subnet by using hello `resourceId` template function.</span></span> <span data-ttu-id="1eb82-179">Tato funkce přebírá hello typ a název prostředku a vrátí hello plně kvalifikovaný identifikátor prostředku.</span><span class="sxs-lookup"><span data-stu-id="1eb82-179">This function takes in hello type and name of a resource and returns hello fully qualified identifier of that resource.</span></span> <span data-ttu-id="1eb82-180">Toto ID má hello formuláře:`/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`</span><span class="sxs-lookup"><span data-stu-id="1eb82-180">This ID has hello form: `/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`</span></span>

<span data-ttu-id="1eb82-181">Ale hello identifikátor hello virtuální sítě není dostatečná.</span><span class="sxs-lookup"><span data-stu-id="1eb82-181">However, hello identifier of hello virtual network is not enough.</span></span> <span data-ttu-id="1eb82-182">Je nutné zadat hello konkrétní podsítě, která hello škálovací sadu virtuálních počítačů by měla být ve.</span><span class="sxs-lookup"><span data-stu-id="1eb82-182">You must specify hello specific subnet that hello scale set VMs should be in.</span></span> <span data-ttu-id="1eb82-183">toodo, zřetězení `/subnets/mySubnet` toohello ID hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="1eb82-183">toodo this, concatenate `/subnets/mySubnet` toohello ID of hello virtual network.</span></span> <span data-ttu-id="1eb82-184">výsledek Hello je hello plně kvalifikovaný ID podsítě hello.</span><span class="sxs-lookup"><span data-stu-id="1eb82-184">hello result is hello fully qualified ID of hello subnet.</span></span> <span data-ttu-id="1eb82-185">Proveďte toto zřetězení s hello `concat` funkce, která přebírá v řadě řetězce a vrátí jejich zřetězení.</span><span class="sxs-lookup"><span data-stu-id="1eb82-185">Do this concatenation with hello `concat` function, which takes in a series of strings and returns their concatenation.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="1eb82-186">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1eb82-186">Next steps</span></span>

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
