---
title: "zásady zálohování aaaStorSimple Snapshot Manager | Microsoft Docs"
description: "Popisuje, jak toouse hello toocreate modul snap-in konzoly MMC Snapshot Manager zařízení StorSimple a spravovat zásady zálohování hello, které řídí naplánovaných záloh."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 04415d0b-42f0-4737-8afa-257fb2dbe5d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: c2ae75a8d0568090add6018da18de73eb56e6590
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-toocreate-and-manage-backup-policies"></a>Použít toocreate Snapshot Manager zařízení StorSimple a spravovat zásady zálohování
## <a name="overview"></a>Přehled
Zásady zálohování vytvoří plán pro zálohování dat svazku místně nebo v cloudu hello. Když vytvoříte zásadu zálohování, můžete také zadat zásady uchovávání informací. (Můžete uchovávat maximálně 64 snímky.) Další informace o zásady zálohování najdete v tématu [zálohování typy](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) v [řady StorSimple 8000: hybridní cloudové řešení](storsimple-overview.md).

Tento kurz vysvětluje postup:

* Vytvořit zásady zálohování
* Upravit zásady zálohování
* Odstranit zásady zálohování

## <a name="create-a-backup-policy"></a>Vytvořit zásady zálohování
Použijte následující postup toocreate nové zásady zálohování hello.

#### <a name="toocreate-a-backup-policy"></a>toocreate zásady zálohování
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně klikněte pravým tlačítkem na **zásady zálohování**a klikněte na tlačítko **vytvořit zásadu zálohování**.

    ![Vytvořit zásady zálohování](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    Hello **vytvořit zásadu** zobrazí se dialogové okno.

    ![Vytvoření zásady – karta Obecné](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. Na hello **Obecné** kartě, dokončení hello následující informace:

   1. V hello **název** textového pole zadejte název zásady hello.
   2. V hello **svazku skupiny** textového pole, název typu hello hello svazku skupiny přidružené k hello zásad.
   3. Vyberte buď **místní snímek** nebo **cloudových snímků**.
   4. Vyberte počet hello tooretain snímky. Pokud vyberete **všechny**, budou se uchovávat 64 snímky (hello maximální).
4. Klikněte na tlačítko hello **plán** kartě.

    ![Vytvoření zásady – karta plán](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. Na hello **plán** kartě, dokončení hello následující informace:

   1. Klikněte na tlačítko hello **povolit** políčko tooschedule hello další zálohování.
   2. V části **nastavení**, vyberte **jednou**, **denní**, **týdenní**, nebo **měsíční**.
   3. V hello **spustit** textového pole, klikněte na ikonu hello kalendáře a vyberte počáteční datum.
   4. V části **Upřesnit nastavení**, můžete nastavit volitelné opakování plány a koncové datum.
   5. Klikněte na **OK**.

Po vytvoření zásady zálohování hello následující informace se zobrazí v hello **výsledky** podokně:

* **Název** – hello název zásady zálohování.
* **Typ** – místní snímek nebo cloudový snímek.
* **Svazek skupiny** – hello spojené s hello zásadami skupiny svazku.
* **Uchování** – hello uchovávají počet snímků; hello maximální 64.
* **Vytvořit** – hello datum vytvoření tuto zásadu.
* **Povolit** – jestli hello zásad je aktuálně v platnosti: **True** označuje, že je v platnosti; **False** signalizuje, že není platný.

## <a name="edit-a-backup-policy"></a>Upravit zásady zálohování
Použijte následující postup tooedit existující zásady zálohování hello.

#### <a name="tooedit-a-backup-policy"></a>tooedit zásady zálohování
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně klikněte na tlačítko hello **zásady zálohování** uzlu. Zobrazí všechny zásady zálohování hello v hello **výsledky** podokně.
3. Klikněte pravým tlačítkem na zásadu hello má tooedit a pak klikněte na tlačítko **upravit**.

    ![Upravit zásady zálohování](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. Když hello **vytvořit zásadu** okno se zobrazí, zadejte změny a pak klikněte na tlačítko **OK**.

## <a name="delete-a-backup-policy"></a>Odstranit zásady zálohování
Použijte následující postup toodelete zásady zálohování hello.

#### <a name="toodelete-a-backup-policy"></a>toodelete zásady zálohování
1. Klikněte na ploše ikona toostart hello StorSimple Snapshot Manager.
2. V hello **oboru** podokně klikněte na tlačítko hello **zásady zálohování** uzlu. Zobrazí všechny zásady zálohování hello v hello **výsledky** podokně.
3. Klikněte pravým tlačítkem na zásady zálohování hello má toodelete a pak klikněte na tlačítko **odstranit**.
4. Jakmile se zobrazí potvrzovací zpráva hello, klikněte na tlačítko **Ano**.

    ![Odstranit zásady zálohování potvrzení](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[pomocí vašeho řešení StorSimple Snapshot Manager zařízení StorSimple tooadminister](storsimple-snapshot-manager-admin.md).
* Zjistěte, jak příliš[použít tooview Snapshot Manager zařízení StorSimple a spravovat úlohy zálohování](storsimple-snapshot-manager-manage-backup-jobs.md).
