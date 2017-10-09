---
title: "aaaCreate skupin zabezpečení - sítě, šablony Azure Resource Manageru | Microsoft Docs"
description: "Zjistěte, jak toocreate a nasazení skupin zabezpečení sítě pomocí šablony Azure Resource Manager."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: f3e7385d-717c-44ff-be20-f9aa450aa99b
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3750168284fea7b41c8c0f908b0d31a9da5e38ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-network-security-groups-using-an-azure-resource-manager-template"></a><span data-ttu-id="2eaee-103">Vytvořit síť pomocí šablony Azure Resource Manager skupin zabezpečení</span><span class="sxs-lookup"><span data-stu-id="2eaee-103">Create network security groups using an Azure Resource Manager template</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="2eaee-104">Tento článek se týká modelu nasazení Resource Manager hello.</span><span class="sxs-lookup"><span data-stu-id="2eaee-104">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="2eaee-105">Můžete také [vytvářet skupiny Nsg v modelu nasazení classic hello](virtual-networks-create-nsg-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="2eaee-105">You can also [create NSGs in hello classic deployment model](virtual-networks-create-nsg-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

## <a name="nsg-resources-in-a-template-file"></a><span data-ttu-id="2eaee-106">Skupina NSG prostředky v souboru šablony</span><span class="sxs-lookup"><span data-stu-id="2eaee-106">NSG resources in a template file</span></span>
<span data-ttu-id="2eaee-107">Můžete zobrazit a stáhnout hello [Ukázka šablony](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).</span><span class="sxs-lookup"><span data-stu-id="2eaee-107">You can view and download hello [sample template](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).</span></span>

<span data-ttu-id="2eaee-108">Hello následující část popisuje hello Definice hello front-end NSG, založené na scénář hello.</span><span class="sxs-lookup"><span data-stu-id="2eaee-108">hello following section shows hello definition of hello front-end NSG, based on hello scenario.</span></span>

```json
"apiVersion": "2015-06-15",
"type": "Microsoft.Network/networkSecurityGroups",
"name": "[parameters('frontEndNSGName')]",
"location": "[resourceGroup().location]",
"tags": {
  "displayName": "NSG - Front End"
},
"properties": {
  "securityRules": [
    {
      "name": "rdp-rule",
      "properties": {
        "description": "Allow RDP",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "3389",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 100,
        "direction": "Inbound"
      }
    },
    {
      "name": "web-rule",
      "properties": {
        "description": "Allow WEB",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "destinationPortRange": "80",
        "sourceAddressPrefix": "Internet",
        "destinationAddressPrefix": "*",
        "access": "Allow",
        "priority": 101,
        "direction": "Inbound"
      }
    }
  ]
}
```
<span data-ttu-id="2eaee-109">tooassociate hello NSG toohello podsítě front-endu, máte toochange hello Definice podsítě v hello šablony a id odkazu hello použijte pro hello NSG.</span><span class="sxs-lookup"><span data-stu-id="2eaee-109">tooassociate hello NSG toohello front-end subnet, you have toochange hello subnet definition in hello template, and use hello reference id for hello NSG.</span></span>

```json
"subnets": [
  {
    "name": "[parameters('frontEndSubnetName')]",
    "properties": {
      "addressPrefix": "[parameters('frontEndSubnetPrefix')]",
      "networkSecurityGroup": {
      "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('frontEndNSGName'))]"
      }
    }
  }, 
```

<span data-ttu-id="2eaee-110">Všimněte si, hello stejné prováděná hello back-end NSG a hello back-end podsítě v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="2eaee-110">Notice hello same being done for hello back-end NSG and hello back-end subnet in hello template.</span></span>

