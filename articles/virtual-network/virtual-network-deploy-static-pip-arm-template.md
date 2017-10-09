---
title: "aaaCreate virtuální počítač s statickou veřejnou IP adresu - šablony Azure Resource Manageru | Microsoft Docs"
description: "Zjistěte, jak toocreate virtuální počítač s statické veřejné IP adresy pomocí šablony Azure Resource Manager."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d551085a-c7ed-4ec6-b4c3-e9e1cebb774c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6a8640ed4fad06b0e09820e6114fd6789db73847
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-an-azure-resource-manager-template"></a><span data-ttu-id="256d2-103">Vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí šablony Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="256d2-103">Create a VM with a static public IP address using an Azure Resource Manager template</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="256d2-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="256d2-104">Azure portal</span></span>](virtual-network-deploy-static-pip-arm-portal.md)
> * [<span data-ttu-id="256d2-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="256d2-105">PowerShell</span></span>](virtual-network-deploy-static-pip-arm-ps.md)
> * [<span data-ttu-id="256d2-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="256d2-106">Azure CLI</span></span>](virtual-network-deploy-static-pip-arm-cli.md)
> * [<span data-ttu-id="256d2-107">Šablona</span><span class="sxs-lookup"><span data-stu-id="256d2-107">Template</span></span>](virtual-network-deploy-static-pip-arm-template.md)
> * [<span data-ttu-id="256d2-108">PowerShell (Classic)</span><span class="sxs-lookup"><span data-stu-id="256d2-108">PowerShell (Classic)</span></span>](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="256d2-109">Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="256d2-109">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="256d2-110">Tento článek se zabývá pomocí modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello modelu nasazení classic.</span><span class="sxs-lookup"><span data-stu-id="256d2-110">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-address-resources-in-a-template-file"></a><span data-ttu-id="256d2-111">Prostředky veřejné adresy IP adresy v souboru šablony</span><span class="sxs-lookup"><span data-stu-id="256d2-111">Public IP address resources in a template file</span></span>
<span data-ttu-id="256d2-112">Můžete zobrazit a stáhnout hello [Ukázka šablony](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="256d2-112">You can view and download hello [sample template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).</span></span>

<span data-ttu-id="256d2-113">Hello následující část popisuje hello Definice hello prostředek veřejné IP, podle hello scénář výše:</span><span class="sxs-lookup"><span data-stu-id="256d2-113">hello following section shows hello definition of hello public IP resource, based on hello scenario above:</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('webVMSetting').pipName]",
  "location": "[variables('location')]",
  "properties": {
    "publicIPAllocationMethod": "Static"
  },
  "tags": {
    "displayName": "PublicIPAddress - Web"
  }
},
```

<span data-ttu-id="256d2-114">Všimněte si hello **publicIPAllocationMethod** vlastnosti, která je nastaven příliš*statické*.</span><span class="sxs-lookup"><span data-stu-id="256d2-114">Notice hello **publicIPAllocationMethod** property, which is set too*Static*.</span></span> <span data-ttu-id="256d2-115">Tato vlastnost může být buď *dynamické* (výchozí hodnota) nebo *statické*.</span><span class="sxs-lookup"><span data-stu-id="256d2-115">This property can be either *Dynamic* (default value) or *Static*.</span></span> <span data-ttu-id="256d2-116">Nastavení toostatic záruky, které se nikdy změní hello veřejnou IP adresu přiřadit.</span><span class="sxs-lookup"><span data-stu-id="256d2-116">Setting it toostatic guarantees that hello public IP address assigned will never change.</span></span>

<span data-ttu-id="256d2-117">Hello následující část popisuje hello přidružení hello veřejné IP adresy síťové rozhraní:</span><span class="sxs-lookup"><span data-stu-id="256d2-117">hello following section shows hello association of hello public IP address with a network interface:</span></span>

```json
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('webVMSetting').nicName]",
    "location": "[variables('location')]",
    "tags": {
    "displayName": "NetworkInterface - Web"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
      "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
    ],
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
          "privateIPAllocationMethod": "Static",
          "privateIPAddress": "[variables('webVMSetting').ipAddress]",
          "publicIPAddress": {
          "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
          },
          "subnet": {
            "id": "[variables('frontEndSubnetRef')]"
          }
        }
      }
    ]
  }
},
```

<span data-ttu-id="256d2-118">Všimněte si hello **publicIPAddress** vlastnost odkazující toohello **Id** prostředku s názvem **variables('webVMSetting').pipName**.</span><span class="sxs-lookup"><span data-stu-id="256d2-118">Notice hello **publicIPAddress** property pointing toohello **Id** of a resource named **variables('webVMSetting').pipName**.</span></span> <span data-ttu-id="256d2-119">To je název hello hello prostředek veřejné IP uvedené výše.</span><span class="sxs-lookup"><span data-stu-id="256d2-119">That is hello name of hello public IP resource shown above.</span></span>

<span data-ttu-id="256d2-120">Nakonec je hello síťové rozhraní výše uvedené v hello **networkProfile** vlastnost hello při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="256d2-120">Finally, hello network interface above is listed in hello **networkProfile** property of hello VM being created.</span></span>

```json
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }
```

## <a name="deploy-hello-template-by-using-click-toodeploy"></a><span data-ttu-id="256d2-121">Nasazení šablony hello pomocí klikněte na tlačítko toodeploy</span><span class="sxs-lookup"><span data-stu-id="256d2-121">Deploy hello template by using click toodeploy</span></span>

<span data-ttu-id="256d2-122">Hello Ukázka šablony k dispozici v úložišti na veřejné hello používá parametr souboru, který obsahuje hello výchozí hodnoty používané toogenerate hello scénář popsaný výše.</span><span class="sxs-lookup"><span data-stu-id="256d2-122">hello sample template available in hello public repository uses a parameter file containing hello default values used toogenerate hello scenario described above.</span></span> <span data-ttu-id="256d2-123">toodeploy pomocí této šablony, klikněte na toodeploy, klikněte na tlačítko **nasazení tooAzure** v souboru Readme.md hello hello [virtuální počítač s statické PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) šablony.</span><span class="sxs-lookup"><span data-stu-id="256d2-123">toodeploy this template using click toodeploy, click **Deploy tooAzure** in hello Readme.md file for hello [VM with static PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) template.</span></span> <span data-ttu-id="256d2-124">V případě potřeby nahradit hello výchozí hodnoty parametrů a zadejte hodnoty pro parametry prázdné hello.</span><span class="sxs-lookup"><span data-stu-id="256d2-124">Replace hello default parameter values if desired and enter values for hello blank parameters.</span></span>  <span data-ttu-id="256d2-125">Postupujte podle pokynů hello hello portálu toocreate virtuální počítač se statickou veřejnou IP adresu.</span><span class="sxs-lookup"><span data-stu-id="256d2-125">Follow hello instructions in hello portal toocreate a virtual machine with a static public IP address.</span></span>

## <a name="deploy-hello-template-by-using-powershell"></a><span data-ttu-id="256d2-126">Nasazení šablony hello pomocí prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="256d2-126">Deploy hello template by using PowerShell</span></span>

<span data-ttu-id="256d2-127">toodeploy hello šablonu, kterou jste stáhli pomocí prostředí PowerShell, postupujte podle následujících kroků hello.</span><span class="sxs-lookup"><span data-stu-id="256d2-127">toodeploy hello template you downloaded by using PowerShell, follow hello steps below.</span></span>

1. <span data-ttu-id="256d2-128">Pokud jste prostředí Azure PowerShell nikdy nepoužívali, dokončení hello kroky v hello [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.</span><span class="sxs-lookup"><span data-stu-id="256d2-128">If you have never used Azure PowerShell, complete hello steps in hello [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) article.</span></span>
2. <span data-ttu-id="256d2-129">V konzole PowerShell, spusťte hello `New-AzureRmResourceGroup` rutiny toocreate novou skupinu prostředků, v případě potřeby.</span><span class="sxs-lookup"><span data-stu-id="256d2-129">In a PowerShell console, run hello `New-AzureRmResourceGroup` cmdlet toocreate a new resource group, if necessary.</span></span> <span data-ttu-id="256d2-130">Pokud už máte skupinu prostředků, který je vytvořen, přejděte toostep 3.</span><span class="sxs-lookup"><span data-stu-id="256d2-130">If you already have a resource group created, go toostep 3.</span></span>

    ```powershell
    New-AzureRmResourceGroup -Name PIPTEST -Location westus
    ```

    <span data-ttu-id="256d2-131">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="256d2-131">Expected output:</span></span>
   
        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. <span data-ttu-id="256d2-132">V konzole PowerShell, spusťte hello `New-AzureRmResourceGroupDeployment` rutiny toodeploy hello šablony.</span><span class="sxs-lookup"><span data-stu-id="256d2-132">In a PowerShell console, run hello `New-AzureRmResourceGroupDeployment` cmdlet toodeploy hello template.</span></span>

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
        -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
        -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json
    ```

    <span data-ttu-id="256d2-133">Očekávaný výstup:</span><span class="sxs-lookup"><span data-stu-id="256d2-133">Expected output:</span></span>
   
        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0
   
        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         
   
        Outputs           :

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a><span data-ttu-id="256d2-134">Nasazení šablony hello pomocí hello rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="256d2-134">Deploy hello template by using hello Azure CLI</span></span>
<span data-ttu-id="256d2-135">Šablona hello toodeploy pomocí hello rozhraní příkazového řádku Azure, dokončení hello následující kroky:</span><span class="sxs-lookup"><span data-stu-id="256d2-135">toodeploy hello template by using hello Azure CLI, complete hello following steps:</span></span>

