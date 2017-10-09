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
# <a name="create-a-vm-with-a-static-public-ip-address-using-an-azure-resource-manager-template"></a>Vytvoření virtuálního počítače se statickou veřejnou IP adresu pomocí šablony Azure Resource Manager

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Šablona](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (Classic)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md). Tento článek se zabývá pomocí modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello modelu nasazení classic.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-address-resources-in-a-template-file"></a>Prostředky veřejné adresy IP adresy v souboru šablony
Můžete zobrazit a stáhnout hello [Ukázka šablony](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).

Hello následující část popisuje hello Definice hello prostředek veřejné IP, podle hello scénář výše:

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

Všimněte si hello **publicIPAllocationMethod** vlastnosti, která je nastaven příliš*statické*. Tato vlastnost může být buď *dynamické* (výchozí hodnota) nebo *statické*. Nastavení toostatic záruky, které se nikdy změní hello veřejnou IP adresu přiřadit.

Hello následující část popisuje hello přidružení hello veřejné IP adresy síťové rozhraní:

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

Všimněte si hello **publicIPAddress** vlastnost odkazující toohello **Id** prostředku s názvem **variables('webVMSetting').pipName**. To je název hello hello prostředek veřejné IP uvedené výše.

Nakonec je hello síťové rozhraní výše uvedené v hello **networkProfile** vlastnost hello při vytváření virtuálního počítače.

```json
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }
```

## <a name="deploy-hello-template-by-using-click-toodeploy"></a>Nasazení šablony hello pomocí klikněte na tlačítko toodeploy

Hello Ukázka šablony k dispozici v úložišti na veřejné hello používá parametr souboru, který obsahuje hello výchozí hodnoty používané toogenerate hello scénář popsaný výše. toodeploy pomocí této šablony, klikněte na toodeploy, klikněte na tlačítko **nasazení tooAzure** v souboru Readme.md hello hello [virtuální počítač s statické PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) šablony. V případě potřeby nahradit hello výchozí hodnoty parametrů a zadejte hodnoty pro parametry prázdné hello.  Postupujte podle pokynů hello hello portálu toocreate virtuální počítač se statickou veřejnou IP adresu.

## <a name="deploy-hello-template-by-using-powershell"></a>Nasazení šablony hello pomocí prostředí PowerShell

toodeploy hello šablonu, kterou jste stáhli pomocí prostředí PowerShell, postupujte podle následujících kroků hello.

1. Pokud jste prostředí Azure PowerShell nikdy nepoužívali, dokončení hello kroky v hello [jak tooInstall a konfigurace prostředí Azure PowerShell](/powershell/azure/overview) článku.
2. V konzole PowerShell, spusťte hello `New-AzureRmResourceGroup` rutiny toocreate novou skupinu prostředků, v případě potřeby. Pokud už máte skupinu prostředků, který je vytvořen, přejděte toostep 3.

    ```powershell
    New-AzureRmResourceGroup -Name PIPTEST -Location westus
    ```

    Očekávaný výstup:
   
        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. V konzole PowerShell, spusťte hello `New-AzureRmResourceGroupDeployment` rutiny toodeploy hello šablony.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
        -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
        -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json
    ```

    Očekávaný výstup:
   
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

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a>Nasazení šablony hello pomocí hello rozhraní příkazového řádku Azure
Šablona hello toodeploy pomocí hello rozhraní příkazového řádku Azure, dokončení hello následující kroky:

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, postupujte podle kroků hello v hello [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) článek tooinstall a nakonfigurujte ji.
2. Spustit hello `azure config mode` příkaz tooswitch tooResource Manager režimu, jak je uvedeno níže.

    ```azurecli
    azure config mode arm
    ```

    Hello očekávaný výstup výše hello příkazu:

        info:    New mode is arm

3. Otevřete hello [soubor parametrů](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), vyberte její obsah a uložte ji tooa soubor v počítači. V tomto příkladu se uloží hello parametry tooa soubor s názvem *Parameters.JSON tímto kódem*. Ke změně hodnot parametrů hello v souboru hello v případě potřeby, ale minimálně, je doporučeno, změnit hodnotu hello hello adminPassword parametr tooa jedinečný a složité heslo.
4. Spustit hello `azure group deployment create` cmd toodeploy hello nové sítě VNet pomocí šablony hello a parametr soubory jste stáhli a upravili v předchozích krocích. V příkazu hello níže nahraďte <path> s cestou hello jste uložili soubor hello. 

    ```azurecli
    azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json
    ```

    Očekávaný výstup (uvádí parametr hodnoty používané):

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

