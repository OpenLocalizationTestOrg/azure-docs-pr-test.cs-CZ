---
title: "aaaMount úložiště Azure file z virtuálního počítače Windows Azure | Microsoft Docs"
description: "Uložte soubor v hello cloudu s Azure file storage a připojte svou cloudovou sdílenou z Azure virtuálního počítače (VM)."
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: 965f1c1b3f0d07fec6d86f9312a05e02e8ce7fe0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-file-shares-with-windows-vms"></a>Použít sdílené složky Azure s virtuálními počítači Windows 

Sdílené složky Azure můžete použít jako způsob toostore a přístup k souborům z virtuálního počítače. Můžete například uložit skript nebo konfiguračního souboru aplikace, které chcete všechny virtuální počítače tooshare. V tomto tématu Ukážeme vám, jak toocreate a připojení Azure pro sdílení souborů a tooupload a stahování souborů.

## <a name="connect-tooa-file-share-from-a-vm"></a>Připojit tooa sdílené složky z virtuálního počítače

V této části se předpokládá, že už máte sdílenou složku, které chcete tooconnect k. Pokud potřebujete toocreate jeden, přečtěte si [vytvoření sdílené složky](#create-a-file-share) dál v tomto tématu.

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V levé nabídce hello, klikněte na tlačítko **účty úložiště**.
3. Vyberte svůj účet úložiště
4. V hello **přehled** v části **služby**, vyberte **soubory**.
5. Vyberte sdílenou složku.
6. Klikněte na tlačítko **připojit** tooopen stránku, která ukazuje hello syntaxe příkazového řádku pro sdílenou složku hello připojení ze systému Windows nebo Linux.
7. Zvýrazněte hello syntaxe příkazu hello a vložte jej do programu Poznámkový blok nebo jiném místě kde můžete snadno k němu přístup. 
8. Upravit hello syntaxe tooremove hello úvodní ** > ** a nahraďte *[písmeno jednotky]* hello písmeno jednotky (například **/Y:**) kam chcete toomount hello sdílené složky.
8. Připojte tooyour virtuálního počítače a otevřete příkazový řádek.
9. Vložením hello upravovat připojení syntaxe a stiskněte tlačítko **Enter**.
10. Po vytvoření připojení hello se zobrazí zpráva hello **hello příkaz úspěšně dokončil.**
11. Zkontrolujte připojení hello zadáním hello jednotky písmeno tooswitch toothat jednotky a pak zadejte **dir** toosee hello obsah hello sdílené složky.



## <a name="create-a-file-share"></a>Vytvoření sdílené složky 
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V levé nabídce hello, klikněte na tlačítko **účty úložiště**.
3. Vyberte svůj účet úložiště
4. V hello **přehled** v části **služby**, vyberte **soubory**.
5. Na stránku hello souboru služby, klikněte na **+ sdílená** toocreate sdílet první soubor. \
6. Zadejte název sdílené složky souborů hello. Názvy sdílených složek můžete použít, malá písmena, číslice a jeden pomlčky. Hello název nemůže začínat pomlčkou a nemůže používat více po sobě jdoucí pomlčky. 
7. Vyplňte omezení v tom, jak velký hello souboru může být až too5120 GB.
8. Klikněte na tlačítko **OK** toodeploy hello sdílené složky.
   
## <a name="upload-files"></a>Nahrání souborů
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V levé nabídce hello, klikněte na tlačítko **účty úložiště**.
3. Vyberte svůj účet úložiště
4. V hello **přehled** v části **služby**, vyberte **soubory**.
5. Vyberte sdílenou složku.
6. Klikněte na tlačítko **nahrát** tooopen hello **nahrání souborů** stránky.
7. Klikněte na hello složky ikonu toobrowse systému místního souboru pro soubor tooupload.   
8. Klikněte na tlačítko **nahrát** tooupload hello souboru toohello sdílené složky.

## <a name="download-files"></a>Stažení souborů
1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V levé nabídce hello, klikněte na tlačítko **účty úložiště**.
3. Vyberte svůj účet úložiště
4. V hello **přehled** v části **služby**, vyberte **soubory**.
5. Vyberte sdílenou složku.
6. Klikněte pravým tlačítkem na soubor hello a zvolte **Stáhnout** toodownload ho tooyour místního počítače.
   

## <a name="next-steps"></a>Další kroky

Můžete také vytvořit a spravovat sdílené složky pomocí prostředí PowerShell. Další informace najdete v tématu [Začínáme s Azure File storage ve Windows](../../storage/files/storage-dotnet-how-to-use-files.md).
