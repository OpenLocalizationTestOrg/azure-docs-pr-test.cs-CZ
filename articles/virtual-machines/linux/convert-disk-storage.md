---
title: "Převést Azure spravované disky úložiště ze standardní, Premium a naopak | Microsoft Docs"
description: "Postup převedení Azure spravované disky úložiště ze standardní, Premium a naopak, pomocí rozhraní příkazového řádku Azure."
services: virtual-machines-linux
documentationcenter: 
author: ramankum
manager: kavithag
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: ramankum
ms.openlocfilehash: 0380b4aaa23b4aaba4c67d05e2d62f3ef41d6a32
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="convert-azure-managed-disks-storage-from-standard-to-premium-and-vice-versa"></a><span data-ttu-id="ae558-103">Převést Azure spravované disky úložiště ze standardní, Premium a naopak</span><span class="sxs-lookup"><span data-stu-id="ae558-103">Convert Azure managed disks storage from standard to premium, and vice versa</span></span>

<span data-ttu-id="ae558-104">Spravované disky nabízí dvě možnosti úložiště: [Premium](../../storage/storage-premium-storage.md) (založené na jednotku SSD) a [standardní](../../storage/storage-standard-storage.md) (založené na HDD).</span><span class="sxs-lookup"><span data-stu-id="ae558-104">Managed disks offers two storage options: [Premium](../../storage/storage-premium-storage.md) (SSD-based) and [Standard](../../storage/storage-standard-storage.md) (HDD-based).</span></span> <span data-ttu-id="ae558-105">Umožňuje snadno přepínat mezi dvě možnosti s minimálními výpadky podle vašim požadavkům na výkon.</span><span class="sxs-lookup"><span data-stu-id="ae558-105">It allows you to easily switch between the two options with minimal downtime based on your performance needs.</span></span> <span data-ttu-id="ae558-106">Tato funkce není k dispozici pro nespravovaná disky.</span><span class="sxs-lookup"><span data-stu-id="ae558-106">This capability is not available for unmanaged disks.</span></span> <span data-ttu-id="ae558-107">Ale můžete snadno [převést na spravované disky](convert-unmanaged-to-managed-disks.md) snadno přepínat mezi dvě možnosti.</span><span class="sxs-lookup"><span data-stu-id="ae558-107">But you can easily [convert to managed disks](convert-unmanaged-to-managed-disks.md) to easily switch between the two options.</span></span>

<span data-ttu-id="ae558-108">Tento článek ukazuje, jak převést spravované disky z standardní, Premium a naopak pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="ae558-108">This article shows you how to convert managed disks from standard to premium, and vice versa by using Azure CLI.</span></span> <span data-ttu-id="ae558-109">Pokud je potřeba nainstalovat nebo upgradovat najdete v tématu [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="ae558-109">If you need to install or upgrade it, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli.md).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="ae558-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="ae558-110">Before you begin</span></span>

* <span data-ttu-id="ae558-111">Převod vyžaduje restartování virtuálního počítače, takže naplánovat migraci úložiště disků existující období údržby.</span><span class="sxs-lookup"><span data-stu-id="ae558-111">The conversion requires a restart of the VM, so schedule the migration of your disks storage during a pre-existing maintenance window.</span></span> 
* <span data-ttu-id="ae558-112">Pokud používáte nespravované disky, nejprve [převést na spravované disky](convert-unmanaged-to-managed-disks.md) používat v tomto článku přepínat mezi dvě možnosti úložiště.</span><span class="sxs-lookup"><span data-stu-id="ae558-112">If you are using unmanaged disks, first [convert to managed disks](convert-unmanaged-to-managed-disks.md) to use this article to switch between the two storage options.</span></span> 


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium-and-vice-versa"></a><span data-ttu-id="ae558-113">Převeďte všechny spravované disky virtuálního počítače ze standardní premium a naopak</span><span class="sxs-lookup"><span data-stu-id="ae558-113">Convert all the managed disks of a VM from standard to premium, and vice versa</span></span>

<span data-ttu-id="ae558-114">V následujícím příkladu ukážeme, jak přepínat všechny disky virtuálního počítače z standard do úložiště úrovně premium.</span><span class="sxs-lookup"><span data-stu-id="ae558-114">In the following example, we show how to switch all the disks of a VM from standard to premium storage.</span></span> <span data-ttu-id="ae558-115">Spravované prémiové disky, virtuální počítač vyžaduje použití [velikost virtuálního počítače](sizes.md) který podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="ae558-115">To use premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="ae558-116">Tento příklad také přepne na velikost, která podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="ae558-116">This example also switches to a size that supports premium storage.</span></span>

 ```azurecli

#resource group that contains the virtual machine
rgName='yourResourceGroup'

#Name of the virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate the VM before changing the size of the VM
az vm deallocate --name $vmName --resource-group $rgName

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update the sku of all the data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update the sku of the OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="convert-a-managed-disk-from-standard-to-premium-and-vice-versa"></a><span data-ttu-id="ae558-117">Převést standardní spravovaných disků na premium a naopak</span><span class="sxs-lookup"><span data-stu-id="ae558-117">Convert a managed disk from standard to premium, and vice versa</span></span>

<span data-ttu-id="ae558-118">Pro vývoj/testování úlohy můžete mít směs standard a premium disky na snížení nákladů na vaše.</span><span class="sxs-lookup"><span data-stu-id="ae558-118">For your dev/test workload, you may want to have mixture of standard and premium disks to reduce your cost.</span></span> <span data-ttu-id="ae558-119">Můžete ji provést upgradem do úložiště úrovně premium, pouze disky, které vyžadují vyšší výkon.</span><span class="sxs-lookup"><span data-stu-id="ae558-119">You can accomplish it by upgrading to premium storage, only the disks that require better performance.</span></span> <span data-ttu-id="ae558-120">V následujícím příkladu ukážeme, jak přepínat jediný disk virtuálního počítače z standard do úložiště úrovně premium a naopak.</span><span class="sxs-lookup"><span data-stu-id="ae558-120">In the following example, we show how to switch a single disk of a VM from standard to premium storage, and vice versa.</span></span> <span data-ttu-id="ae558-121">Spravované prémiové disky, virtuální počítač vyžaduje použití [velikost virtuálního počítače](sizes.md) který podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="ae558-121">To use premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="ae558-122">Tento příklad také přepne na velikost, která podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="ae558-122">This example also switches to a size that supports premium storage.</span></span>

 ```azurecli

#resource group that contains the managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from standard to premium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get the parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate the VM before changing the size of the VM
az vm deallocate --ids $vmId 

#Change the VM size to a size that supports premium storage 
#Skip this step if converting storage from premium to standard
az vm resize --ids $vmId --size $size

# Update the sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="next-steps"></a><span data-ttu-id="ae558-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ae558-123">Next steps</span></span>

<span data-ttu-id="ae558-124">Využít jen pro čtení kopie virtuálního počítače pomocí [snímky](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="ae558-124">Take a read-only copy of a VM by using [snapshots](snapshot-copy-managed-disk.md).</span></span>

