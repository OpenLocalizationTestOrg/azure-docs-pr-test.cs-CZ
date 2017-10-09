---
title: "aaaCreate virtuálního počítače s více síťovými kartami - šablony Azure Resource Manageru | Microsoft Docs"
description: "Vytvoření virtuálního počítače s více síťovými kartami pomocí šablony Azure Resource Manager."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 486f7dd5-cf2f-434c-85d1-b3e85c427def
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f5d9ffcbd40c72dc18ae6de38e739eb5e45bf669
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-a-template"></a>Vytvoření virtuálního počítače s více síťovými kartami pomocí šablony
[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> Azure má dva různé modely nasazení pro vytváření prostředků a práci s nimi: [Resource Manager a klasický model](../resource-manager-deployment-model.md).  Tento článek popisuje použití modelu nasazení Resource Manager hello, které společnost Microsoft doporučuje pro většinu nasazení nové místo hello [modelu nasazení classic](virtual-network-deploy-multinic-classic-ps.md).
> 

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Hello následující postup použijte skupinu prostředků s názvem *IaaSStory* pro hello webové servery a skupinu prostředků s názvem *IaaSStory back-end* pro servery hello DB.

## <a name="prerequisites"></a>Požadavky
Před vytvořením hello servery DB, musíte toocreate hello *IaaSStory* skupina prostředků se všechny hello potřebné prostředky pro tento scénář. dokončení těchto prostředků, toocreate hello následující kroky:

1. Přejděte příliš[stránku hello šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. V stránku hello šablony, toohello napravo od **nadřazené skupiny prostředků**, klikněte na tlačítko **nasazení tooAzure**.
3. V případě potřeby změňte hodnoty parametrů hello a potom postupujte podle kroků hello ve skupině prostředků hello toodeploy portálu Azure preview hello.

> [!IMPORTANT]
> Ujistěte se, že vaše názvy účtů úložiště jsou jedinečné. Názvy účtů úložiště duplicitní nemůže mít v Azure.
> 

## <a name="understand-hello-deployment-template"></a>Pochopení hello nasazení šablony
Před nasazením hello šablony aplikace této dokumentace, ujistěte se, že chápete, co dělá. Následující kroky Hello poskytují dobrý přehled o hello šablony:

1. Přejděte příliš[stránku hello šablony](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Klikněte na tlačítko **azuredeploy.json** soubor šablony tooopen hello.
3. Všimněte si hello *osType* parametr uvedené níže. Tento parametr je použité tooselect co toouse bitové kopie virtuálního počítače pro server databáze hello, společně s více operačního systému související nastavení.

    ```json
    "osType": {
      "type": "string",
      "defaultValue": "Windows",
      "allowedValues": [
        "Windows",
        "Ubuntu"
      ],
      "metadata": {
      "description": "Type of OS toouse for VMs: Windows or Ubuntu."
      }
    },
    ```

4. Posuňte se dolů toohello seznam proměnných a zkontrolujte hello Definice hello **dbVMSetting** proměnné, které jsou uvedeny níže. Jeden z elementů pole hello obsažené v hello obdrží **dbVMSettings** proměnné. Pokud jste obeznámeni s terminologie vývoj softwaru, můžete si prohlédnout hello **dbVMSettings** proměnné jako zatřiďovací tabulku nebo slovník.

    ```json
    "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"
    ```

5. Předpokládejme, že se že rozhodnete toodeploy Windows virtuální počítače se systémem SQL v hello back-end. Potom hello hodnotu **osType** by *Windows*a hello **dbVMSetting** proměnná by obsahovat element hello uvedené níže, který představuje první hodnota hello v hello **dbVMSettings** proměnné.

    ```json
    "Windows": {
      "vmSize": "Standard_DS3",
      "publisher": "MicrosoftSQLServer",
      "offer": "SQL2014SP1-WS2012R2",
      "sku": "Standard",
      "version": "latest",
      "vmName": "DB",
      "osdisk": "osdiskdb",
      "datadisk": "datadiskdb",
      "nicName": "NICDB",
      "ipAddress": "192.168.2.",
      "extensionDeployment": "",
      "avsetName": "ASDB",
      "remotePort": 3389,
      "dbPort": 1433
    },
    ```

6. Všimněte si hello **vmSize** obsahuje hodnotu hello *Standard_DS3*. Pouze určité velikosti virtuálních počítačů povolit pro použití hello několik síťových adaptérů. Můžete ověřit, které velikosti virtuálních počítačů podporují několik síťových adaptérů a čtení hello [velikosti virtuálních počítačů Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) a [velikosti virtuálního počítače s Linuxem](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) články.

7. Posuňte se dolů příliš**prostředky** a Všimněte si hello první prvek. Popisuje účet úložiště. Tento účet úložiště bude použité toomaintain hello datové disky používané každou databázi virtuálních počítačů. V tomto scénáři každou databázi virtuálních počítačů má disk s operačním systémem, který je uložen v pravidelných úložiště a dvě datové disky uložené v úložiště SSD (premium).

    ```json
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('prmStorageName')]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "Storage Account - Premium"
      },
      "properties": {
      "accountType": "[parameters('prmStorageType')]"
      }
    },
    ```

8. Posuňte se dolů toohello další prostředků, jak je uvedeno dále. Tento prostředek představuje hello síťový adaptér používá pro přístup k databázi v každé databázi virtuálních počítačů. Všimněte si použití hello hello **kopie** funkce v tento prostředek. Hello šablona vám umožní toodeploy jako hodně virtuálních počítačů tak, jak chcete, podle hello **dbCount** parametr. Proto je nutné toocreate hello stejnou úroveň síťové karty pro přístup k databázi, jednu pro každý virtuální počítač.

    ```json
    {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
    "location": "[variables('location')]",
    "tags": {
      "displayName": "NetworkInterfaces - DB DA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "privateIPAllocationMethod": "Static",
            "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
            "subnet": {
              "id": "[variables('backEndSubnetRef')]"
            }
          }
         }
       ] 
     }
    },
    ```

9. Posuňte se dolů toohello další prostředků, jak je uvedeno dále. Tento prostředek představuje hello síťový adaptér používá pro správu v každé databázi virtuálních počítačů. Znovu budete potřebovat jeden z tyto síťové adaptéry pro každou databázi virtuálních počítačů. Všimněte si hello **skupinu zabezpečení sítě** elementu propojení skupinu NSG, který umožňuje přístup toothis tooRDP/SSH seskupování jenom.

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "NetworkInterfaces - DB RA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "networkSecurityGroup": {
             "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
             },
             "privateIPAllocationMethod": "Static",
             "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
             "subnet": {
              "id": "[variables('backEndSubnetRef')]"
             }
           }
          }
        ]
      }
    },
