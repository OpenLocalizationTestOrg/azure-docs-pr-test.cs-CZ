---
title: aaaStorSimple Snapshot Manager svazku skupiny | Microsoft Docs
description: "Popisuje, jak toouse hello toocreate modul snap-in konzoly MMC StorSimple Snapshot Manager a Správa skupin svazku."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 7a232414-6a28-4b81-bd7b-cf61e28b33d7
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: f7830b62761db7aa5139456b555b6168fb6a85de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-toocreate-and-manage-volume-groups"></a>Použít toocreate StorSimple Snapshot Manager a Správa skupin svazku
## <a name="overview"></a>Přehled
Můžete použít hello **svazku skupiny** uzlu na hello **oboru** podokně tooassign svazky toovolume skupiny, zobrazení informací o skupinu svazku naplánovat zálohování a upravit skupiny pro svazek.

Svazek skupiny jsou fondy související svazky, které používají tooensure zálohy jsou konzistentní s aplikací. Další informace najdete v tématu [svazky a svazek skupiny](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) a [integrace s Windows služby Stínová kopie svazku](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

> [!IMPORTANT]
> * Všechny svazky ve skupině svazku musí pocházet ze zprostředkovatele služeb jeden cloud.
> * Když konfigurujete skupiny svazku, nemíchat sdílených svazcích clusteru (CSV) a jiný CSV hello stejnou skupinu svazku. Snapshot Manager zařízení StorSimple nepodporuje kombinaci sdílených svazků clusteru a bez CSV v hello stejné snímku.

![Uzel skupiny svazku](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Obrázek 1: Uzel StorSimple Snapshot Manager svazku skupiny** 

Tento kurz vysvětluje, jak můžete pomocí StorSimple Snapshot Manager do:

* Zobrazení informací o svazku skupin
* Vytvořte skupinu svazku
* Zálohování svazku skupiny
* Úprava skupiny svazku
* Odstranit skupinu svazku

Všechny tyto akce jsou také k dispozici na hello **akce** podokně.

## <a name="view-volume-groups"></a>Zobrazit svazku skupiny
Pokud kliknete na tlačítko hello **svazku skupiny** uzlu, hello **výsledky** podokně zobrazí hello následující informace o každé skupiny svazku, v závislosti na vybrané sloupce hello provedete. (hello sloupců v hello **výsledky** podokně se dají konfigurovat. Klikněte pravým tlačítkem na hello **svazky** uzlu, vyberte **zobrazení**a potom vyberte **přidat nebo odebrat sloupce**.)

| Sloupec výsledků | Popis |
|:--- |:--- |
| Name (Název) |Hello **název** sloupec obsahuje název hello hello svazku skupiny. |
| Aplikace |Hello **aplikace** sloupci se zobrazuje počet hello zapisovače VSS aktuálně nainstalovaná a spuštěná hostitele Windows hello. |
| Vybráno |Hello **vybrané** sloupci se zobrazuje hello počtu svazků, které jsou obsaženy ve skupině hello svazku. Nula (0) označuje, že žádná aplikace je přidružen hello svazky ve skupině hello svazku. |
| Importovat |Hello **importovaný** sloupci se zobrazuje hello počtu importovaných svazků. Pokud nastavíte příliš**True**, tento sloupec označuje, že svazek skupinu byla naimportována ze hello portál Azure a nebyl vytvořen v zařízení StorSimple Snapshot Manager. |

> [!NOTE]
> Skupiny svazek StorSimple Snapshot Manager se zobrazuje také na hello **zásady zálohování** ve hello portálu Azure.
> 
> 

## <a name="create-a-volume-group"></a>Vytvořte skupinu svazku
Použijte následující postup toocreate skupinu svazku hello.

#### <a name="toocreate-a-volume-group"></a>toocreate skupinu svazku
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně klikněte pravým tlačítkem na **svazku skupiny**a potom klikněte na **vytvořit skupinu svazku**.
   
    ![Vytvoření svazku skupiny](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
   
    Hello **vytvořte skupinu svazku** zobrazí se dialogové okno.
   
    ![Vytvoření skupiny dialogu svazku](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png)
3. Zadejte hello následující informace:
   
   1. V hello **název** zadejte jedinečný název pro novou skupinu svazku hello.
   2. V hello **aplikace** pole, vyberte aplikací přidružených k hello svazky, že budete přidávat skupiny toohello svazku.
      
       Hello **aplikace** seznamy pro je povoleno pouze aplikace, které používají svazky zařízení StorSimple a mají zapisovače služby Stínová kopie svazku. Zapisovač VSS je povoleno, jen pokud všechny svazky hello je vědět, že zapisovač hello jsou svazky zařízení StorSimple. Pokud pole aplikace hello je prázdná, jsou nainstalovány žádné aplikace, které používají Azure StorSimple svazky a podporovaná zapisovače služby Stínová kopie svazku. (V současné době Azure StorSimple podporuje Microsoft Exchange a SQL Server.) Další informace o zapisovače VSS najdete v tématu [integrace s Windows služby Stínová kopie svazku](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).
      
       Pokud vyberete aplikace, automaticky se vyberou všechny svazky, které jsou s ním spojená. Naopak, pokud jste vybrali svazky, které jsou přidruženy k určité aplikaci, aplikace hello je automaticky vybrán v hello **aplikace** pole. 
   3. V hello **svazky** vyberte skupinu, StorSimple svazky tooadd toohello svazku. 
      
      * Můžete zahrnout svazků s jednoho nebo více oddílů. (Více svazků oddílu může být dynamické disky nebo základní disky s více oddílů.) Svazek, který obsahuje více oddílů je považována za jednu jednotku. V důsledku toho, pokud přidáte pouze jeden svazek skupiny hello oddíly tooa, všechny hello oddíly jsou automaticky přidané toothat svazku skupina na hello stejný čas. Po přidání více tooa svazku skupinu svazku oddílu hello více svazku oddílu pokračuje toobe považován za jednu jednotku.
      * Můžete vytvořit prázdný svazku skupiny není přiřazením toothem žádné svazky. 
      * Nemíchat sdílených svazcích clusteru (CSV) a jiný CSV hello stejnou skupinu svazku. Snapshot Manager zařízení StorSimple nepodporuje směs Sdílené svazky clusteru a bez Sdílené svazky clusteru v hello stejné snímku.
4. Klikněte na tlačítko **OK** toosave hello svazku skupiny.

## <a name="back-up-a-volume-group"></a>Zálohování svazku skupiny
Použijte následující postup tooback skupiny svazku hello.

#### <a name="tooback-up-a-volume-group"></a>tooback skupiny svazku
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně rozbalte hello **svazku skupiny** uzel, klikněte pravým tlačítkem na název skupiny svazku a pak klikněte na tlačítko **provést zálohování**.
   
    ![Zálohování svazku skupiny okamžitě](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)
3. V hello **provést zálohování** dialogové okno, vyberte **místní snímek** nebo **Cloudový snímek**a potom klikněte na **vytvořit**.
   
    ![Trvat zálohování dialogové okno](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png)
4. je spuštěna tooconfirm, který hello zálohování, rozbalte položku hello **úlohy** uzel a pak klikněte na tlačítko **systémem**. Hello zálohování by měl být uvedený.
5. hello tooview dokončit snímku, rozbalte položku hello **katalog zálohování** uzlu, rozbalte název skupiny svazku hello a pak klikněte na tlačítko **místní snímek** nebo **Cloudový snímek**. zálohování Hello se objeví, pokud byl úspěšně dokončen.

## <a name="edit-a-volume-group"></a>Úprava skupiny svazku
Použijte následující postup tooedit skupinu svazku hello.

#### <a name="tooedit-a-volume-group"></a>tooedit skupinu svazku
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně rozbalte hello **svazku skupiny** uzel, klikněte pravým tlačítkem na název skupiny svazku a pak klikněte na tlačítko **upravit**.
3. Hello ** vytvořte skupinu svazku ** zobrazí se dialogové okno. Můžete změnit hello **název**, **aplikace**, a **svazky** položky.
4. Klikněte na tlačítko **OK** toosave změny.

## <a name="delete-a-volume-group"></a>Odstranit skupinu svazku
Použijte následující postup toodelete skupinu svazku hello. 

> [!WARNING]
> To také odstraní všechny zálohy hello přidružené skupině hello svazku.
> 
> 

#### <a name="toodelete-a-volume-group"></a>toodelete skupinu svazku
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně rozbalte hello **svazku skupiny** uzel, klikněte pravým tlačítkem na název skupiny svazku a pak klikněte na tlačítko **odstranit**.
3. Hello **odstranit skupinu svazku** zobrazí se dialogové okno. Typ **potvrdit** v hello textového pole a pak klikněte na **OK**.
   
    skupiny svazku Hello odstranit zmizí ze seznamu hello v hello **výsledky** podokně a všechny zálohy, které jsou přidružené k této skupině svazku se odstraní z katalogu zálohování hello.

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[pomocí vašeho řešení StorSimple Snapshot Manager zařízení StorSimple tooadminister](storsimple-snapshot-manager-admin.md).
* Zjistěte, jak příliš[použít toocreate Snapshot Manager zařízení StorSimple a spravovat zásady zálohování](storsimple-snapshot-manager-manage-backup-policies.md).

