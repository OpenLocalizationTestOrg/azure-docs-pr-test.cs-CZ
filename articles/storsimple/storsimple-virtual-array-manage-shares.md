---
title: "sdílí aaaManage pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje hello Správce zařízení StorSimple a vysvětluje, jak toouse ho toomanage sdílených složek na pole virtuální zařízení StorSimple."
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: 0a799c83-fde5-4f3f-af0e-67535d1882b6
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 9b57d7ec7c0b7de5a22e1b816daa8852d0f32a48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-shares-on-hello-storsimple-virtual-array"></a>Používat hello sdílené složky toomanage service Manager zařízení StorSimple na hello pole virtuální zařízení StorSimple

## <a name="overview"></a>Přehled

Tento kurz vysvětluje, jak toouse hello toocreate service Manager zařízení StorSimple a správu sdílených složek na pole virtuální zařízení StorSimple.

Hello služby StorSimple Manager zařízení je rozšíření hello portálu Azure, která umožňuje spravovat z jedné webové rozhraní vašeho řešení StorSimple. V přidání toomanaging složkami a svazky můžete použít tooview služby StorSimple Manager zařízení hello a spravovat zařízení, Zobrazit výstrahy, spravovat zásady zálohování a správa hello zálohování katalogu.

## <a name="share-types"></a>Typy sdílené složky

Sdílené složky StorSimple může být:

* **Místně vázaný**: Data v těchto sdílených složek v poli hello zůstává po celou dobu a nebudou distribuována toohello cloudu.
* **Víceúrovňová**: Data v těchto sdílených složek můžete distribuována toohello cloudu. Když vytvoříte sdílenou složku vrstvené, přibližně 10 % prostoru hello je zřízený na místní úrovni hello a 90 % prostoru hello se zřizuje v cloudu hello. Například pokud jste zřídili sdílenou složku 1 TB, 100 GB by nacházet v místním prostoru hello a 900 GB se použije v cloudu hello při hello datové vrstvy. Pak z toho vyplývá, že pokud spustíte mimo všechny hello volné místo na hello zařízení, nejde zřídit vrstvené sdílené složky (protože hello 10 % požadované na hello místní vrstvy nebudete mít k dispozici).

### <a name="provisioned-capacity"></a>Zřízená kapacita

Naleznete toohello následující tabulka pro maximální zřízená kapacita pro každý typ sdílené složky.

| **Identifikátor limit** | **Limit** |
| --- | --- |
| Minimální velikost vrstvené sdílené složky |500 GB |
| Maximální velikost vrstvené sdílené složky |20 TB |
| Minimální velikost místně vázaný sdílené složky |50 GB |
| Maximální velikost místně vázaný sdílené složky |2 TB |

## <a name="hello-shares-blade"></a>okno Hello sdílené složky

Hello **sdílené složky** nabídky na souhrnné okně vaší StorSimple služby zobrazí hello seznamu sdílených složek úložiště v daném poli StorSimple a toomanage, můžete je.

![Okno sdílené složky](./media/storsimple-virtual-array-manage-shares/shares-blade.png)

Sdílenou složku se skládá z řady atributy:

* **Název sdílené složky** – popisný název, který musí být jedinečný a pomáhá identifikovat hello sdílené složky.
* **Stav** – může být online nebo offline. Pokud sdílenou složku offline, uživatelé hello sdílené složky nebudou moct tooaccess ho.
* **Typ** – Určuje, zda je sdílená složka hello **nastavování** (hello výchozí) nebo **místně vázaný**.
* **Kapacita** – určuje hello množství dat použít jako porovnání toohello celkové množství dat, které můžou být uložené ve sdílené složce hello.
* **Popis** – volitelné nastavení, které pomáhá popisují hello sdílené složky.
* **Oprávnění** -hello systému souborů NTFS oprávnění toohello složky, kterou je možné spravovat pomocí Průzkumníka Windows.
* **Zálohování** – v případě, že z hello pole virtuální zařízení StorSimple, se automaticky povolí všechny sdílené složky pro zálohování.

![Podrobnosti o sdílených složek](./media/storsimple-virtual-array-manage-shares/share-details.png)

Používejte hello pokyny v této kurz tooperform hello následující úlohy:

* Přidání sdílené složky
* Upravit sdílené složky
* Sdílenou složku do offline
* Odstranění sdílené složky

## <a name="add-a-share"></a>Přidání sdílené složky

1. Z hello StorSimple služby souhrnu okna, klikněte na tlačítko **+ přidat sdílenou složku** z příkazového řádku hello. Otevře se hello **přidat sdílenou složku** okno.

    ![Přidejte sdílenou složku](./media/storsimple-virtual-array-manage-shares/add-share.png)

