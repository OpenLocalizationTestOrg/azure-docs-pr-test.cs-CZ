---
title: "Pomocí spravovaného disků v šablonách Azure Resource Manager | Microsoft Docs"
description: "Podrobnosti o použití spravovaných misks v šablonách Azure Resource Manager"
services: storage
documentationcenter: 
author: jboeshart
manager: 
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/01/2017
ms.author: jaboes
ms.openlocfilehash: 4c502784a57850d6f11200e95f7432d2206e920a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="using-managed-disks-in-azure-resource-manager-templates"></a><span data-ttu-id="d5791-103">Pomocí spravovaného disky v šablonách Azure Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="d5791-103">Using Managed Disks in Azure Resource Manager Templates</span></span>

<span data-ttu-id="d5791-104">Tento dokument vás provede rozdíly mezi spravovanými a nespravovanými disky při zřizování virtuálních počítačů pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="d5791-104">This document walks through the differences between managed and unmanaged disks when using Azure Resource Manager templates to provision virtual machines.</span></span> <span data-ttu-id="d5791-105">To vám pomůže aktualizovat existující šablony, které používají disků nespravované na spravované disky.</span><span class="sxs-lookup"><span data-stu-id="d5791-105">This will help you to update existing templates that are using unmanaged Disks to managed disks.</span></span> <span data-ttu-id="d5791-106">Pro referenci používáme [101-vm jednoduché – windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) šablony jako vodítko.</span><span class="sxs-lookup"><span data-stu-id="d5791-106">For reference, we are using the [101-vm-simple-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) template as a guide.</span></span> <span data-ttu-id="d5791-107">Můžete zobrazit šablonu pomocí obou [discích spravovaných](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) a předchozí verze pomocí [nespravované disky](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) Pokud byste chtěli přímo jejich porovnání.</span><span class="sxs-lookup"><span data-stu-id="d5791-107">You can see the template using both [managed Disks](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) and a prior version using [unmanaged disks](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) if you'd like to directly compare them.</span></span>

## <a name="unmanaged-disks-template-formatting"></a><span data-ttu-id="d5791-108">Nespravované disky šablony, formátování</span><span class="sxs-lookup"><span data-stu-id="d5791-108">Unmanaged Disks template formatting</span></span>

<span data-ttu-id="d5791-109">Pokud chcete začít, budeme se podívejte na tom, jak nespravované disky jsou nasazeny.</span><span class="sxs-lookup"><span data-stu-id="d5791-109">To begin, we take a look at how unmanaged disks are deployed.</span></span> <span data-ttu-id="d5791-110">Při vytváření nespravované disky, potřebujete účet úložiště pro soubory virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="d5791-110">When creating unmanaged disks, you need a storage account to hold the VHD files.</span></span> <span data-ttu-id="d5791-111">Můžete vytvořit nový účet úložiště nebo použít jednu, která již existuje.</span><span class="sxs-lookup"><span data-stu-id="d5791-111">You can create a new storage account or use one that already exists.</span></span> <span data-ttu-id="d5791-112">Tento článek vám ukáže, jak vytvořit nový účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5791-112">This article will show you how to create a new storage account.</span></span> <span data-ttu-id="d5791-113">K tomu musíte prostředek účet úložiště v bloku prostředků, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="d5791-113">To accomplish this, you need a storage account resource in the resources block as shown below.</span></span>

```
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2016-01-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
}
```

<span data-ttu-id="d5791-114">V rámci objektu virtuálního počítače potřebujeme závislost na účtu úložiště a ověřte, že je vytvořen před virtuální počítač.</span><span class="sxs-lookup"><span data-stu-id="d5791-114">Within the virtual machine object, we need a dependency on the storage account to ensure that it's created before the virtual machine.</span></span> <span data-ttu-id="d5791-115">V rámci `storageProfile` části, jsme pak zadejte úplný identifikátor URI umístění virtuálního pevného disku, který odkazuje na účet úložiště a je potřebná pro disk operačního systému a všech datových disků.</span><span class="sxs-lookup"><span data-stu-id="d5791-115">Within the `storageProfile` section, we then specify the full URI of the VHD location, which references the storage account and is needed for the OS disk and any data disks.</span></span> 

