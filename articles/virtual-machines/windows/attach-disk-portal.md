---
title: "aaaAttach nespravovaná data disku tooa virtuální počítač s Windows - Azure | Microsoft Docs"
description: "Jak tooattach nový nebo existující nespravovaná data disku tooa virtuální počítač s Windows v Azure pomocí portálu hello hello modelu nasazení Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3790fc59-7264-41df-b7a3-8d1226799885
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: cynthn
ms.openlocfilehash: 923a6663974143bf359970f9b0eb0d36cabcba9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-an-unmanaged-data-disk-tooa-windows-vm-in-hello-azure-portal"></a>Jak tooattach nespravovaná data na disku tooa virtuální počítač s Windows v hello portálu Azure

Tento článek ukazuje, jak nespravované tooattach nové i stávající disky tooWindows virtuálním počítačům prostřednictvím hello portálu Azure. Můžete také [připojit datový disk pomocí prostředí PowerShell](./attach-disk-ps.md). Než to uděláte, projděte si tyto tipy:

* velikost Hello hello virtuálního počítače určuje, kolik datových disků můžete připojit. Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md).
* toouse storage úrovně Premium, budete potřebovat DS-series nebo GS-series virtuálního počítače. Můžete použít disky z účty úložiště Premium a Standard s těchto virtuálních počítačů. Storage úrovně Premium je k dispozici v určité oblasti. Podrobnosti najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Pro nový disk, nepotřebujete toocreate ho první protože Azure ji vytvoří, když jeho připojení.


Můžete také [připojit datový disk pomocí prostředí Powershell](attach-disk-ps.md).


## <a name="find-hello-virtual-machine"></a>Najít hello virtuální počítač
1. Přihlaste se toohello [portál Azure](https://portal.azure.com/).
2. V nabídce hello hello vlevo, klikněte na **virtuální počítače**.
3. Vyberte virtuální počítač hello hello seznamu.
4. V okně hello virtuální počítače, klikněte na tlačítko **disky**.
   
Pokračujte podle pokynů pro připojení buď [nový disk](#option-1-attach-a-new-disk) nebo [stávající disk](#option-2-attach-an-existing-disk).

## <a name="option-1-attach-and-initialize-a-new-disk"></a>Možnost 1: Připojte a inicializace nový disk
1. Na hello **disky** okně klikněte na tlačítko **+ přidat datový disk**.
2. V hello **spravovaných disků připojit** okno, zadejte název disku hello **název** a pak vyberte **nový (prázdný disk)** v **typ zdroje**.
3. V části **kontejner úložiště**, klikněte na tlačítko hello **Procházet** tlačítko a toohello účet úložiště a kontejneru, kde chcete vytvořit hello nový virtuální pevný disk toobe uložené a pak klikněte na tlačítko Procházet **vyberte**. 
  
   ![Zkontrolujte nastavení disku](./media/attach-disk-portal/attach-empty-unmanaged.png)
   
3. Až skončíte s hello nastavení pro hello datový disk, klikněte na tlačítko **OK**.
4. Zpět v hello **disky** okně klikněte na tlačítko **Uložit** tooadd hello disku toohello konfiguraci.


### <a name="initialize-a-new-data-disk"></a>Inicializace nový datový disk

1. Připojte toohello virtuálního počítače. Pokyny najdete v tématu [jak se tooconnect a protokol na tooan Azure virtuální počítač, systém Windows spuštěný](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
1. Klikněte na tlačítko hello **spustit** nabídky uvnitř hello virtuálních počítačů a typ **diskmgmt.msc** a počtu **Enter**. Otevře se modul snap-in Správa disků hello.
2. Správa disků rozpozná, že máte nové, neinicializovaného disku a bude překryvné okno inicializovat Disk hello.
3. Musí být vybrána hello nový disk a klikněte na tlačítko **OK** tooinitialize ho.
4. Hello nový disk se teď zobrazí jako **nepřidělené**. Klikněte pravým tlačítkem na libovolné místo na disku hello a vyberte **nový jednoduchý svazek**. Hello **Průvodci vytvořením jednoduchého svazku** spustí.
5. Projděte hello průvodce, udržuje všechny výchozí hodnoty hello, po dokončení vyberte **Dokončit**.
6. Zavřete Správa disků.
7. Získat automaticky otevíraného okna je nutné tooformat hello nový disk, abyste mohli používat. Klikněte na tlačítko **formát disku**.
8. V hello **nový disk formát** dialogové okno, zkontrolujte hello nastavení a pak klikněte na tlačítko **spustit**.
9. Zobrazí upozornění, že formátování hello disky vymaže všechna hello data, klikněte na tlačítko **OK**.
10. Po dokončení hello Formát klikněte na tlačítko **OK**.


## <a name="option-2-attach-an-existing-disk"></a>Možnost 2: Připojit stávající disk
1. Na hello **disky** okně klikněte na tlačítko **+ přidat datový disk**.
2. Na hello **nespravované disk připojit** okno v **typ zdroje** vyberte **existující objekt blob**.

    ![Zkontrolujte nastavení disku](./media/attach-disk-portal/attach-existing-unmanaged.png)

    3. Klikněte na tlačítko **Procházet** toonavigate toohello účtu úložiště a kontejneru, kde hello existující virtuální pevný disk nachází. Klikněte na tlačítko hello virtuálního pevného disku a pak klikněte na tlačítko **vyberte**.
4. Klikněte na tlačítko **OK** v hello **nespravované disk připojit** okno.
5. V hello **disky** okně klikněte na tlačítko **Uložit** tooadd hello disku toohello konfigurace pro hello virtuálních počítačů.
   


## <a name="use-trim-with-standard-storage"></a>Použít standardní úložiště uvolnění dočasné paměti

Pokud chcete použít standardní úložiště (HDD), měli byste povolit uvolnění dočasné paměti. TRIM zahodí nepoužívané bloky na disku hello tak fakturuje se pouze pro úložiště, které skutečně používáte. To můžete uložit na náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je. 

Můžete spustit toto OŘÍZNUTÍ nastavení příkaz toocheck hello. Otevřete příkazový řádek na virtuální počítač s Windows a zadejte:

```
fsutil behavior query DisableDeleteNotify
```

Pokud příkaz hello vrátí hodnotu 0, TRIM povolený správně. Pokud vrátí hodnotu 1, spusťte následující příkaz tooenable TRIM hello:
```
fsutil behavior set DisableDeleteNotify 0
```

Po odstranění dat z disku, můžete zajistit, že hello TRIM operace vyprázdnění správně spuštěním defragmentační s uvolnění dočasné paměti:

```
defrag.exe <volume:> -l
```

Můžete také zajistit, že hello celý svazek je oříznut podle formátování svazku hello.


## <a name="next-steps"></a>Další kroky
Pokud je aplikace potřebuje toouse hello D: jednotky toostore data, můžete [změnit písmeno jednotky hello dočasné disk systému Windows hello](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

