---
title: "aaaUse moderní úložiště záloh s v2 serveru Azure Backup | Microsoft Docs"
description: "Další informace o nových funkcích serveru Azure Backup v2 hello. Tento článek popisuje, jak tooupgrade instalaci zálohování serveru."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: masaran;markgal
ms.openlocfilehash: b2a1ed27a6a682bd611fea1d2df9ef93314404e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="add-storage-tooazure-backup-server-v2"></a>Přidání úložiště tooAzure v2 zálohování serveru

Azure Backup Server v2 se dodává s System Center 2016 Data Protection Manager moderních úložiště záloh. Moderní úložiště záloh nabízí úspory úložiště 50 procent, zálohování, které jsou třikrát rychlejší a efektivnější úložiště. Nabízí také deklaracemi pracovního vytížení úložiště. 

> [!NOTE]
> toouse moderní úložiště záloh, je nutné spustit v2 zálohování serveru v systému Windows Server 2016. Pokud spustíte v2 zálohování serveru ve starší verzi systému Windows Server, Azure Backup Server nelze využít výhod moderní úložiště záloh. Místo toho chrání úlohy, stejně jako s v1 zálohování serveru. Další informace najdete v tématu verze zálohování serveru hello [ochrany matice](backup-mabs-protection-matrix.md).

## <a name="volumes-in-backup-server-v2"></a>Svazky v v2 zálohování serveru

Zálohování serveru v2 přijme svazky úložiště. Při přidání svazek naformátuje zálohovat Server hello tooResilient svazku systému souborů (ReFS), který vyžaduje moderní úložiště záloh. tooadd svazek a tooexpand později Pokud budete potřebovat, doporučujeme použít tento pracovní postup:

1.  Nastavte zálohování serveru v2 na virtuálním počítači.
2.  Vytvoření svazku na virtuálním disku ve fondu úložiště:
    1.  Přidejte fond úložiště disku tooa a virtuální disk vytvořit s jednoduché rozložení.
    2.  Přidejte všechny další disky a rozšířit hello virtuálního disku.
    3.  Vytvořte svazky na virtuálním disku hello.
3.  Přidejte hello svazky tooBackup serveru.
4.  Konfigurace úlohy používající úložiště.

## <a name="create-a-volume-for-modern-backup-storage"></a>Vytvoření svazku pro moderní úložiště záloh

Pomocí zálohování serveru v2 pro svazky, diskového úložiště vám pomůže udržet kontrolu nad úložištěm. Svazek může být jediný disk. Pokud chcete tooextend úložiště v hello budoucí, ale vytvořte svazek mimo disk, který vytvořili pomocí prostorů úložiště. To může pomoct, pokud chcete tooexpand hello svazku pro úložiště záloh. Tato část nabízí osvědčené postupy pro vytváření svazku s tímto nastavením.

1. Ve Správci serveru vyberte **Souborová služba a služba úložiště** > **svazky** > **fondy úložiště**. V části **fyzické disky**, vyberte **nový fond úložiště**. 

    ![Vytvořit nový fond úložiště](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. V hello **úlohy** rozevíracího seznamu vyberte **nový virtuální Disk**.

    ![Přidání virtuálního disku](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. Vyberte fond úložiště hello a pak vyberte **přidat fyzický Disk**.

    ![Přidat fyzický disk](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. Vyberte fyzický disk hello a pak vyberte **zvětšit virtuální Disk**.

    ![Rozšíření virtuálního disku hello](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. Vyberte hello virtuální disk a pak vyberte **nový svazek**.

    ![Vytvořte nový svazek](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. V hello **vyberte hello server a disk** dialogové okno, vyberte hello server a hello nový disk. Pak vyberte **Další**.

    ![Vyberte hello server a disku](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-toobackup-server-disk-storage"></a>Přidat svazky tooBackup serveru diskového úložiště

tooadd tooBackup svazku serveru v hello **správy** podokně Prohledat znovu hello úložiště a pak vyberte **přidat**. Zobrazí se seznam všech hello svazky dostupné toobe přidat pro úložiště záloh serveru. Po přidání dostupných svazků toohello seznam vybraných svazků, můžete přiřadit popisný název toohelp, můžete spravovat. tyto svazky tooReFS tak zálohování serveru můžete použít hello výhod moderní úložiště záloh, vyberte tooformat **OK**.

![Přidat k dispozici svazky](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a>Nastavení úlohy používající úložiště

S deklaracemi zatížení úložiště můžete vybrat hello svazků, které přednostně ukládají určité druhy zátěží. Například můžete nastavit nákladné svazky, které podporují vysoký počet vstupně-výstupních operací na druhý (IOPS) toostore pouze hello úlohy, které vyžadují časté, vysoký počet záloh. Příkladem je SQL Server s protokoly transakcí. Další úlohy, které se dají zálohovat méně často, jako jsou virtuální počítače, můžete zálohovat svazky toolow náklady.

### <a name="update-dpmdiskstorage"></a>Aktualizace DPMDiskStorage

Úlohy využívající úložiště můžete nastavit pomocí rutiny prostředí PowerShell hello aktualizace DPMDiskStorage, která aktualizuje hello vlastnosti svazku ve fondu úložiště hello na serveru Data Protection Manager.

Syntaxe:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```
Hello následující snímek obrazovky ukazuje rutinu Update-DPMDiskStorage hello v okně PowerShell hello.

![Hello aktualizace DPMDiskStorage příkazu v okně PowerShell hello](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

Hello změny, které provedete pomocí prostředí PowerShell se projeví v hello konzoly správce zálohování serveru.

![Disky a svazky v konzole pro správu hello](./media/backup-mabs-add-storage/mabs-add-storage-9.png)

## <a name="next-steps"></a>Další kroky
Když nainstalujete Backup Server, zjistěte, jak tooprepare váš server, nebo začít chránit zatížení.

- [Příprava úlohy zálohování serveru](backup-azure-microsoft-azure-backup.md)
- [Pomocí zálohování serveru tooback server VMware](backup-azure-backup-server-vmware.md)
- [Použít tooback zálohování serveru SQL Server](backup-azure-sql-mabs.md)

