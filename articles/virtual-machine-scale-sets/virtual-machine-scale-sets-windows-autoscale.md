---
title: "Sadách škálování virtuálních počítačů Windows škálování | Microsoft Docs"
description: "Nastavení automatického škálování pro Windows sadu škálování virtuálního počítače pomocí Azure PowerShell"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: 91f731bec46c005221f4e66e95822994070d4c26
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="d5587-103">Automatické škálování počítače ve škálovací sadě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="d5587-103">Automatically scale machines in a virtual machine scale set</span></span>
<span data-ttu-id="d5587-104">Sady škálování virtuálního počítače můžete snadno můžete nasadit a spravovat jako sada identickými virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="d5587-104">Virtual machine scale sets make it easy for you to deploy and manage identical virtual machines as a set.</span></span> <span data-ttu-id="d5587-105">Sady škálování pro aplikace hyperškálovatelný systém zajistit vysoce škálovatelného a přizpůsobitelné výpočetní vrstvy a podporují Image pro platformu Windows, Image platformy Linux, vlastních bitových kopií a rozšíření.</span><span class="sxs-lookup"><span data-stu-id="d5587-105">Scale sets provide a highly scalable and customizable compute layer for hyperscale applications, and they support Windows platform images, Linux platform images, custom images, and extensions.</span></span> <span data-ttu-id="d5587-106">Další informace o sadách škálování najdete v tématu [sadách škálování virtuálního počítače](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d5587-106">For more information about scale sets, see [Virtual Machine Scale Sets](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="d5587-107">V tomto kurzu se dozvíte, jak vytvořit škálovací sadu virtuálních počítačů s Windows a automaticky škálovat na počítače v sadě.</span><span class="sxs-lookup"><span data-stu-id="d5587-107">This tutorial shows you how to create a scale set of Windows virtual machines and automatically scale the machines in the set.</span></span> <span data-ttu-id="d5587-108">Můžete vytvořit měřítko nastavení a nastavení škálování vytvořením šablonu Azure Resource Manager a nasazení pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d5587-108">You create the scale set and set up scaling by creating an Azure Resource Manager template and deploying it using Azure PowerShell.</span></span> <span data-ttu-id="d5587-109">Další informace o šablonách najdete v tématu o [vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="d5587-109">For more information about templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> <span data-ttu-id="d5587-110">Další informace o automatické škálování sad škálování najdete v tématu [automatické škálování a sadách škálování virtuálního počítače](virtual-machine-scale-sets-autoscale-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d5587-110">To learn more about automatic scaling of scale sets, see [Automatic scaling and Virtual Machine Scale Sets](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

<span data-ttu-id="d5587-111">V tomto článku můžete nasadit následující prostředků a rozšíření:</span><span class="sxs-lookup"><span data-stu-id="d5587-111">In this article, you deploy the following resources and extensions:</span></span>

* <span data-ttu-id="d5587-112">Microsoft.Storage/storageAccounts.</span><span class="sxs-lookup"><span data-stu-id="d5587-112">Microsoft.Storage/storageAccounts</span></span>
* <span data-ttu-id="d5587-113">Microsoft.Network/virtualNetworks</span><span class="sxs-lookup"><span data-stu-id="d5587-113">Microsoft.Network/virtualNetworks</span></span>
* <span data-ttu-id="d5587-114">Microsoft.Network/publicIPAddresses</span><span class="sxs-lookup"><span data-stu-id="d5587-114">Microsoft.Network/publicIPAddresses</span></span>
* <span data-ttu-id="d5587-115">Microsoft.Network/loadBalancers</span><span class="sxs-lookup"><span data-stu-id="d5587-115">Microsoft.Network/loadBalancers</span></span>
* <span data-ttu-id="d5587-116">Microsoft.Network/networkInterfaces</span><span class="sxs-lookup"><span data-stu-id="d5587-116">Microsoft.Network/networkInterfaces</span></span>
* <span data-ttu-id="d5587-117">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="d5587-117">Microsoft.Compute/virtualMachines</span></span>
* <span data-ttu-id="d5587-118">Microsoft.Compute/virtualMachineScaleSets</span><span class="sxs-lookup"><span data-stu-id="d5587-118">Microsoft.Compute/virtualMachineScaleSets</span></span>
* <span data-ttu-id="d5587-119">Microsoft.Insights.VMDiagnosticsSettings</span><span class="sxs-lookup"><span data-stu-id="d5587-119">Microsoft.Insights.VMDiagnosticsSettings</span></span>
* <span data-ttu-id="d5587-120">Microsoft.Insights/autoscaleSettings</span><span class="sxs-lookup"><span data-stu-id="d5587-120">Microsoft.Insights/autoscaleSettings</span></span>

<span data-ttu-id="d5587-121">Další informace o prostředky Resource Manageru najdete v tématu [Azure Resource Manager oproti nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d5587-121">For more information about Resource Manager resources, see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

## <a name="step-1-install-azure-powershell"></a><span data-ttu-id="d5587-122">Krok1: Nainstalování prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d5587-122">Step 1: Install Azure PowerShell</span></span>
<span data-ttu-id="d5587-123">V tématu [postup instalace a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) informace o instalaci nejnovější verzi prostředí Azure PowerShell, výběr předplatného a přihlášení do Azure.</span><span class="sxs-lookup"><span data-stu-id="d5587-123">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for information about installing the latest version of Azure PowerShell, selecting your subscription, and signing in to Azure.</span></span>

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a><span data-ttu-id="d5587-124">Krok 2: Vytvoření skupiny prostředků a účet úložiště</span><span class="sxs-lookup"><span data-stu-id="d5587-124">Step 2: Create a resource group and a storage account</span></span>
1. <span data-ttu-id="d5587-125">**Vytvořte skupinu prostředků** – všechny prostředky se musí nasadit do skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="d5587-125">**Create a resource group** – All resources must be deployed to a resource group.</span></span> <span data-ttu-id="d5587-126">Použití [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) vytvořit skupinu prostředků s názvem **vmsstestrg1**.</span><span class="sxs-lookup"><span data-stu-id="d5587-126">Use [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) to create a resource group named **vmsstestrg1**.</span></span>
2. <span data-ttu-id="d5587-127">**Vytvoření účtu úložiště** – tento účet úložiště je, kde je šablona uložena.</span><span class="sxs-lookup"><span data-stu-id="d5587-127">**Create a storage account** – This storage account is where the template is stored.</span></span> <span data-ttu-id="d5587-128">Použití [nový AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) chcete vytvořit účet úložiště s názvem **vmsstestsa**.</span><span class="sxs-lookup"><span data-stu-id="d5587-128">Use [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) to create a storage account named **vmsstestsa**.</span></span>

## <a name="step-3-create-the-template"></a><span data-ttu-id="d5587-129">Krok 3: Vytvoření šablony</span><span class="sxs-lookup"><span data-stu-id="d5587-129">Step 3: Create the template</span></span>
<span data-ttu-id="d5587-130">Šablonu Azure Resource Manager umožňuje nasadit a spravovat prostředky Azure společně s použitím popis JSON prostředků a parametry přidružené nasazení.</span><span class="sxs-lookup"><span data-stu-id="d5587-130">An Azure Resource Manager template makes it possible for you to deploy and manage Azure resources together by using a JSON description of the resources and associated deployment parameters.</span></span>

1. <span data-ttu-id="d5587-131">Ve svém oblíbeném editoru vytvořte soubor C:\VMSSTemplate.json a přidejte počáteční struktuře JSON pro podporu šablony.</span><span class="sxs-lookup"><span data-stu-id="d5587-131">In your favorite editor, create the file C:\VMSSTemplate.json and add the initial JSON structure to support the template.</span></span>

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

2. <span data-ttu-id="d5587-132">Parametry nejsou vždy vyžaduje, ale poskytují způsob, jak zadat hodnoty při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="d5587-132">Parameters are not always required, but they provide a way to input values when the template is deployed.</span></span> <span data-ttu-id="d5587-133">Přidejte tyto parametry v rámci nadřazeného elementu parametry, které jste přidali do šablony.</span><span class="sxs-lookup"><span data-stu-id="d5587-133">Add these parameters under the parameters parent element that you added to the template.</span></span>

    ```json   
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
    * <span data-ttu-id="d5587-134">Nastavte název pro samostatné virtuální počítač, který se používá k přístupu k počítačům v měřítka.</span><span class="sxs-lookup"><span data-stu-id="d5587-134">A name for the separate virtual machine that is used to access the machines in the scale set.</span></span>
    * <span data-ttu-id="d5587-135">Název účtu úložiště, které šablona uložena.</span><span class="sxs-lookup"><span data-stu-id="d5587-135">The name of the storage account where the template is stored.</span></span>
    * <span data-ttu-id="d5587-136">Počet virtuálních počítačů na začátku vytvořit v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="d5587-136">The number of virtual machines to initially create in the scale set.</span></span>
    * <span data-ttu-id="d5587-137">Jméno a heslo účtu správce na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="d5587-137">The name and password of the administrator account on the virtual machines.</span></span>
    * <span data-ttu-id="d5587-138">Předpona názvu pro prostředky, které jsou vytvořené pro podporu nastavit.</span><span class="sxs-lookup"><span data-stu-id="d5587-138">A name prefix for the resources that are created to support the scale set.</span></span>

3. <span data-ttu-id="d5587-139">Proměnné můžete použít v šabloně a zadejte hodnoty, které může často mění nebo hodnoty, které je potřeba vytvořit z kombinace hodnot parametrů.</span><span class="sxs-lookup"><span data-stu-id="d5587-139">Variables can be used in a template to specify values that may change frequently or values that need to be created from a combination of parameter values.</span></span> <span data-ttu-id="d5587-140">Přidejte tyto proměnné v rámci nadřazeného elementu proměnné, které jste přidali do šablony.</span><span class="sxs-lookup"><span data-stu-id="d5587-140">Add these variables under the variables parent element that you added to the template.</span></span>

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
      "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```
   
    * <span data-ttu-id="d5587-141">Názvy DNS, které používají rozhraní sítě.</span><span class="sxs-lookup"><span data-stu-id="d5587-141">DNS names that are used by the network interfaces.</span></span>

        * <span data-ttu-id="d5587-142">Názvy adres IP a předpony pro virtuální sítě a podsítě.</span><span class="sxs-lookup"><span data-stu-id="d5587-142">The IP address names and prefixes for the virtual network and subnets.</span></span>
        * <span data-ttu-id="d5587-143">Názvy a identifikátory ve virtuální síti, zatížení vyrovnávání a síťových rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d5587-143">The names and identifiers of the virtual network, load balancer, and network interfaces.</span></span>
        * <span data-ttu-id="d5587-144">Nastavit názvy účtů úložiště pro účty spojené s počítači v měřítka.</span><span class="sxs-lookup"><span data-stu-id="d5587-144">Storage account names for the accounts associated with the machines in the scale set.</span></span>
        * <span data-ttu-id="d5587-145">Nastavení pro rozšíření diagnostiky, který je nainstalován na virtuálních počítačích.</span><span class="sxs-lookup"><span data-stu-id="d5587-145">Settings for the Diagnostics extension that is installed on the virtual machines.</span></span> <span data-ttu-id="d5587-146">Další informace o rozšíření diagnostiky najdete v tématu [vytvořit Windows virtuální počítač s monitorování a Diagnostika pomocí šablony Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d5587-146">For more information about the Diagnostics extension, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

4. <span data-ttu-id="d5587-147">Přidáte prostředek účet úložiště v rámci nadřazeného elementu prostředky, které jste přidali do šablony.</span><span class="sxs-lookup"><span data-stu-id="d5587-147">Add the storage account resource under the resources parent element that you added to the template.</span></span> <span data-ttu-id="d5587-148">Tato šablona používá smyčku vytvoření doporučené pět účtů úložiště uložení disků operačního systému a diagnostická data.</span><span class="sxs-lookup"><span data-stu-id="d5587-148">This template uses a loop to create the recommended five storage accounts where the operating system disks and diagnostic data are stored.</span></span> <span data-ttu-id="d5587-149">Tuto sadu účtů může podporovat až 100 virtuální počítače ve škálovací sadě, což je aktuální maximální.</span><span class="sxs-lookup"><span data-stu-id="d5587-149">This set of accounts can support up to 100 virtual machines in a scale set, which is the current maximum.</span></span> <span data-ttu-id="d5587-150">Každý účet úložiště je s názvem se písmeno označení, která byla definována v proměnné v kombinaci s předponu, která zadáte v parametrech šablony.</span><span class="sxs-lookup"><span data-stu-id="d5587-150">Each storage account is named with a letter designator that was defined in the variables combined with the prefix that you provide in the parameters for the template.</span></span>

    ```json
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
    ```

5. <span data-ttu-id="d5587-151">Přidáte prostředek virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="d5587-151">Add the virtual network resource.</span></span> <span data-ttu-id="d5587-152">Další informace najdete v tématu [poskytovatele síťových prostředků](../virtual-network/resource-groups-networking.md).</span><span class="sxs-lookup"><span data-stu-id="d5587-152">For more information, see [Network Resource Provider](../virtual-network/resource-groups-networking.md).</span></span>

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

6. <span data-ttu-id="d5587-153">Přidáte prostředky adresy veřejné IP adresy, které se používají k vyrovnávání zatížení a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="d5587-153">Add the public IP address resources that are used by the load balancer and network interface.</span></span>

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

7. <span data-ttu-id="d5587-154">Přidáte prostředek pro vyrovnávání zatížení, který se používá škálovací sada.</span><span class="sxs-lookup"><span data-stu-id="d5587-154">Add the load balancer resource that is used by the scale set.</span></span> <span data-ttu-id="d5587-155">Další informace najdete v tématu [podporu správce prostředků Azure pro nástroj pro vyrovnávání zatížení](../load-balancer/load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="d5587-155">For more information, see [Azure Resource Manager Support for Load Balancer](../load-balancer/load-balancer-arm.md).</span></span>

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
                "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
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
              "backendPort": 3389
            }
          }
        ]
      }
    },
    ```

8. <span data-ttu-id="d5587-156">Přidáte prostředek rozhraní sítě, který používá samostatný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d5587-156">Add the network interface resource that is used by the separate virtual machine.</span></span> <span data-ttu-id="d5587-157">Protože počítače ve škálovací sadě nejsou přístupné přes veřejnou IP adresu, samostatný virtuální počítač se vytvoří ve stejné virtuální síti vzdáleně přístupu na počítače.</span><span class="sxs-lookup"><span data-stu-id="d5587-157">Because machines in a scale set aren't accessible through a public IP address, a separate virtual machine is created in the same virtual network to remotely access the machines.</span></span>

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

9. <span data-ttu-id="d5587-158">Přidáte samostatné virtuální počítače ve stejné síti jako sada škálování.</span><span class="sxs-lookup"><span data-stu-id="d5587-158">Add the separate virtual machine in the same network as the scale set.</span></span>

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
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2012-R2-Datacenter",
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

10. <span data-ttu-id="d5587-159">Přidejte prostředků sady škálování virtuálních počítačů a nastavte rozšíření diagnostiky, který je nainstalován na všechny virtuální počítače v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="d5587-159">Add the virtual machine scale set resource and specify the diagnostics extension that is installed on all virtual machines in the scale set.</span></span> <span data-ttu-id="d5587-160">Řadu nastavení pro tento prostředek je podobný jako prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d5587-160">Many of the settings for this resource are similar with the virtual machine resource.</span></span> <span data-ttu-id="d5587-161">Hlavní rozdíly jsou kapacity elementu, který určuje počet virtuálních počítačů v sadě škálování a upgradePolicy, která určuje, jak jsou aktualizace provedené na virtuální počítače.</span><span class="sxs-lookup"><span data-stu-id="d5587-161">The main differences are the capacity element that specifies the number of virtual machines in the scale set, and upgradePolicy that specifies how updates are made to virtual machines.</span></span> <span data-ttu-id="d5587-162">Sada škálování vytvořen až všechny účty úložiště jsou vytvořeny jako zadaný dependsOn element.</span><span class="sxs-lookup"><span data-stu-id="d5587-162">The scale set is not created until all the storage accounts are created as specified with the dependsOn element.</span></span>

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
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "MicrosoftWindowsServer",
              "offer": "WindowsServer",
              "sku": "2012-R2-Datacenter",
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
                "name": "Microsoft.Insights.VMDiagnosticsSettings",
                "properties": {
                  "publisher": "Microsoft.Azure.Diagnostics",
                  "type": "IaaSDiagnostics",
                  "typeHandlerVersion": "1.5",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint": "https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. <span data-ttu-id="d5587-163">Přidáte autoscaleSettings prostředek, který definuje, jak byly sadou škálování upraví podle využití procesoru na počítačích v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="d5587-163">Add the autoscaleSettings resource that defines how the scale set adjusts based on the processor usage on the machines in the scale set.</span></span>

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
                  "metricName": "\\Processor(_Total)\\% Processor Time",
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

    <span data-ttu-id="d5587-164">V tomto kurzu jsou důležité tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d5587-164">For this tutorial, these values are important:</span></span>
    
    * <span data-ttu-id="d5587-165">**metricName**</span><span class="sxs-lookup"><span data-stu-id="d5587-165">**metricName**</span></span>  
    <span data-ttu-id="d5587-166">Tato hodnota je stejná jako čítače výkonu, který jsme definovali v wadperfcounter proměnné.</span><span class="sxs-lookup"><span data-stu-id="d5587-166">This value is the same as the performance counter that we defined in the wadperfcounter variable.</span></span> <span data-ttu-id="d5587-167">Pomocí této proměnné, rozšíření diagnostiky shromažďuje **procesor(_celkem)\% času procesoru** čítače.</span><span class="sxs-lookup"><span data-stu-id="d5587-167">Using that variable, the Diagnostics extension collects the  **Processor(_Total)\% Processor Time** counter.</span></span>
    
    * <span data-ttu-id="d5587-168">**metricResourceUri**</span><span class="sxs-lookup"><span data-stu-id="d5587-168">**metricResourceUri**</span></span>  
    <span data-ttu-id="d5587-169">Tato hodnota je identifikátor prostředku sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d5587-169">This value is the resource identifier of the virtual machine scale set.</span></span>
    
    * <span data-ttu-id="d5587-170">**časovými úseky**</span><span class="sxs-lookup"><span data-stu-id="d5587-170">**timeGrain**</span></span>  
    <span data-ttu-id="d5587-171">Tato hodnota je členitost metriky, které se shromažďují.</span><span class="sxs-lookup"><span data-stu-id="d5587-171">This value is the granularity of the metrics that are collected.</span></span> <span data-ttu-id="d5587-172">V této šabloně je nastavena na jednu minutu.</span><span class="sxs-lookup"><span data-stu-id="d5587-172">In this template, it is set to one minute.</span></span>
    
    * <span data-ttu-id="d5587-173">**statistiky**</span><span class="sxs-lookup"><span data-stu-id="d5587-173">**statistic**</span></span>  
    <span data-ttu-id="d5587-174">Tato hodnota určuje, jak se zkombinují metriky pro přizpůsobení automatické škálování akci.</span><span class="sxs-lookup"><span data-stu-id="d5587-174">This value determines how the metrics are combined to accommodate the automatic scaling action.</span></span> <span data-ttu-id="d5587-175">Možné hodnoty jsou: průměr, minimum, maximum.</span><span class="sxs-lookup"><span data-stu-id="d5587-175">The possible values are: Average, Min, Max.</span></span> <span data-ttu-id="d5587-176">V této šabloně se shromažďují průměrné využití procesoru celkový počet virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d5587-176">In this template, the average total CPU usage of the virtual machines is collected.</span></span>
    
    * <span data-ttu-id="d5587-177">**Hodnota timeWindow**</span><span class="sxs-lookup"><span data-stu-id="d5587-177">**timeWindow**</span></span>  
    <span data-ttu-id="d5587-178">Tato hodnota je doba, ve kterém se shromažďují instance data.</span><span class="sxs-lookup"><span data-stu-id="d5587-178">This value is the range of time in which instance data is collected.</span></span> <span data-ttu-id="d5587-179">Musí být mezi 5 minutami a 12 hodin.</span><span class="sxs-lookup"><span data-stu-id="d5587-179">It must be between 5 minutes and 12 hours.</span></span>
    
    * <span data-ttu-id="d5587-180">**Agregace času**</span><span class="sxs-lookup"><span data-stu-id="d5587-180">**timeAggregation**</span></span>  
    <span data-ttu-id="d5587-181">jeho hodnota určuje, jak by měla být kombinovány data, která se shromažďují v čase.</span><span class="sxs-lookup"><span data-stu-id="d5587-181">his value determines how the data that is collected should be combined over time.</span></span> <span data-ttu-id="d5587-182">Výchozí hodnota je průměr.</span><span class="sxs-lookup"><span data-stu-id="d5587-182">The default value is Average.</span></span> <span data-ttu-id="d5587-183">Možné hodnoty jsou: průměr, minimální, maximální, poslední, celkový počet, počet.</span><span class="sxs-lookup"><span data-stu-id="d5587-183">The possible values are: Average, Minimum, Maximum, Last, Total, Count.</span></span>
    
    * <span data-ttu-id="d5587-184">**operátor**</span><span class="sxs-lookup"><span data-stu-id="d5587-184">**operator**</span></span>  
    <span data-ttu-id="d5587-185">Tato hodnota je operátor, který se používá k porovnání data metriky a prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d5587-185">This value is the operator that is used to compare the metric data and the threshold.</span></span> <span data-ttu-id="d5587-186">Možné hodnoty jsou: rovná NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span><span class="sxs-lookup"><span data-stu-id="d5587-186">The possible values are: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span></span>
    
    * <span data-ttu-id="d5587-187">**Prahová hodnota**</span><span class="sxs-lookup"><span data-stu-id="d5587-187">**threshold**</span></span>  
    <span data-ttu-id="d5587-188">Tato hodnota je hodnota, která aktivuje akci škálování.</span><span class="sxs-lookup"><span data-stu-id="d5587-188">This value is the value that triggers the scale action.</span></span> <span data-ttu-id="d5587-189">V této šabloně počítače se přidají do sad po více než 50 % průměrné využití procesoru mezi počítači v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="d5587-189">In this template, machines are added to the scale set when the average CPU usage among machines in the set is over 50%.</span></span>
    
    * <span data-ttu-id="d5587-190">**směr**</span><span class="sxs-lookup"><span data-stu-id="d5587-190">**direction**</span></span>  
    <span data-ttu-id="d5587-191">Tato hodnota určuje akce, která se provede, když je dosaženo prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d5587-191">This value determines the action that is taken when the threshold value is achieved.</span></span> <span data-ttu-id="d5587-192">Možné hodnoty jsou zvýšení nebo snížení.</span><span class="sxs-lookup"><span data-stu-id="d5587-192">The possible values are Increase or Decrease.</span></span> <span data-ttu-id="d5587-193">V této šabloně se zvyšuje počet virtuálních počítačů v sadě škálování, když je prahová hodnota je více než 50 % v okně definovaného časového.</span><span class="sxs-lookup"><span data-stu-id="d5587-193">In this template, the number of virtual machines in the scale set is increased if the threshold is over 50% in the defined time window.</span></span>
    
    * <span data-ttu-id="d5587-194">**Typ**</span><span class="sxs-lookup"><span data-stu-id="d5587-194">**type**</span></span>  
    <span data-ttu-id="d5587-195">Tato hodnota je typ akce, který má vzniknout a musí být nastavena na ChangeCount.</span><span class="sxs-lookup"><span data-stu-id="d5587-195">This value is the type of action that should occur and must be set to ChangeCount.</span></span>
    
    * <span data-ttu-id="d5587-196">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="d5587-196">**value**</span></span>  
    <span data-ttu-id="d5587-197">Tato hodnota je počet virtuálních počítačů, které jsou přidány nebo odebrány ze sady škálování.</span><span class="sxs-lookup"><span data-stu-id="d5587-197">This value is the number of virtual machines that are added or removed from the scale set.</span></span> <span data-ttu-id="d5587-198">Tato hodnota musí být 1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="d5587-198">This value must be 1 or greater.</span></span> <span data-ttu-id="d5587-199">Výchozí hodnota je 1.</span><span class="sxs-lookup"><span data-stu-id="d5587-199">The default value is 1.</span></span> <span data-ttu-id="d5587-200">V této šabloně počet počítačů v měřítka nastavit zvýší o 1 při splnění prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="d5587-200">In this template, the number of machines in the scale set increases by 1 when the threshold is met.</span></span>
    
    * <span data-ttu-id="d5587-201">**cooldown**</span><span class="sxs-lookup"><span data-stu-id="d5587-201">**cooldown**</span></span>  
    <span data-ttu-id="d5587-202">Tato hodnota je množství času má počkat od poslední akce škálování, než dojde k další akce.</span><span class="sxs-lookup"><span data-stu-id="d5587-202">This value is the amount of time to wait since the last scaling action before the next action occurs.</span></span> <span data-ttu-id="d5587-203">Tato hodnota musí být mezi minutu a jeden týden.</span><span class="sxs-lookup"><span data-stu-id="d5587-203">This value must be between one minute and one week.</span></span>

12. <span data-ttu-id="d5587-204">Uložte soubor šablony.</span><span class="sxs-lookup"><span data-stu-id="d5587-204">Save the template file.</span></span>    

## <a name="step-4-upload-the-template-to-storage"></a><span data-ttu-id="d5587-205">Krok 4: Nahrání šablony do úložiště</span><span class="sxs-lookup"><span data-stu-id="d5587-205">Step 4: Upload the template to storage</span></span>
<span data-ttu-id="d5587-206">Šablonu je možné uložit tak dlouho, dokud znáte název a primární klíč účtu úložiště, který jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="d5587-206">The template can be uploaded as long as you know the name and primary key of the storage account that you created in step 1.</span></span>

1. <span data-ttu-id="d5587-207">V okně prostředí Microsoft Azure PowerShell nastavte proměnné, která určuje název účtu úložiště, který jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="d5587-207">In the Microsoft Azure PowerShell window, set a variable that specifies the name of the storage account that you created in step 1.</span></span>
   
    ```powershell
    $storageAccountName = "vmstestsa"
    ```

2. <span data-ttu-id="d5587-208">Nastavte proměnnou, která určuje primární klíč účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5587-208">Set a variable that specifies the primary key of the storage account.</span></span>
   
    ```powershell
    $storageAccountKey = "<primary-account-key>"
    ```
   
   <span data-ttu-id="d5587-209">Tento klíč můžete získat kliknutím na ikonu klíče při zobrazení prostředků účtu úložiště na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="d5587-209">You can get this key by clicking the key icon when viewing the storage account resource in the Azure portal.</span></span>
3. <span data-ttu-id="d5587-210">Vytvořte objekt kontext účtu úložiště, který se používá k ověření operací s účtem úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5587-210">Create the storage account context object that is used to validate operations with the storage account.</span></span>
   
    ```powershell
    $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
    ```

4. <span data-ttu-id="d5587-211">Vytvořte kontejner pro uložení šablony.</span><span class="sxs-lookup"><span data-stu-id="d5587-211">Create the container for storing the template.</span></span>

    ```powershell
    $containerName = "templates"
    New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob
    ```

5. <span data-ttu-id="d5587-212">Nahrajte soubor šablony do nového kontejneru.</span><span class="sxs-lookup"><span data-stu-id="d5587-212">Upload the template file to the new container.</span></span>

    ```powershell   
    $blobName = "VMSSTemplate.json"
    $fileName = "C:\" + $BlobName
    Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx
    ```

## <a name="step-5-deploy-the-template"></a><span data-ttu-id="d5587-213">Krok 5: Nasazení šablony</span><span class="sxs-lookup"><span data-stu-id="d5587-213">Step 5: Deploy the template</span></span>
<span data-ttu-id="d5587-214">Teď, když jste vytvořili šablonu, můžete začít nasazovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="d5587-214">Now that you created the template, you can start deploying the resources.</span></span> <span data-ttu-id="d5587-215">Použijte tento příkaz ke spuštění procesu:</span><span class="sxs-lookup"><span data-stu-id="d5587-215">Use this command to start the process:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"
```

<span data-ttu-id="d5587-216">Po stisknutí klávesy zadejte, zobrazí se výzva k zadání hodnot pro proměnné, které jste přiřadili.</span><span class="sxs-lookup"><span data-stu-id="d5587-216">When you press enter, you are prompted to provide values for the variables you assigned.</span></span> <span data-ttu-id="d5587-217">Zadejte tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="d5587-217">Provide these values:</span></span>

```powershell
vmName: vmsstestvm1
  vmSSName: vmsstest1
  instanceCount: 5
  adminUserName: vmadmin1
  adminPassword: VMpass1
  resourcePrefix: vmsstest
```

<span data-ttu-id="d5587-218">Má trvat přibližně 15 minut pro všechny prostředky. Chcete-li úspěšně nasadit.</span><span class="sxs-lookup"><span data-stu-id="d5587-218">It should take about 15 minutes for all the resources to successfully be deployed.</span></span>

> [!NOTE]
> <span data-ttu-id="d5587-219">Možnost na portálu můžete taky nasadit tyto prostředky.</span><span class="sxs-lookup"><span data-stu-id="d5587-219">You can also use the portal’s ability to deploy the resources.</span></span> <span data-ttu-id="d5587-220">Použít tento odkaz: "https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>"</span><span class="sxs-lookup"><span data-stu-id="d5587-220">Use this link: "https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>"</span></span>
> 
> 

## <a name="step-6-monitor-resources"></a><span data-ttu-id="d5587-221">Krok 6: Sledování prostředků</span><span class="sxs-lookup"><span data-stu-id="d5587-221">Step 6: Monitor resources</span></span>
<span data-ttu-id="d5587-222">Můžete získat některé informace o sady škálování virtuálního počítače pomocí těchto metod:</span><span class="sxs-lookup"><span data-stu-id="d5587-222">You can get some information about virtual machine scale sets using these methods:</span></span>

* <span data-ttu-id="d5587-223">Portál Azure – můžete aktuálně získat omezené množství informací pomocí portálu.</span><span class="sxs-lookup"><span data-stu-id="d5587-223">The Azure portal - You can currently get a limited amount of information using the portal.</span></span>
* <span data-ttu-id="d5587-224">[Průzkumníka prostředků Azure](https://resources.azure.com/) – tento nástroj je nejvhodnější pro zkoumání aktuálního stavu škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="d5587-224">The [Azure Resource Explorer](https://resources.azure.com/) - This tool is the best for exploring the current state of your scale set.</span></span> <span data-ttu-id="d5587-225">Postupujte podle této cestě a měli byste vidět zobrazení instance škálovací sady, který jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="d5587-225">Follow this path and you should see the instance view of the scale set that you created:</span></span>
  
    <span data-ttu-id="d5587-226">odběry > {vaše předplatné} > Skupinyprostředků > vmsstestrg1 > poskytovatelé > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="d5587-226">subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines</span></span>

* <span data-ttu-id="d5587-227">Azure PowerShell – použijte tento příkaz určité informace:</span><span class="sxs-lookup"><span data-stu-id="d5587-227">Azure PowerShell - Use this command to get some information:</span></span>

  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  ```
  
  <span data-ttu-id="d5587-228">Nebo</span><span class="sxs-lookup"><span data-stu-id="d5587-228">Or</span></span>
  
  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView
  ```

* <span data-ttu-id="d5587-229">Připojte k samostatný virtuální počítač stejně, jako by všechny ostatní počítače a potom můžete virtuální počítače v sad pro monitorování jednotlivých procesů škálování vzdálený přístup.</span><span class="sxs-lookup"><span data-stu-id="d5587-229">Connect to the separate virtual machine just like you would any other machine and then you can remotely access the virtual machines in the scale set to monitor individual processes.</span></span>

> [!NOTE]
> <span data-ttu-id="d5587-230">Dokončení rozhraní REST API k získání informací o sady škálování najdete v [sadách škálování virtuálního počítače](https://msdn.microsoft.com/library/mt589023.aspx)</span><span class="sxs-lookup"><span data-stu-id="d5587-230">A complete REST API for obtaining information about scale sets can be found in [Virtual Machine Scale Sets](https://msdn.microsoft.com/library/mt589023.aspx)</span></span>

## <a name="step-7-remove-the-resources"></a><span data-ttu-id="d5587-231">Krok 7: Odebrání prostředky</span><span class="sxs-lookup"><span data-stu-id="d5587-231">Step 7: Remove the resources</span></span>
<span data-ttu-id="d5587-232">Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, vždycky je dobrým zvykem odstranit prostředky, které už nejsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="d5587-232">Because you are charged for resources used in Azure, it is always a good practice to delete resources that are no longer needed.</span></span> <span data-ttu-id="d5587-233">Nemusíte samostatně odstranit jednotlivé prostředky ze skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="d5587-233">You don’t need to delete each resource separately from a resource group.</span></span> <span data-ttu-id="d5587-234">Můžete odstranit skupinu prostředků a všechny její prostředky se automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="d5587-234">You can delete the resource group and all its resources are automatically deleted.</span></span>

  ```powershell
  Remove-AzureRmResourceGroup -Name vmsstestrg1
  ```

<span data-ttu-id="d5587-235">Pokud chcete zachovat vaší skupiny prostředků, můžete odstranit pouze sad škálování.</span><span class="sxs-lookup"><span data-stu-id="d5587-235">If you want to keep your resource group, you can delete the scale set only.</span></span>

  ```powershell
  Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"
  ```

## <a name="next-steps"></a><span data-ttu-id="d5587-236">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d5587-236">Next steps</span></span>
* <span data-ttu-id="d5587-237">Spravovat byly sadou škálování, kterou jste právě vytvořili, pomocí informací v [spravovat virtuální počítače ve Škálovací sadě virtuálního počítače](virtual-machine-scale-sets-windows-manage.md).</span><span class="sxs-lookup"><span data-stu-id="d5587-237">Manage the scale set that you just created using the information in [Manage virtual machines in a Virtual Machine Scale Set](virtual-machine-scale-sets-windows-manage.md).</span></span>
* <span data-ttu-id="d5587-238">Další informace o vertikálním škálování najdete v tématu [Vertikální automatické škálování se škálovacími sadami virtuálních počítačů](virtual-machine-scale-sets-vertical-scale-reprovision.md).</span><span class="sxs-lookup"><span data-stu-id="d5587-238">Learn more about vertical scaling by reviewing [Vertical autoscale with Virtual Machine Scale sets](virtual-machine-scale-sets-vertical-scale-reprovision.md)</span></span>
* <span data-ttu-id="d5587-239">Najít příklady Azure monitorování monitorování funkcí v [Azure PowerShell monitorování rychlý start ukázky](../monitoring-and-diagnostics/insights-powershell-samples.md)</span><span class="sxs-lookup"><span data-stu-id="d5587-239">Find examples of Azure Monitor monitoring features in [Azure Monitor PowerShell quick start samples](../monitoring-and-diagnostics/insights-powershell-samples.md)</span></span>
* <span data-ttu-id="d5587-240">Další informace o funkcích oznámení v [používat akce škálování k odesílání e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="d5587-240">Learn about notification features in [Use autoscale actions to send email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span></span>
* <span data-ttu-id="d5587-241">Zjistěte, jak [protokoly auditu použijte k odesílání e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="d5587-241">Learn how to [Use audit logs to send email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span></span>

