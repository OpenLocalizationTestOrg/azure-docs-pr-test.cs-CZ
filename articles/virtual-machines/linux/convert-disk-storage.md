---
title: "aaaConvert Azure spravované disky úložiště ze standardní toopremium a naopak | Microsoft Docs"
description: "Jak tooconvert Azure spravované disky úložiště ze standardní toopremium a naopak, pomocí rozhraní příkazového řádku Azure."
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
ms.openlocfilehash: 261d77202f7fd381085c4e25211a5d0026f43b01
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="convert-azure-managed-disks-storage-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="62721-103">Převést Azure spravované disky úložiště ze standardní toopremium a naopak</span><span class="sxs-lookup"><span data-stu-id="62721-103">Convert Azure managed disks storage from standard toopremium, and vice versa</span></span>

<span data-ttu-id="62721-104">Spravované disky nabízí dvě možnosti úložiště: [Premium](../../storage/storage-premium-storage.md) (založené na jednotku SSD) a [standardní](../../storage/storage-standard-storage.md) (založené na HDD).</span><span class="sxs-lookup"><span data-stu-id="62721-104">Managed disks offers two storage options: [Premium](../../storage/storage-premium-storage.md) (SSD-based) and [Standard](../../storage/storage-standard-storage.md) (HDD-based).</span></span> <span data-ttu-id="62721-105">Umožňuje vám tooeasily přepínat mezi hello dvě možnosti s minimálními výpadky podle vašim požadavkům na výkon.</span><span class="sxs-lookup"><span data-stu-id="62721-105">It allows you tooeasily switch between hello two options with minimal downtime based on your performance needs.</span></span> <span data-ttu-id="62721-106">Tato funkce není k dispozici pro nespravovaná disky.</span><span class="sxs-lookup"><span data-stu-id="62721-106">This capability is not available for unmanaged disks.</span></span> <span data-ttu-id="62721-107">Ale můžete snadno [převést disky toomanaged](convert-unmanaged-to-managed-disks.md) tooeasily přepínat mezi hello dvě možnosti.</span><span class="sxs-lookup"><span data-stu-id="62721-107">But you can easily [convert toomanaged disks](convert-unmanaged-to-managed-disks.md) tooeasily switch between hello two options.</span></span>

<span data-ttu-id="62721-108">Tento článek ukazuje, jak spravovat tooconvert disky z standardní toopremium a naopak pomocí rozhraní příkazového řádku Azure.</span><span class="sxs-lookup"><span data-stu-id="62721-108">This article shows you how tooconvert managed disks from standard toopremium, and vice versa by using Azure CLI.</span></span> <span data-ttu-id="62721-109">Pokud potřebujete tooinstall nebo ho upgradovat, přečtěte si téma [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="62721-109">If you need tooinstall or upgrade it, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli.md).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="62721-110">Než začnete</span><span class="sxs-lookup"><span data-stu-id="62721-110">Before you begin</span></span>

* <span data-ttu-id="62721-111">Hello převod vyžaduje restartování hello virtuálních počítačů, takže naplánovat hello migrace úložiště disků existující období údržby.</span><span class="sxs-lookup"><span data-stu-id="62721-111">hello conversion requires a restart of hello VM, so schedule hello migration of your disks storage during a pre-existing maintenance window.</span></span> 
* <span data-ttu-id="62721-112">Pokud používáte nespravované disky, nejprve [převést disky toomanaged](convert-unmanaged-to-managed-disks.md) toouse Tento článek tooswitch mezi hello dvě možnosti úložiště.</span><span class="sxs-lookup"><span data-stu-id="62721-112">If you are using unmanaged disks, first [convert toomanaged disks](convert-unmanaged-to-managed-disks.md) toouse this article tooswitch between hello two storage options.</span></span> 


## <a name="convert-all-hello-managed-disks-of-a-vm-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="62721-113">Převést všechny hello spravované disky virtuálního počítače z standardní toopremium a naopak</span><span class="sxs-lookup"><span data-stu-id="62721-113">Convert all hello managed disks of a VM from standard toopremium, and vice versa</span></span>