1. <span data-ttu-id="256d2-136">Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, postupujte podle kroků hello v hello [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) článek tooinstall a nakonfigurujte ji.</span><span class="sxs-lookup"><span data-stu-id="256d2-136">If you have never used Azure CLI, follow hello steps in hello [Install and Configure hello Azure CLI](../cli-install-nodejs.md) article tooinstall and configure it.</span></span>
2. <span data-ttu-id="256d2-137">Spustit hello `azure config mode` příkaz tooswitch tooResource Manager režimu, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="256d2-137">Run hello `azure config mode` command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="256d2-138">Hello očekávaný výstup výše hello příkazu:</span><span class="sxs-lookup"><span data-stu-id="256d2-138">hello expected output for hello command above:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="256d2-139">Otevřete hello [soubor parametrů](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), vyberte její obsah a uložte ji tooa soubor v počítači.</span><span class="sxs-lookup"><span data-stu-id="256d2-139">Open hello [parameter file](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), select its content, and save it tooa file in your computer.</span></span> <span data-ttu-id="256d2-140">V tomto příkladu se uloží hello parametry tooa soubor s názvem *Parameters.JSON tímto kódem*.</span><span class="sxs-lookup"><span data-stu-id="256d2-140">For this example, hello parameters are saved tooa file named *parameters.json*.</span></span> <span data-ttu-id="256d2-141">Ke změně hodnot parametrů hello v souboru hello v případě potřeby, ale minimálně, je doporučeno, změnit hodnotu hello hello adminPassword parametr tooa jedinečný a složité heslo.</span><span class="sxs-lookup"><span data-stu-id="256d2-141">Change hello parameter values within hello file if desired, but at a minimum, it's recommended that you change hello value for hello adminPassword parameter tooa unique, complex password.</span></span>
4. <span data-ttu-id="256d2-142">Spustit hello `azure group deployment create` cmd toodeploy hello nové sítě VNet pomocí šablony hello a parametr soubory jste stáhli a upravili v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="256d2-142">Run hello `azure group deployment create` cmd toodeploy hello new VNet by using hello template and parameter files you downloaded and modified above.</span></span> <span data-ttu-id="256d2-143">V příkazu hello níže nahraďte <path> s cestou hello jste uložili soubor hello.</span><span class="sxs-lookup"><span data-stu-id="256d2-143">In hello command below, replace <path> with hello path you saved hello file to.</span></span> 

    ```azurecli
    azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json
    ```

    <span data-ttu-id="256d2-144">Očekávaný výstup (uvádí parametr hodnoty používané):</span><span class="sxs-lookup"><span data-stu-id="256d2-144">Expected output (lists parameter values used):</span></span>

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/[Subscription ID]/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

