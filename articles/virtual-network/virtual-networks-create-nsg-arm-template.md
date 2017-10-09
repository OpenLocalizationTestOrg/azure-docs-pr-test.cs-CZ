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
# <a name="create-network-security-groups-using-an-azure-resource-manager-template"></a>Vytvořit síť pomocí šablony Azure Resource Manager skupin zabezpečení

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Tento článek se týká modelu nasazení Resource Manager hello. Můžete také [vytvářet skupiny Nsg v modelu nasazení classic hello](virtual-networks-create-nsg-classic-ps.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

## <a name="nsg-resources-in-a-template-file"></a>Skupina NSG prostředky v souboru šablony
Můžete zobrazit a stáhnout hello [Ukázka šablony](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json).

Hello následující část popisuje hello Definice hello front-end NSG, založené na scénář hello.

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
tooassociate hello NSG toohello podsítě front-endu, máte toochange hello Definice podsítě v hello šablony a id odkazu hello použijte pro hello NSG.

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

Všimněte si, hello stejné prováděná hello back-end NSG a hello back-end podsítě v šabloně hello.

## <a name="deploy-hello-arm-template-by-using-click-toodeploy"></a>Nasazení šablony ARM hello pomocí klikněte na tlačítko toodeploy
Hello Ukázka šablony k dispozici v úložišti na veřejné hello používá parametr souboru, který obsahuje hello výchozí hodnoty používané toogenerate hello scénář popsaný výše. toodeploy pomocí této šablony, klikněte na toodeploy, postupujte podle [tento odkaz](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG), klikněte na tlačítko **nasazení tooAzure**, nahraďte hello výchozí hodnoty parametrů v případě potřeby a postupujte podle pokynů hello hello portálu.

## <a name="deploy-hello-arm-template-by-using-powershell"></a>Nasazení šablony ARM hello pomocí prostředí PowerShell
šablony ARM hello toodeploy, které jste stáhli pomocí prostředí PowerShell, postupujte podle následujících kroků hello.

1. Pokud jste prostředí Azure PowerShell nikdy nepoužívali, postupujte podle pokynů hello v hello [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) tooinstall a nakonfigurujte ji.
2. Spustit hello  **`New-AzureRmResourceGroup`**  hello rutiny toocreate skupinu prostředků pomocí šablony.

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location uswest `
    -TemplateFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' `
    -TemplateParameterFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    Očekávaný výstup:

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

## <a name="deploy-hello-arm-template-by-using-hello-azure-cli"></a>Nasazení šablony ARM hello pomocí hello rozhraní příkazového řádku Azure
toodeploy hello šablony ARM pomocí rozhraní příkazového řádku Azure, hello postupujte podle následujících kroků hello.

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.
2. Spustit hello  **`azure config mode`**  příkaz tooswitch tooResource Manager režimu, jak je uvedeno níže.

    ```azurecli
    azure config mode arm
    ```

    Hello následuje hello očekávaný výstup hello příkazu:

        info:    New mode is arm

3. Spustit hello  **`azure group deployment create`**  rutiny toodeploy hello nové sítě VNet pomocí šablony hello a parametr soubory jste stáhli a upravili v předchozích krocích. Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.

    ```azurecli
    azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' -e 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'
    ```

    Očekávaný výstup:
   
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
   
   * **-n (nebo --name)**. Název toobe skupiny prostředků hello vytvořili.
   * **-l (nebo --location)**. Oblast Azure, kde bude vytvořena skupina prostředků hello.
   * **-f (nebo --template-file)**. Cesta k souboru šablony ARM tooyour.
   * **-e (nebo --parameters-file)**. Cesta k souboru parametrů ARM tooyour.