<span data-ttu-id="62721-114">V následujícím příkladu hello, ukážeme, jak tooswitch všechny disky virtuálního počítače z úložiště standard toopremium hello.</span><span class="sxs-lookup"><span data-stu-id="62721-114">In hello following example, we show how tooswitch all hello disks of a VM from standard toopremium storage.</span></span> <span data-ttu-id="62721-115">toouse premium spravované disky, musíte použít virtuální počítač [velikost virtuálního počítače](sizes.md) který podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="62721-115">toouse premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="62721-116">Tento příklad také přepínačů tooa velikost, která podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="62721-116">This example also switches tooa size that supports premium storage.</span></span>

 ```azurecli

#resource group that contains hello virtual machine
rgName='yourResourceGroup'

#Name of hello virtual machine
vmName='yourVM'

#Premium capable size 
#Required only if converting from standard toopremium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Deallocate hello VM before changing hello size of hello VM
az vm deallocate --name $vmName --resource-group $rgName

#Change hello VM size tooa size that supports premium storage 
#Skip this step if converting storage from premium toostandard
az vm resize --resource-group $rgName --name $vmName --size $size

#Update hello sku of all hello data disks 
az vm show -n $vmName -g $rgName --query storageProfile.dataDisks[*].managedDisk -o tsv \
 | awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

#Update hello sku of hello OS disk
az vm show -n $vmName -g $rgName --query storageProfile.osDisk.managedDisk -o tsv \
| awk -v sku=$sku '{system("az disk update --sku "sku" --ids "$1)}'

az vm start --name $vmName --resource-group $rgName

```
## <a name="convert-a-managed-disk-from-standard-toopremium-and-vice-versa"></a><span data-ttu-id="62721-117">Převést se spravovaným diskem z standardní toopremium a naopak</span><span class="sxs-lookup"><span data-stu-id="62721-117">Convert a managed disk from standard toopremium, and vice versa</span></span>

<span data-ttu-id="62721-118">Pro vývoj/testování úlohy můžete toohave směs standard a premium disky tooreduce vaše náklady.</span><span class="sxs-lookup"><span data-stu-id="62721-118">For your dev/test workload, you may want toohave mixture of standard and premium disks tooreduce your cost.</span></span> <span data-ttu-id="62721-119">Můžete ji provést upgradem toopremium úložiště pouze hello disky, které vyžadují vyšší výkon.</span><span class="sxs-lookup"><span data-stu-id="62721-119">You can accomplish it by upgrading toopremium storage, only hello disks that require better performance.</span></span> <span data-ttu-id="62721-120">V následujícím příkladu hello, ukážeme, jak tooswitch jediný disk virtuálního počítače z úložiště standard toopremium a naopak.</span><span class="sxs-lookup"><span data-stu-id="62721-120">In hello following example, we show how tooswitch a single disk of a VM from standard toopremium storage, and vice versa.</span></span> <span data-ttu-id="62721-121">toouse premium spravované disky, musíte použít virtuální počítač [velikost virtuálního počítače](sizes.md) který podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="62721-121">toouse premium managed disks, your VM must use a [VM size](sizes.md) that supports premium storage.</span></span> <span data-ttu-id="62721-122">Tento příklad také přepínačů tooa velikost, která podporuje službu premium storage.</span><span class="sxs-lookup"><span data-stu-id="62721-122">This example also switches tooa size that supports premium storage.</span></span>

 ```azurecli

#resource group that contains hello managed disk
rgName='yourResourceGroup'

#Name of your managed disk
diskName='yourManagedDiskName'

#Premium capable size 
#Required only if converting from standard toopremium
size='Standard_DS2_v2'

#Choose between Standard_LRS and Premium_LRS based on your scenario
sku='Premium_LRS'

#Get hello parent VM Id 
vmId=$(az disk show --name $diskName --resource-group $rgName --query managedBy --output tsv)

#Deallocate hello VM before changing hello size of hello VM
az vm deallocate --ids $vmId 

#Change hello VM size tooa size that supports premium storage 
#Skip this step if converting storage from premium toostandard
az vm resize --ids $vmId --size $size

# Update hello sku
az disk update --sku $sku --name $diskName --resource-group $rgName 

az vm start --ids $vmId 
```

## <a name="next-steps"></a><span data-ttu-id="62721-123">Další kroky</span><span class="sxs-lookup"><span data-stu-id="62721-123">Next steps</span></span>

<span data-ttu-id="62721-124">Využít jen pro čtení kopie virtuálního počítače pomocí [snímky](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="62721-124">Take a read-only copy of a VM by using [snapshots](snapshot-copy-managed-disk.md).</span></span>