```
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "name": "osdisk",
                "vhd": {
                    "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/osdisk.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "name": "datadisk1",
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "vhd": {
                        "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/datadisk1.vhd')]"
                    },
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

## <a name="managed-disks-template-formatting"></a><span data-ttu-id="d5791-116">Spravované disky šablony formátování</span><span class="sxs-lookup"><span data-stu-id="d5791-116">Managed disks template formatting</span></span>

<span data-ttu-id="d5791-117">S Azure spravované disky disk stane nejvyšší úrovně prostředků a už vyžaduje účet úložiště, který být vytvořený uživatelem.</span><span class="sxs-lookup"><span data-stu-id="d5791-117">With Azure Managed Disks, the disk becomes a top-level resource and no longer requires a storage account to be created by the user.</span></span> <span data-ttu-id="d5791-118">Spravované disky nejprve byly vystaveny v `2016-04-30-preview` verze rozhraní API, jsou k dispozici ve všech dalších verzích rozhraní API a se teď výchozí typ disku.</span><span class="sxs-lookup"><span data-stu-id="d5791-118">Managed disks were first exposed in the `2016-04-30-preview` API version, they are available in all subsequent API versions and are now the default disk type.</span></span> <span data-ttu-id="d5791-119">Následující části provede výchozí nastavení a jsou upřesněny postupy k dalšímu přizpůsobení vaše disky.</span><span class="sxs-lookup"><span data-stu-id="d5791-119">The following sections walk through the default settings and detail how to further customize your disks.</span></span>

> [!NOTE]
> <span data-ttu-id="d5791-120">Doporučujeme použít verzi rozhraní API pozdější než `2016-04-30-preview` jako došlo k narušující změny mezi `2016-04-30-preview` a `2017-03-30`.</span><span class="sxs-lookup"><span data-stu-id="d5791-120">It is recommended to use an API version later than `2016-04-30-preview` as there were breaking changes between `2016-04-30-preview` and `2017-03-30`.</span></span>
>
>

### <a name="default-managed-disk-settings"></a><span data-ttu-id="d5791-121">Výchozí nastavení spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="d5791-121">Default managed disk settings</span></span>

<span data-ttu-id="d5791-122">Pokud chcete vytvořit virtuální počítač s spravované disky, už musíte vytvořit úložiště účtu prostředků a následujícím způsobem můžete aktualizovat prostředek virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d5791-122">To create a VM with managed disks, you no longer need to create the storage account resource and can update your virtual machine resource as follows.</span></span> <span data-ttu-id="d5791-123">Specificky Upozorňujeme, že `apiVersion` odráží `2017-03-30` a `osDisk` a `dataDisks` nadále odkazovat na konkrétní identifikátoru URI virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="d5791-123">Specifically note that the `apiVersion` reflects `2017-03-30` and the `osDisk` and `dataDisks` no longer refer to a specific URI for the VHD.</span></span> <span data-ttu-id="d5791-124">Při nasazování bez zadání dalších vlastností, bude disk používat [úložiště LRS standardní](storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="d5791-124">When deploying without specifying additional properties, the disk will use [Standard LRS storage](storage-redundancy.md).</span></span> <span data-ttu-id="d5791-125">Pokud není zadán žádný název, jak dlouho trvá formát `<VMName>_OsDisk_1_<randomstring>` pro disk operačního systému a `<VMName>_disk<#>_<randomstring>` pro každý datový disk.</span><span class="sxs-lookup"><span data-stu-id="d5791-125">If no name is specified, it takes the format of `<VMName>_OsDisk_1_<randomstring>` for the OS disk and `<VMName>_disk<#>_<randomstring>` for each data disk.</span></span> <span data-ttu-id="d5791-126">Ve výchozím nastavení je Azure disk encryption zakázaný. ukládání do mezipaměti je pro čtení a zápis pro disk operačního systému a jeden pro datové disky.</span><span class="sxs-lookup"><span data-stu-id="d5791-126">By default, Azure disk encryption is disabled; caching is Read/Write for the OS disk and None for data disks.</span></span> <span data-ttu-id="d5791-127">Můžete si povšimnout v následujícím příkladu, že je stále závislost účtu úložiště, i když toto je pouze pro úložiště diagnostiky a není potřeba ukládání na disk.</span><span class="sxs-lookup"><span data-stu-id="d5791-127">You may notice in the example below there is still a storage account dependency, though this is only for storage of diagnostics and is not needed for disk storage.</span></span>

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="using-a-top-level-managed-disk-resource"></a><span data-ttu-id="d5791-128">Použití spravovaných disků na nejvyšší úrovni prostředků</span><span class="sxs-lookup"><span data-stu-id="d5791-128">Using a top-level managed disk resource</span></span>

