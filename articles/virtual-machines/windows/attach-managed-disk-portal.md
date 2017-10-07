---
title: "aaaAttach spravovaných dat disku tooa virtuální počítač s Windows - Azure | Microsoft Docs"
description: "Způsob správy dat disku tooa tooattach nový virtuální počítač s Windows v Azure pomocí portálu hello hello modelu nasazení Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: cynthn
ms.openlocfilehash: bacc0589ad2d93e4d3d055c8f837f8db27291ead
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooattach-a-managed-data-disk-tooa-windows-vm-in-hello-azure-portal"></a>Jak tooattach spravovaná data na disku tooa virtuální počítač s Windows v hello portálu Azure

Tento článek ukazuje, jak tooattach nová data spravovaného disku tooWindows virtuálním počítačům prostřednictvím hello portálu Azure. Než to uděláte, projděte si tyto tipy:

* velikost Hello hello virtuálního počítače určuje, kolik datových disků můžete připojit. Podrobnosti najdete v tématu [velikosti virtuálních počítačů](sizes.md).
* Pro nový disk, nepotřebujete toocreate ho první protože Azure ji vytvoří, když jeho připojení.

Můžete také [připojit datový disk pomocí prostředí Powershell](attach-disk-ps.md).



## <a name="add-a-data-disk"></a>Přidat datový disk
1. V nabídce hello hello vlevo, klikněte na **virtuální počítače**.
2. Vyberte virtuální počítač hello hello seznamu.
3. V okně hello virtuální počítač, klikněte na tlačítko **disky**.
   4. Na hello **disky** okně klikněte na tlačítko **+ přidat datový disk**.
5. V hello rozevírací seznam hello nový disk, vyberte **vytvořit prázdný**.
6. V hello **spravovaných disků na vytvořit** okno, zadejte název pro hello disk a upravte hello další nastavení podle potřeby. Až budete hotovi, klikněte na tlačítko **vytvořit**.
7. V hello **disky** okně klikněte na tlačítko Uložit toosave hello nové konfigurace disku hello virtuálních počítačů.
6. Jakmile Azure vytvoří hello disk a připojí jej toohello virtuálního počítače, hello nového disku je uvedena v nastavení disku hello virtuálního počítače v části **datových disků**.


## <a name="initialize-a-new-data-disk"></a>Inicializace nový datový disk

1. Připojte toohello virtuálních počítačů.
1. Klikněte na položku nabídky start hello uvnitř hello virtuálních počítačů a typ **diskmgmt.msc** a počtu **Enter**. Tím se spustí modul snap-in Správa disků hello.
2. Správa disků rozpozná, že máte nový, zrušení inicializovaného disku a bude překryvné okno inicializovat Disk hello.
3. Musí být vybrána hello nový disk a klikněte na tlačítko **OK** tooinitialize ho.
4. nový disk Hello se nyní zobrazí jako **nepřidělené**. Klikněte pravým tlačítkem na libovolné místo na disku hello a vyberte **nový jednoduchý svazek**. Hello **Průvodci vytvořením jednoduchého svazku** spustí.
5. Projděte hello průvodce, udržuje všechny výchozí hodnoty hello, po dokončení vyberte **Dokončit**.
6. Zavřete Správa disků.
7. Zobrazí se automaticky otevíraného okna, je nutné tooformat hello nový disk, abyste mohli používat. Klikněte na tlačítko **formát disku**.
8. V hello **nový disk formát** dialogové okno, zkontrolujte hello nastavení a pak klikněte na tlačítko **spustit**.
9. Zobrazí se upozornění, že formátování hello disky vymaže všechna hello data, klikněte na tlačítko **OK**.
10. Po dokončení hello Formát klikněte na tlačítko **OK**.

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
