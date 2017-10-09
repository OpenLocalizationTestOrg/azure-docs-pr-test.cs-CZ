---
title: "aaaAutoscale sady škálování virtuálního počítače Windows | Microsoft Docs"
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
ms.openlocfilehash: 67cf1c5063ceba4fc076dc270090defdbddbcfe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="5da2a-103">Automatické škálování počítače ve škálovací sadě virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5da2a-103">Automatically scale machines in a virtual machine scale set</span></span>
<span data-ttu-id="5da2a-104">Sady škálování virtuálního počítače můžete snadno můžete toodeploy a spravovat identickými virtuálními počítači jako sada.</span><span class="sxs-lookup"><span data-stu-id="5da2a-104">Virtual machine scale sets make it easy for you toodeploy and manage identical virtual machines as a set.</span></span> <span data-ttu-id="5da2a-105">Sady škálování pro aplikace hyperškálovatelný systém zajistit vysoce škálovatelného a přizpůsobitelné výpočetní vrstvy a podporují Image pro platformu Windows, Image platformy Linux, vlastních bitových kopií a rozšíření.</span><span class="sxs-lookup"><span data-stu-id="5da2a-105">Scale sets provide a highly scalable and customizable compute layer for hyperscale applications, and they support Windows platform images, Linux platform images, custom images, and extensions.</span></span> <span data-ttu-id="5da2a-106">Další informace o sadách škálování najdete v tématu [sadách škálování virtuálního počítače](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5da2a-106">For more information about scale sets, see [Virtual Machine Scale Sets](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="5da2a-107">Tento kurz ukazuje, jak nastavit toocreate škálovací sadu virtuálních počítačích s Windows a automaticky škálování hello počítačů v hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-107">This tutorial shows you how toocreate a scale set of Windows virtual machines and automatically scale hello machines in hello set.</span></span> <span data-ttu-id="5da2a-108">Můžete vytvořit hello škálování a vytvořit škálování vytvořením šablonu Azure Resource Manager a nasazení pomocí prostředí Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5da2a-108">You create hello scale set and set up scaling by creating an Azure Resource Manager template and deploying it using Azure PowerShell.</span></span> <span data-ttu-id="5da2a-109">Další informace o šablonách najdete v tématu o [vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="5da2a-109">For more information about templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> <span data-ttu-id="5da2a-110">toolearn Další informace o automatické škálování sad škálování, najdete v části [automatické škálování a sadách škálování virtuálního počítače](virtual-machine-scale-sets-autoscale-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5da2a-110">toolearn more about automatic scaling of scale sets, see [Automatic scaling and Virtual Machine Scale Sets](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

<span data-ttu-id="5da2a-111">V tomto článku, můžete nasadit následující hello prostředků a rozšíření:</span><span class="sxs-lookup"><span data-stu-id="5da2a-111">In this article, you deploy hello following resources and extensions:</span></span>

* <span data-ttu-id="5da2a-112">Microsoft.Storage/storageAccounts.</span><span class="sxs-lookup"><span data-stu-id="5da2a-112">Microsoft.Storage/storageAccounts</span></span>
* <span data-ttu-id="5da2a-113">Microsoft.Network/virtualNetworks</span><span class="sxs-lookup"><span data-stu-id="5da2a-113">Microsoft.Network/virtualNetworks</span></span>
* <span data-ttu-id="5da2a-114">Microsoft.Network/publicIPAddresses</span><span class="sxs-lookup"><span data-stu-id="5da2a-114">Microsoft.Network/publicIPAddresses</span></span>
* <span data-ttu-id="5da2a-115">Microsoft.Network/loadBalancers</span><span class="sxs-lookup"><span data-stu-id="5da2a-115">Microsoft.Network/loadBalancers</span></span>
* <span data-ttu-id="5da2a-116">Microsoft.Network/networkInterfaces</span><span class="sxs-lookup"><span data-stu-id="5da2a-116">Microsoft.Network/networkInterfaces</span></span>
* <span data-ttu-id="5da2a-117">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="5da2a-117">Microsoft.Compute/virtualMachines</span></span>
* <span data-ttu-id="5da2a-118">Microsoft.Compute/virtualMachineScaleSets</span><span class="sxs-lookup"><span data-stu-id="5da2a-118">Microsoft.Compute/virtualMachineScaleSets</span></span>
* <span data-ttu-id="5da2a-119">Microsoft.Insights.VMDiagnosticsSettings</span><span class="sxs-lookup"><span data-stu-id="5da2a-119">Microsoft.Insights.VMDiagnosticsSettings</span></span>
* <span data-ttu-id="5da2a-120">Microsoft.Insights/autoscaleSettings</span><span class="sxs-lookup"><span data-stu-id="5da2a-120">Microsoft.Insights/autoscaleSettings</span></span>

<span data-ttu-id="5da2a-121">Další informace o prostředky Resource Manageru najdete v tématu [Azure Resource Manager oproti nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5da2a-121">For more information about Resource Manager resources, see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

## <a name="step-1-install-azure-powershell"></a><span data-ttu-id="5da2a-122">Krok1: Nainstalování prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="5da2a-122">Step 1: Install Azure PowerShell</span></span>
<span data-ttu-id="5da2a-123">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) informace o instalaci hello nejnovější verzi prostředí Azure PowerShell, výběr předplatného a přihlášení tooAzure.</span><span class="sxs-lookup"><span data-stu-id="5da2a-123">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for information about installing hello latest version of Azure PowerShell, selecting your subscription, and signing in tooAzure.</span></span>

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a><span data-ttu-id="5da2a-124">Krok 2: Vytvoření skupiny prostředků a účet úložiště</span><span class="sxs-lookup"><span data-stu-id="5da2a-124">Step 2: Create a resource group and a storage account</span></span>
1. <span data-ttu-id="5da2a-125">**Vytvořte skupinu prostředků** – všechny prostředky musí být nasazené tooa skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="5da2a-125">**Create a resource group** – All resources must be deployed tooa resource group.</span></span> <span data-ttu-id="5da2a-126">Použití [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) toocreate skupinu prostředků s názvem **vmsstestrg1**.</span><span class="sxs-lookup"><span data-stu-id="5da2a-126">Use [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) toocreate a resource group named **vmsstestrg1**.</span></span>
2. <span data-ttu-id="5da2a-127">**Vytvoření účtu úložiště** – tento účet úložiště se uloží hello šablony.</span><span class="sxs-lookup"><span data-stu-id="5da2a-127">**Create a storage account** – This storage account is where hello template is stored.</span></span> <span data-ttu-id="5da2a-128">Použití [nový AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) toocreate s názvem účtu úložiště **vmsstestsa**.</span><span class="sxs-lookup"><span data-stu-id="5da2a-128">Use [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) toocreate a storage account named **vmsstestsa**.</span></span>

## <a name="step-3-create-hello-template"></a><span data-ttu-id="5da2a-129">Krok 3: Vytvoření šablony hello</span><span class="sxs-lookup"><span data-stu-id="5da2a-129">Step 3: Create hello template</span></span>
<span data-ttu-id="5da2a-130">Šablonu Azure Resource Manager umožňuje vám toodeploy a spravovat prostředky Azure společně s použitím popis JSON hello prostředků a parametry přidružené nasazení.</span><span class="sxs-lookup"><span data-stu-id="5da2a-130">An Azure Resource Manager template makes it possible for you toodeploy and manage Azure resources together by using a JSON description of hello resources and associated deployment parameters.</span></span>

1. <span data-ttu-id="5da2a-131">Ve svém oblíbeném editoru vytvořte soubor hello C:\VMSSTemplate.json a přidejte hello počáteční JSON struktura toosupport hello šablony.</span><span class="sxs-lookup"><span data-stu-id="5da2a-131">In your favorite editor, create hello file C:\VMSSTemplate.json and add hello initial JSON structure toosupport hello template.</span></span>

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

2. <span data-ttu-id="5da2a-132">Parametry nejsou vždy vyžaduje, ale poskytují způsob, jak tooinput hodnoty při nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-132">Parameters are not always required, but they provide a way tooinput values when hello template is deployed.</span></span> <span data-ttu-id="5da2a-133">Přidejte tyto parametry v části hello parametry nadřazený element, zda jste přidali toohello šablony.</span><span class="sxs-lookup"><span data-stu-id="5da2a-133">Add these parameters under hello parameters parent element that you added toohello template.</span></span>

    ```json   
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
    * <span data-ttu-id="5da2a-134">Název hello samostatný virtuální počítač, který je použité tooaccess hello počítače ve škálovací sadě hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-134">A name for hello separate virtual machine that is used tooaccess hello machines in hello scale set.</span></span>
    * <span data-ttu-id="5da2a-135">Hello název účtu úložiště hello se uloží hello šablony.</span><span class="sxs-lookup"><span data-stu-id="5da2a-135">hello name of hello storage account where hello template is stored.</span></span>
    * <span data-ttu-id="5da2a-136">Hello počet virtuálních počítačů tooinitially vytvořit v sadě škálování hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-136">hello number of virtual machines tooinitially create in hello scale set.</span></span>
    * <span data-ttu-id="5da2a-137">Hello jméno a heslo účtu správce hello hello virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="5da2a-137">hello name and password of hello administrator account on hello virtual machines.</span></span>
    * <span data-ttu-id="5da2a-138">Předpona názvu pro hello prostředky, které jsou vytvořené toosupport hello škálování nastavit.</span><span class="sxs-lookup"><span data-stu-id="5da2a-138">A name prefix for hello resources that are created toosupport hello scale set.</span></span>

3. <span data-ttu-id="5da2a-139">Proměnné lze použít v hodnotách toospecify šablony, které se může často mění, nebo hodnoty, které je třeba toobe vytvořena z kombinace hodnot parametrů.</span><span class="sxs-lookup"><span data-stu-id="5da2a-139">Variables can be used in a template toospecify values that may change frequently or values that need toobe created from a combination of parameter values.</span></span> <span data-ttu-id="5da2a-140">Přidejte tyto proměnné v části hello proměnné nadřazený element, zda jste přidali toohello šablony.</span><span class="sxs-lookup"><span data-stu-id="5da2a-140">Add these variables under hello variables parent element that you added toohello template.</span></span>

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
   
    * <span data-ttu-id="5da2a-141">Názvy DNS, které jsou používány hello síťových rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5da2a-141">DNS names that are used by hello network interfaces.</span></span>

        * <span data-ttu-id="5da2a-142">Hello IP adresy názvů a předpony pro hello virtuální sítě a podsítě.</span><span class="sxs-lookup"><span data-stu-id="5da2a-142">hello IP address names and prefixes for hello virtual network and subnets.</span></span>
        * <span data-ttu-id="5da2a-143">Hello názvy a identifikátory hello virtuální sítě, zatížení vyrovnávání a síťových rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5da2a-143">hello names and identifiers of hello virtual network, load balancer, and network interfaces.</span></span>
        * <span data-ttu-id="5da2a-144">Názvy účtů úložiště pro hello účty přidružené k hello počítače ve škálovací sadě hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-144">Storage account names for hello accounts associated with hello machines in hello scale set.</span></span>
        * <span data-ttu-id="5da2a-145">Nastavení pro hello rozšíření diagnostiky, který je nainstalován na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5da2a-145">Settings for hello Diagnostics extension that is installed on hello virtual machines.</span></span> <span data-ttu-id="5da2a-146">Další informace o hello rozšíření diagnostiky najdete v tématu [vytvořit Windows virtuální počítač s monitorování a Diagnostika pomocí šablony Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5da2a-146">For more information about hello Diagnostics extension, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

4. <span data-ttu-id="5da2a-147">Přidáte prostředek účet úložiště hello pod hello prostředky nadřazeného elementu, zda jste přidali toohello šablony.</span><span class="sxs-lookup"><span data-stu-id="5da2a-147">Add hello storage account resource under hello resources parent element that you added toohello template.</span></span> <span data-ttu-id="5da2a-148">Tato šablona používá hello toocreate smyčky doporučená pět účty úložiště, kde jsou uloženy hello disky operačního systému a diagnostických dat.</span><span class="sxs-lookup"><span data-stu-id="5da2a-148">This template uses a loop toocreate hello recommended five storage accounts where hello operating system disks and diagnostic data are stored.</span></span> <span data-ttu-id="5da2a-149">Tuto sadu účtů může podporovat až too100 virtuální počítače ve škálovací sadě, což je maximální aktuální hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-149">This set of accounts can support up too100 virtual machines in a scale set, which is hello current maximum.</span></span> <span data-ttu-id="5da2a-150">Každý účet úložiště je s názvem se písmeno označení, která byla definována v kombinaci s hello předponu, která zadáte v hello parametry šablony hello proměnné hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-150">Each storage account is named with a letter designator that was defined in hello variables combined with hello prefix that you provide in hello parameters for hello template.</span></span>

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

5. <span data-ttu-id="5da2a-151">Přidáte prostředek hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="5da2a-151">Add hello virtual network resource.</span></span> <span data-ttu-id="5da2a-152">Další informace najdete v tématu [poskytovatele síťových prostředků](../virtual-network/resource-groups-networking.md).</span><span class="sxs-lookup"><span data-stu-id="5da2a-152">For more information, see [Network Resource Provider](../virtual-network/resource-groups-networking.md).</span></span>

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

6. <span data-ttu-id="5da2a-153">Přidáte hello veřejnou IP adresu prostředky, které jsou používány hello zatížení vyrovnávání a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="5da2a-153">Add hello public IP address resources that are used by hello load balancer and network interface.</span></span>

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

7. <span data-ttu-id="5da2a-154">Přidejte hello prostředek pro vyrovnávání zatížení, který se používá škálovací sada hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-154">Add hello load balancer resource that is used by hello scale set.</span></span> <span data-ttu-id="5da2a-155">Další informace najdete v tématu [podporu správce prostředků Azure pro nástroj pro vyrovnávání zatížení](../load-balancer/load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="5da2a-155">For more information, see [Azure Resource Manager Support for Load Balancer](../load-balancer/load-balancer-arm.md).</span></span>

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

8. <span data-ttu-id="5da2a-156">Přidejte hello síťového rozhraní prostředku, který je používán hello samostatný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="5da2a-156">Add hello network interface resource that is used by hello separate virtual machine.</span></span> <span data-ttu-id="5da2a-157">Protože počítače ve škálovací sadě nejsou přístupné přes veřejnou IP adresu, samostatný virtuální počítač se vytvoří v hello stejné virtuální sítě počítače hello tooremotely přístup.</span><span class="sxs-lookup"><span data-stu-id="5da2a-157">Because machines in a scale set aren't accessible through a public IP address, a separate virtual machine is created in hello same virtual network tooremotely access hello machines.</span></span>

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

9. <span data-ttu-id="5da2a-158">Přidejte hello samostatný virtuální počítač do stejné sítě jako hello škálovací sadu hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-158">Add hello separate virtual machine in hello same network as hello scale set.</span></span>

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

10. <span data-ttu-id="5da2a-159">Přidat sady škálování virtuálního počítače hello prostředků a zadejte hello rozšíření diagnostiky, který je nainstalován na všechny virtuální počítače ve škálovací sadě hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-159">Add hello virtual machine scale set resource and specify hello diagnostics extension that is installed on all virtual machines in hello scale set.</span></span> <span data-ttu-id="5da2a-160">Mnoho hello nastavení pro tento prostředek je podobný s hello prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="5da2a-160">Many of hello settings for this resource are similar with hello virtual machine resource.</span></span> <span data-ttu-id="5da2a-161">Hlavní rozdíly Hello jsou hello kapacity element, který určuje hello počet virtuálních počítačů v sadě škálování hello a upgradePolicy, která určuje, jak jsou provedeny aktualizace toovirtual počítače.</span><span class="sxs-lookup"><span data-stu-id="5da2a-161">hello main differences are hello capacity element that specifies hello number of virtual machines in hello scale set, and upgradePolicy that specifies how updates are made toovirtual machines.</span></span> <span data-ttu-id="5da2a-162">Hello škálovací sadu vytvořen až všechny účty úložiště hello jsou vytvořeny jako zadaný hello dependsOn element.</span><span class="sxs-lookup"><span data-stu-id="5da2a-162">hello scale set is not created until all hello storage accounts are created as specified with hello dependsOn element.</span></span>

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

11. <span data-ttu-id="5da2a-163">Přidejte hello autoscaleSettings prostředek, který definuje, jak upraví podle využití procesoru hello na počítačích hello v sadě škálování hello hello škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="5da2a-163">Add hello autoscaleSettings resource that defines how hello scale set adjusts based on hello processor usage on hello machines in hello scale set.</span></span>

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

    <span data-ttu-id="5da2a-164">V tomto kurzu jsou důležité tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5da2a-164">For this tutorial, these values are important:</span></span>
    
    * <span data-ttu-id="5da2a-165">**metricName**</span><span class="sxs-lookup"><span data-stu-id="5da2a-165">**metricName**</span></span>  
    <span data-ttu-id="5da2a-166">Tato hodnota je hello stejné jako hello čítačů výkonu, které jsme definovali hello wadperfcounter proměnné.</span><span class="sxs-lookup"><span data-stu-id="5da2a-166">This value is hello same as hello performance counter that we defined in hello wadperfcounter variable.</span></span> <span data-ttu-id="5da2a-167">Pomocí této proměnné, hello rozšíření diagnostiky shromažďuje hello **procesor(_celkem)\% času procesoru** čítače.</span><span class="sxs-lookup"><span data-stu-id="5da2a-167">Using that variable, hello Diagnostics extension collects hello  **Processor(_Total)\% Processor Time** counter.</span></span>
    
    * <span data-ttu-id="5da2a-168">**metricResourceUri**</span><span class="sxs-lookup"><span data-stu-id="5da2a-168">**metricResourceUri**</span></span>  
    <span data-ttu-id="5da2a-169">Tato hodnota je identifikátor prostředku hello škálovací sadu virtuálních počítačů hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-169">This value is hello resource identifier of hello virtual machine scale set.</span></span>
    
    * <span data-ttu-id="5da2a-170">**časovými úseky**</span><span class="sxs-lookup"><span data-stu-id="5da2a-170">**timeGrain**</span></span>  
    <span data-ttu-id="5da2a-171">Tato hodnota je hello členitost hello metriky, které jsou shromážděny.</span><span class="sxs-lookup"><span data-stu-id="5da2a-171">This value is hello granularity of hello metrics that are collected.</span></span> <span data-ttu-id="5da2a-172">V této šabloně je nastavit tooone minutu.</span><span class="sxs-lookup"><span data-stu-id="5da2a-172">In this template, it is set tooone minute.</span></span>
    
    * <span data-ttu-id="5da2a-173">**statistiky**</span><span class="sxs-lookup"><span data-stu-id="5da2a-173">**statistic**</span></span>  
    <span data-ttu-id="5da2a-174">Tato hodnota určuje, jak hello metriky jsou kombinované tooaccommodate hello automatického škálování akce.</span><span class="sxs-lookup"><span data-stu-id="5da2a-174">This value determines how hello metrics are combined tooaccommodate hello automatic scaling action.</span></span> <span data-ttu-id="5da2a-175">Hello možné hodnoty jsou: průměr, minimum, maximum.</span><span class="sxs-lookup"><span data-stu-id="5da2a-175">hello possible values are: Average, Min, Max.</span></span> <span data-ttu-id="5da2a-176">V této šabloně se shromažďují hello průměrné celkové využití procesoru hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="5da2a-176">In this template, hello average total CPU usage of hello virtual machines is collected.</span></span>
    
    * <span data-ttu-id="5da2a-177">**Hodnota timeWindow**</span><span class="sxs-lookup"><span data-stu-id="5da2a-177">**timeWindow**</span></span>  
    <span data-ttu-id="5da2a-178">Tato hodnota je rozsah hello času, ve kterém se shromažďují instance data.</span><span class="sxs-lookup"><span data-stu-id="5da2a-178">This value is hello range of time in which instance data is collected.</span></span> <span data-ttu-id="5da2a-179">Musí být mezi 5 minutami a 12 hodin.</span><span class="sxs-lookup"><span data-stu-id="5da2a-179">It must be between 5 minutes and 12 hours.</span></span>
    
    * <span data-ttu-id="5da2a-180">**Agregace času**</span><span class="sxs-lookup"><span data-stu-id="5da2a-180">**timeAggregation**</span></span>  
    <span data-ttu-id="5da2a-181">jeho hodnota určuje, jak by měla být kombinovány hello data, která se shromažďují v čase.</span><span class="sxs-lookup"><span data-stu-id="5da2a-181">his value determines how hello data that is collected should be combined over time.</span></span> <span data-ttu-id="5da2a-182">Hello výchozí hodnota je průměr.</span><span class="sxs-lookup"><span data-stu-id="5da2a-182">hello default value is Average.</span></span> <span data-ttu-id="5da2a-183">Hello možné hodnoty jsou: průměr, minimální, maximální, poslední, celkový počet, počet.</span><span class="sxs-lookup"><span data-stu-id="5da2a-183">hello possible values are: Average, Minimum, Maximum, Last, Total, Count.</span></span>
    
    * <span data-ttu-id="5da2a-184">**operátor**</span><span class="sxs-lookup"><span data-stu-id="5da2a-184">**operator**</span></span>  
    <span data-ttu-id="5da2a-185">Tato hodnota je hello operátor, který je použité toocompare hello metriky dat a hello prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5da2a-185">This value is hello operator that is used toocompare hello metric data and hello threshold.</span></span> <span data-ttu-id="5da2a-186">Hello možné hodnoty jsou: rovná NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span><span class="sxs-lookup"><span data-stu-id="5da2a-186">hello possible values are: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span></span>
    
    * <span data-ttu-id="5da2a-187">**Prahová hodnota**</span><span class="sxs-lookup"><span data-stu-id="5da2a-187">**threshold**</span></span>  
    <span data-ttu-id="5da2a-188">Tato hodnota je hello hodnotu, která aktivuje hello akce škálování.</span><span class="sxs-lookup"><span data-stu-id="5da2a-188">This value is hello value that triggers hello scale action.</span></span> <span data-ttu-id="5da2a-189">V této šabloně se přidávají počítače sad po více než 50 % hello průměrné využití procesoru mezi počítači v sadě hello toohello škálování.</span><span class="sxs-lookup"><span data-stu-id="5da2a-189">In this template, machines are added toohello scale set when hello average CPU usage among machines in hello set is over 50%.</span></span>
    
    * <span data-ttu-id="5da2a-190">**směr**</span><span class="sxs-lookup"><span data-stu-id="5da2a-190">**direction**</span></span>  
    <span data-ttu-id="5da2a-191">Tato hodnota určuje hello akce, která se provede, když je dosaženo hello prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5da2a-191">This value determines hello action that is taken when hello threshold value is achieved.</span></span> <span data-ttu-id="5da2a-192">Hello možné hodnoty jsou zvýšení nebo snížení.</span><span class="sxs-lookup"><span data-stu-id="5da2a-192">hello possible values are Increase or Decrease.</span></span> <span data-ttu-id="5da2a-193">V této šabloně hello počet virtuálních počítačů v sadě škálování hello se zvyšuje, když prahové hodnoty hello je více než 50 % hello definované časové okno.</span><span class="sxs-lookup"><span data-stu-id="5da2a-193">In this template, hello number of virtual machines in hello scale set is increased if hello threshold is over 50% in hello defined time window.</span></span>
    
    * <span data-ttu-id="5da2a-194">**Typ**</span><span class="sxs-lookup"><span data-stu-id="5da2a-194">**type**</span></span>  
    <span data-ttu-id="5da2a-195">Tato hodnota je hello typ akce, který má vzniknout a musí být nastaven tooChangeCount.</span><span class="sxs-lookup"><span data-stu-id="5da2a-195">This value is hello type of action that should occur and must be set tooChangeCount.</span></span>
    
    * <span data-ttu-id="5da2a-196">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="5da2a-196">**value**</span></span>  
    <span data-ttu-id="5da2a-197">Tato hodnota je hello počet virtuálních počítačů, které jsou přidány nebo odebrány z hello škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="5da2a-197">This value is hello number of virtual machines that are added or removed from hello scale set.</span></span> <span data-ttu-id="5da2a-198">Tato hodnota musí být 1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="5da2a-198">This value must be 1 or greater.</span></span> <span data-ttu-id="5da2a-199">Hello výchozí hodnota je 1.</span><span class="sxs-lookup"><span data-stu-id="5da2a-199">hello default value is 1.</span></span> <span data-ttu-id="5da2a-200">V této šabloně hello počet počítačů v hello škálování nastavit zvýší o 1 při splnění hello prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="5da2a-200">In this template, hello number of machines in hello scale set increases by 1 when hello threshold is met.</span></span>
    
    * <span data-ttu-id="5da2a-201">**cooldown**</span><span class="sxs-lookup"><span data-stu-id="5da2a-201">**cooldown**</span></span>  
    <span data-ttu-id="5da2a-202">Tato hodnota je hello množství toowait času od poslední akce škálování hello předtím, než dojde k hello další akce.</span><span class="sxs-lookup"><span data-stu-id="5da2a-202">This value is hello amount of time toowait since hello last scaling action before hello next action occurs.</span></span> <span data-ttu-id="5da2a-203">Tato hodnota musí být mezi minutu a jeden týden.</span><span class="sxs-lookup"><span data-stu-id="5da2a-203">This value must be between one minute and one week.</span></span>

12. <span data-ttu-id="5da2a-204">Uložte soubor šablony hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-204">Save hello template file.</span></span>    

## <a name="step-4-upload-hello-template-toostorage"></a><span data-ttu-id="5da2a-205">Krok 4: Nahrání toostorage šablony hello</span><span class="sxs-lookup"><span data-stu-id="5da2a-205">Step 4: Upload hello template toostorage</span></span>
<span data-ttu-id="5da2a-206">lze uložit šablonu Hello tak dlouho, dokud znáte název hello a primární klíč účtu úložiště hello, kterou jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="5da2a-206">hello template can be uploaded as long as you know hello name and primary key of hello storage account that you created in step 1.</span></span>

1. <span data-ttu-id="5da2a-207">V okně prostředí Microsoft Azure PowerShell hello nastavte proměnné, která určuje název hello hello účtu úložiště, který jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="5da2a-207">In hello Microsoft Azure PowerShell window, set a variable that specifies hello name of hello storage account that you created in step 1.</span></span>
   
    ```powershell
    $storageAccountName = "vmstestsa"
    ```

2. <span data-ttu-id="5da2a-208">Nastavte proměnné, která určuje hello primární klíč účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-208">Set a variable that specifies hello primary key of hello storage account.</span></span>
   
    ```powershell
    $storageAccountKey = "<primary-account-key>"
    ```
   
   <span data-ttu-id="5da2a-209">Tento klíč můžete získat kliknutím na ikonu klíče hello při zobrazení prostředků účtu úložiště hello hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="5da2a-209">You can get this key by clicking hello key icon when viewing hello storage account resource in hello Azure portal.</span></span>
3. <span data-ttu-id="5da2a-210">Vytvořte hello úložiště účet kontext objekt, který je použité toovalidate operací s účtem úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-210">Create hello storage account context object that is used toovalidate operations with hello storage account.</span></span>
   
    ```powershell
    $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
    ```

4. <span data-ttu-id="5da2a-211">Vytvoření kontejneru hello uložení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-211">Create hello container for storing hello template.</span></span>

    ```powershell
    $containerName = "templates"
    New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob
    ```

5. <span data-ttu-id="5da2a-212">Nahrajte hello šablony souboru toohello nový kontejner.</span><span class="sxs-lookup"><span data-stu-id="5da2a-212">Upload hello template file toohello new container.</span></span>

    ```powershell   
    $blobName = "VMSSTemplate.json"
    $fileName = "C:\" + $BlobName
    Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx
    ```

## <a name="step-5-deploy-hello-template"></a><span data-ttu-id="5da2a-213">Krok 5: Nasazení šablony hello</span><span class="sxs-lookup"><span data-stu-id="5da2a-213">Step 5: Deploy hello template</span></span>
<span data-ttu-id="5da2a-214">Teď, když jste vytvořili hello šablony, můžete začít nasazovat hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="5da2a-214">Now that you created hello template, you can start deploying hello resources.</span></span> <span data-ttu-id="5da2a-215">Použijte tento příkaz toostart hello proces:</span><span class="sxs-lookup"><span data-stu-id="5da2a-215">Use this command toostart hello process:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"
```

<span data-ttu-id="5da2a-216">Po stisknutí klávesy zadejte, jsou výzvami tooprovide hodnoty pro proměnné hello, které jste přiřadili.</span><span class="sxs-lookup"><span data-stu-id="5da2a-216">When you press enter, you are prompted tooprovide values for hello variables you assigned.</span></span> <span data-ttu-id="5da2a-217">Zadejte tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="5da2a-217">Provide these values:</span></span>

```powershell
vmName: vmsstestvm1
  vmSSName: vmsstest1
  instanceCount: 5
  adminUserName: vmadmin1
  adminPassword: VMpass1
  resourcePrefix: vmsstest
```

<span data-ttu-id="5da2a-218">Pro všechny prostředky toosuccessfully hello nasadit má trvat přibližně 15 minut.</span><span class="sxs-lookup"><span data-stu-id="5da2a-218">It should take about 15 minutes for all hello resources toosuccessfully be deployed.</span></span>

> [!NOTE]
> <span data-ttu-id="5da2a-219">Můžete také použít portál hello možnost toodeploy hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="5da2a-219">You can also use hello portal’s ability toodeploy hello resources.</span></span> <span data-ttu-id="5da2a-220">Použít tento odkaz: "https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template>"</span><span class="sxs-lookup"><span data-stu-id="5da2a-220">Use this link: "https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template>"</span></span>
> 
> 

## <a name="step-6-monitor-resources"></a><span data-ttu-id="5da2a-221">Krok 6: Sledování prostředků</span><span class="sxs-lookup"><span data-stu-id="5da2a-221">Step 6: Monitor resources</span></span>
<span data-ttu-id="5da2a-222">Můžete získat některé informace o sady škálování virtuálního počítače pomocí těchto metod:</span><span class="sxs-lookup"><span data-stu-id="5da2a-222">You can get some information about virtual machine scale sets using these methods:</span></span>

* <span data-ttu-id="5da2a-223">Hello portál Azure – nyní můžete získat omezené množství informací pomocí portálu hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-223">hello Azure portal - You can currently get a limited amount of information using hello portal.</span></span>
* <span data-ttu-id="5da2a-224">Hello [Průzkumníka prostředků Azure](https://resources.azure.com/) – tento nástroj je nejlepší pro zkoumání hello aktuální stav škálovací sady hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-224">hello [Azure Resource Explorer](https://resources.azure.com/) - This tool is hello best for exploring hello current state of your scale set.</span></span> <span data-ttu-id="5da2a-225">Postupujte podle této cestě a měli byste vidět sadu zobrazení instance hello hello měřítka, kterou jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="5da2a-225">Follow this path and you should see hello instance view of hello scale set that you created:</span></span>
  
    <span data-ttu-id="5da2a-226">odběry > {vaše předplatné} > Skupinyprostředků > vmsstestrg1 > poskytovatelé > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="5da2a-226">subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines</span></span>

* <span data-ttu-id="5da2a-227">Prostředí Azure PowerShell - použít tento příkaz tooget některé informace:</span><span class="sxs-lookup"><span data-stu-id="5da2a-227">Azure PowerShell - Use this command tooget some information:</span></span>

  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  ```
  
  <span data-ttu-id="5da2a-228">Nebo</span><span class="sxs-lookup"><span data-stu-id="5da2a-228">Or</span></span>
  
  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView
  ```

* <span data-ttu-id="5da2a-229">Připojte toohello samostatný virtuální počítač stejně, jako by všechny ostatní počítače a pak můžete vzdáleně přistupovat hello virtuálních počítačů v hello škálování sadu toomonitor jednotlivých procesů.</span><span class="sxs-lookup"><span data-stu-id="5da2a-229">Connect toohello separate virtual machine just like you would any other machine and then you can remotely access hello virtual machines in hello scale set toomonitor individual processes.</span></span>

> [!NOTE]
> <span data-ttu-id="5da2a-230">Dokončení rozhraní REST API k získání informací o sady škálování najdete v [sadách škálování virtuálního počítače](https://msdn.microsoft.com/library/mt589023.aspx)</span><span class="sxs-lookup"><span data-stu-id="5da2a-230">A complete REST API for obtaining information about scale sets can be found in [Virtual Machine Scale Sets](https://msdn.microsoft.com/library/mt589023.aspx)</span></span>

## <a name="step-7-remove-hello-resources"></a><span data-ttu-id="5da2a-231">Krok 7: Odebrání hello prostředky</span><span class="sxs-lookup"><span data-stu-id="5da2a-231">Step 7: Remove hello resources</span></span>
<span data-ttu-id="5da2a-232">Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, je vždy prostředky toodelete osvědčených postupů, které už nejsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="5da2a-232">Because you are charged for resources used in Azure, it is always a good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="5da2a-233">Nepotřebujete toodelete každého prostředku nezávisle na skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="5da2a-233">You don’t need toodelete each resource separately from a resource group.</span></span> <span data-ttu-id="5da2a-234">Odstraněním skupiny prostředků hello a všechny její prostředky se automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="5da2a-234">You can delete hello resource group and all its resources are automatically deleted.</span></span>

  ```powershell
  Remove-AzureRmResourceGroup -Name vmsstestrg1
  ```

<span data-ttu-id="5da2a-235">Pokud chcete tookeep vaší skupiny prostředků, můžete odstranit pouze sad škálování hello.</span><span class="sxs-lookup"><span data-stu-id="5da2a-235">If you want tookeep your resource group, you can delete hello scale set only.</span></span>

  ```powershell
  Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"
  ```

## <a name="next-steps"></a><span data-ttu-id="5da2a-236">Další kroky</span><span class="sxs-lookup"><span data-stu-id="5da2a-236">Next steps</span></span>
* <span data-ttu-id="5da2a-237">Spravovat hello sadě škálování, který jste právě vytvořili pomocí hello informace v [spravovat virtuální počítače ve Škálovací sadě virtuálního počítače](virtual-machine-scale-sets-windows-manage.md).</span><span class="sxs-lookup"><span data-stu-id="5da2a-237">Manage hello scale set that you just created using hello information in [Manage virtual machines in a Virtual Machine Scale Set](virtual-machine-scale-sets-windows-manage.md).</span></span>
* <span data-ttu-id="5da2a-238">Další informace o vertikálním škálování najdete v tématu [Vertikální automatické škálování se škálovacími sadami virtuálních počítačů](virtual-machine-scale-sets-vertical-scale-reprovision.md).</span><span class="sxs-lookup"><span data-stu-id="5da2a-238">Learn more about vertical scaling by reviewing [Vertical autoscale with Virtual Machine Scale sets](virtual-machine-scale-sets-vertical-scale-reprovision.md)</span></span>
* <span data-ttu-id="5da2a-239">Najít příklady Azure monitorování monitorování funkcí v [Azure PowerShell monitorování rychlý start ukázky](../monitoring-and-diagnostics/insights-powershell-samples.md)</span><span class="sxs-lookup"><span data-stu-id="5da2a-239">Find examples of Azure Monitor monitoring features in [Azure Monitor PowerShell quick start samples](../monitoring-and-diagnostics/insights-powershell-samples.md)</span></span>
* <span data-ttu-id="5da2a-240">Další informace o funkcích oznámení v [použití automatického škálování akce toosend e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="5da2a-240">Learn about notification features in [Use autoscale actions toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span></span>
* <span data-ttu-id="5da2a-241">Zjistěte, jak příliš[protokoly auditu použití toosend e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="5da2a-241">Learn how too[Use audit logs toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span></span>