2. V hello **přidat sdílenou složku** okně hello následující:
   
    1. V hello **název sdílené složky** pole, zadejte jedinečný název pro vaši sdílenou složku. Název Hello musí být řetězec, který obsahuje 3 too127 znaky.

    2. Volitelný **popis** hello sdílené složky. Popis Hello pomůže identifikovat hello vlastníkům sdílené složky.

    3. V hello **typ** rozevíracího seznamu, zadejte zda toocreate **nastavování** nebo **místně vázaný** sdílet. Pro úlohy, které vyžadují místní záruky, nízkou latenci a vyšší výkon, vyberte **místně vázaný sdílené složky**. Všechna ostatní data, vyberte **nastavování** sdílet.

    4. V hello **kapacity** pole, zadejte velikost hello hello sdílené složky. Vrstvený sdílené složky musí být mezi 500 GB a 20 TB a místně vázaný sdílené složky musí být mezi 50 GB a 2 TB.

    5. V hello **nastavit výchozí úplná oprávnění** pole, přiřadit hello oprávnění toohello uživatele nebo skupinu hello, který přistupuje k této sdílené složce. Zadejte název hello hello uživatele nebo skupiny uživatelů hello v  _john@contoso.com_  formátu. Doporučujeme používat tooaccess oprávnění správce uživatele skupiny (ne jenom jednoho konkrétního uživatele) tooallow tyto sdílené složky. Po přiřazení oprávnění hello sem, pak můžete Průzkumníka souborů toomodify tato oprávnění.
3. Po dokončení konfigurace vaší sdílené složky, klikněte na tlačítko **vytvořit**. Vytvoří se sdílenou složku s hello zadaná nastavení a můžete se zobrazí oznámení. Ve výchozím nastavení zálohování budou povolené pro sdílenou složku hello.
4. tooconfirm, který hello sdílená složka byla úspěšně vytvořil, přejděte toohello **sdílené složky** okno. Měli byste vidět hello sdílené složky uvedené.
   
    ![Úspěšné vytvoření sdílené složky](./media/storsimple-virtual-array-manage-shares/share-success.png)

## <a name="modify-a-share"></a>Upravit sdílené složky

Upravte sdílenou složku, pokud budete potřebovat toochange hello popis hello sdílené složky. Žádné jiné vlastnosti sdílené složky můžete změnit po vytvoření sdílené složky hello.

#### <a name="toomodify-a-share"></a>toomodify sdílené složky

1. Z hello **sdílené složky** nastavit hello StorSimple služby souhrnu okna, vyberte hello virtuální pole, na které hello sdílené složky, které chcete toomodify nachází.
2. **Vyberte** hello sdílenou složku tooview hello aktuální popis a upravit ho.
3. Uložte změny kliknutím hello **Uložit** panelu příkazů. Zadané nastavení uplatní a zobrazí se oznámení.
   
    ![ Upravit sdílené složky](./media/storsimple-virtual-array-manage-shares/share-edit.png)

## <a name="take-a-share-offline"></a>Sdílenou složku do offline

Můžete potřebovat tootake sdílenou složku do offline režimu při plánování toomodify ho nebo odstraňte ji. Když sdílenou složku do režimu offline, není k dispozici pro přístup pro čtení a zápis. Budete potřebovat tootake hello sdílenou složku do offline režimu na hostiteli hello i hello zařízení.

#### <a name="tootake-a-share-offline"></a>tootake sdílenou složku do offline režimu

1. Zajistěte, aby že se v této sdílené složce hello nepoužívá před přepnutím do režimu offline.
2. Provedením následujících kroků hello trvat hello sdílené složky v poli hello offline:
   
    1. Z hello **sdílené složky** nastavit hello StorSimple služby souhrnu okna, vyberte hello virtuální pole, na které hello se nachází sdílená složka je chcete tootake do režimu offline.

    2. **Vyberte** hello sdílené složky a klikněte na tlačítko **...**  (případně klikněte pravým tlačítkem na tento řádek) a z hello kontextové nabídky, vyberte **převést do režimu offline**.
     
        ![Offline sdílené složky](./media/storsimple-virtual-array-manage-shares/shares-offline.png)

    3. Zkontrolujte informace hello v hello **převést do režimu offline** okno a potvrďte, že přijímáte hello operace. Klikněte na tlačítko **převést do režimu offline** sdílené složky hello tootake do režimu offline. Zobrazí se oznámení hello operace probíhá.

    4. tooconfirm, který hello sdílené složky byla úspěšně provedena toohello offline, přejděte **sdílené složky** okno. Měli byste vidět stav hello sdílené složky hello jako v režimu offline.

## <a name="delete-a-share"></a>Odstranění sdílené složky

> [!IMPORTANT]
> Sdílené složky lze odstranit pouze v případě, že je offline.


Proveďte následující kroky toodelete sdílenou složku hello.

#### <a name="toodelete-a-share"></a>toodelete sdílené složky

1. Z hello **sdílené složky** nastavit hello StorSimple služby souhrnu okna, vyberte hello virtuální pole, ve které složce hello chcete toodelete nachází.
2. **Vyberte** hello sdílené složky a klikněte na tlačítko **...**  (případně klikněte pravým tlačítkem na tento řádek) a z hello kontextové nabídky, vyberte **odstranit**.
   
    ![Odstranit sdílenou složku](./media/storsimple-virtual-array-manage-shares/share-delete.png)
3. Zkontrolujte stav hello hello sdílenou složku, kterou chcete toodelete. Pokud není sdílená složka hello chcete toodelete offline, odpojte jej nejprve. Postupujte podle kroků hello v [do offline režimu sdílenou složku](#take-a-share-offline).
4. Po zobrazení výzvy k potvrzení v hello **odstranit** přijměte potvrzení hello a klikněte na tlačítko **odstranit**. sdílené složky Hello bude odstraněno a hello **sdílené složky** okno zobrazuje hello aktualizovat seznam sdílených složek v rámci pole virtuálním hello.

## <a name="next-steps"></a>Další kroky
Zjistěte, jak příliš[klonovat sdílenou složku StorSimple](storsimple-virtual-array-clone.md).

