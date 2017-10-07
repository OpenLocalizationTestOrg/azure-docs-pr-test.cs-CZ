---
title: aaaStorSimple Snapshot Manager a svazky | Microsoft Docs
description: "Popisuje, jak toouse hello tooview modul snap-in konzoly MMC Snapshot Manager zařízení StorSimple a svazky a tooconfigure zálohy."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 78896323-e57c-431e-bbe2-0cbde1cf43a2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: b8ce102d082b7c773d667a9d3ec747be9e1d3064
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-tooview-and-manage-volumes"></a>Použijte tooview StorSimple Snapshot Manager a správa svazků
## <a name="overview"></a>Přehled
Můžete použít hello StorSimple Snapshot Manager **svazky** uzlu (na hello **oboru** podokně) tooselect svazky a zobrazení informací o nich. Hello svazky jsou uvedené jako jednotky, které odpovídají toohello svazky připojené hello hostitele. Hello **svazky** uzlu se zobrazuje místní svazky a svazek typy, které jsou podporuje StorSimple, včetně svazků o zjištěný prostřednictvím použití hello iSCSI a zařízení. 

Další informace o podporovaných svazky, přejděte příliš[podporu pro více typů svazku](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types).

![Seznam svazků v podokně výsledků](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

Hello **svazky** uzlu také umožňuje prohledat znovu nebo odstraňte svazky po StorSimple Snapshot Manager zjistí. 

Tento kurz vysvětluje, jak můžete připojit, inicializovat a formátování svazků a pak použít StorSimple Snapshot Manager do:

* Zobrazení informací o svazcích 
* Odstraňte svazky
* Prohledat znovu svazky 
* Nakonfigurujte vytvoření základního svazku a vytvořte zálohu
* Konfigurace dynamické zrcadleného svazku a vytvořte zálohu

> [!NOTE]
> Všechny hello **svazku** uzlu akce jsou také k dispozici v hello **akce** podokně.
> 
> 

## <a name="mount-volumes"></a>Připojit svazky
Hello použijte následující postup toomount, inicializace a formátování svazků zařízení StorSimple. Tento postup používá Správa disků, systémový nástroj pro správu pevné disky a svazky odpovídající hello nebo oddílů. Další informace o nástroji Správa disků, přejděte příliš[Správa disků](https://technet.microsoft.com/library/cc770943.aspx) na webu Microsoft TechNet hello.

#### <a name="toomount-volumes"></a>toomount svazky
1. V hostitelském počítači spusťte iniciátor iSCSI společnosti Microsoft hello.
2. Zadejte IP adresy rozhraní hello jako hello cílový portál nebo zjišťování IP adresy a připojte toohello zařízení. Po připojení zařízení hello, nebudou hello svazky dostupné tooyour systému Windows. Další informace o použití iniciátoru iSCSI společnosti Microsoft hello, přejděte části toohello "Připojení tooan iSCSI target zařízení" [instalace a konfigurace Microsoft iSCSI Initiator][1].
3. Použijte některou z hello následující možnosti toostart Správa disků:
   
   * Zadejte Diskmgmt.msc hello **spustit** pole.
   * Spusťte správce serveru, rozbalte položku hello **úložiště** uzel a potom vyberte **Správa disků**.
   * Spustit **nástroje pro správu**, rozbalte položku hello **Správa počítače** uzel a potom vyberte **Správa disků**. 
     
     > [!NOTE]
     > Je nutné použít správce oprávnění toorun Správa disků.
     > 
     > 
4. Online trvat hello svazky:
   
   1. V nástroji Správa disků klikněte pravým tlačítkem na jakýkoli svazek označen **Offline**.
   2. Klikněte na tlačítko **aktivovat Disk**. Hello disku by měl být označen **Online** po aktivaci hello.
5. Inicializace hello svazky:
   
   1. Klikněte pravým tlačítkem na hello zjištěné svazky.
   2. V nabídce hello vyberte **inicializovat Disk**.
   3. V hello **inicializovat Disk** dialogové okno, vyberte hello disky mají tooinitialize a pak klikněte na tlačítko **OK**.
6. Formát jednoduchými svazky:
   
   1. Klikněte pravým tlačítkem na svazku, které chcete tooformat.
   2. V nabídce hello vyberte **nový jednoduchý svazek**.
   3. Použijte hello nový jednoduchý svazek Průvodce tooformat hello svazku:
      
      * Zadejte velikost svazku hello.
      * Zadejte písmeno jednotky.
      * Vyberte hello systému souborů NTFS.
      * Zvolte velikost alokační jednotky 64 kB.
      * Proveďte rychlé formátování.
7. Formátování oddílu více svazků. Pokyny naleznete části toohello "Oddíly a svazky" [implementace Správa disků](https://msdn.microsoft.com/library/dd163556.aspx).

## <a name="view-information-about-your-volumes"></a>Zobrazit informace o svých svazků
Použijte následující postup tooview informace o místní a svazků Azure StorSimple hello.

#### <a name="tooview-volume-information"></a>informace o svazku tooview
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager. 
2. V hello **oboru** podokně klikněte na tlačítko hello **svazky** uzlu. Místní a připojené svazky, včetně všech svazků Azure StorSimple, objeví se v hello **výsledky** podokně. Hello sloupců v hello **výsledky** podokně se dají konfigurovat. (Klikněte pravým tlačítkem na hello **svazky** uzlu, vyberte **zobrazení**a potom vyberte **přidat nebo odebrat sloupce**.)
   
    ![Konfigurace sloupce hello](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)
   
   | Sloupec výsledků | Popis |
   |:--- |:--- |
   |  Name (Název) |Hello **název** sloupec obsahuje hello jednotky písmeno přiřazené tooeach zjištěné svazku. |
   |  Zařízení |Hello **zařízení** sloupec obsahuje IP adresu hello hello zařízení připojených toohello hostitelského počítače. |
   |  Název svazku zařízení |Hello **název svazku zařízení** sloupec obsahuje název hello hello zařízení svazku toowhich hello vybraný svazek patří. Toto je název svazku hello definované v hello portál Azure pro tento konkrétní svazek. |
   |  Cesty přístupu |Hello **cesty přístupu** sloupec zobrazuje hello přístup k cestě toohello svazku. Toto je hello písmeno jednotky nebo přípojný bod v které hello svazek je přístupný v hostitelském počítači hello. |

## <a name="delete-a-volume"></a>Odstranění svazku
Použijte následující postup toodelete svazku z StorSimple Snapshot Manager hello.

> [!NOTE]
> Svazek nelze odstranit, pokud je součástí žádné skupiny svazku. (možnost odstranění hello není k dispozici pro svazky, které jsou členy skupiny svazku.) Je nutné odstranit hello celý svazek skupiny toodelete hello svazku.

#### <a name="toodelete-a-volume"></a>toodelete svazku
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně klikněte na tlačítko hello **svazky** uzlu. 
3. V hello **výsledky** podokně, klikněte pravým tlačítkem na hello svazku, které chcete toodelete.
4. V nabídce hello, klikněte na tlačítko **odstranit**. 
   
    ![Odstranění svazku](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 
5. Hello **Odstranit svazek** zobrazí se dialogové okno. Typ **potvrdit** v hello textového pole a pak klikněte na **OK**.
6. Ve výchozím nastavení StorSimple Snapshot Manager zálohuje svazek před odstraněním. Toto opatření vám může chránit před ztrátou dat, pokud nebyla záměrná hello odstranění. Zobrazí StorSimple Snapshot Manager **automatické snímku** zprávu o průběhu během zálohuje hello svazku. 
   
    ![Zpráva automatické snímku](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Prohledat znovu svazky
Použijte následující postup toorescan hello svazky připojené tooStorSimple Snapshot Manager hello.

#### <a name="toorescan-hello-volumes"></a>toorescan hello svazky
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně klikněte pravým tlačítkem na **svazky**a potom klikněte na **Prohledat znovu svazky**.
   
    ![Prohledat znovu svazky](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
   
    Tento postup synchronizuje hello seznam svazků s StorSimple Snapshot Manager. Všechny změny, jako je například nové svazky nebo odstraněné svazky, se zobrazí ve výsledcích hello.

## <a name="configure-and-back-up-a-basic-volume"></a>Konfigurace a zálohovat vytvoření základního svazku
Použijte následující postup tooconfigure zálohu vytvoření základního svazku hello a spustit okamžitě nebo vytvořte zásadu pro plánované zálohování.

### <a name="prerequisites"></a>Požadavky
Než začnete:

* Ujistěte se, zda jsou správně nakonfigurovány hello zařízení StorSimple a hostitelský počítač. Další informace, přejděte příliš[nasazení místního zařízení StorSimple](storsimple-deployment-walkthrough-u2.md).
* Instalace a konfigurace zařízení StorSimple Snapshot Manager. Další informace, přejděte příliš[nasazení zařízení StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).

#### <a name="tooconfigure-backup-of-a-basic-volume"></a>zálohování tooconfigure základního svazku
1. Vytvořte základní svazek v zařízení StorSimple hello.
2. Připojení, inicializace a formátování svazku hello, jak je popsáno v [připojit svazky](#mount-volumes). 
3. Klikněte na ikonu StorSimple Snapshot Manager hello na ploše. Zobrazí se okno Hello StorSimple Snapshot Manager. 
4. V hello **oboru** podokně, klikněte pravým tlačítkem na hello **svazky** uzel a potom vyberte **Prohledat znovu svazky**. Po dokončení kontroly hello seznam svazků by se měla objevit v hello **výsledky** podokně. 
5. V hello **výsledky** podokně klikněte pravým tlačítkem na hello svazek a potom vyberte **vytvořit skupinu svazku**. 
   
    ![Vytvoření svazku skupiny](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 
6. V hello **vytvořit skupinu svazku** dialogové okno, zadejte název pro skupinu hello svazku, přiřadit svazky tooit a pak klikněte na tlačítko **OK**.
7. V hello **oboru** podokně rozbalte hello **svazku skupiny** uzlu. Nová skupina svazku Hello by měl být pod hello **svazku skupiny** uzlu. 
8. Klikněte pravým tlačítkem na název skupiny hello svazku.
   
   * toostart interaktivní (na vyžádání) zálohování úlohu, klikněte na tlačítko **provést zálohování**. 
   * tooschedule automatické zálohování, klikněte na tlačítko **vytvořit zásady zálohování**. Na hello **Obecné** stránky, vyberte skupinu svazku hello seznamu. Na hello **plán** zadejte podrobnosti plánu hello. Až budete hotovi, klikněte na tlačítko **OK**. 
9. bylo zahájeno tooconfirm, který hello úloha zálohování, rozbalte položku hello **úlohy** uzel v hello **oboru** podokně a pak klikněte na tlačítko hello **systémem** uzlu. Hello seznam aktuálně spuštěných úloh se zobrazí v hello **výsledky** podokně. 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Konfigurace a zálohovat dynamické zrcadleného svazku
Proveďte následující kroky tooconfigure zálohování dynamické zrcadleného svazku hello:

* Krok 1: Použití nástroje Správa disků toocreate dynamického zrcadlového svazku. 
* Krok 2: Použití StorSimple Snapshot Manager tooconfigure zálohování.

### <a name="prerequisites"></a>Požadavky
Než začnete:

* Ujistěte se, zda jsou správně nakonfigurovány hello zařízení StorSimple a hostitelský počítač. Další informace, přejděte příliš[nasazení místního zařízení StorSimple](storsimple-8000-deployment-walkthrough-u2.md).
* Instalace a konfigurace zařízení StorSimple Snapshot Manager. Další informace, přejděte příliš[nasazení zařízení StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).
* Nakonfigurujte dva svazky na zařízení StorSimple hello. (V příkladech hello, jsou k dispozici svazky hello **Disk 1** a **disku 2**.) 

### <a name="step-1-use-disk-management-toocreate-a-dynamic-mirrored-volume"></a>Krok 1: Použití nástroje Správa disků toocreate dynamické zrcadleného svazku
Správa disků je systémový nástroj pro správu pevné disky a svazky hello nebo oddíly, které obsahují. Další informace o nástroji Správa disků, přejděte příliš[Správa disků](https://technet.microsoft.com/library/cc770943.aspx) na webu Microsoft TechNet hello.

#### <a name="toocreate-a-dynamic-mirrored-volume"></a>toocreate dynamické zrcadleného svazku
1. Použijte některou z hello následující možnosti toostart Správa disků: 
   
   * Otevřete hello **spustit** zadejte **Diskmgmt.msc**, a stiskněte klávesu Enter.
   * Spusťte správce serveru, rozbalte položku hello **úložiště** uzel a potom vyberte **Správa disků**. 
   * Spustit **nástroje pro správu**, rozbalte položku hello **Správa počítače** uzel a potom vyberte **Správa disků**. 
2. Ujistěte se, že máte dva svazky, které jsou k dispozici pro zařízení StorSimple hello. (V příkladu hello jsou k dispozici svazky hello **Disk 1** a **disku 2**.) 
3. V okně hello Správa disků, v pravém sloupci hello hello spodní části podokna klikněte pravým tlačítkem na **Disk 1** a vyberte **nový svazek zrcadleny**. 
   
    ![Nový zrcadlený svazek](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 
4. Na hello **nový svazek zrcadleny** stránka průvodce, klikněte na tlačítko **Další**.
5. Na hello **vyberte disky** vyberte **disku 2** v hello **vybrané** podokně klikněte na tlačítko **přidat**a pak klikněte na tlačítko **Další**. 
6. Na hello **přiřadit písmeno jednotky nebo cesta** stránky, přijměte výchozí hodnoty hello a pak klikněte na tlačítko **Další**. 
7. Na hello **formátování svazku** stránku hello **velikost alokační jednotky** vyberte **64 tisíc**. Vyberte hello **rychlé formátování** zaškrtněte políčko a potom klikněte na **Další**. 
8. Na hello **dokončení hello nový svazek zrcadleny** stránka, zkontrolujte nastavení a pak klikněte na tlačítko **Dokončit**. 
9. Tooindicate, který hello základní disk bude převedený tooa dynamický disk, zobrazí se zpráva. Klikněte na **Ano**.
   
    ![Zpráva pro převod dynamického disku](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 
10. Ve správě disků ověřte, jestli Disk 1 a 2 disku se zobrazují jako dynamické zrcadlové svazky. (**Dynamické** by se měla objevit ve sloupci Stav hello a barvu panelu kapacity hello by se měl změnit toored, která určuje zrcadleného svazku.) 
    
    ![Dynamické disky správu zrcadlení disku](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 

### <a name="step-2-use-storsimple-snapshot-manager-tooconfigure-backup"></a>Krok 2: Použití StorSimple Snapshot Manager tooconfigure zálohování
Použijte následující postup tooconfigure dynamické zrcadleného svazku hello a spustit okamžitě nebo vytvořte zásadu pro plánované zálohy.

#### <a name="tooconfigure-backup-of-a-dynamic-mirrored-volume"></a>zálohování tooconfigure dynamického zrcadleného svazku
1. Klikněte na ikonu StorSimple Snapshot Manager hello na ploše. Zobrazí se okno Hello StorSimple Snapshot Manager. 
2. V hello **oboru** podokně, klikněte pravým tlačítkem na hello **svazky** uzel a vyberte možnost **Prohledat znovu svazky**. Po dokončení kontroly hello seznam svazků by se měla objevit v hello **výsledky** podokně. dynamické zrcadleného svazku Hello je uveden jako jeden svazek. 
3. V hello **výsledky** podokně klikněte pravým tlačítkem na hello dynamické zrcadleného svazku a pak klikněte na **vytvořit skupinu svazku**. 
4. V hello **vytvořit skupinu svazku** dialogové okno, zadejte název pro skupinu hello svazku, přiřaďte hello dynamické zrcadleného svazku toothis skupinu a potom klikněte na **OK**. 
5. V hello **oboru** podokně rozbalte hello **svazku skupiny** uzlu. Nová skupina svazku Hello by měl být pod hello **svazku skupiny** uzlu. 
6. Klikněte pravým tlačítkem na název skupiny hello svazku. 
   
   * toostart interaktivní (na vyžádání) zálohování úlohu, klikněte na tlačítko **provést zálohování**. 
   * tooschedule automatické zálohování, klikněte na tlačítko **vytvořit zásady zálohování**. Na hello **Obecné** stránky, vyberte hello svazku skupiny ze seznamu hello. Na hello **plán** zadejte podrobnosti plánu hello. Až budete hotovi, klikněte na tlačítko **OK**. 
7. Úloha zálohování hello můžete monitorovat při jeho spuštění. V hello **oboru** podokně rozbalte hello **úlohy** uzel a pak klikněte na tlačítko **systémem**, hello podrobnosti úlohy se zobrazí v hello **výsledky** podokně. Po dokončení úlohy zálohování hello hello podrobnosti jsou přenášená toohello **posledních 24** čas úlohy seznamu. 

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[pomocí vašeho řešení StorSimple Snapshot Manager zařízení StorSimple tooadminister](storsimple-snapshot-manager-admin.md).
* Zjistěte, jak příliš[použít toocreate StorSimple Snapshot Manager a Správa skupin svazku](storsimple-snapshot-manager-manage-volume-groups.md).

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
