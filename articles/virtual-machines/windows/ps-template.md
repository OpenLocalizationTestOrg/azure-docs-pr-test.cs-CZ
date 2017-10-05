---
title: "Vytvoření virtuálního počítače s Windows pomocí šablony v Azure | Microsoft Docs"
description: "Snadno vytvářet nový virtuální počítač s Windows pomocí šablony Resource Manageru a prostředí PowerShell."
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
ms.openlocfilehash: ddab80262fe27c1f5995858ec7de75d7c46df081
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a><span data-ttu-id="62134-103">Vytvoření virtuálního počítače s Windows pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="62134-103">Create a Windows virtual machine from a Resource Manager template</span></span>

<span data-ttu-id="62134-104">Tento článek ukazuje, jak nasadit šablonu Azure Resource Manager pomocí prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="62134-104">This article shows you how to deploy an Azure Resource Manager template using PowerShell.</span></span> <span data-ttu-id="62134-105">Šablona, kterou vytvoříte nasadí jednoho virtuálního počítače se systémem Windows Server v nové virtuální sítě s jedinou podsítí.</span><span class="sxs-lookup"><span data-stu-id="62134-105">The template that you create deploys a single virtual machine running Windows Server in a new virtual network with a single subnet.</span></span>

<span data-ttu-id="62134-106">Podrobný popis prostředek virtuálního počítače naleznete v tématu [virtuálních počítačů v šablonu Azure Resource Manager](template-description.md).</span><span class="sxs-lookup"><span data-stu-id="62134-106">For a detailed description of the virtual machine resource, see [Virtual machines in an Azure Resource Manager template](template-description.md).</span></span> <span data-ttu-id="62134-107">Další informace o všechny prostředky v šabloně najdete v tématu [názorný Průvodce šablonou Azure Resource Manager](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="62134-107">For more information about all the resources in a template, see [Azure Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

<span data-ttu-id="62134-108">Proveďte kroky v tomto článku má trvat asi pět minut.</span><span class="sxs-lookup"><span data-stu-id="62134-108">It should take about five minutes to do the steps in this article.</span></span>

## <a name="install-azure-powershell"></a><span data-ttu-id="62134-109">Instalace prostředí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="62134-109">Install Azure PowerShell</span></span>

<span data-ttu-id="62134-110">Projděte si článek [Jak nainstalovat a nakonfigurovat Azure PowerShell](../../powershell-install-configure.md), kde najdete informace o instalaci nejnovější verze prostředí Azure PowerShell, výběru předplatného a přihlášení k účtu.</span><span class="sxs-lookup"><span data-stu-id="62134-110">See [How to install and configure Azure PowerShell](../../powershell-install-configure.md) for information about installing the latest version of Azure PowerShell, selecting your subscription, and signing in to your account.</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="62134-111">Vytvoření skupiny prostředků</span><span class="sxs-lookup"><span data-stu-id="62134-111">Create a resource group</span></span>

<span data-ttu-id="62134-112">Všechny prostředky musí být nasazený v [skupiny prostředků](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="62134-112">All resources must be deployed in a [resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

1. <span data-ttu-id="62134-113">Získejte seznamu dostupných umístění, kde je možné prostředky vytvořit.</span><span class="sxs-lookup"><span data-stu-id="62134-113">Get a list of available locations where resources can be created.</span></span>
   
    ```powershell   
    Get-AzureRmLocation | sort DisplayName | Select DisplayName
    ```

2. <span data-ttu-id="62134-114">Vytvořte skupinu prostředků v místě, které vyberete.</span><span class="sxs-lookup"><span data-stu-id="62134-114">Create the resource group in the location that you select.</span></span> <span data-ttu-id="62134-115">Tento příklad ukazuje vytvoření skupiny prostředků s názvem **myResourceGroup** v **západní USA** umístění:</span><span class="sxs-lookup"><span data-stu-id="62134-115">This example shows the creation of a resource group named **myResourceGroup** in the **West US** location:</span></span>

    ```powershell   
    New-AzureRmResourceGroup -Name "myResourceGroup" -Location "West US"
    ```

## <a name="create-the-files"></a><span data-ttu-id="62134-116">Vytvoření souborů</span><span class="sxs-lookup"><span data-stu-id="62134-116">Create the files</span></span>

<span data-ttu-id="62134-117">V tomto kroku vytvoříte soubor šablony, která nasazuje prostředky a soubor parametrů, který poskytuje hodnoty parametrů šablony.</span><span class="sxs-lookup"><span data-stu-id="62134-117">In this step, you create a template file that deploys the resources and a parameters file that supplies parameter values to the template.</span></span> <span data-ttu-id="62134-118">Můžete také vytvořit soubor autorizace, který se používá k provádění operací Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="62134-118">You also create an authorization file that is used to perform Azure Resource Manager operations.</span></span>

1. <span data-ttu-id="62134-119">Vytvořte soubor s názvem *CreateVMTemplate.json* a přidejte do ní tento kód JSON:</span><span class="sxs-lookup"><span data-stu-id="62134-119">Create a file named *CreateVMTemplate.json* and add this JSON code to it:</span></span>

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

2. <span data-ttu-id="62134-120">Vytvořte soubor s názvem *Parameters.JSON tímto kódem* a přidejte do ní tento kód JSON:</span><span class="sxs-lookup"><span data-stu-id="62134-120">Create a file named *Parameters.json* and add this JSON code to it:</span></span>

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

3. <span data-ttu-id="62134-121">Vytvořte nový účet úložiště a kontejneru:</span><span class="sxs-lookup"><span data-stu-id="62134-121">Create a new storage account and container:</span></span>

    ```powershell
    $storageName = "st" + (Get-Random)
    New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" -AccountName $storageName -Location "West US" -SkuName "Standard_LRS" -Kind Storage
    $accountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName myResourceGroup -Name $storageName).Value[0]
    $context = New-AzureStorageContext -StorageAccountName $storageName -StorageAccountKey $accountKey 
    New-AzureStorageContainer -Name "templates" -Context $context -Permission Container
    ```

4. <span data-ttu-id="62134-122">Nahrání souborů do účtu úložiště:</span><span class="sxs-lookup"><span data-stu-id="62134-122">Upload the files to the storage account:</span></span>

    ```powershell
    Set-AzureStorageBlobContent -File "C:\templates\CreateVMTemplate.json" -Context $context -Container "templates"
    Set-AzureStorageBlobContent -File "C:\templates\Parameters.json" -Context $context -Container templates
    ```

    <span data-ttu-id="62134-123">Změna cesty k umístění, kam jste uložili soubory-souborům.</span><span class="sxs-lookup"><span data-stu-id="62134-123">Change the -File paths to the location where you stored the files.</span></span>

## <a name="create-the-resources"></a><span data-ttu-id="62134-124">Vytvořit prostředky</span><span class="sxs-lookup"><span data-stu-id="62134-124">Create the resources</span></span>

<span data-ttu-id="62134-125">Nasazení šablony pomocí parametrů:</span><span class="sxs-lookup"><span data-stu-id="62134-125">Deploy the template using the parameters:</span></span>

```powershell
$templatePath = "https://" + $storageName + ".blob.core.windows.net/templates/CreateVMTemplate.json"
$parametersPath = "https://" + $storageName + ".blob.core.windows.net/templates/Parameters.json"
New-AzureRmResourceGroupDeployment -ResourceGroupName "myResourceGroup" -Name "myDeployment" -TemplateUri $templatePath -TemplateParameterUri $parametersPath 
```

> [!NOTE]
> <span data-ttu-id="62134-126">Můžete také nasadit parametry z místní soubory a šablony.</span><span class="sxs-lookup"><span data-stu-id="62134-126">You can also deploy templates and parameters from local files.</span></span> <span data-ttu-id="62134-127">Další informace najdete v tématu [použití Azure Powershellu s Azure Storage](../../storage/common/storage-powershell-guide-full.md).</span><span class="sxs-lookup"><span data-stu-id="62134-127">To learn more, see [Using Azure PowerShell with Azure Storage](../../storage/common/storage-powershell-guide-full.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="62134-128">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62134-128">Next Steps</span></span>

- <span data-ttu-id="62134-129">Pokud byly nějaké problémy s nasazením, může si prohlédněte [odstraňování běžných chyb nasazení Azure pomocí Azure Resource Manageru](../../resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="62134-129">If there were issues with the deployment, you might take a look at [Troubleshoot common Azure deployment errors with Azure Resource Manager](../../resource-manager-common-deployment-errors.md).</span></span>
- <span data-ttu-id="62134-130">Naučte se vytvářet a spravovat virtuální počítač v [vytvořit a spravovat virtuální počítače Windows pomocí modulu Azure PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="62134-130">Learn how to create and manage a virtual machine in [Create and manage Windows VMs with the Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

