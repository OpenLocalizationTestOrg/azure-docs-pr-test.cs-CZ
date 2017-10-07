---
title: "aaaAutoscale Škálovací sady virtuálních počítačů Linux | Microsoft Docs"
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
ms.openlocfilehash: 4352b94ec2973c37bc5616e3be25bd0c9442632b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-scale-linux-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="8d9c5-103">Automatické škálování počítače se systémem Linux v škálovací sadu virtuálních počítačů</span><span class="sxs-lookup"><span data-stu-id="8d9c5-103">Automatically scale Linux machines in a virtual machine scale set</span></span>
<span data-ttu-id="8d9c5-104">Sady škálování virtuálního počítače můžete snadno můžete toodeploy a spravovat identickými virtuálními počítači jako sada.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-104">Virtual machine scale sets make it easy for you toodeploy and manage identical virtual machines as a set.</span></span> <span data-ttu-id="8d9c5-105">Sady škálování pro aplikace hyperškálovatelný systém zajistit vysoce škálovatelného a přizpůsobitelné výpočetní vrstvy a podporují Image pro platformu Windows, Image platformy Linux, vlastních bitových kopií a rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-105">Scale sets provide a highly scalable and customizable compute layer for hyperscale applications, and they support Windows platform images, Linux platform images, custom images, and extensions.</span></span> <span data-ttu-id="8d9c5-106">Další, najdete v části toolearn [Přehled sady škálování virtuálních počítačů](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8d9c5-106">toolearn more, see [Virtual Machine Scale Sets Overview](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="8d9c5-107">Tento kurz ukazuje, jak nastavit toocreate škálování virtuálních počítačů Linux pomocí nejnovější verze Ubuntu Linux hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-107">This tutorial shows you how toocreate a scale set of Linux virtual machines using hello latest version of Ubuntu Linux.</span></span> <span data-ttu-id="8d9c5-108">Hello kurz také ukazuje, jak nastavit tooautomatically škálování hello počítačů v hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-108">hello tutorial also shows you how tooautomatically scale hello machines in hello set.</span></span> <span data-ttu-id="8d9c5-109">Můžete vytvořit hello škálování a vytvořit škálování vytvořením šablonu Azure Resource Manager a nasazení pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-109">You create hello scale set and set up scaling by creating an Azure Resource Manager template and deploying it using Azure CLI.</span></span> <span data-ttu-id="8d9c5-110">Další informace o šablonách najdete v tématu o [vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="8d9c5-110">For more information about templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span> <span data-ttu-id="8d9c5-111">toolearn Další informace o automatické škálování sad škálování, najdete v části [automatické škálování a sadách škálování virtuálního počítače](virtual-machine-scale-sets-autoscale-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8d9c5-111">toolearn more about automatic scaling of scale sets, see [Automatic scaling and Virtual Machine Scale Sets](virtual-machine-scale-sets-autoscale-overview.md).</span></span>

<span data-ttu-id="8d9c5-112">V tomto kurzu nasadíte následující hello prostředků a rozšíření:</span><span class="sxs-lookup"><span data-stu-id="8d9c5-112">In this tutorial, you deploy hello following resources and extensions:</span></span>

* <span data-ttu-id="8d9c5-113">Microsoft.Storage/storageAccounts.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-113">Microsoft.Storage/storageAccounts</span></span>
* <span data-ttu-id="8d9c5-114">Microsoft.Network/virtualNetworks</span><span class="sxs-lookup"><span data-stu-id="8d9c5-114">Microsoft.Network/virtualNetworks</span></span>
* <span data-ttu-id="8d9c5-115">Microsoft.Network/publicIPAddresses</span><span class="sxs-lookup"><span data-stu-id="8d9c5-115">Microsoft.Network/publicIPAddresses</span></span>
* <span data-ttu-id="8d9c5-116">Microsoft.Network/loadBalancers</span><span class="sxs-lookup"><span data-stu-id="8d9c5-116">Microsoft.Network/loadBalancers</span></span>
* <span data-ttu-id="8d9c5-117">Microsoft.Network/networkInterfaces</span><span class="sxs-lookup"><span data-stu-id="8d9c5-117">Microsoft.Network/networkInterfaces</span></span>
* <span data-ttu-id="8d9c5-118">Microsoft.Compute/virtualMachines</span><span class="sxs-lookup"><span data-stu-id="8d9c5-118">Microsoft.Compute/virtualMachines</span></span>
* <span data-ttu-id="8d9c5-119">Microsoft.Compute/virtualMachineScaleSets</span><span class="sxs-lookup"><span data-stu-id="8d9c5-119">Microsoft.Compute/virtualMachineScaleSets</span></span>
* <span data-ttu-id="8d9c5-120">Microsoft.Insights.VMDiagnosticsSettings</span><span class="sxs-lookup"><span data-stu-id="8d9c5-120">Microsoft.Insights.VMDiagnosticsSettings</span></span>
* <span data-ttu-id="8d9c5-121">Microsoft.Insights/autoscaleSettings</span><span class="sxs-lookup"><span data-stu-id="8d9c5-121">Microsoft.Insights/autoscaleSettings</span></span>

<span data-ttu-id="8d9c5-122">Další informace o prostředky Resource Manageru najdete v tématu [Azure Resource Manager oproti nasazení classic](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="8d9c5-122">For more information about Resource Manager resources, see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

<span data-ttu-id="8d9c5-123">Před zahájením práce s hello kroky v tomto kurzu [nainstalovat hello rozhraní příkazového řádku Azure](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="8d9c5-123">Before you get started with hello steps in this tutorial, [install hello Azure CLI](../cli-install-nodejs.md).</span></span>

## <a name="step-1-create-a-resource-group-and-a-storage-account"></a><span data-ttu-id="8d9c5-124">Krok 1: Vytvoření skupiny prostředků a účet úložiště</span><span class="sxs-lookup"><span data-stu-id="8d9c5-124">Step 1: Create a resource group and a storage account</span></span>

1. <span data-ttu-id="8d9c5-125">**Přihlaste se tooMicrosoft Azure**</span><span class="sxs-lookup"><span data-stu-id="8d9c5-125">**Sign in tooMicrosoft Azure**</span></span>  
<span data-ttu-id="8d9c5-126">Ve svém rozhraní příkazového řádku (Bash, terminálu, příkazového řádku), přepínače režimu tooResource Manager a potom [přihlásit pomocí id váš pracovní nebo školní](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Postupujte podle výzvy hello k tooyour prostředí interaktivní přihlašovací účet Azure.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-126">In your command-line interface (Bash, Terminal, Command prompt), switch tooResource Manager mode, and then [log in with your work or school id](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Follow hello prompts for an interactive login experience tooyour Azure account.</span></span>

    ```cli   
    azure config mode arm

    azure login
    ```
   
    > [!NOTE]
    > <span data-ttu-id="8d9c5-127">Pokud máte pracovní nebo školní ID a není povoleno dvoufaktorové ověřování, použijte `azure login -u` s ID toolog hello v bez interaktivní relace.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-127">If you have a work or school ID and you do not have two-factor authentication enabled, use `azure login -u` with hello ID toolog in without an interactive session.</span></span> <span data-ttu-id="8d9c5-128">Pokud nemáte pracovní nebo školní ID, můžete [vytvořit pracovní nebo školní id z vašeho osobního účtu Microsoft](../active-directory/active-directory-users-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8d9c5-128">If you don't have a work or school ID, you can [create a work or school id from your personal Microsoft account](../active-directory/active-directory-users-create-azure-portal.md).</span></span>
    
2. <span data-ttu-id="8d9c5-129">**Vytvořte skupinu prostředků**</span><span class="sxs-lookup"><span data-stu-id="8d9c5-129">**Create a resource group**</span></span>  
<span data-ttu-id="8d9c5-130">Všechny prostředky musí být nasazené tooa skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-130">All resources must be deployed tooa resource group.</span></span> <span data-ttu-id="8d9c5-131">V tomto kurzu, název skupiny prostředků hello **vmsstest1**.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-131">For this tutorial, name hello resource group **vmsstest1**.</span></span>
   
    ```cli
    azure group create vmsstestrg1 centralus
    ```

3. <span data-ttu-id="8d9c5-132">**Nasazení účtu úložiště do nové skupiny prostředků hello**</span><span class="sxs-lookup"><span data-stu-id="8d9c5-132">**Deploy a storage account into hello new resource group**</span></span>  
<span data-ttu-id="8d9c5-133">Tento účet úložiště se uloží hello šablony.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-133">This storage account is where hello template is stored.</span></span> <span data-ttu-id="8d9c5-134">Vytvořit účet úložiště s názvem **vmsstestsa**.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-134">Create a storage account named **vmsstestsa**.</span></span>
   
    ```cli
    azure storage account create -g vmsstestrg1 -l centralus --kind Storage --sku-name LRS vmsstestsa
    ```

## <a name="step-2-create-hello-template"></a><span data-ttu-id="8d9c5-135">Krok 2: Vytvoření šablony hello</span><span class="sxs-lookup"><span data-stu-id="8d9c5-135">Step 2: Create hello template</span></span>
<span data-ttu-id="8d9c5-136">Šablonu Azure Resource Manager umožňuje vám toodeploy a spravovat prostředky Azure společně s použitím popis JSON hello prostředků a parametry přidružené nasazení.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-136">An Azure Resource Manager template makes it possible for you toodeploy and manage Azure resources together by using a JSON description of hello resources and associated deployment parameters.</span></span>

1. <span data-ttu-id="8d9c5-137">Ve svém oblíbeném editoru vytvořte soubor hello VMSSTemplate.json a přidejte hello počáteční JSON struktura toosupport hello šablony.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-137">In your favorite editor, create hello file VMSSTemplate.json and add hello initial JSON structure toosupport hello template.</span></span>

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

2. <span data-ttu-id="8d9c5-138">Parametry nejsou vždy vyžaduje, ale poskytují způsob, jak tooinput hodnoty při nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-138">Parameters are not always required, but they provide a way tooinput values when hello template is deployed.</span></span> <span data-ttu-id="8d9c5-139">Přidejte tyto parametry v části hello parametry nadřazený element, zda jste přidali toohello šablony.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-139">Add these parameters under hello parameters parent element that you added toohello template.</span></span>

    ```json
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
   * <span data-ttu-id="8d9c5-140">Název hello samostatný virtuální počítač, který je použité tooaccess hello počítače ve škálovací sadě hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-140">A name for hello separate virtual machine that is used tooaccess hello machines in hello scale set.</span></span>
   * <span data-ttu-id="8d9c5-141">Název účtu úložiště hello se uloží hello šablony.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-141">A name for hello storage account where hello template is stored.</span></span>
   * <span data-ttu-id="8d9c5-142">Hello počet instancí virtuálních počítačů tooinitially vytvořit v sadě škálování hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-142">hello number of instances of virtual machines tooinitially create in hello scale set.</span></span>
   * <span data-ttu-id="8d9c5-143">Název a heslo účtu správce hello hello virtuálními počítači.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-143">A name and password of hello administrator account on hello virtual machines.</span></span>
   * <span data-ttu-id="8d9c5-144">Předpona názvu pro hello prostředky, které jsou vytvořené toosupport hello škálování nastavit.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-144">A name prefix for hello resources that are created toosupport hello scale set.</span></span>

3. <span data-ttu-id="8d9c5-145">Proměnné lze použít v hodnotách toospecify šablony, které se může často mění, nebo hodnoty, které je třeba toobe vytvořena z kombinace hodnot parametrů.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-145">Variables can be used in a template toospecify values that may change frequently or values that need toobe created from a combination of parameter values.</span></span> <span data-ttu-id="8d9c5-146">Přidejte tyto proměnné v části hello proměnné nadřazený element, zda jste přidali toohello šablony.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-146">Add these variables under hello variables parent element that you added toohello template.</span></span>

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

   * <span data-ttu-id="8d9c5-147">Názvy DNS, které jsou používány hello síťových rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-147">DNS names that are used by hello network interfaces.</span></span>
   * <span data-ttu-id="8d9c5-148">Hello IP adresy názvů a předpony pro hello virtuální sítě a podsítě.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-148">hello IP address names and prefixes for hello virtual network and subnets.</span></span>
   * <span data-ttu-id="8d9c5-149">Hello názvy a identifikátory hello virtuální sítě, zatížení vyrovnávání a síťových rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-149">hello names and identifiers of hello virtual network, load balancer, and network interfaces.</span></span>
   * <span data-ttu-id="8d9c5-150">Názvy účtů úložiště pro hello účty přidružené k hello počítače ve škálovací sadě hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-150">Storage account names for hello accounts associated with hello machines in hello scale set.</span></span>
   * <span data-ttu-id="8d9c5-151">Nastavení pro hello rozšíření diagnostiky, který je nainstalován na hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-151">Settings for hello Diagnostics extension that is installed on hello virtual machines.</span></span> <span data-ttu-id="8d9c5-152">Další informace o hello rozšíření diagnostiky najdete v tématu [vytvořit Windows virtuální počítač s monitorování a Diagnostika pomocí šablony Azure Resource Manageru](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8d9c5-152">For more information about hello Diagnostics extension, see [Create a Windows Virtual machine with monitoring and diagnostics using Azure Resource Manager Template](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

4. <span data-ttu-id="8d9c5-153">Přidáte prostředek účet úložiště hello pod hello prostředky nadřazeného elementu, zda jste přidali toohello šablony.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-153">Add hello storage account resource under hello resources parent element that you added toohello template.</span></span> <span data-ttu-id="8d9c5-154">Tato šablona používá hello toocreate smyčky doporučená pět účty úložiště, kde jsou uloženy hello disky operačního systému a diagnostických dat.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-154">This template uses a loop toocreate hello recommended five storage accounts where hello operating system disks and diagnostic data are stored.</span></span> <span data-ttu-id="8d9c5-155">Tuto sadu účtů může podporovat až too100 virtuální počítače ve škálovací sadě, což je maximální aktuální hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-155">This set of accounts can support up too100 virtual machines in a scale set, which is hello current maximum.</span></span> <span data-ttu-id="8d9c5-156">Každý účet úložiště je s názvem se písmeno označení, která byla definována v kombinaci s příponou hello, který zadáte v hello parametry šablony hello proměnné hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-156">Each storage account is named with a letter designator that was defined in hello variables combined with hello suffix that you provide in hello parameters for hello template.</span></span>
   
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

5. <span data-ttu-id="8d9c5-157">Přidáte prostředek hello virtuální sítě.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-157">Add hello virtual network resource.</span></span> <span data-ttu-id="8d9c5-158">Další informace najdete v tématu [poskytovatele síťových prostředků](../virtual-network/resource-groups-networking.md).</span><span class="sxs-lookup"><span data-stu-id="8d9c5-158">For more information, see [Network Resource Provider](../virtual-network/resource-groups-networking.md).</span></span>

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

6. <span data-ttu-id="8d9c5-159">Přidáte hello veřejnou IP adresu prostředky, které jsou používány hello zatížení vyrovnávání a síťové rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-159">Add hello public IP address resources that are used by hello load balancer and network interface.</span></span>

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

7. <span data-ttu-id="8d9c5-160">Přidejte hello prostředek pro vyrovnávání zatížení, který se používá škálovací sada hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-160">Add hello load balancer resource that is used by hello scale set.</span></span> <span data-ttu-id="8d9c5-161">Další informace najdete v tématu [podporu správce prostředků Azure pro nástroj pro vyrovnávání zatížení](../load-balancer/load-balancer-arm.md).</span><span class="sxs-lookup"><span data-stu-id="8d9c5-161">For more information, see [Azure Resource Manager Support for Load Balancer](../load-balancer/load-balancer-arm.md).</span></span>

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

8. <span data-ttu-id="8d9c5-162">Přidejte hello síťového rozhraní prostředku, který je používán hello samostatný virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-162">Add hello network interface resource that is used by hello separate virtual machine.</span></span> <span data-ttu-id="8d9c5-163">Protože počítače ve škálovací sadě nejsou přístupné přes veřejnou IP adresu, samostatný virtuální počítač se vytvoří v hello stejné virtuální sítě počítače hello tooremotely přístup.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-163">Because machines in a scale set aren't accessible through a public IP address, a separate virtual machine is created in hello same virtual network tooremotely access hello machines.</span></span>

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

9. <span data-ttu-id="8d9c5-164">Přidejte hello samostatný virtuální počítač do stejné sítě jako hello škálovací sadu hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-164">Add hello separate virtual machine in hello same network as hello scale set.</span></span>

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

10. <span data-ttu-id="8d9c5-165">Přidat sady škálování virtuálního počítače hello prostředků a zadejte hello rozšíření diagnostiky, který je nainstalován na všechny virtuální počítače ve škálovací sadě hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-165">Add hello virtual machine scale set resource and specify hello diagnostics extension that is installed on all virtual machines in hello scale set.</span></span> <span data-ttu-id="8d9c5-166">Mnoho hello nastavení pro tento prostředek je podobný s hello prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-166">Many of hello settings for this resource are similar with hello virtual machine resource.</span></span> <span data-ttu-id="8d9c5-167">Hlavní rozdíly Hello jsou hello kapacity element, který určuje hello počet virtuálních počítačů v sadě škálování hello a upgradePolicy, která určuje, jak jsou provedeny aktualizace toovirtual počítače.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-167">hello main differences are hello capacity element that specifies hello number of virtual machines in hello scale set and upgradePolicy that specifies how updates are made toovirtual machines.</span></span> <span data-ttu-id="8d9c5-168">Hello škálovací sadu vytvořen až všechny účty úložiště hello jsou vytvořeny jako zadaný hello dependsOn element.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-168">hello scale set is not created until all hello storage accounts are created as specified with hello dependsOn element.</span></span>

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

11. <span data-ttu-id="8d9c5-169">Přidejte hello autoscaleSettings prostředek, který definuje, jak upraví podle využití procesoru na počítačích hello sady hello hello škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-169">Add hello autoscaleSettings resource that defines how hello scale set adjusts based on processor usage on hello machines in hello set.</span></span>

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
    
    <span data-ttu-id="8d9c5-170">V tomto kurzu jsou důležité tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="8d9c5-170">For this tutorial, these values are important:</span></span>
    
    * <span data-ttu-id="8d9c5-171">**metricName**</span><span class="sxs-lookup"><span data-stu-id="8d9c5-171">**metricName**</span></span>  
    <span data-ttu-id="8d9c5-172">Tato hodnota je hello stejné jako hello čítačů výkonu, které jsme definovali hello wadperfcounter proměnné.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-172">This value is hello same as hello performance counter that we defined in hello wadperfcounter variable.</span></span> <span data-ttu-id="8d9c5-173">Pomocí této proměnné, hello rozšíření diagnostiky shromažďuje hello **Processor\PercentProcessorTime** čítače.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-173">Using that variable, hello Diagnostics extension collects hello **Processor\PercentProcessorTime** counter.</span></span>
    
    * <span data-ttu-id="8d9c5-174">**metricResourceUri**</span><span class="sxs-lookup"><span data-stu-id="8d9c5-174">**metricResourceUri**</span></span>  
    <span data-ttu-id="8d9c5-175">Tato hodnota je identifikátor prostředku hello škálovací sadu virtuálních počítačů hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-175">This value is hello resource identifier of hello virtual machine scale set.</span></span>
    
    * <span data-ttu-id="8d9c5-176">**časovými úseky**</span><span class="sxs-lookup"><span data-stu-id="8d9c5-176">**timeGrain**</span></span>  
    <span data-ttu-id="8d9c5-177">Tato hodnota je hello členitost hello metriky, které jsou shromážděny.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-177">This value is hello granularity of hello metrics that are collected.</span></span> <span data-ttu-id="8d9c5-178">V této šabloně je nastavit tooone minutu.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-178">In this template, it is set tooone minute.</span></span>
    
    * <span data-ttu-id="8d9c5-179">**statistiky**</span><span class="sxs-lookup"><span data-stu-id="8d9c5-179">**statistic**</span></span>  
    <span data-ttu-id="8d9c5-180">Tato hodnota určuje, jak hello metriky jsou kombinované tooaccommodate hello automatického škálování akce.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-180">This value determines how hello metrics are combined tooaccommodate hello automatic scaling action.</span></span> <span data-ttu-id="8d9c5-181">Hello možné hodnoty jsou: průměr, minimum, maximum.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-181">hello possible values are: Average, Min, Max.</span></span> <span data-ttu-id="8d9c5-182">V této šabloně se shromažďují hello průměrné celkové využití procesoru hello virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-182">In this template, hello average total CPU usage of hello virtual machines is collected.</span></span>

    * <span data-ttu-id="8d9c5-183">**Hodnota timeWindow**</span><span class="sxs-lookup"><span data-stu-id="8d9c5-183">**timeWindow**</span></span>  
    <span data-ttu-id="8d9c5-184">Tato hodnota je rozsah hello času, ve kterém se shromažďují instance data.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-184">This value is hello range of time in which instance data is collected.</span></span> <span data-ttu-id="8d9c5-185">Musí být mezi 5 minutami a 12 hodin.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-185">It must be between 5 minutes and 12 hours.</span></span>
    
    * <span data-ttu-id="8d9c5-186">**Agregace času**</span><span class="sxs-lookup"><span data-stu-id="8d9c5-186">**timeAggregation**</span></span>  
    <span data-ttu-id="8d9c5-187">jeho hodnota určuje, jak by měla být kombinovány hello data, která se shromažďují v čase.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-187">his value determines how hello data that is collected should be combined over time.</span></span> <span data-ttu-id="8d9c5-188">Hello výchozí hodnota je průměr.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-188">hello default value is Average.</span></span> <span data-ttu-id="8d9c5-189">Hello možné hodnoty jsou: průměr, minimální, maximální, poslední, celkový počet, počet.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-189">hello possible values are: Average, Minimum, Maximum, Last, Total, Count.</span></span>
    
    * <span data-ttu-id="8d9c5-190">**operátor**</span><span class="sxs-lookup"><span data-stu-id="8d9c5-190">**operator**</span></span>  
    <span data-ttu-id="8d9c5-191">Tato hodnota je hello operátor, který je použité toocompare hello metriky dat a hello prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-191">This value is hello operator that is used toocompare hello metric data and hello threshold.</span></span> <span data-ttu-id="8d9c5-192">Hello možné hodnoty jsou: rovná NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-192">hello possible values are: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span></span>
    
    * <span data-ttu-id="8d9c5-193">**Prahová hodnota**</span><span class="sxs-lookup"><span data-stu-id="8d9c5-193">**threshold**</span></span>  
    <span data-ttu-id="8d9c5-194">Tato hodnota se aktivuje hello akce škálování.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-194">This value triggers hello scale action.</span></span> <span data-ttu-id="8d9c5-195">V této šabloně se přidávají počítače sad po více než 50 % hello průměrné využití procesoru mezi počítači v sadě hello toohello škálování.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-195">In this template, machines are added toohello scale set when hello average CPU usage among machines in hello set is over 50%.</span></span>
    
    * <span data-ttu-id="8d9c5-196">**směr**</span><span class="sxs-lookup"><span data-stu-id="8d9c5-196">**direction**</span></span>  
    <span data-ttu-id="8d9c5-197">Tato hodnota určuje hello akce, která se provede, když je dosaženo hello prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-197">This value determines hello action that is taken when hello threshold value is achieved.</span></span> <span data-ttu-id="8d9c5-198">Hello možné hodnoty jsou zvýšení nebo snížení.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-198">hello possible values are Increase or Decrease.</span></span> <span data-ttu-id="8d9c5-199">V této šabloně hello počet virtuálních počítačů v sadě škálování hello se zvyšuje, když prahové hodnoty hello je více než 50 % hello definované časové okno.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-199">In this template, hello number of virtual machines in hello scale set is increased if hello threshold is over 50% in hello defined time window.</span></span>

    * <span data-ttu-id="8d9c5-200">**Typ**</span><span class="sxs-lookup"><span data-stu-id="8d9c5-200">**type**</span></span>  
    <span data-ttu-id="8d9c5-201">Tato hodnota je hello typ akce, který má vzniknout a musí být nastaven tooChangeCount.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-201">This value is hello type of action that should occur and must be set tooChangeCount.</span></span>
    
    * <span data-ttu-id="8d9c5-202">**Hodnota**</span><span class="sxs-lookup"><span data-stu-id="8d9c5-202">**value**</span></span>  
    <span data-ttu-id="8d9c5-203">Tato hodnota je hello počet virtuálních počítačů, které jsou přidány nebo odebrány z hello škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-203">This value is hello number of virtual machines that are added or removed from hello scale set.</span></span> <span data-ttu-id="8d9c5-204">Tato hodnota musí být 1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-204">This value must be 1 or greater.</span></span> <span data-ttu-id="8d9c5-205">Hello výchozí hodnota je 1.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-205">hello default value is 1.</span></span> <span data-ttu-id="8d9c5-206">V této šabloně hello počet počítačů v hello škálování nastavit zvýší o 1 při splnění hello prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-206">In this template, hello number of machines in hello scale set increases by 1 when hello threshold is met.</span></span>

    * <span data-ttu-id="8d9c5-207">**cooldown**</span><span class="sxs-lookup"><span data-stu-id="8d9c5-207">**cooldown**</span></span>  
    <span data-ttu-id="8d9c5-208">Tato hodnota je hello množství toowait času od poslední akce škálování hello předtím, než dojde k hello další akce.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-208">This value is hello amount of time toowait since hello last scaling action before hello next action occurs.</span></span> <span data-ttu-id="8d9c5-209">Tato hodnota musí být mezi minutu a jeden týden.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-209">This value must be between one minute and one week.</span></span>

12. <span data-ttu-id="8d9c5-210">Uložte soubor šablony hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-210">Save hello template file.</span></span>    

## <a name="step-3-upload-hello-template-toostorage"></a><span data-ttu-id="8d9c5-211">Krok 3: Nahrát na server hello šablony toostorage</span><span class="sxs-lookup"><span data-stu-id="8d9c5-211">Step 3: Upload hello template toostorage</span></span>
<span data-ttu-id="8d9c5-212">lze uložit šablonu Hello tak dlouho, dokud znáte název hello a primární klíč účtu úložiště hello, kterou jste vytvořili v kroku 1.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-212">hello template can be uploaded as long as you know hello name and primary key of hello storage account that you created in step 1.</span></span>

1. <span data-ttu-id="8d9c5-213">Ve svém rozhraní příkazového řádku (Bash, terminálu, příkazového řádku) spuštěním těchto příkazů proměnné prostředí hello tooset potřeby tooaccess hello účet úložiště:</span><span class="sxs-lookup"><span data-stu-id="8d9c5-213">In your command-line interface (Bash, Terminal, Command prompt), run these commands tooset hello environment variables needed tooaccess hello storage account:</span></span>

    ```cli   
    export AZURE_STORAGE_ACCOUNT={account_name}
    export AZURE_STORAGE_ACCESS_KEY={key}
    ```
    
    <span data-ttu-id="8d9c5-214">Hello klíč můžete získat kliknutím na ikonu klíče hello při zobrazení prostředků účtu úložiště hello hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-214">You can get hello key by clicking hello key icon when viewing hello storage account resource in hello Azure portal.</span></span> <span data-ttu-id="8d9c5-215">Při použití příkazového řádku Windows, zadejte **nastavit** místo exportu.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-215">When using a Windows command prompt, type **set** instead of export.</span></span>

2. <span data-ttu-id="8d9c5-216">Vytvoření kontejneru hello uložení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-216">Create hello container for storing hello template.</span></span>
   
    ```cli
    azure storage container create -p Blob templates
    ```

3. <span data-ttu-id="8d9c5-217">Nahrajte hello šablony souboru toohello nový kontejner.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-217">Upload hello template file toohello new container.</span></span>
   
    ```cli
    azure storage blob upload VMSSTemplate.json templates VMSSTemplate.json
    ```

## <a name="step-4-deploy-hello-template"></a><span data-ttu-id="8d9c5-218">Krok 4: Nasazení šablony hello</span><span class="sxs-lookup"><span data-stu-id="8d9c5-218">Step 4: Deploy hello template</span></span>
<span data-ttu-id="8d9c5-219">Teď, když jste vytvořili hello šablony, můžete začít nasazovat hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-219">Now that you created hello template, you can start deploying hello resources.</span></span> <span data-ttu-id="8d9c5-220">Použijte tento příkaz toostart hello proces:</span><span class="sxs-lookup"><span data-stu-id="8d9c5-220">Use this command toostart hello process:</span></span>

```cli
azure group deployment create --template-uri https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json vmsstestrg1 vmsstestdp1
```

<span data-ttu-id="8d9c5-221">Po stisknutí klávesy zadejte, jsou výzvami tooprovide hodnoty pro proměnné hello, které jste přiřadili.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-221">When you press enter, you are prompted tooprovide values for hello variables you assigned.</span></span> <span data-ttu-id="8d9c5-222">Zadejte tyto hodnoty:</span><span class="sxs-lookup"><span data-stu-id="8d9c5-222">Provide these values:</span></span>

```cli
vmName: vmsstestvm1
vmSSName: vmsstest1
instanceCount: 5
adminUserName: vmadmin1
adminPassword: VMpass1
resourcePrefix: vmsstest
```

<span data-ttu-id="8d9c5-223">Pro všechny prostředky toosuccessfully hello nasadit má trvat přibližně 15 minut.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-223">It should take about 15 minutes for all hello resources toosuccessfully be deployed.</span></span>

> [!NOTE]
> <span data-ttu-id="8d9c5-224">Můžete také použít portál hello možnost toodeploy hello prostředky.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-224">You can also use hello portal’s ability toodeploy hello resources.</span></span> <span data-ttu-id="8d9c5-225">Použít tento odkaz: https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template></span><span class="sxs-lookup"><span data-stu-id="8d9c5-225">Use this link: https://portal.azure.com/#create/Microsoft.Template/uri/<link tooVM Scale Set JSON template></span></span>


## <a name="step-5-monitor-resources"></a><span data-ttu-id="8d9c5-226">Krok 5: Sledování prostředků</span><span class="sxs-lookup"><span data-stu-id="8d9c5-226">Step 5: Monitor resources</span></span>
<span data-ttu-id="8d9c5-227">Můžete získat některé informace o sady škálování virtuálního počítače pomocí těchto metod:</span><span class="sxs-lookup"><span data-stu-id="8d9c5-227">You can get some information about virtual machine scale sets using these methods:</span></span>

* <span data-ttu-id="8d9c5-228">Hello portál Azure – nyní můžete získat omezené množství informací pomocí portálu hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-228">hello Azure portal - You can currently get a limited amount of information using hello portal.</span></span>

* <span data-ttu-id="8d9c5-229">Hello [Průzkumníka prostředků Azure](https://resources.azure.com/) – tento nástroj je nejlepší pro zkoumání hello aktuální stav škálovací sady hello.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-229">hello [Azure Resource Explorer](https://resources.azure.com/) - This tool is hello best for exploring hello current state of your scale set.</span></span> <span data-ttu-id="8d9c5-230">Postupujte podle této cestě a měli byste vidět sadu zobrazení instance hello hello měřítka, kterou jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="8d9c5-230">Follow this path and you should see hello instance view of hello scale set that you created:</span></span>
  
    ```cli
    subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines
    ```

* <span data-ttu-id="8d9c5-231">Rozhraní příkazového řádku Azure - použít tento příkaz tooget některé informace:</span><span class="sxs-lookup"><span data-stu-id="8d9c5-231">Azure CLI - Use this command tooget some information:</span></span>

    ```cli  
    azure resource show -n vmsstest1 -r Microsoft.Compute/virtualMachineScaleSets -o 2015-06-15 -g vmsstestrg1
    ```

* <span data-ttu-id="8d9c5-232">Připojte toohello jumpbox virtuálního počítače stejně, jako by všechny ostatní počítače a pak můžete vzdáleně přistupovat hello virtuálních počítačů v hello škálování sadu toomonitor jednotlivých procesů.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-232">Connect toohello jumpbox virtual machine just like you would any other machine and then you can remotely access hello virtual machines in hello scale set toomonitor individual processes.</span></span>

> [!NOTE]
> <span data-ttu-id="8d9c5-233">Dokončení rozhraní REST API k získání informací o sady škálování najdete v [sadách škálování virtuálního počítače](https://msdn.microsoft.com/library/mt589023.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d9c5-233">A complete REST API for obtaining information about scale sets can be found in [Virtual Machine Scale Sets](https://msdn.microsoft.com/library/mt589023.aspx).</span></span>

## <a name="step-6-remove-hello-resources"></a><span data-ttu-id="8d9c5-234">Krok 6: Odebrat prostředky hello</span><span class="sxs-lookup"><span data-stu-id="8d9c5-234">Step 6: Remove hello resources</span></span>
<span data-ttu-id="8d9c5-235">Vzhledem k tomu, že se vám účtovat prostředky využívané v Azure, je vždy prostředky toodelete osvědčených postupů, které už nejsou potřeba.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-235">Because you are charged for resources used in Azure, it is always a good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="8d9c5-236">Nepotřebujete toodelete každého prostředku nezávisle na skupinu prostředků.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-236">You don’t need toodelete each resource separately from a resource group.</span></span> <span data-ttu-id="8d9c5-237">Odstraněním skupiny prostředků hello a všechny její prostředky se automaticky odstraní.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-237">You can delete hello resource group and all its resources are automatically deleted.</span></span>

```cli
azure group delete vmsstestrg1
```

## <a name="next-steps"></a><span data-ttu-id="8d9c5-238">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8d9c5-238">Next steps</span></span>
* <span data-ttu-id="8d9c5-239">Najít příklady Azure monitorování monitorování funkcí v [Azure monitorování napříč platformami CLI rychlý start ukázky](../monitoring-and-diagnostics/insights-cli-samples.md)</span><span class="sxs-lookup"><span data-stu-id="8d9c5-239">Find examples of Azure Monitor monitoring features in [Azure Monitor Cross-platform CLI quick start samples](../monitoring-and-diagnostics/insights-cli-samples.md)</span></span>
* <span data-ttu-id="8d9c5-240">Další informace o funkcích oznámení v [použití automatického škálování akce toosend e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="8d9c5-240">Learn about notification features in [Use autoscale actions toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)</span></span>
* <span data-ttu-id="8d9c5-241">Zjistěte, jak příliš[protokoly auditu použití toosend e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="8d9c5-241">Learn how too[Use audit logs toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span></span>
* <span data-ttu-id="8d9c5-242">Podívejte se na hello [škálování ukázkovou aplikaci na Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) šablonu, která nastavuje Python nebo bottle aplikace tooexercise hello automatického škálování funkce sadách škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="8d9c5-242">Check out hello [Autoscale demo app on Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) template that sets up a Python/bottle app tooexercise hello automatic scaling functionality of Virtual Machine Scale Sets.</span></span>