```

10. Posuňte se dolů toohello další prostředků, jak je uvedeno dále. Tento prostředek představuje toobe sadu dostupnosti sdílí všechny virtuální počítače databáze. Tímto způsobem můžete zaručit, že bude vždy jeden virtuální počítač v hello nastavit během údržby.

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/availabilitySets",
      "name": "[variables('dbVMSetting').avsetName]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "AvailabilitySet - DB"
      }
    },
    ```

11. Posuňte se dolů toohello další prostředků. Tento prostředek představuje hello databáze virtuálních počítačů, jak je vidět v hello nejprve několika řádků, které jsou uvedeny níže. Všimněte si použití hello hello **kopie** znovu fungovat, zajistíte, že jsou vytvořeny víc virtuálních počítačů podle hello **dbCount** parametr. Všimněte si také hello **dependsOn** kolekce. Zobrazí se seznam dva síťové adaptéry je nutné toobe vytvořil před hello virtuální počítač nasazen, společně s hello skupinu dostupnosti a účet úložiště hello.

    ```json
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
    "location": "[variables('location')]",
    "dependsOn": [
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
      "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
      "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
    ],
    "tags": {
      "displayName": "VMs - DB"
    },
    "copy": {
      "name": "dbvmcount",
      "count": "[parameters('dbCount')]"
    },
    ```

