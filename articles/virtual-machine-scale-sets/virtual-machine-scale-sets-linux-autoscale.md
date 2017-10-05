---
title: "Sadách škálování virtuálních počítačů Linux škálování | Microsoft Docs"
description: "Nastavení automatického škálování pro Linux sadu škálování virtuálního počítače pomocí rozhraní příkazového řádku Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 83e93d9c-cac0-41d3-8316-6016f5ed0ce4
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: eff4add1cb16fe25022787668dc1d2277845dd95
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="automatically-scale-linux-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="6f0d9-103">Automatické škálování počítače se systémem Linux v škálovací sadu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="6f0d9-103">Automatically scale Linux machines in a virtual machine scale set</span></span>
<span data-ttu-id="6f0d9-104">Sady škálování virtuálního počítače můžete snadno můžete nasadit a spravovat jako sada identickými virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-104">Virtual machine scale sets make it easy for you to deploy and manage identical virtual machines as a set.</span></span> <span data-ttu-id="6f0d9-105">Sady škálování pro aplikace hyperškálovatelný systém zajistit vysoce škálovatelného a přizpůsobitelné výpočetní vrstvy a podporují Image pro platformu Windows, Image platformy Linux, vlastních bitových kopií a rozšíření.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-105">Scale sets provide a highly scalable and customizable compute layer for hyperscale applications, and they support Windows platform images, Linux platform images, custom images, and extensions.</span></span> <span data-ttu-id="6f0d9-106">Další informace najdete v tématu [Přehled sady škálování virtuálních počítačů](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6f0d9-106">To learn more, see [Virtual Machine Scale Sets Overview](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="6f0d9-107">V tomto kurzu se dozvíte, jak vytvořit škálovací sadu virtuálních počítačů Linux pomocí nejnovější verze Ubuntu Linux.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-107">This tutorial shows you how to create a scale set of Linux virtual machines using the latest version of Ubuntu Linux.</span></span> <span data-ttu-id="6f0d9-108">Tento kurz také ukazuje, jak automaticky škálovat na počítače v sadě.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-108">The tutorial also shows you how to automatically scale the machines in the set.</span></span> <span data-ttu-id="6f0d9-109">Můžete vytvořit měřítko nastavení a nastavení škálování vytvořením šablonu Azure Resource Manager a nasazení pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-109">You create the scale set and set up scaling by creating an Azure Resource Manager template and deploying it using Azure CLI.</span></span> <span data-ttu-id="6f0d9-110">Další informace o šablonách najdete v tématu o [vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="6f0d9-110">For more information about templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> <span data-ttu-id="6f0d9-111">Další informace o automatické škálování sad škálování najdete v tématu [automatické škálování a sadách škálování virtuálního počítače](virtual-machine-scale-sets-autoscale-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6f0d9-111">To learn more about automatic scaling of scale sets, see [Automatic scaling and Virtual Machine Scale Sets](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

<span data-ttu-id="6f0d9-112">V tomto kurzu nasadíte následující prostředků a rozšíření:</span><span class="sxs-lookup"><span data-stu-id="6f0d9-112">In this tutorial, you deploy the following resources and extensions:</span></span>

* <span data-ttu-id="6f0d9-113">Microsoft.Storage/storageAccounts.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-113">Microsoft.Storage/storageAccounts</span></span>
* <span data-ttu-id="6f0d9-114">Microsoft.Network/virtualNetworks</span><span class="sxs-lookup"><span data-stu-id="6f0d9-114">Microsoft.Network/virtualNetworks</span></span>
* <span data-ttu-id="6f0d9-115">Microsoft.Network/publicIPAddresses</span><span class="sxs-lookup"><span data-stu-id="6f0d9-115">Microsoft.Network/publicIPAddresses</span></span>
* <span data-ttu-id="6f0d9-116">Microsoft.Network/loadBalancers</span><span class="sxs-lookup"><span data-stu-id="6f0d9-116">Microsoft.Network/loadBalancers</span></span>
* <span data-ttu-id="6f0d9-117">Microsoft.Network/networkInterfaces</span><span class="sxs-lookup"><span data-stu-id="6f0d9-117">Microsoft.Network/networkInterfaces</span></span>
* <span data-ttu-id="6f0d9-118">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="6f0d9-118">Microsoft.Compute/virtualMachines</span></span>
* <span data-ttu-id="6f0d9-119">Microsoft.Compute/virtualMachineScaleSets</span><span class="sxs-lookup"><span data-stu-id="6f0d9-119">Microsoft.Compute/virtualMachineScaleSets</span></span>
* <span data-ttu-id="6f0d9-120">Microsoft.Insights.VMDiagnosticsSettings</span><span class="sxs-lookup"><span data-stu-id="6f0d9-120">Microsoft.Insights.VMDiagnosticsSettings</span></span>
* <span data-ttu-id="6f0d9-121">Microsoft.Insights/autoscaleSettings</span><span class="sxs-lookup"><span data-stu-id="6f0d9-121">Microsoft.Insights/autoscaleSettings</span></span>

<span data-ttu-id="6f0d9-122">Další informace o prostředky Resource Manageru najdete v tématu [Azure Resource Manager oproti nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="6f0d9-122">For more information about Resource Manager resources, see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

<span data-ttu-id="6f0d9-123">Než začnete s kroky v tomto kurzu [nainstalovat Azure CLI](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="6f0d9-123">Before you get started with the steps in this tutorial, [install the Azure CLI](../cli-install-nodejs.md).</span></span>

## <a name="step-1-create-a-resource-group-and-a-storage-account"></a><span data-ttu-id="6f0d9-124">Krok 1: Vytvoření skupiny prostředků a účet úložiště</span><span class="sxs-lookup"><span data-stu-id="6f0d9-124">Step 1: Create a resource group and a storage account</span></span>

1. <span data-ttu-id="6f0d9-125">**Přihlaste se k Microsoft Azure**</span><span class="sxs-lookup"><span data-stu-id="6f0d9-125">**Sign in to Microsoft Azure**</span></span>  
<span data-ttu-id="6f0d9-126">Ve svém rozhraní příkazového řádku (Bash, terminálu, příkazového řádku), přepněte do režimu Resource Manager a potom [přihlásit pomocí id váš pracovní nebo školní](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Postupujte podle pokynů pro prostředí interaktivní přihlášení k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-126">In your command-line interface (Bash, Terminal, Command prompt), switch to Resource Manager mode, and then [log in with your work or school id](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Follow the prompts for an interactive login experience to your Azure account.</span></span>

    ```cli   
    azure config mode arm

    azure login
    ```
   
    > [!NOTE]
    > <span data-ttu-id="6f0d9-127">Pokud máte pracovní nebo školní ID a není povoleno dvoufaktorové ověřování, použijte `azure login -u` s ID přihlásit bez interaktivní relace.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-127">If you have a work or school ID and you do not have two-factor authentication enabled, use `azure login -u` with the ID to log in without an interactive session.</span></span> <span data-ttu-id="6f0d9-128">Pokud nemáte pracovní nebo školní ID, můžete [vytvořit pracovní nebo školní id z vašeho osobního účtu Microsoft](../active-directory/active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6f0d9-128">If you don't have a work or school ID, you can [create a work or school id from your personal Microsoft account](../active-directory/active-directory-users-create-azure-portal.md).</span></span>
    
2. <span data-ttu-id="6f0d9-129">**Vytvořte skupinu prostředků**</span><span class="sxs-lookup"><span data-stu-id="6f0d9-129">**Create a resource group**</span></span>  
<span data-ttu-id="6f0d9-130">Všechny prostředky musí být nasazeny do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-130">All resources must be deployed to a resource group.</span></span> <span data-ttu-id="6f0d9-131">V tomto kurzu, název skupiny prostředků **vmsstest1**.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-131">For this tutorial, name the resource group **vmsstest1**.</span></span>
   
    ```cli
    azure group create vmsstestrg1 centralus
    ```

3. <span data-ttu-id="6f0d9-132">**Nasazení účtu úložiště do nové skupiny prostředků**</span><span class="sxs-lookup"><span data-stu-id="6f0d9-132">**Deploy a storage account into the new resource group**</span></span>  
<span data-ttu-id="6f0d9-133">Tento účet úložiště je, kde je šablona uložena.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-133">This storage account is where the template is stored.</span></span> <span data-ttu-id="6f0d9-134">Vytvořit účet úložiště s názvem **vmsstestsa**.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-134">Create a storage account named **vmsstestsa**.</span></span>
   
    ```cli
    azure storage account create -g vmsstestrg1 -l centralus --kind Storage --sku-name LRS vmsstestsa
    ```

## <a name="step-2-create-the-template"></a><span data-ttu-id="6f0d9-135">Krok 2: Vytvoření šablony</span><span class="sxs-lookup"><span data-stu-id="6f0d9-135">Step 2: Create the template</span></span>
<span data-ttu-id="6f0d9-136">Šablonu Azure Resource Manager umožňuje nasadit a spravovat prostředky Azure společně s použitím popis JSON prostředků a parametry přidružené nasazení.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-136">An Azure Resource Manager template makes it possible for you to deploy and manage Azure resources together by using a JSON description of the resources and associated deployment parameters.</span></span>

1. <span data-ttu-id="6f0d9-137">Ve svém oblíbeném editoru vytvořte soubor VMSSTemplate.json a přidejte počáteční struktuře JSON pro podporu šablony.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-137">In your favorite editor, create the file VMSSTemplate.json and add the initial JSON structure to support the template.</span></span>

    ```json
    {
      "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
      },
      "variables": {
      },
      "resources": [
      ]
    }
    ```

2. <span data-ttu-id="6f0d9-138">Parametry nejsou vždy vyžaduje, ale poskytují způsob, jak zadat hodnoty při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-138">Parameters are not always required, but they provide a way to input values when the template is deployed.</span></span> <span data-ttu-id="6f0d9-139">Přidejte tyto parametry v rámci nadřazeného elementu parametry, které jste přidali do šablony.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-139">Add these parameters under the parameters parent element that you added to the template.</span></span>

    ```json
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
   * <span data-ttu-id="6f0d9-140">Nastavte název pro samostatné virtuální počítač, který se používá k přístupu k počítačům v měřítka.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-140">A name for the separate virtual machine that is used to access the machines in the scale set.</span></span>
   * <span data-ttu-id="6f0d9-141">Název pro účet úložiště, které šablona uložena.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-141">A name for the storage account where the template is stored.</span></span>
   * <span data-ttu-id="6f0d9-142">Nastavit počet instancí virtuálních počítačů v měřítka nejprve vytvořit.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-142">The number of instances of virtual machines to initially create in the scale set.</span></span>
   * <span data-ttu-id="6f0d9-143">Název a heslo účtu správce na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-143">A name and password of the administrator account on the virtual machines.</span></span>
   * <span data-ttu-id="6f0d9-144">Předpona názvu pro prostředky, které jsou vytvořené pro podporu nastavit.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-144">A name prefix for the resources that are created to support the scale set.</span></span>

3. <span data-ttu-id="6f0d9-145">Proměnné můžete použít v šabloně a zadejte hodnoty, které může často mění nebo hodnoty, které je potřeba vytvořit z kombinace hodnot parametrů.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-145">Variables can be used in a template to specify values that may change frequently or values that need to be created from a combination of parameter values.</span></span> <span data-ttu-id="6f0d9-146">Přidejte tyto proměnné v rámci nadřazeného elementu proměnné, které jste přidali do šablony.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-146">Add these variables under the variables parent element that you added to the template.</span></span>

    ```json
    "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
    "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
    "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
    "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
    "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
    "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
    "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
    "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
    "wadlogs": "<WadCfg><DiagnosticMonitorConfiguration>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor\\PercentProcessorTime\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU percentage guest OS\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```

   * <span data-ttu-id="6f0d9-147">Názvy DNS, které používají rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-147">DNS names that are used by the network interfaces.</span></span>
   * <span data-ttu-id="6f0d9-148">Názvy adres IP a předpony pro virtuální sítě a podsítě.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-148">The IP address names and prefixes for the virtual network and subnets.</span></span>
   * <span data-ttu-id="6f0d9-149">Názvy a identifikátory ve virtuální síti, zatížení vyrovnávání a síťových rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-149">The names and identifiers of the virtual network, load balancer, and network interfaces.</span></span>
   * <span data-ttu-id="6f0d9-150">Nastavit názvy účtů úložiště pro účty spojené s počítači v měřítka.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-150">Storage account names for the accounts associated with the machines in the scale set.</span></span>
   * <span data-ttu-id="6f0d9-151">Nastavení pro rozšíření diagnostiky, který je nainstalován na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-151">Settings for the Diagnostics extension that is installed on the virtual machines.</span></span> <span data-ttu-id="6f0d9-152">Další informace o rozšíření diagnostiky najdete v tématu [vytvořit Windows virtuální počítač s monitorování a Diagnostika pomocí šablony Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6f0d9-152">For more information about the Diagnostics extension, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

4. <span data-ttu-id="6f0d9-153">Přidáte prostředek účet úložiště v rámci nadřazeného elementu prostředky, které jste přidali do šablony.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-153">Add the storage account resource under the resources parent element that you added to the template.</span></span> <span data-ttu-id="6f0d9-154">Tato šablona používá smyčku vytvoření doporučené pět účtů úložiště uložení disků operačního systému a diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-154">This template uses a loop to create the recommended five storage accounts where the operating system disks and diagnostic data are stored.</span></span> <span data-ttu-id="6f0d9-155">Tuto sadu účtů může podporovat až 100 virtuální počítače ve škálovací sadě, což je aktuální maximální.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-155">This set of accounts can support up to 100 virtual machines in a scale set, which is the current maximum.</span></span> <span data-ttu-id="6f0d9-156">Každý účet úložiště je s názvem se písmeno označení, která byla definována v proměnné v kombinaci s příponou, které poskytujete parametry šablony.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-156">Each storage account is named with a letter designator that was defined in the variables combined with the suffix that you provide in the parameters for the template.</span></span>
   
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": { "accountType": "Standard_LRS" }
        },

5. <span data-ttu-id="6f0d9-157">Přidáte prostředek virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-157">Add the virtual network resource.</span></span> <span data-ttu-id="6f0d9-158">Další informace najdete v tématu [poskytovatele síťových prostředků](../virtual-network/resource-groups-networking.md).</span><span class="sxs-lookup"><span data-stu-id="6f0d9-158">For more information, see [Network Resource Provider](../virtual-network/resource-groups-networking.md).</span></span>

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
        "subnets": [
          {
            "name": "subnet1",
            "properties": { "addressPrefix": "10.0.0.0/24" }
          }
        ]
      }
    },
    ```

6. <span data-ttu-id="6f0d9-159">Přidáte prostředky adresy veřejné IP adresy, které se používají k vyrovnávání zatížení a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-159">Add the public IP address resources that are used by the load balancer and network interface.</span></span>

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP1')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName1')]"
        }
      }
    },
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP2')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName2')]"
        }
      }
    },
    ```

7. <span data-ttu-id="6f0d9-160">Přidáte prostředek pro vyrovnávání zatížení, který se používá škálovací sada.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-160">Add the load balancer resource that is used by the scale set.</span></span> <span data-ttu-id="6f0d9-161">Další informace najdete v tématu [podporu správce prostředků Azure pro nástroj pro vyrovnávání zatížení](../load-balancer/load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="6f0d9-161">For more information, see [Azure Resource Manager Support for Load Balancer](../load-balancer/load-balancer-arm.md).</span></span>

    ```json   
    {
      "apiVersion": "2015-06-15",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
              }
            }
          }
        ],
        "backendAddressPools": [ { "name": "bepool1" } ],
        "inboundNatPools": [
          {
            "name": "natpool1",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPortRangeStart": 50000,
              "frontendPortRangeEnd": 50500,
              "backendPort": 22
            }
          }
        ]
      }
    },
    ```

8. <span data-ttu-id="6f0d9-162">Přidáte prostředek rozhraní sítě, který používá samostatný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-162">Add the network interface resource that is used by the separate virtual machine.</span></span> <span data-ttu-id="6f0d9-163">Protože počítače ve škálovací sadě nejsou přístupné přes veřejnou IP adresu, samostatný virtuální počítač se vytvoří ve stejné virtuální síti vzdáleně přístupu na počítače.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-163">Because machines in a scale set aren't accessible through a public IP address, a separate virtual machine is created in the same virtual network to remotely access the machines.</span></span>

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
              },
              "subnet": {
                "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
              }
            }
          }
        ]
      }
    },
    ```

9. <span data-ttu-id="6f0d9-164">Přidáte samostatné virtuální počítače ve stejné síti jako sada škálování.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-164">Add the separate virtual machine in the same network as the scale set.</span></span>

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": { "vmSize": "Standard_A1" },
        "osProfile": {
          "computername": "[parameters('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "Canonical",
            "offer": "UbuntuServer",
            "sku": "14.04.4-LTS",
            "version": "latest"
          },
          "osDisk": {
            "name": "[concat(parameters('resourcePrefix'), 'os1')]",
            "vhd": {
              "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        }
      }
    },
    ```

