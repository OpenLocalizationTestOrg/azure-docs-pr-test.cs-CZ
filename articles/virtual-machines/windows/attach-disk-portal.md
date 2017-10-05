---
title: "Připojit nespravované datový disk k virtuálnímu počítači Windows - Azure | Microsoft Docs"
description: "Jak připojit nový nebo existující nespravované datový disk pro virtuální počítač s Windows v portálu Azure pomocí modelu nasazení Resource Manager."
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
ms.openlocfilehash: c0886302c82641f8cc1a00d3972870d58ba8afb4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-attach-an-unmanaged-data-disk-to-a-windows-vm-in-the-azure-portal"></a>Tom, jak připojit nespravované datový disk k virtuálnímu počítači Windows na portálu Azure

Tento článek ukazuje, jak připojit nové i stávající nespravované disky pro virtuální počítače s Windows pomocí portálu Azure. Můžete také [připojit datový disk pomocí prostředí PowerShell](./attach-disk-ps.md). Než to uděláte, projděte si tyto tipy:

* Velikost virtuálního počítače určuje, kolik datových disků můžete připojit. Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md).
* Chcete-li používat úložiště úrovně Premium, je třeba DS-series nebo GS-series virtuálního počítače. Můžete použít disky z účty úložiště Premium a Standard s těchto virtuálních počítačů. Storage úrovně Premium je k dispozici v určité oblasti. Podrobnosti najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Pro nový disk nemusíte nejdřív ji vytvořit, protože Azure ji vytvoří, když jeho připojení.


Můžete také [připojit datový disk pomocí prostředí Powershell](attach-disk-ps.md).


## <a name="find-the-virtual-machine"></a>Najít virtuální počítač
1. Přihlaste se k webu [Azure Portal](https://portal.azure.com/).
2. V nabídce na levé straně klikněte na tlačítko **virtuální počítače**.
3. Ze seznamu vyberte virtuální počítač.
4. V okně virtuální počítače, klikněte na tlačítko **disky**.
   
Pokračujte podle pokynů pro připojení buď [nový disk](#option-1-attach-a-new-disk) nebo [stávající disk](#option-2-attach-an-existing-disk).

## <a name="option-1-attach-and-initialize-a-new-disk"></a>Možnost 1: Připojte a inicializace nový disk
1. Na **disky** okně klikněte na tlačítko **+ přidat datový disk**.
2. V **spravovaných disků připojit** okno, zadejte název disku **název** a pak vyberte **nový (prázdný disk)** v **typ zdroje**.
3. V části **kontejner úložiště**, klikněte **Procházet** tlačítko a přejděte do účtu úložiště a kontejner, kam chcete uložit a potom klikněte na nový virtuální pevný disk **vyberte**. 
  
   ![Zkontrolujte nastavení disku](./media/attach-disk-portal/attach-empty-unmanaged.png)
   
3. Až skončíte s nastavením pro datový disk, klikněte na tlačítko **OK**.
4. Zpět v **disky** okně klikněte na tlačítko **Uložit** disk přidejte do konfigurace virtuálního počítače.


### <a name="initialize-a-new-data-disk"></a>Inicializace nový datový disk

1. Připojte k virtuálnímu počítači. Pokyny najdete v tématu [jak se připojit a přihlásit se na virtuálním počítači Azure s Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
1. Klikněte na tlačítko **spustit** nabídky uvnitř virtuálního počítače a typ **diskmgmt.msc** a počtu **Enter**. Otevře se modul snap-in Správa disků.
2. Správa disků rozpozná, že máte nové, neinicializovaného disku a bude překryvné okno inicializovat Disk.
3. Zkontrolujte, že je vybraný nový disk a klikněte na tlačítko **OK** k chybě při inicializaci ho.
4. Nový disk se zobrazí jako **nepřidělené**. Klepněte pravým tlačítkem myši na disk a vyberte **nový jednoduchý svazek**. **Průvodci vytvořením jednoduchého svazku** spustí.
5. Postupujte dle pokynů průvodce, udržuje všechny výchozí hodnoty, po dokončení vyberte **Dokončit**.
6. Zavřete Správa disků.
7. Zobrazí automaticky otevírané okno, které potřebujete k formátování nový disk, abyste mohli používat. Klikněte na tlačítko **formát disku**.
8. V **nový disk formát** dialogové okno, zkontrolujte nastavení a pak klikněte na tlačítko **spustit**.
9. Zobrazí upozornění, že formátování disky vymaže všechna data, klikněte na tlačítko **OK**.
10. Po dokončení Formát klikněte na tlačítko **OK**.


## <a name="option-2-attach-an-existing-disk"></a>Možnost 2: Připojit stávající disk
1. Na **disky** okně klikněte na tlačítko **+ přidat datový disk**.
2. Na **nespravované disk připojit** okno v **typ zdroje** vyberte **existující objekt blob**.

    ![Zkontrolujte nastavení disku](./media/attach-disk-portal/attach-existing-unmanaged.png)

    3. Klikněte na tlačítko **Procházet** přejděte na účet úložiště a kontejneru, kde se nachází existující virtuální pevný disk. Klikněte na tlačítko a virtuální pevný disk a pak klikněte na tlačítko **vyberte**.
4. Klikněte na tlačítko **OK** v **nespravované disk připojit** okno.
5. V **disky** okně klikněte na tlačítko **Uložit** přidání disku do konfigurace pro virtuální počítač.
   


## <a name="use-trim-with-standard-storage"></a>Použít standardní úložiště uvolnění dočasné paměti

Pokud chcete použít standardní úložiště (HDD), měli byste povolit uvolnění dočasné paměti. TRIM zahodí nepoužívané bloky na disku, můžete se účtují pouze pro úložiště, které skutečně používáte. To můžete uložit na náklady, pokud chcete vytvořit velkých souborů a pak odstraňte je. 

Můžete spustit tento příkaz a zkontrolujte nastavení uvolnění dočasné paměti. Otevřete příkazový řádek na virtuální počítač s Windows a zadejte:

```
fsutil behavior query DisableDeleteNotify
```

Pokud příkaz vrátí hodnotu 0, TRIM správně povolené. Pokud vrátí hodnotu 1, spusťte následující příkaz k povolení uvolnění dočasné paměti:
```
fsutil behavior set DisableDeleteNotify 0
```

Po odstranění dat z disku, můžete zajistit TRIM operace vyprázdnění správně spuštěním defragmentační s uvolnění dočasné paměti:

```
defrag.exe <volume:> -l
```

Můžete také zajistit, že celý svazek je oříznut podle formátování svazku.


## <a name="next-steps"></a>Další kroky
Pokud jste aplikaci potřebuje používat D: disk k uložení dat, můžete [změnit písmeno jednotky dočasné disk systému Windows](change-drive-letter.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

