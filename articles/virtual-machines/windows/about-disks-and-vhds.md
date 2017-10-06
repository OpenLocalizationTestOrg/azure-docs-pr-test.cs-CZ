---
title: "aaaAbout disky a virtuální pevné disky pro virtuální počítače Windows Microsoft Azure | Microsoft Docs"
description: "Další informace o hello základy disky a virtuální počítače virtuální pevné disky pro Windows v Azure."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 0142c64d-5e8c-4d62-aa6f-06d6261f485a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2017
ms.author: robinsh
ms.openlocfilehash: 1b0d6bf05237bb3d1497b2c60f79a0159b730108
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="about-disks-and-vhds-for-azure-windows-vms"></a>O disky a virtuální pevné disky pro virtuální počítače Windows Azure
Stejně jako všechny ostatní počítače virtuální počítače v Azure používat disky jako místní toostore operačního systému, aplikace a data. Všechny virtuální počítače Azure mít aspoň dva disky – disk operačního systému Windows a dočasný disk. vytváření Hello disku operačního systému z bitové kopie a disku operačního systému hello i hello image jsou virtuální pevné disky (VHD) uložené v účtu úložiště Azure. Virtuální počítače také může mít jeden nebo více datových disků, které jsou také uloženy jako virtuální pevné disky. 

V tomto článku jsme bude v souvislosti se používá jinou hello hello disků a potom popisují hello různým typům disků můžete vytvořit a použít. Tento článek je také k dispozici pro [virtuální počítače s Linuxem](about-disks-and-vhds.md).

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="disks-used-by-vms"></a>Disky, které jsou používány virtuálními počítači

Podívejme se na použití hello disky podle hello virtuálních počítačů.

### <a name="operating-system-disk"></a>Disk operačním systému
Každý virtuální počítač má jeden disk připojené operačního systému. Má registrován jako jednotky SATA a označené jako jednotky C: hello ve výchozím nastavení. Tento disk má maximální kapacita 2 048 gigabajtů (GB). 

### <a name="temporary-disk"></a>Dočasné disku
Každý virtuální počítač obsahuje dočasné disk. Hello dočasným diskovým poskytuje krátkodobé úložiště pro aplikace a procesy a je určený tooonly úložiště dat jako jsou soubory stránky nebo odkládacího souboru. Data na dočasné disku hello mohou být ztraceny při [údržby](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) nebo když jste [znovu nasadit virtuální počítač](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Během restartu standardní hello virtuální počítač by měl zachovat hello data na dočasné jednotce hello.

dočasným diskovým Hello je označený jako hello D: jednotky ve výchozím nastavení a používá se pro ukládání pagefile.sys. tooremap tento disk tooa jiné písmeno jednotky, najdete v části [změnit písmeno jednotky hello dočasné disku Windows hello](change-drive-letter.md). velikost Hello hello dočasné disku se liší, na základě velikosti hello hello virtuálního počítače. Další informace najdete v tématu [virtuální počítače s velikostí pro Windows](sizes.md).

Další informace o tom, jak Azure používá dočasným diskovým hello najdete v tématu [pochopení hello jednotku ve virtuálních počítačích Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)


### <a name="data-disk"></a>Datový disk
Datový disk je virtuálního pevného disku, který se data aplikací toostore připojené tooa virtuálního počítače nebo jiná data, je nutné tookeep. Datové disky jsou zaregistrované jako disky SCSI a jsou označeny písmenem, který zvolíte. Každý datový disk má maximální kapacitu 4095 GB. Hello velikost hello virtuálního počítače určuje, kolik datových disků můžete připojit tooit a hello typ úložiště, můžete použít toohost hello disky.

> [!NOTE]
> Další informace o virtuální počítače kapacity najdete v tématu [virtuální počítače s velikostí pro Windows](sizes.md).
> 

Azure vytvoří disk operačního systému, když vytvoříte virtuální počítač z bitové kopie. Pokud používáte image, která obsahuje datové disky, Azure vytvoří také hello datových disků vytváří hello virtuálního počítače. Jinak datových disků přidat po vytvoření virtuálního počítače hello.

Datové disky tooa virtuálního počítače kdykoli, můžete přidat pomocí **připojení** hello disku toohello virtuálního počítače. Můžete vytvořit virtuální pevný disk, který jste nahráli nebo zkopírovat tooyour účet úložiště, nebo jeden této Azure vytvoří automaticky. Připojením datového disku přidruží souboru virtuálního pevného disku hello hello virtuálních počítačů umístěním zapůjčení na hello virtuálního pevného disku, nelze jej odstranit z úložiště při je stále připojen.


[!INCLUDE [storage-about-vhds-and-disks-windows-and-linux](../../../includes/storage-about-vhds-and-disks-windows-and-linux.md)]

## <a name="one-last-recommendation-use-trim-with-unmanaged-standard-disks"></a>Jedním z poslední doporučení: použití TRIM s nespravované standardní disky 

Pokud používáte nespravované standardní disky (HDD), měli byste povolit uvolnění dočasné paměti. TRIM zahodí nepoužívané bloky na disku hello tak fakturuje se pouze pro úložiště, které skutečně používáte. To můžete uložit na náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je. 

Můžete spustit toto OŘÍZNUTÍ nastavení příkaz toocheck hello. Otevřete příkazový řádek na virtuální počítač s Windows a zadejte:


```
fsutil behavior query DisableDeleteNotify
```

Pokud příkaz hello vrátí hodnotu 0, TRIM povolený správně. Pokud vrátí hodnotu 1, spusťte následující příkaz tooenable TRIM hello:

```
fsutil behavior set DisableDeleteNotify 0
```

> [!NOTE]
> Poznámka: Podpora uvolnění dočasné paměti spustí s Windows serverem 2012 nebo Windows 8 a vyšší, najdete v tématu [nové rozhraní API umožňuje aplikacím toosend "TRIM a zrušit mapování" pomocné parametry toostorage média](https://msdn.microsoft.com/windows/compatibility/new-api-allows-apps-to-send-trim-and-unmap-hints).
> 

<!-- Might want toomatch next-steps from overview of managed disks -->
## <a name="next-steps"></a>Další kroky
* [Připojit disk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooadd dodatečné úložiště pro virtuální počítač.
* [Změnit písmeno jednotky hello dočasné disku Windows hello](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) tak vaše aplikace může použít hello D: disku pro data.