<span data-ttu-id="d5791-129">Jako alternativu k určení konfiguraci disku v objektu virtuálního počítače můžete vytvořit nejvyšší úrovně diskový prostředek a připojte ji jako součást vytvoření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d5791-129">As an alternative to specifying the disk configuration in the virtual machine object, you can create a top-level disk resource and attach it as part of the virtual machine creation.</span></span> <span data-ttu-id="d5791-130">Například můžeme vytvořit prostředek disku takto chcete použít jako datový disk.</span><span class="sxs-lookup"><span data-stu-id="d5791-130">For example, we can create a disk resource as follows to use as a data disk.</span></span>

```
{
    "type": "Microsoft.Compute/disks",
    "name": "[concat(variables('vmName'),'-datadisk1')]",
    "apiVersion": "2017-03-30",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "properties": {
        "creationData": {
            "createOption": "Empty"
        },
        "diskSizeGB": 1023
    }
}
```

<span data-ttu-id="d5791-131">V rámci objekt virtuálního počítače jsme pak můžete odkazovat tento objekt disku má být přiřazena.</span><span class="sxs-lookup"><span data-stu-id="d5791-131">Within the VM object, we can then reference this disk object to be attached.</span></span> <span data-ttu-id="d5791-132">Určení Identifikátoru zdroje spravovaného disku jsme vytvořili v `managedDisk` vlastnost umožňuje připojení disku při vytváření virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="d5791-132">Specifying the resource ID of the managed disk we created in the `managedDisk` property allows the attachment of the disk as the VM is created.</span></span> <span data-ttu-id="d5791-133">Všimněte si, že `apiVersion` pro virtuální počítač zdroj je nastaven pro `2017-03-30`.</span><span class="sxs-lookup"><span data-stu-id="d5791-133">Note that the `apiVersion` for the VM resource is set to `2017-03-30`.</span></span> <span data-ttu-id="d5791-134">Všimněte si také, že vytvořili jsme závislost na prostředku disku, ujistěte se, že je úspěšně vytvořen před vytvořením virtuálních počítačů.</span><span class="sxs-lookup"><span data-stu-id="d5791-134">Also note that we've created a dependency on the disk resource to ensure it's successfully created before VM creation.</span></span> 

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]",
        "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "lun": 0,
                    "name": "[concat(variables('vmName'),'-datadisk1')]",
                    "createOption": "attach",
                    "managedDisk": {
                        "id": "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
                    }
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="create-managed-availability-sets-with-vms-using-managed-disks"></a><span data-ttu-id="d5791-135">Vytvoření skupiny dostupnosti spravované s virtuálními počítači pomocí spravovaných disků</span><span class="sxs-lookup"><span data-stu-id="d5791-135">Create managed availability sets with VMs using managed disks</span></span>