## <a name="deploy-hello-arm-template-by-using-click-toodeploy"></a><span data-ttu-id="2eaee-111">Nasazení šablony ARM hello pomocí klikněte na tlačítko toodeploy</span><span class="sxs-lookup"><span data-stu-id="2eaee-111">Deploy hello ARM template by using click toodeploy</span></span>
<span data-ttu-id="2eaee-112">Hello Ukázka šablony k dispozici v úložišti na veřejné hello používá parametr souboru, který obsahuje hello výchozí hodnoty používané toogenerate hello scénář popsaný výše.</span><span class="sxs-lookup"><span data-stu-id="2eaee-112">hello sample template available in hello public repository uses a parameter file containing hello default values used toogenerate hello scenario described above.</span></span> <span data-ttu-id="2eaee-113">toodeploy pomocí této šablony, klikněte na toodeploy, postupujte podle [tento odkaz](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), klikněte na tlačítko **nasazení tooAzure**, nahraďte hello výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů hello hello portálu.</span><span class="sxs-lookup"><span data-stu-id="2eaee-113">toodeploy this template using click toodeploy, follow [this link](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

## <a name="deploy-hello-arm-template-by-using-powershell"></a><span data-ttu-id="2eaee-114">Nasazení šablony ARM hello pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="2eaee-114">Deploy hello ARM template by using PowerShell</span></span>
<span data-ttu-id="2eaee-115">šablony ARM hello toodeploy, které jste stáhli pomocí prostředí PowerShell, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="2eaee-115">toodeploy hello ARM template you downloaded by using PowerShell, follow hello steps below.</span></span>

1. <span data-ttu-id="2eaee-116">Pokud jste prostředí Azure PowerShell nikdy nepoužívali, postupujte podle pokynů hello v hello [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) tooinstall a nakonfigurujte ji.</span><span class="sxs-lookup"><span data-stu-id="2eaee-116">If you have never used Azure PowerShell, follow hello instructions in hello [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) tooinstall and configure it.</span></span>
2. <span data-ttu-id="2eaee-117">Spustit hello  **`New-AzureRmResourceGroup`**  hello rutiny toocreate skupinu prostředků pomocí šablony.</span><span class="sxs-lookup"><span data-stu-id="2eaee-117">Run hello **`New-AzureRmResourceGroup`** cmdlet toocreate a resource group using hello template.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location uswest `
    -TemplateFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' `
    -TemplateParameterFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    <span data-ttu-id="2eaee-118">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="2eaee-118">Expected output:</span></span>

        ResourceGroupName : TestRG
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  
   
        Resources         :
                            Name                Type                                     Location
                            ==================  =======================================  ========
                            sqlAvSet            Microsoft.Compute/availabilitySets       westus  
                            webAvSet            Microsoft.Compute/availabilitySets       westus  
                            SQL1                Microsoft.Compute/virtualMachines        westus  
                            SQL2                Microsoft.Compute/virtualMachines        westus  
                            Web1                Microsoft.Compute/virtualMachines        westus  
                            Web2                Microsoft.Compute/virtualMachines        westus  
                            TestNICSQL1         Microsoft.Network/networkInterfaces      westus  
                            TestNICSQL2         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb1         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb2         Microsoft.Network/networkInterfaces      westus  
                            NSG-BackEnd         Microsoft.Network/networkSecurityGroups  westus  
                            NSG-FrontEnd        Microsoft.Network/networkSecurityGroups  westus  
                            TestPIPSQL1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPSQL2         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb2         Microsoft.Network/publicIPAddresses      westus  
                            TestVNet            Microsoft.Network/virtualNetworks        westus  
                            testvnetstorageprm  Microsoft.Storage/storageAccounts        westus  
                            testvnetstoragestd  Microsoft.Storage/storageAccounts        westus  
   
        ResourceId        : /subscriptions/[Subscription Id]/resourceGroups/TestRG

## <a name="deploy-hello-arm-template-by-using-hello-azure-cli"></a><span data-ttu-id="2eaee-119">Nasazení šablony ARM hello pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="2eaee-119">Deploy hello ARM template by using hello Azure CLI</span></span>
<span data-ttu-id="2eaee-120">toodeploy hello šablony ARM pomocí rozhraní příkazového řádku Azure, hello postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="2eaee-120">toodeploy hello ARM template by using hello Azure CLI, follow hello steps below.</span></span>

1. <span data-ttu-id="2eaee-121">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.</span><span class="sxs-lookup"><span data-stu-id="2eaee-121">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="2eaee-122">Spustit hello  **`azure config mode`**  příkaz tooswitch tooResource Manager režimu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="2eaee-122">Run hello **`azure config mode`** command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="2eaee-123">Hello následuje hello očekávaný výstup hello příkazu:</span><span class="sxs-lookup"><span data-stu-id="2eaee-123">hello following is hello expected output for hello command:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="2eaee-124">Spustit hello  **`azure group deployment create`**  rutiny toodeploy hello nové sítě VNet pomocí šablony hello a parametr soubory jste stáhli a upravili v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="2eaee-124">Run hello **`azure group deployment create`** cmdlet toodeploy hello new VNet by using hello template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="2eaee-125">Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.</span><span class="sxs-lookup"><span data-stu-id="2eaee-125">hello list shown after hello output explains hello parameters used.</span></span>

    ```azurecli
    azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' -e 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    <span data-ttu-id="2eaee-126">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="2eaee-126">Expected output:</span></span>
   
        info:    Executing command group create
        info:    Getting resource group TestRG
        info:    Creating resource group TestRG
        info:    Created resource group TestRG
        info:    Initializing template configurations and parameters
        info:    Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:    
        info:    group create command OK
   
   * <span data-ttu-id="2eaee-127">**-n (nebo --name)**.</span><span class="sxs-lookup"><span data-stu-id="2eaee-127">**-n (or --name)**.</span></span> <span data-ttu-id="2eaee-128">Název toobe skupiny prostředků hello vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2eaee-128">Name of hello resource group toobe created.</span></span>
   * <span data-ttu-id="2eaee-129">**-l (nebo --location)**.</span><span class="sxs-lookup"><span data-stu-id="2eaee-129">**-l (or --location)**.</span></span> <span data-ttu-id="2eaee-130">Oblast Azure, kde bude vytvořena skupina prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="2eaee-130">Azure region where hello resource group will be created.</span></span>
   * <span data-ttu-id="2eaee-131">**-f (nebo --template-file)**.</span><span class="sxs-lookup"><span data-stu-id="2eaee-131">**-f (or --template-file)**.</span></span> <span data-ttu-id="2eaee-132">Cesta k souboru šablony ARM tooyour.</span><span class="sxs-lookup"><span data-stu-id="2eaee-132">Path tooyour ARM template file.</span></span>
   * <span data-ttu-id="2eaee-133">**-e (nebo --parameters-file)**.</span><span class="sxs-lookup"><span data-stu-id="2eaee-133">**-e (or --parameters-file)**.</span></span> <span data-ttu-id="2eaee-134">Cesta k souboru parametrů ARM tooyour.</span><span class="sxs-lookup"><span data-stu-id="2eaee-134">Path tooyour ARM parameters file.</span></span>

