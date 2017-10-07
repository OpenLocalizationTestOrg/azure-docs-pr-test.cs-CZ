---
title: "aaaTroubleshoot odstraňování účtů úložiště Azure, kontejnery nebo virtuální pevné disky v nasazení classic | Microsoft Docs"
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
ms.openlocfilehash: 6bbfa032e1968718c623227bb426d553e2951075
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a>Řešení potíží s odstraňování účtů úložiště Azure, kontejnery nebo virtuální pevné disky v nasazení classic
[!INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Taky může docházet k chybám při pokusu o účtu úložiště Azure hello toodelete, kontejneru nebo virtuální pevný disk v hello [portál Azure](https://portal.azure.com/) nebo hello [portál Azure classic](https://manage.windowsazure.com/). Hello problémů může být způsobeno hello následující okolnosti:

* Při odstranění virtuálního počítače hello disku a virtuálního pevného disku nejsou automaticky odstraněny. Který může být hello důvod selhání v odstranění účtu úložiště. Neodstraňujte jsme hello disku tak, aby můžete hello disku toomount jiným virtuálním Počítačem.
* Na disku nebo hello objektů blob, který je spojen s hello disk stále není zapůjčení.
* Je stále image virtuálního počítače, který používá účet úložiště, kontejner nebo objekt blob.

Pokud není Azure problém řešený v tomto článku, navštivte hello fóra Azure na [MSDN a hello Stack Overflow](https://azure.microsoft.com/support/forums/). Problém můžete zveřejnit na tyto fóra nebo too@AzureSupport na Twitteru. Navíc můžete soubor žádost o podporu Azure tak, že vyberete **získat podporu** na hello [podporu Azure](https://azure.microsoft.com/support/options/) lokality.

## <a name="symptoms"></a>Příznaky
Hello následující části jsou uvedeny běžné chyby, které se může zobrazit při pokusu o účtech Azure storage hello toodelete, kontejnery nebo virtuální pevné disky.

### <a name="scenario-1-unable-toodelete-a-storage-account"></a>Scénář 1: Nelze toodelete účet úložiště
Když přejdete účet úložiště classic toohello hello [portál Azure](https://portal.azure.com/) a vyberte **odstranit**, může být zobrazí se seznam objektů, které brání odstranění účtu úložiště hello:

  ![Obrázek chyby při odstranění účtu úložiště hello](./media/storage-cannot-delete-storage-account-container-vhd/newerror.png)

Když přejdete toohello účet úložiště v hello [portál Azure classic](https://manage.windowsazure.com/) a vyberte **odstranit**, můžete sledovat některé hello následujícím chybám:

- *Účet úložiště StorageAccountName obsahuje Image virtuálních počítačů. Zkontrolujte, zda že jsou tyto Image virtuálních počítačů odebrány před odstraněním tohoto účtu úložiště.*

- *Nepodařilo účet úložiště toodelete < vm-storage účet name >. Účet úložiště nejde toodelete < vm-storage účet name >: "účet úložiště < vm-storage účet name > má některé aktivní Image a/nebo disky. Tyto bitové kopie a/nebo disky odebrání před odstraněním tohoto účtu úložiště.'.*

- *Účet úložiště < vm-storage účet name > obsahuje některé aktivní Image a/nebo disky, například xxxxxxxxx-xxxxxxxxx-O-209490240936090599. Ujistěte se, tyto Image a/nebo disky jsou odebrány před odstraněním tohoto účtu úložiště.*

- *Účet úložiště < vm-storage účet name > má 1 kontejnery, které mít aktivní image a/nebo disk artefakty. Ujistěte se, odeberte tyto artefakty z úložiště imagí hello před odstraněním tohoto účtu úložiště*.

- *Odešlete, že má účet úložiště se nezdařilo < vm-storage účet name > 1 kontejnery, které mít aktivní image a/nebo disk artefakty. Zajistěte, aby že tyto artefakty z úložiště imagí hello před odstraněním tohoto účtu úložiště. Když se pokusíte toodelete účet úložiště a stále aktivní disky s ním spojená, zobrazí se zpráva s upozorněním, nejsou aktivní disky, které je třeba odstranit toobe*.

### <a name="scenario-2-unable-toodelete-a-container"></a>Scénář 2: Nelze toodelete kontejner
Když zkusíte kontejner úložiště hello toodelete, může se zobrazit hello následující chybě:

*Kontejner úložiště se nezdařilo toodelete <container name>. Chyba: ' hello kontejneru je aktuálně zapůjčení a žádné ID zapůjčení byl zadaný v požadavku hello*.

Nebo

*Hello následující disky virtuálního počítače použít objekty BLOB v tomto kontejneru, nelze ho odstranit kontejner hello: VirtualMachineDiskName1, VirtualMachineDiskName2,...*

### <a name="scenario-3-unable-toodelete-a-vhd"></a>Scénář 3: Nelze toodelete virtuální pevný disk
Po odstranění virtuálního počítače a potom zkuste toodelete hello objektů blob pro hello přidružené virtuální pevné disky, se může zobrazit hello následující zprávou:

*Objekt blob se nezdařilo toodelete "cesta/XXXXXX-XXXXXX-os-1447379084699.vhd'. Chyba: ' u objektu blob hello je aktuálně zapůjčení a žádné ID zapůjčení byl zadaný v požadavku hello.*

Nebo

*Objekt BLOB BlobName.vhd je používán jako disk virtuálního počítače, VirtualMachineDiskName', nelze ho odstranit objekt blob hello.*

## <a name="solution"></a>Řešení
tooresolve hello nejběžnějších problémů, zkuste hello následující metodu:

### <a name="step-1-delete-any-disks-that-are-preventing-deletion-of-hello-storage-account-container-or-vhd"></a>Krok 1: Odstranění disky, které brání odstranění účtu úložiště hello, kontejneru nebo virtuální pevný disk
1. Přepínač toohello [portál Azure classic](https://manage.windowsazure.com/).
2. Vyberte **VIRTUÁLNÍHO počítače** > **disky**.

    ![Obrázek disky na virtuální počítače na portálu Azure classic.](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)
3. Vyhledejte hello disky, které jsou přidruženy k účtu úložiště hello, kontejneru nebo virtuálního pevného disku, které chcete toodelete. Pokud zaškrtnete hello umístění disku hello, zjistíte, že hello přidruženého účtu úložiště, kontejneru nebo virtuální pevný disk.

    ![Obrázek, který obsahuje informace o umístění pro disky na portálu Azure classic](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)
4. Odstraňte hello disky pomocí jedné z následujících metod hello:

  - Pokud je uvedený žádný virtuální počítač na hello **připojené k** pole hello disk, můžete odstranit hello disk přímo.

  - Pokud hello disk datový disk, postupujte takto:

    1. Zkontrolujte, zda text hello název Dobrý den, který je připojený virtuální počítač, který hello disku.
    2. Přejděte příliš**virtuální počítače** > **instance**a vyhledejte hello virtuálních počítačů.
    3. Ujistěte se, že nic aktivně používá hello disku.
    4. Vyberte **odpojit Disk** dolnímu hello hello portálu toodetach hello disku.
    5. Přejděte příliš**virtuální počítače** > **disky**a počkat na hello **připojené k** tooturn pole prázdné. To znamená, že hello disku se úspěšně odpojil od hello virtuálních počítačů.
    6. Vyberte **odstranit** hello dolnímu okraji **virtuální počítače** > **disky** toodelete hello disku.

  - Pokud je disk operačního systému disku hello (hello **obsahuje operační systém** pole má hodnotu, jako je Windows) a připojené tooa virtuálních počítačů, postupujte podle těchto kroků toodelete hello virtuálních počítačů. disk s operačním systémem Hello nejde odpojit, abychom měli toodelete hello virtuálních počítačů toorelease hello zapůjčení.

    1. Zkontrolujte název virtuálního počítače hello hello datový Disk je připojen k hello.  
    2. Přejděte příliš**virtuální počítače** > **instance**, a pak vyberte hello virtuální počítač, který hello disku je připojen k.
    3. Ujistěte se, že nic aktivně používá hello virtuálního počítače a že jste už potřebovat hello virtuálního počítače.
    4. Vyberte hello virtuálních počítačů hello disk připojen k, pak vyberte **odstranit** > **hello odstranění připojených disků**.
    5. Přejděte příliš**virtuální počítače** > **disky**a počkat na disku toodisappear hello.  Může trvat několik minut, než tato toooccur a může být nutné stránku hello toorefresh.
    6. Pokud hello disk nezmizí, počkejte hello **připojené k** tooturn pole prázdné. To znamená, že hello disku má plně odpojený od hello virtuálních počítačů.  Potom vyberte hello disk a vyberte **odstranit** dole hello hello stránky toodelete hello disk.


   > [!NOTE]
   > Pokud disk připojené tooa virtuálního počítače, nebudete moct toodelete ho. Disky jsou asynchronně odpojit z odstraněné virtuálního počítače. Může trvat několik minut, po odstranění hello virtuálního počítače pro toto pole tooclear nahoru.
   >
   >


### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-hello-storage-account-or-container"></a>Krok 2: Odstraňte všechny Image virtuálních počítačů, které brání odstranění účtu úložiště hello nebo kontejneru
1. Přepínač toohello [portál Azure classic](https://manage.windowsazure.com/).
2. Vyberte **VIRTUÁLNÍHO počítače** > **bitové kopie**a potom odstraňte hello bitové kopie, které jsou přidruženy k účtu úložiště hello, kontejneru nebo virtuální pevný disk.

    Potom zkuste znovu účet úložiště hello toodelete, kontejneru nebo virtuální pevný disk.

> [!WARNING]
> Být jisti tooback až nic chcete toosave před odstraněním účtu hello. Jakmile odstraníte virtuální pevný disk, objektů blob, tabulka, fronta nebo soubor, je trvale odstraněn. Ujistěte se, že hello prostředek není používán.
>
>

## <a name="about-hello-stopped-deallocated-status"></a>O hello zastaveném (nepřiřazeném) stavu
Virtuální počítače, které byly vytvořené v modelu nasazení classic hello a které byly ponechány bude mít hello **zastaveném (nepřiřazeném)** stav buď hello [portál Azure](https://portal.azure.com/) nebo [portál Azure classic ](https://manage.windowsazure.com/).

**Portál Azure classic**:

![Zastavit stav (Deallocated) pro virtuální počítače na portálu Azure.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)

**Portál Azure**:

![Zastavit (deallocated) stav pro virtuální počítače na portálu Azure classic.](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

Stav "Zastaveném (nepřiřazeném)" uvolní prostředky počítače hello, jako je například hello procesoru, paměti a sítě. Hello disky, ale stále zachovány, takže můžete rychle znovu vytvořit hello virtuálních počítačů v případě potřeby. Tyto disky se vytvářejí na virtuálních pevných disků, které by podporovala úložiště Azure. účet úložiště Hello má tyto virtuální pevné disky a hello disky mají zapůjčení na těchto virtuálních pevných discích.

## <a name="next-steps"></a>Další kroky
* [Odstranit účet úložiště](storage-create-storage-account.md#delete-a-storage-account)