10. <span data-ttu-id="6f0d9-165">Přidejte prostředků sady škálování virtuálních počítačů a nastavte rozšíření diagnostiky, který je nainstalován na všechny virtuální počítače v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-165">Add the virtual machine scale set resource and specify the diagnostics extension that is installed on all virtual machines in the scale set.</span></span> <span data-ttu-id="6f0d9-166">Řadu nastavení pro tento prostředek je podobný jako prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-166">Many of the settings for this resource are similar with the virtual machine resource.</span></span> <span data-ttu-id="6f0d9-167">Hlavní rozdíly jsou kapacity elementu, který určuje počet virtuálních počítačů v sadě škálování a upgradePolicy, která určuje, jak jsou aktualizace provedené na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-167">The main differences are the capacity element that specifies the number of virtual machines in the scale set and upgradePolicy that specifies how updates are made to virtual machines.</span></span> <span data-ttu-id="6f0d9-168">Sada škálování vytvořen až všechny účty úložiště jsou vytvořeny jako zadaný dependsOn element.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-168">The scale set is not created until all the storage accounts are created as specified with the dependsOn element.</span></span>

    ```json
    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "apiVersion": "2016-03-30",
      "name": "[parameters('vmSSName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "sku": {
        "name": "Standard_A1",
        "tier": "Standard",
        "capacity": "[parameters('instanceCount')]"
      },
      "properties": {
        "upgradePolicy": {
          "mode": "Manual"
        },
        "virtualMachineProfile": {
          "storageProfile": {
            "osDisk": {
              "vhdContainers": [
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4],'.blob.core.windows.net/vmss')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "Canonical",
              "offer": "UbuntuServer",
              "sku": "14.04.4-LTS",
              "version": "latest"
            }
          },
          "osProfile": {
            "computerNamePrefix": "[parameters('vmSSName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
          },
          "networkProfile": {
            "networkInterfaceConfigurations": [
              {
                "name": "networkconfig1",
                "properties": {
                  "primary": "true",
                  "ipConfigurations": [
                    {
                      "name": "ip1",
                      "properties": {
                        "subnet": {
                          "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                        },
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                          }
                        ],
                        "loadBalancerInboundNatPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          },
          "extensionProfile": {
            "extensions": [
              {
                "name":"LinuxDiagnostic",
                "properties": {
                  "publisher":"Microsoft.OSTCExtensions",
                  "type":"LinuxDiagnostic",
                  "typeHandlerVersion":"2.1",
                  "autoUpgradeMinorVersion":false,
                  "settings": {
                    "xmlCfg":"[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount":"[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName":"[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey":"[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint":"https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. <span data-ttu-id="6f0d9-169">Přidejte autoscaleSettings prostředek, který definuje, jak byly sadou škálování upraví podle využití procesoru na počítačích v sadě.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-169">Add the autoscaleSettings resource that defines how the scale set adjusts based on processor usage on the machines in the set.</span></span>

    ```json
    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Processor\\PercentProcessorTime",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 50.0
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      }
    }
    ```
    
    <span data-ttu-id="6f0d9-170">V tomto kurzu jsou důležité tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="6f0d9-170">For this tutorial, these values are important:</span></span>
    
    * <span data-ttu-id="6f0d9-171">**metricName**</span><span class="sxs-lookup"><span data-stu-id="6f0d9-171">**metricName**</span></span>  
    <span data-ttu-id="6f0d9-172">Tato hodnota je stejná jako čítače výkonu, který jsme definovali v wadperfcounter proměnné.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-172">This value is the same as the performance counter that we defined in the wadperfcounter variable.</span></span> <span data-ttu-id="6f0d9-173">Pomocí této proměnné, rozšíření diagnostiky shromažďuje **Processor\PercentProcessorTime** čítače.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-173">Using that variable, the Diagnostics extension collects the **Processor\PercentProcessorTime** counter.</span></span>
    
    * <span data-ttu-id="6f0d9-174">**metricResourceUri**</span><span class="sxs-lookup"><span data-stu-id="6f0d9-174">**metricResourceUri**</span></span>  
    <span data-ttu-id="6f0d9-175">Tato hodnota je identifikátor prostředku sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-175">This value is the resource identifier of the virtual machine scale set.</span></span>
    
    * <span data-ttu-id="6f0d9-176">**časovými úseky**</span><span class="sxs-lookup"><span data-stu-id="6f0d9-176">**timeGrain**</span></span>  
    <span data-ttu-id="6f0d9-177">Tato hodnota je členitost metriky, které se shromažďují.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-177">This value is the granularity of the metrics that are collected.</span></span> <span data-ttu-id="6f0d9-178">V této šabloně je nastavena na jednu minutu.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-178">In this template, it is set to one minute.</span></span>
    
    * <span data-ttu-id="6f0d9-179">**statistiky**</span><span class="sxs-lookup"><span data-stu-id="6f0d9-179">**statistic**</span></span>  
    <span data-ttu-id="6f0d9-180">Tato hodnota určuje, jak se zkombinují metriky pro přizpůsobení automatické škálování akci.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-180">This value determines how the metrics are combined to accommodate the automatic scaling action.</span></span> <span data-ttu-id="6f0d9-181">Možné hodnoty jsou: průměr, minimum, maximum.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-181">The possible values are: Average, Min, Max.</span></span> <span data-ttu-id="6f0d9-182">V této šabloně se shromažďují průměrné využití procesoru celkový počet virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-182">In this template, the average total CPU usage of the virtual machines is collected.</span></span>

    * <span data-ttu-id="6f0d9-183">**Hodnota timeWindow**</span><span class="sxs-lookup"><span data-stu-id="6f0d9-183">**timeWindow**</span></span>  
    <span data-ttu-id="6f0d9-184">Tato hodnota je doba, ve kterém se shromažďují instance data.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-184">This value is the range of time in which instance data is collected.</span></span> <span data-ttu-id="6f0d9-185">Musí být mezi 5 minutami a 12 hodin.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-185">It must be between 5 minutes and 12 hours.</span></span>
    
    * <span data-ttu-id="6f0d9-186">**Agregace času**</span><span class="sxs-lookup"><span data-stu-id="6f0d9-186">**timeAggregation**</span></span>  
    <span data-ttu-id="6f0d9-187">jeho hodnota určuje, jak by měla být kombinovány data, která se shromažďují v čase.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-187">his value determines how the data that is collected should be combined over time.</span></span> <span data-ttu-id="6f0d9-188">Výchozí hodnota je průměr.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-188">The default value is Average.</span></span> <span data-ttu-id="6f0d9-189">Možné hodnoty jsou: průměr, minimální, maximální, poslední, celkový počet, počet.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-189">The possible values are: Average, Minimum, Maximum, Last, Total, Count.</span></span>
    
    * <span data-ttu-id="6f0d9-190">**operátor**</span><span class="sxs-lookup"><span data-stu-id="6f0d9-190">**operator**</span></span>  
    <span data-ttu-id="6f0d9-191">Tato hodnota je operátor, který se používá k porovnání data metriky a prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-191">This value is the operator that is used to compare the metric data and the threshold.</span></span> <span data-ttu-id="6f0d9-192">Možné hodnoty jsou: rovná NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-192">The possible values are: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span></span>
    
    * <span data-ttu-id="6f0d9-193">**Prahová hodnota**</span><span class="sxs-lookup"><span data-stu-id="6f0d9-193">**threshold**</span></span>  
    <span data-ttu-id="6f0d9-194">Tato hodnota se aktivuje akci škálování.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-194">This value triggers the scale action.</span></span> <span data-ttu-id="6f0d9-195">V této šabloně počítače se přidají do sad po více než 50 % průměrné využití procesoru mezi počítači v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-195">In this template, machines are added to the scale set when the average CPU usage among machines in the set is over 50%.</span></span>
    
    * <span data-ttu-id="6f0d9-196">**směr**</span><span class="sxs-lookup"><span data-stu-id="6f0d9-196">**direction**</span></span>  
    <span data-ttu-id="6f0d9-197">Tato hodnota určuje akce, která se provede, když je dosaženo prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-197">This value determines the action that is taken when the threshold value is achieved.</span></span> <span data-ttu-id="6f0d9-198">Možné hodnoty jsou zvýšení nebo snížení.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-198">The possible values are Increase or Decrease.</span></span> <span data-ttu-id="6f0d9-199">V této šabloně se zvyšuje počet virtuálních počítačů v sadě škálování, když je prahová hodnota je více než 50 % v okně definovaného časového.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-199">In this template, the number of virtual machines in the scale set is increased if the threshold is over 50% in the defined time window.</span></span>

    * <span data-ttu-id="6f0d9-200">**Typ**</span><span class="sxs-lookup"><span data-stu-id="6f0d9-200">**type**</span></span>  
    <span data-ttu-id="6f0d9-201">Tato hodnota je typ akce, který má vzniknout a musí být nastavena na ChangeCount.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-201">This value is the type of action that should occur and must be set to ChangeCount.</span></span>
    
    * <span data-ttu-id="6f0d9-202">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="6f0d9-202">**value**</span></span>  
    <span data-ttu-id="6f0d9-203">Tato hodnota je počet virtuálních počítačů, které jsou přidány nebo odebrány ze sady škálování.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-203">This value is the number of virtual machines that are added or removed from the scale set.</span></span> <span data-ttu-id="6f0d9-204">Tato hodnota musí být 1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-204">This value must be 1 or greater.</span></span> <span data-ttu-id="6f0d9-205">Výchozí hodnota je 1.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-205">The default value is 1.</span></span> <span data-ttu-id="6f0d9-206">V této šabloně počet počítačů v měřítka nastavit zvýší o 1 při splnění prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-206">In this template, the number of machines in the scale set increases by 1 when the threshold is met.</span></span>

    * <span data-ttu-id="6f0d9-207">**cooldown**</span><span class="sxs-lookup"><span data-stu-id="6f0d9-207">**cooldown**</span></span>  
    <span data-ttu-id="6f0d9-208">Tato hodnota je množství času má počkat od poslední akce škálování, než dojde k další akce.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-208">This value is the amount of time to wait since the last scaling action before the next action occurs.</span></span> <span data-ttu-id="6f0d9-209">Tato hodnota musí být mezi minutu a jeden týden.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-209">This value must be between one minute and one week.</span></span>

12. <span data-ttu-id="6f0d9-210">Uložte soubor šablony.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-210">Save the template file.</span></span>    

## <a name="step-3-upload-the-template-to-storage"></a><span data-ttu-id="6f0d9-211">Krok 3: Nahrát šablonu do úložiště</span><span class="sxs-lookup"><span data-stu-id="6f0d9-211">Step 3: Upload the template to storage</span></span>
<span data-ttu-id="6f0d9-212">Šablonu je možné uložit tak dlouho, dokud znáte název a primární klíč účtu úložiště, který jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-212">The template can be uploaded as long as you know the name and primary key of the storage account that you created in step 1.</span></span>

1. <span data-ttu-id="6f0d9-213">Ve svém rozhraní příkazového řádku (Bash, terminálu, příkazového řádku) spusťte tyto příkazy a nastavení proměnných prostředí, které jsou potřebné pro přístup k účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="6f0d9-213">In your command-line interface (Bash, Terminal, Command prompt), run these commands to set the environment variables needed to access the storage account:</span></span>

    ```cli   
    export AZURE_STORAGE_ACCOUNT={account_name}
    export AZURE_STORAGE_ACCESS_KEY={key}
    ```
    
    <span data-ttu-id="6f0d9-214">Klíč můžete získat kliknutím na ikonu klíče při zobrazení prostředků účtu úložiště na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-214">You can get the key by clicking the key icon when viewing the storage account resource in the Azure portal.</span></span> <span data-ttu-id="6f0d9-215">Při použití příkazového řádku Windows, zadejte **nastavit** místo exportu.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-215">When using a Windows command prompt, type **set** instead of export.</span></span>

2. <span data-ttu-id="6f0d9-216">Vytvořte kontejner pro uložení šablony.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-216">Create the container for storing the template.</span></span>
   
    ```cli
    azure storage container create -p Blob templates
    ```

3. <span data-ttu-id="6f0d9-217">Nahrajte soubor šablony do nového kontejneru.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-217">Upload the template file to the new container.</span></span>
   
    ```cli
    azure storage blob upload VMSSTemplate.json templates VMSSTemplate.json
    ```

## <a name="step-4-deploy-the-template"></a><span data-ttu-id="6f0d9-218">Krok 4: Nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="6f0d9-218">Step 4: Deploy the template</span></span>
<span data-ttu-id="6f0d9-219">Teď, když jste vytvořili šablonu, můžete začít nasazovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-219">Now that you created the template, you can start deploying the resources.</span></span> <span data-ttu-id="6f0d9-220">Použijte tento příkaz ke spuštění procesu:</span><span class="sxs-lookup"><span data-stu-id="6f0d9-220">Use this command to start the process:</span></span>

```cli
azure group deployment create --template-uri https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json vmsstestrg1 vmsstestdp1
```

<span data-ttu-id="6f0d9-221">Po stisknutí klávesy zadejte, zobrazí se výzva k zadání hodnot pro proměnné, které jste přiřadili.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-221">When you press enter, you are prompted to provide values for the variables you assigned.</span></span> <span data-ttu-id="6f0d9-222">Zadejte tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="6f0d9-222">Provide these values:</span></span>

```cli
vmName: vmsstestvm1
vmSSName: vmsstest1
instanceCount: 5
adminUserName: vmadmin1
adminPassword: VMpass1
resourcePrefix: vmsstest
```

<span data-ttu-id="6f0d9-223">Má trvat přibližně 15 minut pro všechny prostředky. Chcete-li úspěšně nasadit.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-223">It should take about 15 minutes for all the resources to successfully be deployed.</span></span>

> [!NOTE]
> <span data-ttu-id="6f0d9-224">Možnost na portálu můžete taky nasadit tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-224">You can also use the portal’s ability to deploy the resources.</span></span> <span data-ttu-id="6f0d9-225">Použít tento odkaz: https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template></span><span class="sxs-lookup"><span data-stu-id="6f0d9-225">Use this link: https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template></span></span>


## <a name="step-5-monitor-resources"></a><span data-ttu-id="6f0d9-226">Krok 5: Sledování prostředků</span><span class="sxs-lookup"><span data-stu-id="6f0d9-226">Step 5: Monitor resources</span></span>
<span data-ttu-id="6f0d9-227">Můžete získat některé informace o sady škálování virtuálního počítače pomocí těchto metod:</span><span class="sxs-lookup"><span data-stu-id="6f0d9-227">You can get some information about virtual machine scale sets using these methods:</span></span>

* <span data-ttu-id="6f0d9-228">Portál Azure – můžete aktuálně získat omezené množství informací pomocí portálu.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-228">The Azure portal - You can currently get a limited amount of information using the portal.</span></span>

* <span data-ttu-id="6f0d9-229">[Průzkumníka prostředků Azure](https://resources.azure.com/) – tento nástroj je nejvhodnější pro zkoumání aktuálního stavu škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-229">The [Azure Resource Explorer](https://resources.azure.com/) - This tool is the best for exploring the current state of your scale set.</span></span> <span data-ttu-id="6f0d9-230">Postupujte podle této cestě a měli byste vidět zobrazení instance škálovací sady, který jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="6f0d9-230">Follow this path and you should see the instance view of the scale set that you created:</span></span>
  
    ```cli
    subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines
    ```

* <span data-ttu-id="6f0d9-231">Azure CLI - použijte tento příkaz určité informace:</span><span class="sxs-lookup"><span data-stu-id="6f0d9-231">Azure CLI - Use this command to get some information:</span></span>

    ```cli  
    azure resource show -n vmsstest1 -r Microsoft.Compute/virtualMachineScaleSets -o 2015-06-15 -g vmsstestrg1
    ```

* <span data-ttu-id="6f0d9-232">Připojte k virtuálnímu počítači jumpbox stejně, jako by všechny ostatní počítače a potom můžete virtuální počítače v sad pro monitorování jednotlivých procesů škálování vzdálený přístup.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-232">Connect to the jumpbox virtual machine just like you would any other machine and then you can remotely access the virtual machines in the scale set to monitor individual processes.</span></span>

> [!NOTE]
> <span data-ttu-id="6f0d9-233">Dokončení rozhraní REST API k získání informací o sady škálování najdete v [sadách škálování virtuálního počítače](https://msdn.microsoft.com/library/mt589023.aspx).</span><span class="sxs-lookup"><span data-stu-id="6f0d9-233">A complete REST API for obtaining information about scale sets can be found in [Virtual Machine Scale Sets](https://msdn.microsoft.com/library/mt589023.aspx).</span></span>

## <a name="step-6-remove-the-resources"></a><span data-ttu-id="6f0d9-234">Krok 6: Odebrat prostředky</span><span class="sxs-lookup"><span data-stu-id="6f0d9-234">Step 6: Remove the resources</span></span>
<span data-ttu-id="6f0d9-235">Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, vždycky je dobrým zvykem odstranit prostředky, které už nejsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-235">Because you are charged for resources used in Azure, it is always a good practice to delete resources that are no longer needed.</span></span> <span data-ttu-id="6f0d9-236">Nemusíte samostatně odstranit jednotlivé prostředky ze skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-236">You don’t need to delete each resource separately from a resource group.</span></span> <span data-ttu-id="6f0d9-237">Můžete odstranit skupinu prostředků a všechny její prostředky se automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-237">You can delete the resource group and all its resources are automatically deleted.</span></span>

```cli
azure group delete vmsstestrg1
```

## <a name="next-steps"></a><span data-ttu-id="6f0d9-238">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6f0d9-238">Next steps</span></span>
* <span data-ttu-id="6f0d9-239">Najít příklady Azure monitorování monitorování funkcí v [Azure monitorování napříč platformami CLI rychlý start ukázky](../monitoring-and-diagnostics/insights-cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="6f0d9-239">Find examples of Azure Monitor monitoring features in [Azure Monitor Cross-platform CLI quick start samples](../monitoring-and-diagnostics/insights-cli-samples.md)</span></span>
* <span data-ttu-id="6f0d9-240">Další informace o funkcích oznámení v [používat akce škálování k odesílání e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="6f0d9-240">Learn about notification features in [Use autoscale actions to send email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span></span>
* <span data-ttu-id="6f0d9-241">Zjistěte, jak [protokoly auditu použijte k odesílání e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="6f0d9-241">Learn how to [Use audit logs to send email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span></span>
* <span data-ttu-id="6f0d9-242">Podívejte se [škálování ukázkovou aplikaci na Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) šablony, která nastaví aplikaci Python nebo bottle vykonávat automatické škálování funkce sadách škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="6f0d9-242">Check out the [Autoscale demo app on Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) template that sets up a Python/bottle app to exercise the automatic scaling functionality of Virtual Machine Scale Sets.</span></span>

