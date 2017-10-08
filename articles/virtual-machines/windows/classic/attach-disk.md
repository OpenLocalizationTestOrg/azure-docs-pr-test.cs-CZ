---
title: "aaaAttach tooa disku classic virtuálního počítače Azure | Microsoft Docs"
description: "Připojit datový disk tooa virtuálního počítače s Windows vytvořené pomocí modelu nasazení classic hello a provést jeho inicializaci."
services: virtual-machines-windows, storage
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: be4e3e74-05bc-4527-969f-84f10a1d66a7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: cynthn
ms.openlocfilehash: bfe1b0fa066277d28d3862a18da4b1023cb4452d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="attach-a-data-disk-tooa-windows-virtual-machine-created-with-hello-classic-deployment-model"></a>Připojit datový disk tooa virtuálního počítače s Windows vytvořené pomocí modelu nasazení classic hello
<!--
Refernce article:
    If you want toouse hello new portal, see [How tooattach a data disk tooa Windows VM in hello Azure portal](../../virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

Tento článek ukazuje, jak tooattach nových nebo stávajících disky vytvořené pomocí hello klasického nasazení modelu tooa virtuálního počítače Windows hello pomocí portálu Azure.

Můžete také [připojit tooa disku data virtuálního počítače s Linuxem v hello portál Azure](../../linux/attach-disk-portal.md).

Než připojíte disk, projděte si tyto tipy:

* velikost Hello hello virtuálního počítače určuje, kolik datových disků můžete připojit. Podrobnosti najdete v tématu [velikosti virtuálních počítačů](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* toouse storage úrovně Premium, budete potřebovat DS-series nebo GS-series virtuálního počítače. Můžete použít disky z účty úložiště Premium a Standard s těchto virtuálních počítačů. Storage úrovně Premium je k dispozici v určité oblasti. Podrobnosti najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Pro nový disk, nepotřebujete toocreate ho první protože Azure ji vytvoří, když jeho připojení.

Můžete také [připojit datový disk pomocí prostředí Powershell](../../virtual-machines-windows-attach-disk-ps.md).

> [!IMPORTANT]
> Azure má dva různé modely nasazení pro vytváření a práci s prostředky: [Resource Manager a klasický](../../../resource-manager-deployment-model.md).

## <a name="find-hello-virtual-machine"></a>Najít hello virtuální počítač
1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. Virtuální počítač vyberte hello z prostředku hello uvedené na řídicím panelu hello.
3. V levém podokně hello pod **nastavení**, klikněte na tlačítko **disky**.

    ![Otevřete nastavení disku](./media/attach-disk/virtualmachinedisks.png)

Pokračujte podle pokynů pro připojení buď [nový disk](#option-1-attach-a-new-disk) nebo [stávající disk](#option-2-attach-an-existing-disk).

## <a name="option-1-attach-and-initialize-a-new-disk"></a>Možnost 1: Připojte a inicializace nový disk

1. Na hello **disky** okně klikněte na tlačítko **připojit nový**.
2. Zkontrolujte hello výchozí nastavení, aktualizovat podle potřeby a pak klikněte na tlačítko **OK**.

   ![Zkontrolujte nastavení disku](./media/attach-disk/attach-new.png)

3. Jakmile Azure vytvoří hello disk a připojí jej toohello virtuálního počítače, hello nového disku je uvedena v nastavení disku hello virtuálního počítače v části **datových disků**.

### <a name="initialize-a-new-data-disk"></a>Inicializace nový datový disk

1. Připojte toohello virtuálního počítače. Pokyny najdete v tématu [jak se tooconnect a protokol na tooan Azure virtuální počítač, systém Windows spuštěný](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. Po přihlášení toohello virtuální počítač, otevřete **správce serveru**. V levém podokně hello vyberte **Souborová služba a služba úložiště**.

    ![Otevřete správce serveru](../media/attach-disk-portal/fileandstorageservices.png)

3. Vyberte **disky**.
4. Hello **disky** části jsou uvedené disky hello. Virtuální počítač má nejčastěji disk 0, disk 1 a 2 disku. Disk 0 je hello operačního systému, disk 1 je hello dočasné a disk 2 je, aby byl datový disk hello nově připojen toohello virtuálního počítače. Hello datového disku seznamy hello oddílu jako **neznámé**.

 Klikněte pravým tlačítkem na hello disk a vyberte **inicializovat**.

5. Zobrazení upozornění, že všechna data vymažou se při hello disk inicializován. Klikněte na tlačítko **Ano** tooacknowledge hello upozornění a inicializovat hello disku. Po dokončení hello oddíl bude uvedený jako **GPT**. Klikněte pravým tlačítkem na hello disk znovu a vyberte **nový svazek**.

6. Dokončete Průvodce hello hello výchozí hodnoty. Po dokončení Průvodce hello hello **svazky** části je uveden seznam hello nový svazek. Hello disk je teď online a připravená toostore data.

    ![Svazek úspěšně inicializován.](./media/attach-disk/newdiskafterinitialization.png)

## <a name="option-2-attach-an-existing-disk"></a>Možnost 2: Připojit stávající disk
1. Na hello **disky** okně klikněte na tlačítko **připojit existující**.
2. V části **připojit stávající disk**, klikněte na tlačítko **umístění**.

   ![Připojit stávající disk](./media/attach-disk/attachexistingdisksettings.png)
3. V části **účty úložiště**, vyberte účet hello a kontejner, který obsahuje soubor VHD hello.

   ![Vyhledejte umístění virtuálního pevného disku](./media/attach-disk/existdiskstorageaccountandcontainer.png)

4. Vyberte soubor VHD hello.
5. V části **připojit stávající disk**, právě vybraný soubor hello je uveden v části **souboru virtuálního pevného disku**. Klikněte na **OK**.
6. Po Azure připojí hello disku toohello virtuálního počítače, je uvedena v nastavení disku hello virtuálního počítače v části **datových disků**.

## <a name="use-trim-with-standard-storage"></a>Použít standardní úložiště uvolnění dočasné paměti

Pokud chcete použít standardní úložiště (HDD), měli byste povolit uvolnění dočasné paměti. TRIM zahodí nepoužívané bloky na disku hello tak fakturuje se pouze pro úložiště, které skutečně používáte. Pomocí uvolnění dočasné paměti může významně snížit náklady, včetně nepoužívané bloky, které jsou výsledkem odstranění velkých souborů.

Můžete spustit toto OŘÍZNUTÍ nastavení příkaz toocheck hello. Otevřete příkazový řádek na virtuální počítač s Windows a zadejte:

```
fsutil behavior query DisableDeleteNotify
```

Pokud příkaz hello vrátí hodnotu 0, TRIM povolený správně. Pokud vrátí hodnotu 1, spusťte následující příkaz tooenable TRIM hello:
```
fsutil behavior set DisableDeleteNotify 0
```

## <a name="next-steps"></a>Další kroky
Pokud aplikace potřebuje toouse hello D: jednotky toostore data, můžete [změnit písmeno jednotky hello dočasné disk systému Windows hello](../../virtual-machines-windows-change-drive-letter.md).

## <a name="additional-resources"></a>Další zdroje
[O disky a virtuální pevné disky pro virtuální počítače](../../virtual-machines-linux-about-disks-vhds.md)
