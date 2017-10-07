---
title: "Ujistěte se, hello D: disku virtuálního počítače datový disk | Microsoft Docs"
description: "Popisuje, jak písmena jednotky toochange pro virtuální počítač s Windows tak, aby hello D: jednotky můžete použít jako datový disk."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 0867a931-0055-4e31-8403-9b38a3eeb904
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: cynthn
ms.openlocfilehash: 43939da1a890ac4049266487951e3889aa0ed9d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-d-drive-as-a-data-drive-on-a-windows-vm"></a>Použití disků D: hello jako datový disk na virtuální počítač s Windows
Pokud aplikace potřebuje toouse hello D jednotky toostore dat, postupujte podle těchto pokynů toouse jiné písmeno jednotky pro dočasné disk hello. Nikdy nepoužívejte hello dočasným diskovým toostore dat, je nutné, aby tookeep.

Při změně velikosti nebo **zastavení (Deallocate)** virtuální počítač, to může aktivovat umístění nového hypervisoru tooa hello virtuálního počítače. Události plánované i neplánované údržby, může se spustit toto umístění. V tomto scénáři bude dočasným diskovým hello přeřadit toohello první dostupné písmeno jednotky. Pokud máte aplikaci, která vyžaduje konkrétně hello D: jednotky, je třeba toofollow tyto kroky tootemporarily přesunutí hello pagefile.sys připojit nový datový disk a přiřaďte ho hello písmeno D a potom přesunutí hello pagefile.sys zpět toohello dočasné jednotce. Po dokončení Azure nebude zpět hello D: Pokud hello virtuální počítač přesune tooa různých hypervisoru.

Další informace o tom, jak Azure používá dočasným diskovým hello najdete v tématu [pochopení hello jednotku ve virtuálních počítačích Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="attach-hello-data-disk"></a>Připojit datový disk hello
Nejdřív musíte tooattach hello datového disku toohello virtuálního počítače. toodo tento pomocí hello portálu, najdete v tématu [jak tooattach spravovaná data na disku v hello portál Azure](attach-managed-disk-portal.md).

## <a name="temporarily-move-pagefilesys-tooc-drive"></a>Dočasně přesunout pagefile.sys tooC jednotky
1. Připojte toohello virtuálního počítače. 
2. Klikněte pravým tlačítkem na hello **spustit** nabídku a vyberte **systému**.
3. V levé nabídce hello vyberte **Upřesnit nastavení systému**.
4. V hello **výkonu** vyberte **nastavení**.
5. Vyberte hello **Upřesnit** kartě.
6. V hello **virtuální paměti** vyberte **změnu**.
7. Vyberte hello **C** jednotky a pak klikněte na **velikost určí systém** a pak klikněte na **nastavit**.
8. Vyberte hello **D** jednotky a pak klikněte na **stránkovací soubor** a pak klikněte na **nastavit**.
9. Kliknutím na tlačítko použít. Zobrazí se upozornění, že tento počítač hello je toobe restartovat, aby se mohla ovlivnit tootake změny hello.
10. Restartujte hello virtuálního počítače.

## <a name="change-hello-drive-letters"></a>Změnit písmena jednotek hello
1. Jednou hello restartování virtuálního počítače, přihlaste se zpět toohello virtuálního počítače.
2. Klikněte na tlačítko hello **spustit** nabídce a typ **diskmgmt.msc** a stiskněte Enter. Správa disků se spustí.
3. Klikněte pravým tlačítkem na **D**hello dočasné úložiště jednotky a vyberte **změnit písmeno jednotky a cesty**.
4. V části písmeno jednotky, vyberte na nový disk **T** a pak klikněte na **OK**. 
5. Klikněte pravým tlačítkem na hello datový disk a vyberte **změnit písmeno jednotky a cesty**.
6. V části písmeno jednotky, vyberte jednotky **D** a pak klikněte na **OK**. 

## <a name="move-pagefilesys-back-toohello-temporary-storage-drive"></a>Přesunout pagefile.sys back toohello dočasné úložiště disku
1. Klikněte pravým tlačítkem na hello **spustit** nabídku a vyberte **systému**
2. V levé nabídce hello vyberte **Upřesnit nastavení systému**.
3. V hello **výkonu** vyberte **nastavení**.
4. Vyberte hello **Upřesnit** kartě.
5. V hello **virtuální paměti** vyberte **změnu**.
6. Vyberte jednotku hello OS **C** a klikněte na tlačítko **stránkovací soubor** a pak klikněte na **nastavit**.
7. Vyberte jednotku dočasné úložiště hello **T** a pak klikněte na **velikost určí systém** a pak klikněte na **nastavit**.
8. Klikněte na tlačítko **Použít**. Zobrazí se upozornění, že tento počítač hello je toobe restartovat, aby se mohla ovlivnit tootake změny hello.
9. Restartujte hello virtuálního počítače.

## <a name="next-steps"></a>Další kroky
* Můžete zvýšit hello úložiště k dispozici tooyour virtuálního počítače pomocí [další datový disk se připojuje](attach-managed-disk-portal.md).

