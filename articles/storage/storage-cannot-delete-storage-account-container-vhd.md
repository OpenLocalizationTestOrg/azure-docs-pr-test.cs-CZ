---
title: "Řešení potíží s odstraňování účtů úložiště Azure, kontejnery nebo virtuální pevné disky v nasazení classic | Microsoft Docs"
description: "Řešení potíží s odstraňování účtů úložiště Azure, kontejnery nebo virtuální pevné disky v nasazení classic"
services: storage
documentationcenter: 
author: genlin
manager: felixwu
editor: tysonn
tags: storage
ms.assetid: 0f7a8243-d8dc-432a-9d37-1272a0cb3a5c
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.openlocfilehash: 9f3e824414ad6c1a0aba98a3d549ee63ddc7272f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a>Řešení potíží s odstraňování účtů úložiště Azure, kontejnery nebo virtuální pevné disky v nasazení classic
[!INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Taky může docházet k chybám při pokusu o odstranění účtu úložiště Azure, kontejneru nebo virtuální pevný disk v [portál Azure](https://portal.azure.com/) nebo [portál Azure classic](https://manage.windowsazure.com/). Tyto problémy můžou být způsobené těmito okolnostmi:

* Když odstraníte virtuální počítač, disk a virtuální pevný disk se neodstraní automaticky. To může být důvod chyby při odstraňování účtu úložiště. Neodstraňujte jsme disku tak, aby disk můžete připojit jiným virtuálním Počítačem.
* Disk nebo objekt blob, který je k tomuto disku přidružený, je stále ještě zapůjčený.
* Je stále image virtuálního počítače, který používá účet úložiště, kontejner nebo objekt blob.

Pokud není Azure problém řešený v tomto článku, navštivte fórech Azure na [MSDN a Stack Overflow](https://azure.microsoft.com/support/forums/). Problém můžete účtovat na tyto fóra nebo na @AzureSupport na Twitteru. Navíc můžete soubor žádost o podporu Azure tak, že vyberete **získat podporu** na [podporu Azure](https://azure.microsoft.com/support/options/) lokality.

## <a name="symptoms"></a>Příznaky
V následující části jsou uvedeny běžné chyby, které může dojít při pokusu o odstranění účtů úložiště Azure, kontejnery nebo virtuální pevné disky.

### <a name="scenario-1-unable-to-delete-a-storage-account"></a>Scénář 1: Nelze odstranit účet úložiště
Když přejdete k účtu úložiště classic v [portál Azure](https://portal.azure.com/) a vyberte **odstranit**, může být zobrazí se seznam objektů, které brání odstranění účtu úložiště:

  ![Obrázek chyby při odstranění účtu úložiště](./media/storage-cannot-delete-storage-account-container-vhd/newerror.png)

Když přejdete k účtu úložiště v [portál Azure classic](https://manage.windowsazure.com/) a vyberte **odstranit**, můžete sledovat některé z těchto chyb:

- *Účet úložiště StorageAccountName obsahuje Image virtuálních počítačů. Zkontrolujte, zda že jsou tyto Image virtuálních počítačů odebrány před odstraněním tohoto účtu úložiště.*

- *Nepodařilo se odstranit účet úložiště < vm-storage účet name >. Nelze odstranit účet úložiště < vm-storage účet name >: "účet úložiště < vm-storage účet name > má některé aktivní Image a/nebo disky. Tyto bitové kopie a/nebo disky odebrání před odstraněním tohoto účtu úložiště.'.*

- *Účet úložiště < vm-storage účet name > obsahuje některé aktivní Image a/nebo disky, například xxxxxxxxx-xxxxxxxxx-O-209490240936090599. Ujistěte se, tyto Image a/nebo disky jsou odebrány před odstraněním tohoto účtu úložiště.*

- *Účet úložiště < vm-storage účet name > má 1 kontejnery, které mít aktivní image a/nebo disk artefakty. Ujistěte se, odeberte tyto artefakty z úložiště imagí před odstraněním tohoto účtu úložiště*.

- *Odešlete, že má účet úložiště se nezdařilo < vm-storage účet name > 1 kontejnery, které mít aktivní image a/nebo disk artefakty. Zajistěte, aby že tyto artefakty z úložiště imagí před odstraněním tohoto účtu úložiště. Při pokusu o odstranění účtu úložiště a je stále aktivní disky s ním spojená, zobrazí se zpráva s upozorněním, nejsou aktivní disky, které je potřeba odstranit*.

### <a name="scenario-2-unable-to-delete-a-container"></a>Scénář 2: Nelze odstranit kontejner
Když se pokusíte odstranit kontejner úložiště, může se zobrazit chybová zpráva:

*Nepodařilo se odstranit kontejner úložiště <container name>. Chyba: ' není aktuálně k zapůjčení adresy v kontejneru a žádné ID zapůjčení zadaná v žádosti o*.

Nebo

*Následující disky virtuálního počítače používat objekty BLOB v tomto kontejneru, nelze ho odstranit kontejner: VirtualMachineDiskName1, VirtualMachineDiskName2,...*

### <a name="scenario-3-unable-to-delete-a-vhd"></a>Scénář 3: Nelze odstranit virtuální pevný disk
Po odstranění virtuálního počítače a potom se pokuste odstranit objekty BLOB pro přidružené virtuální pevné disky, zobrazí se následující zpráva:

*Nepodařilo se odstranit objekt blob "cesta/XXXXXX-XXXXXX-os-1447379084699.vhd'. Chyba: ' u objektu blob je momentálně zapůjčení a žádné ID zapůjčení zadaná v žádosti.*

Nebo

*Objekt BLOB BlobName.vhd je používán jako disk virtuálního počítače "VirtualMachineDiskName", nelze ho odstranit objekt blob.*

## <a name="solution"></a>Řešení
Chcete-li vyřešit nejběžnější problémy, zkuste následující metodu:

### <a name="step-1-delete-any-disks-that-are-preventing-deletion-of-the-storage-account-container-or-vhd"></a>Krok 1: Odstranění disky, které brání odstranění účtu úložiště, kontejneru nebo virtuální pevný disk
1. Přepnout [portál Azure classic](https://manage.windowsazure.com/).
2. Vyberte **VIRTUÁLNÍHO počítače** > **disky**.

    ![Obrázek disky na virtuální počítače na portálu Azure classic.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)
3. Vyhledejte disky přidružené k virtuálnímu pevnému disku, kontejneru nebo účtu úložiště, který chcete odstranit. Přidružený virtuální pevný disk, kontejner nebo účet úložiště najdete, když zkontrolujete umístění tohoto disku.

    ![Obrázek, který obsahuje informace o umístění pro disky na portálu Azure classic](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)
4. Odstraňte disky pomocí jedné z následujících metod:

  - Pokud neexistuje žádné virtuální uvedený na **připojené k** pole disku, můžete odstranit disk přímo.

  - Pokud je disk datový disk, postupujte takto:

    1. Zkontrolujte název virtuálního počítače, který je disk připojen k.
    2. Přejděte na **virtuální počítače** > **instance**a vyhledejte virtuálního počítače.
    3. Ujistěte se, že nic aktivně používá disk.
    4. Vyberte **odpojit Disk** v dolní části portálu se odpojit disk.
    5. Přejděte na **virtuální počítače** > **disky**a počkejte **připojené k** pole, které chcete zapnout prázdné. To znamená, že je disk se úspěšně odpojil od virtuálního počítače.
    6. Vyberte **odstranit** v dolní části **virtuální počítače** > **disky** odstranit disk.

  - Pokud je disk disk s operačním systémem ( **obsahuje OS** pole má hodnotu, jako je Windows) a připojené k virtuálnímu počítači, postupujte podle těchto kroků se odstranit virtuální počítač. Disk operačního systému nelze odpojit, takže jsme muset odstranit virtuální počítač k uvolnění zapůjčení.

    1. Zkontrolujte název virtuálního počítače je datový Disk připojen k.  
    2. Přejděte na **virtuální počítače** > **instance**a potom vyberte virtuální počítač, který je disk připojen k.
    3. Ujistěte se, že nic aktivně používá virtuální počítač a virtuální počítač už nepotřebujete.
    4. Vyberte virtuální disk je připojen k, pak vyberte **odstranit** > **odstranění připojených disků**.
    5. Přejděte na **virtuální počítače** > **disky**a počkejte, než se disk zmizí.  Může trvat několik minut, než tuto funkci používat, a budete muset aktualizujte stránku.
    6. Pokud disk nezmizí, počkejte **připojené k** pole, které chcete zapnout prázdné. To znamená, že na disku má plně odpojený od virtuálního počítače.  Pak vyberte disk a vyberte **odstranit** v dolní části stránky se odstranit disk.


   > [!NOTE]
   > Pokud je disk připojen k virtuálnímu počítači, nebude možné jej odstranit. Disky jsou asynchronně odpojit z odstraněné virtuálního počítače. Může trvat několik minut, po odstranění virtuálního počítače pro toto pole, aby vymazány.
   >
   >


### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-the-storage-account-or-container"></a>Krok 2: Odstraňte všechny Image virtuálních počítačů, které brání odstranění účtu úložiště nebo kontejneru
1. Přepnout [portál Azure classic](https://manage.windowsazure.com/).
2. Vyberte **VIRTUÁLNÍHO počítače** > **bitové kopie**a potom odstraňte obrázky, které jsou přidruženy k účtu úložiště, kontejneru nebo virtuální pevný disk.

    Potom se pokuste odstranit účet úložiště, kontejneru nebo virtuální pevný disk znovu.

> [!WARNING]
> Nezapomeňte si před odstraněním účtu zazálohovat všechno, co chcete uložit. Jakmile odstraníte virtuální pevný disk, objektů blob, tabulka, fronta nebo soubor, je trvale odstraněn. Ujistěte se, že prostředek není používán.
>
>

## <a name="about-the-stopped-deallocated-status"></a>O zastaveném (nepřiřazeném) stavu
Bude mít virtuální počítače, které byly vytvořené v modelu nasazení classic a které byly ponechány **zastaveném (nepřiřazeném)** stavu na buď [portál Azure](https://portal.azure.com/) nebo [portál Azure classic](https://manage.windowsazure.com/).

**Portál Azure classic**:

![Zastavit stav (Deallocated) pro virtuální počítače na portálu Azure.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)

**Portál Azure**:

![Zastavit (deallocated) stav pro virtuální počítače na portálu Azure classic.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

Stav "Zastaveném (nepřiřazeném)" uvolní prostředky počítače, jako je například procesoru, paměti a sítě. Disky, ale stále zachovány, takže můžete rychle znovu vytvořit virtuální počítač v případě potřeby. Tyto disky se vytvářejí na virtuálních pevných disků, které by podporovala úložiště Azure. Účet úložiště obsahuje tyto virtuální pevné disky a disky jsou zapůjčení na těchto virtuálních pevných discích.

## <a name="next-steps"></a>Další kroky
* [Odstranit účet úložiště](storage-create-storage-account.md#delete-a-storage-account)
