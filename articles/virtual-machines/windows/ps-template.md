---
title: "aaaCreate virtuální počítač s Windows pomocí šablony v Azure | Microsoft Docs"
description: "Pomocí šablony Resource Manageru a tooeasily prostředí PowerShell vytvořit nový virtuální počítač s Windows."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 19129d61-8c04-4aa9-a01f-361a09466805
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: davidmu
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 630111482c7dc046091632e2ed458ac143325d59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a>Vytvoření virtuálního počítače s Windows pomocí šablony Resource Manageru

Tento článek ukazuje, jak toodeploy Azure Resource Manageru šablony pomocí prostředí PowerShell. Hello šablonu, kterou vytvoříte nasadí jednoho virtuálního počítače se systémem Windows Server v nové virtuální sítě s jedinou podsítí.

Podrobný popis hello prostředek virtuálního počítače naleznete v tématu [virtuálních počítačů v šablonu Azure Resource Manager](template-description.md). Další informace o všech hello prostředcích v šabloně najdete v tématu [názorný Průvodce šablonou Azure Resource Manager](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Má trvat přibližně pět minut, po které toodo hello kroky v tomto článku.

## <a name="install-azure-powershell"></a>Instalace prostředí Azure PowerShell

V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](../../powershell-install-configure.md) informace o instalaci hello nejnovější verzi prostředí Azure PowerShell, výběr předplatného a přihlášení tooyour účtu.

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Všechny prostředky musí být nasazený v [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md).

1. Získejte seznamu dostupných umístění, kde je možné prostředky vytvořit.
   
    ```powershell   
    Get-AzureRmLocation | sort DisplayName | Select DisplayName
    ```

2. Vytvořte skupinu prostředků hello v hello umístění, které vyberete. Tento příklad ukazuje vytvoření hello skupinu prostředků s názvem **myResourceGroup** v hello **západní USA** umístění:

    ```powershell   
    New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
    ```

## <a name="create-hello-files"></a>Vytvoření souborů hello

V tomto kroku vytvoříte soubor šablony, která nasazuje hello prostředky a soubor parametrů, který poskytuje šablony toohello hodnoty parametru. Můžete také vytvořit soubor autorizace, který je použité tooperform Azure Resource Manager operace.

1. Vytvořte soubor s názvem *CreateVMTemplate.json* a přidejte tento tooit kódu JSON:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" }
      },
      "variables": {
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks','myVNet')]", 
        "subnetRef": "[concat(variables('vnetID'),'/subnets/mySubnet')]", 
      },
      "resources": [
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "myPublicIPAddress",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "myresourcegroupdns1"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "myVNet",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "mySubnet",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "myNic",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/publicIPAddresses/', 'myPublicIPAddress')]",
            "[resourceId('Microsoft.Network/virtualNetworks/', 'myVNet')]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": { "id": "[resourceId('Microsoft.Network/publicIPAddresses','myPublicIPAddress')]" },
                  "subnet": { "id": "[variables('subnetRef')]" }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-04-30-preview",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "myVM",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkInterfaces/', 'myNic')]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_DS1" },
            "osProfile": {
              "computerName": "myVM",
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
                "name": "myManagedOSDisk",
                "caching": "ReadWrite",
                "createOption": "FromImage"
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces','myNic')]"
                }
              ]
            }
          }
        }
      ]
    }
    ```

2. Vytvořte soubor s názvem *Parameters.JSON tímto kódem* a přidejte tento tooit kódu JSON:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
      "adminUserName": { "value": "azureuser" },
        "adminPassword": { "value": "Azure12345678" }
      }
    }
    ```

3. Vytvořte nový účet úložiště a kontejneru:

    ```powershell
    $storageName = "st" + (Get-Random)
    New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" -AccountName $storageName -Location "West US" -SkuName "Standard_LRS" -Kind Storage
    $accountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name $storageName).Value[0]
    $context = New-AzureStorageContext -StorageAccountName $storageName -StorageAccountKey $accountKey 
    New-AzureStorageContainer -Name "templates" -Context $context -Permission Container
    ```

4. Odešlete účet úložiště toohello hello soubory:

    ```powershell
    Set-AzureStorageBlobContent -File "C:\templates\CreateVMTemplate.json" -Context $context -Container "templates"
    Set-AzureStorageBlobContent -File "C:\templates\Parameters.json" -Context $context -Container templates
    ```

    Změna hello - souboru cesty toohello umístění pro uložení souborů hello.

## <a name="create-hello-resources"></a>Vytvořit prostředky hello

Nasazení šablony hello pomocí parametrů hello:

```powershell
$templatePath = "https://" + $storageName + ".blob.core.windows.net/templates/CreateVMTemplate.json"
$parametersPath = "https://" + $storageName + ".blob.core.windows.net/templates/Parameters.json"
New-AzureRmResourceGroupDeployment -ResourceGroupName "myResourceGroup" -Name "myDeployment" -TemplateUri $templatePath -TemplateParameterUri $parametersPath 
```

> [!NOTE]
> Můžete také nasadit parametry z místní soubory a šablony. Další, najdete v části toolearn [použití Azure Powershellu s Azure Storage](../../storage/common/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Další kroky

- Pokud byly nějaké problémy s nasazením hello, může si prohlédněte [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](../../resource-manager-common-deployment-errors.md).
- Zjistěte, jak toocreate a spravovat virtuální počítač v [vytvořit a spravovat virtuální počítače Windows hello modul Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