<span data-ttu-id="d5791-136">K vytvoření spravovaného dostupnost sady s virtuálními počítači pomocí spravovaných disků, přidejte `sku` objektu na dostupnost prostředků a nastavte a `name` vlastnost `Aligned`.</span><span class="sxs-lookup"><span data-stu-id="d5791-136">To create managed availability sets with VMs using managed disks, add the `sku` object to the availability set resource and set the `name` property to `Aligned`.</span></span> <span data-ttu-id="d5791-137">Tím se zajistí disky pro jednotlivé virtuální počítače dostatečně od sebe navzájem oddělené k Vyhýbejte se jediným bodů selhání.</span><span class="sxs-lookup"><span data-stu-id="d5791-137">This ensures that the disks for each VM are sufficiently isolated from each other to avoid single points of failure.</span></span> <span data-ttu-id="d5791-138">Všimněte si také, že `apiVersion` pro dostupnost nastavení prostředku je nastaven na `2017-03-30`.</span><span class="sxs-lookup"><span data-stu-id="d5791-138">Also note that the `apiVersion` for the availability set resource is set to `2017-03-30`.</span></span>

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/availabilitySets",
    "location": "[resourceGroup().location]",
    "name": "[variables('avSetName')]",
    "properties": {
        "PlatformUpdateDomainCount": 3,
        "PlatformFaultDomainCount": 2
    },
    "sku": {
        "name": "Aligned"
    }
}
```

### <a name="additional-scenarios-and-customizations"></a><span data-ttu-id="d5791-139">Další scénáře a přizpůsobení</span><span class="sxs-lookup"><span data-stu-id="d5791-139">Additional scenarios and customizations</span></span>

<span data-ttu-id="d5791-140">K vyhledání úplné informace o specifikacích REST API, přečtěte si [vytvoření spravovaného disku dokumentace k REST API](/rest/api/manageddisks/disks/disks-create-or-update).</span><span class="sxs-lookup"><span data-stu-id="d5791-140">To find full information on the REST API specifications, please review the [create a managed disk REST API documentation](/rest/api/manageddisks/disks/disks-create-or-update).</span></span> <span data-ttu-id="d5791-141">Zjistíte další scénáře, a také výchozí a přijatelných hodnot, které se dají odeslat do rozhraní API prostřednictvím šablony nasazení.</span><span class="sxs-lookup"><span data-stu-id="d5791-141">You will find additional scenarios, as well as default and acceptable values that can be submitted to the API through template deployments.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="d5791-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d5791-142">Next steps</span></span>

* <span data-ttu-id="d5791-143">Pro úplnou šablony, které používají spravovaný disky získáte pomocí následujících odkazů v úložišti Azure rychlý start.</span><span class="sxs-lookup"><span data-stu-id="d5791-143">For full templates that use managed disks visit the following Azure Quickstart Repo links.</span></span>
    * [<span data-ttu-id="d5791-144">Virtuální počítač s Windows spravované disku</span><span class="sxs-lookup"><span data-stu-id="d5791-144">Windows VM with managed disk</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
    * [<span data-ttu-id="d5791-145">Virtuální počítač s Linuxem pomocí spravovaného disku</span><span class="sxs-lookup"><span data-stu-id="d5791-145">Linux VM with managed disk</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
    * [<span data-ttu-id="d5791-146">Úplný seznam spravovaných disků na šablony</span><span class="sxs-lookup"><span data-stu-id="d5791-146">Full list of managed disk templates</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* <span data-ttu-id="d5791-147">Přejděte [přehled disky spravované Azure](storage-managed-disks-overview.md) dokumentu další informace o spravovaných disky.</span><span class="sxs-lookup"><span data-stu-id="d5791-147">Visit the [Azure Managed Disks Overview](storage-managed-disks-overview.md) document to learn more about managed disks.</span></span>
* <span data-ttu-id="d5791-148">Zkontrolujte šablonu referenční dokumentaci pro prostředky virtuálního počítače navštivte stránky [odkaz na šablonu Microsoft.Compute/virtualMachines](/templates/microsoft.compute/virtualmachines) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d5791-148">Review the template reference documentation for virtual machine resources by visiting the [Microsoft.Compute/virtualMachines template reference](/templates/microsoft.compute/virtualmachines) document.</span></span>
* <span data-ttu-id="d5791-149">Zkontrolujte šablonu referenční dokumentaci pro prostředky disku navštivte stránky [odkaz na šablonu Microsoft.Compute/disks](/templates/microsoft.compute/disks) dokumentu.</span><span class="sxs-lookup"><span data-stu-id="d5791-149">Review the template reference documentation for disk resources by visiting the [Microsoft.Compute/disks template reference](/templates/microsoft.compute/disks) document.</span></span>
 
