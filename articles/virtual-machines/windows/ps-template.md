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
# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a><span data-ttu-id="e370e-103">Vytvoření virtuálního počítače s Windows pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="e370e-103">Create a Windows virtual machine from a Resource Manager template</span></span>

<span data-ttu-id="e370e-104">Tento článek ukazuje, jak toodeploy Azure Resource Manageru šablony pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="e370e-104">This article shows you how toodeploy an Azure Resource Manager template using PowerShell.</span></span> <span data-ttu-id="e370e-105">Hello šablonu, kterou vytvoříte nasadí jednoho virtuálního počítače se systémem Windows Server v nové virtuální sítě s jedinou podsítí.</span><span class="sxs-lookup"><span data-stu-id="e370e-105">hello template that you create deploys a single virtual machine running Windows Server in a new virtual network with a single subnet.</span></span>

<span data-ttu-id="e370e-106">Podrobný popis hello prostředek virtuálního počítače naleznete v tématu [virtuálních počítačů v šablonu Azure Resource Manager](template-description.md).</span><span class="sxs-lookup"><span data-stu-id="e370e-106">For a detailed description of hello virtual machine resource, see [Virtual machines in an Azure Resource Manager template](template-description.md).</span></span> <span data-ttu-id="e370e-107">Další informace o všech hello prostředcích v šabloně najdete v tématu [názorný Průvodce šablonou Azure Resource Manager](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="e370e-107">For more information about all hello resources in a template, see [Azure Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

<span data-ttu-id="e370e-108">Má trvat přibližně pět minut, po které toodo hello kroky v tomto článku.</span><span class="sxs-lookup"><span data-stu-id="e370e-108">It should take about five minutes toodo hello steps in this article.</span></span>

## <a name="install-azure-powershell"></a><span data-ttu-id="e370e-109">Instalace prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="e370e-109">Install Azure PowerShell</span></span>

<span data-ttu-id="e370e-110">V tématu [jak tooinstall a konfigurace prostředí Azure PowerShell](../../powershell-install-configure.md) informace o instalaci hello nejnovější verzi prostředí Azure PowerShell, výběr předplatného a přihlášení tooyour účtu.</span><span class="sxs-lookup"><span data-stu-id="e370e-110">See [How tooinstall and configure Azure PowerShell](../../powershell-install-configure.md) for information about installing hello latest version of Azure PowerShell, selecting your subscription, and signing in tooyour account.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="e370e-111">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="e370e-111">Create a resource group</span></span>

<span data-ttu-id="e370e-112">Všechny prostředky musí být nasazený v [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e370e-112">All resources must be deployed in a [resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

1. <span data-ttu-id="e370e-113">Získejte seznamu dostupných umístění, kde je možné prostředky vytvořit.</span><span class="sxs-lookup"><span data-stu-id="e370e-113">Get a list of available locations where resources can be created.</span></span>
   
    ```powershell   
    Get-AzureRmLocation | sort DisplayName | Select DisplayName
    ```

2. <span data-ttu-id="e370e-114">Vytvořte skupinu prostředků hello v hello umístění, které vyberete.</span><span class="sxs-lookup"><span data-stu-id="e370e-114">Create hello resource group in hello location that you select.</span></span> <span data-ttu-id="e370e-115">Tento příklad ukazuje vytvoření hello skupinu prostředků s názvem **myResourceGroup** v hello **západní USA** umístění:</span><span class="sxs-lookup"><span data-stu-id="e370e-115">This example shows hello creation of a resource group named **myResourceGroup** in hello **West US** location:</span></span>

    ```powershell   
    New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
    ```

## <a name="create-hello-files"></a><span data-ttu-id="e370e-116">Vytvoření souborů hello</span><span class="sxs-lookup"><span data-stu-id="e370e-116">Create hello files</span></span>

<span data-ttu-id="e370e-117">V tomto kroku vytvoříte soubor šablony, která nasazuje hello prostředky a soubor parametrů, který poskytuje šablony toohello hodnoty parametru.</span><span class="sxs-lookup"><span data-stu-id="e370e-117">In this step, you create a template file that deploys hello resources and a parameters file that supplies parameter values toohello template.</span></span> <span data-ttu-id="e370e-118">Můžete také vytvořit soubor autorizace, který je použité tooperform Azure Resource Manager operace.</span><span class="sxs-lookup"><span data-stu-id="e370e-118">You also create an authorization file that is used tooperform Azure Resource Manager operations.</span></span>

1. <span data-ttu-id="e370e-119">Vytvořte soubor s názvem *CreateVMTemplate.json* a přidejte tento tooit kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="e370e-119">Create a file named *CreateVMTemplate.json* and add this JSON code tooit:</span></span>

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

2. <span data-ttu-id="e370e-120">Vytvořte soubor s názvem *Parameters.JSON tímto kódem* a přidejte tento tooit kódu JSON:</span><span class="sxs-lookup"><span data-stu-id="e370e-120">Create a file named *Parameters.json* and add this JSON code tooit:</span></span>

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

3. <span data-ttu-id="e370e-121">Vytvořte nový účet úložiště a kontejneru:</span><span class="sxs-lookup"><span data-stu-id="e370e-121">Create a new storage account and container:</span></span>

    ```powershell
    $storageName = "st" + (Get-Random)
    New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" -AccountName $storageName -Location "West US" -SkuName "Standard_LRS" -Kind Storage
    $accountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name $storageName).Value[0]
    $context = New-AzureStorageContext -StorageAccountName $storageName -StorageAccountKey $accountKey 
    New-AzureStorageContainer -Name "templates" -Context $context -Permission Container
    ```

4. <span data-ttu-id="e370e-122">Odešlete účet úložiště toohello hello soubory:</span><span class="sxs-lookup"><span data-stu-id="e370e-122">Upload hello files toohello storage account:</span></span>

    ```powershell
    Set-AzureStorageBlobContent -File "C:\templates\CreateVMTemplate.json" -Context $context -Container "templates"
    Set-AzureStorageBlobContent -File "C:\templates\Parameters.json" -Context $context -Container templates
    ```

    <span data-ttu-id="e370e-123">Změna hello - souboru cesty toohello umístění pro uložení souborů hello.</span><span class="sxs-lookup"><span data-stu-id="e370e-123">Change hello -File paths toohello location where you stored hello files.</span></span>

## <a name="create-hello-resources"></a><span data-ttu-id="e370e-124">Vytvořit prostředky hello</span><span class="sxs-lookup"><span data-stu-id="e370e-124">Create hello resources</span></span>

<span data-ttu-id="e370e-125">Nasazení šablony hello pomocí parametrů hello:</span><span class="sxs-lookup"><span data-stu-id="e370e-125">Deploy hello template using hello parameters:</span></span>

```powershell
$templatePath = "https://" + $storageName + ".blob.core.windows.net/templates/CreateVMTemplate.json"
$parametersPath = "https://" + $storageName + ".blob.core.windows.net/templates/Parameters.json"
New-AzureRmResourceGroupDeployment -ResourceGroupName "myResourceGroup" -Name "myDeployment" -TemplateUri $templatePath -TemplateParameterUri $parametersPath 
```

> [!NOTE]
> <span data-ttu-id="e370e-126">Můžete také nasadit parametry z místní soubory a šablony.</span><span class="sxs-lookup"><span data-stu-id="e370e-126">You can also deploy templates and parameters from local files.</span></span> <span data-ttu-id="e370e-127">Další, najdete v části toolearn [použití Azure Powershellu s Azure Storage](../../storage/common/storage-powershell-guide-full.md).</span><span class="sxs-lookup"><span data-stu-id="e370e-127">toolearn more, see [Using Azure PowerShell with Azure Storage](../../storage/common/storage-powershell-guide-full.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e370e-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="e370e-128">Next Steps</span></span>

- <span data-ttu-id="e370e-129">Pokud byly nějaké problémy s nasazením hello, může si prohlédněte [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](../../resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="e370e-129">If there were issues with hello deployment, you might take a look at [Troubleshoot common Azure deployment errors with Azure Resource Manager](../../resource-manager-common-deployment-errors.md).</span></span>
- <span data-ttu-id="e370e-130">Zjistěte, jak toocreate a spravovat virtuální počítač v [vytvořit a spravovat virtuální počítače Windows hello modul Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e370e-130">Learn how toocreate and manage a virtual machine in [Create and manage Windows VMs with hello Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

