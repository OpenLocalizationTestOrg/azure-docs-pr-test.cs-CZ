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
# <a name="convert-azure-managed-disks-storage-from-standard-toopremium-and-vice-versa"></a>Převést Azure spravované disky úložiště ze standardní toopremium a naopak

Spravované disky nabízí dvě možnosti úložiště: [Premium](../../storage/storage-premium-storage.md) (založené na jednotku SSD) a [standardní](../../storage/storage-standard-storage.md) (založené na HDD). Umožňuje vám tooeasily přepínat mezi hello dvě možnosti s minimálními výpadky podle vašim požadavkům na výkon. Tato funkce není k dispozici pro nespravovaná disky. Ale můžete snadno [převést disky toomanaged](convert-unmanaged-to-managed-disks.md) tooeasily přepínat mezi hello dvě možnosti.

Tento článek ukazuje, jak spravovat tooconvert disky z standardní toopremium a naopak pomocí rozhraní příkazového řádku Azure. Pokud potřebujete tooinstall nebo ho upgradovat, přečtěte si téma [nainstalovat Azure CLI 2.0](/cli/azure/install-azure-cli.md). 

## <a name="before-you-begin"></a>Než začnete

* Hello převod vyžaduje restartování hello virtuálních počítačů, takže naplánovat hello migrace úložiště disků existující období údržby. 
* Pokud používáte nespravované disky, nejprve [převést disky toomanaged](convert-unmanaged-to-managed-disks.md) toouse Tento článek tooswitch mezi hello dvě možnosti úložiště. 


## <a name="convert-all-hello-managed-disks-of-a-vm-from-standard-toopremium-and-vice-versa"></a>Převést všechny hello spravované disky virtuálního počítače z standardní toopremium a naopak

V následujícím příkladu hello, ukážeme, jak tooswitch všechny disky virtuálního počítače z úložiště standard toopremium hello. toouse premium spravované disky, musíte použít virtuální počítač [velikost virtuálního počítače](sizes.md) který podporuje službu premium storage. Tento příklad také přepínačů tooa velikost, která podporuje službu premium storage.

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
## <a name="convert-a-managed-disk-from-standard-toopremium-and-vice-versa"></a>Převést se spravovaným diskem z standardní toopremium a naopak

Pro vývoj/testování úlohy můžete toohave směs standard a premium disky tooreduce vaše náklady. Můžete ji provést upgradem toopremium úložiště pouze hello disky, které vyžadují vyšší výkon. V následujícím příkladu hello, ukážeme, jak tooswitch jediný disk virtuálního počítače z úložiště standard toopremium a naopak. toouse premium spravované disky, musíte použít virtuální počítač [velikost virtuálního počítače](sizes.md) který podporuje službu premium storage. Tento příklad také přepínačů tooa velikost, která podporuje službu premium storage.

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

## <a name="next-steps"></a>Další kroky

Využít jen pro čtení kopie virtuálního počítače pomocí [snímky](snapshot-copy-managed-disk.md).