12. Přejděte dolů v toohello prostředků virtuálních počítačů hello **networkProfile** elementu, jak je uvedeno dále. Všimněte si, že existují dva síťové adaptéry se referenční informace pro každý virtuální počítač. Když vytvoříte několik síťových adaptérů pro virtuální počítač, je nutné nastavit hello **primární** vlastnost hello síťové adaptéry příliš*true*, a příliš hello rest*false*.

    ```json
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
          "properties": { "primary": true }
        },
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
          "properties": { "primary": false }
        }
      ]
    }
    ```

## <a name="deploy-hello-arm-template-by-using-click-toodeploy"></a>Nasazení šablony ARM hello pomocí klikněte na tlačítko toodeploy

> [!IMPORTANT]
> Ujistěte se, postupujte hello [předpoklady](#Pre-requisites) kroky před hello pokynů níže.
> 

Hello Ukázka šablony k dispozici v úložišti na veřejné hello používá parametr souboru, který obsahuje hello výchozí hodnoty používané toogenerate hello scénář popsaný výše. toodeploy pomocí této šablony, klikněte na toodeploy, postupujte podle [tento odkaz](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), toohello napravo od **skupina prostředků back-end (viz dokumentace)** klikněte na tlačítko **nasazení tooAzure**, nahraďte výchozí hodnoty parametrů v případě potřeby Hello a postupujte podle pokynů hello hello portálu.

Hello obrázek níže ukazuje hello obsah hello novou skupinu prostředků, po nasazení.

![Skupina prostředků back-end](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-hello-template-by-using-powershell"></a>Nasazení šablony hello pomocí prostředí PowerShell
Šablona hello toodeploy jste stáhli pomocí prostředí PowerShell, instalace a konfigurace prostředí PowerShell pomocí kroků hello v hello [instalace a konfigurace prostředí PowerShell](/powershell/azure/overview) článek a pak dokončete hello následující kroky:

Spustit hello  **`New-AzureRmResourceGroup`**  hello rutiny toocreate skupinu prostředků pomocí šablony.

```powershell
New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
-TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'
```

Očekávaný výstup:

    ResourceGroupName : IaaSStory-Backend
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions  NotActions
                        =======  ==========
                        *
        Resources         :
                        Name                 Type                                 Location
                        ===================  ===================================  ========
                        ASDB                 Microsoft.Compute/availabilitySets   westus  
                        DB1                  Microsoft.Compute/virtualMachines    westus  
                        DB2                  Microsoft.Compute/virtualMachines    westus  
                        NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                        wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  
    ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a>Nasazení šablony hello pomocí hello rozhraní příkazového řádku Azure
šablony hello toodeploy pomocí hello rozhraní příkazového řádku Azure, postupujte podle kroků hello níže.

1. Pokud jste rozhraní příkazového řádku Azure nikdy nepoužívali, projděte si téma [instalace a konfigurace rozhraní příkazového řádku Azure hello](../cli-install-nodejs.md) a postupujte podle pokynů hello až toohello bodu, kde můžete vybrat svůj účet Azure a předplatné.
2. Spustit hello  **`azure config mode`**  příkaz tooswitch tooResource Manager režimu, jak je uvedeno níže.

    ```azurecli
    azure config mode arm
    ```

    Hello očekávaný výstup následovně:

        info:    New mode is arm

3. Otevřete hello [soubor parametrů](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), vyberte její obsah a uložte ji tooa soubor v počítači. V tomto příkladu jsme příliš uložili soubor parametrů hello*Parameters.JSON tímto kódem*.
4. Spustit hello  **`azure group deployment create`**  rutiny toodeploy hello nové sítě VNet pomocí šablony hello a parametr soubory jste stáhli a upravili v předchozích krocích. Hello seznam uvedený za výstup hello vysvětluje použité parametry hello.

    ```azurecli
    azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json
    ```

    Očekávaný výstup:
   
        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

