---
title: "aaaAbout disky a virtuální pevné disky pro virtuální počítače Microsoft Azure Linux | Microsoft Docs"
description: "Další informace o hello základy disky a virtuální pevné disky pro virtuální počítače s Linuxem v Azure."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 7be8dd52-98f7-4187-9b78-55197915bc9b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: 1155d773136677553d263c17fbf8b224035d96bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-disks-and-vhds-for-azure-linux-vms"></a>O disky a virtuální pevné disky pro virtuální počítače Azure s Linuxem
Stejně jako všechny ostatní počítače virtuální počítače v Azure používat disky jako místní toostore operačního systému, aplikace a data. Všechny virtuální počítače Azure mít aspoň dva disky – disk operačního systému Linux a dočasný disk. vytváření Hello disku operačního systému z bitové kopie a disku operačního systému hello i hello image jsou ve skutečnosti virtuální pevné disky (VHD) uložené v účtu úložiště Azure. Virtuální počítače také může mít jeden nebo více datových disků, které jsou také uloženy jako virtuální pevné disky. 

V tomto článku jsme bude v souvislosti se používá jinou hello hello disků a potom popisují hello různým typům disků můžete vytvořit a použít. Tento článek je také k dispozici pro [virtuální počítače s Windows](storage-about-disks-and-vhds-windows.md).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Disky, které jsou používány virtuálními počítači

Podívejme se na použití hello disky podle hello virtuálních počítačů.

## <a name="operating-system-disk"></a>Disk operačním systému
Každý virtuální počítač má jeden disk připojené operačního systému. Je zaregistrován jako jednotka SATA a je označeno/dev/sda ve výchozím nastavení. Tento disk má maximální kapacita 2 048 gigabajtů (GB). 

## <a name="temporary-disk"></a>Dočasné disku
Každý virtuální počítač obsahuje dočasné disk. Hello dočasným diskovým poskytuje krátkodobé úložiště pro aplikace a procesy a je určený tooonly úložiště dat jako jsou soubory stránky nebo odkládacího souboru. Data na dočasné disku hello mohou být ztraceny při [údržby](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) nebo když jste [znovu nasadit virtuální počítač](../virtual-machines/linux/redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Během restartu standardní hello virtuální počítač by měl zachovat hello data na dočasné jednotce hello.

Na virtuální počítače s Linuxem, je obvykle hello disku **/dev/sdb** a je naformátován a připojit příliš**/mnt** podle hello Azure Linux Agent. velikost Hello hello dočasné disku se liší, na základě velikosti hello hello virtuálního počítače. Další informace najdete v tématu [velikosti virtuálních počítačů Linux](../virtual-machines/linux/sizes.md).

Další informace o tom, jak Azure používá dočasným diskovým hello najdete v tématu [pochopení hello jednotku ve virtuálních počítačích Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="data-disk"></a>Datový disk
Datový disk je virtuálního pevného disku, který se data aplikací toostore připojené tooa virtuálního počítače nebo jiná data, je nutné tookeep. Datové disky jsou zaregistrované jako disky SCSI a jsou označeny písmenem, který zvolíte. Každý datový disk má maximální kapacitu 4095 GB. Hello velikost hello virtuálního počítače určuje, kolik datových disků můžete připojit tooit a hello typ úložiště, můžete použít toohost hello disky.

> [!NOTE]
> Další podrobnosti o kapacity virtuálních počítačů najdete v tématu [velikosti virtuálních počítačů Linux](../virtual-machines/linux/sizes.md).
> 

Azure vytvoří disk operačního systému, když vytvoříte virtuální počítač z bitové kopie. Pokud používáte image, která obsahuje datové disky, Azure vytvoří také hello datových disků vytváří hello virtuálního počítače. Jinak datových disků přidat po vytvoření virtuálního počítače hello.

Datové disky tooa virtuálního počítače kdykoli, můžete přidat pomocí **připojení** hello disku toohello virtuálního počítače. Můžete vytvořit virtuální pevný disk, který jste nahráli nebo zkopírovat tooyour účet úložiště, nebo jeden této Azure vytvoří automaticky. Připojením datového disku přidruží souboru virtuálního pevného disku hello hello virtuálních počítačů, umístěním zapůjčení na hello virtuálního pevného disku, nelze jej odstranit z úložiště při je stále připojen.

[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="troubleshooting"></a>Řešení potíží
[!INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Další kroky
* [Připojit disk](../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooadd dodatečné úložiště pro virtuální počítač.
* [Konfigurace softwaru diskového pole RAID](../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) redundanci.
* [Zachytit virtuální počítač s Linuxem](../virtual-machines/linux/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) , můžete rychle nasadit další virtuální počítače.

