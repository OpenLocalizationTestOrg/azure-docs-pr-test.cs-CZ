---
title: "aaaManage svazky na pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje hello Správce zařízení StorSimple a vysvětluje, jak toouse ho toomanage svazky na pole virtuální zařízení StorSimple."
services: storsimple
documentationcenter: 
author: manuaery
manager: syadav
editor: 
ms.assetid: caa6a26b-b7ba-4a05-b092-1a79450225cf
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 46aa6d7508b3e62f75a3b78ed73302b88320a0f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-device-manager-service-toomanage-volumes-on-hello-storsimple-virtual-array"></a>Pomocí Správce zařízení StorSimple služby toomanage svazky na hello pole virtuální zařízení StorSimple

## <a name="overview"></a>Přehled

Tento kurz vysvětluje, jak toouse hello toocreate service Manager zařízení StorSimple a spravovat svazky na pole virtuální zařízení StorSimple.

Hello služby StorSimple Manager zařízení je rozšíření hello portálu Azure, která umožňuje spravovat z jedné webové rozhraní vašeho řešení StorSimple. V přidání toomanaging složkami a svazky můžete použít tooview služby StorSimple Manager zařízení hello a spravovat zařízení, zobrazovat výstrahy a zobrazení a Správa zásady zálohování a zálohování katalogu hello.

## <a name="volume-types"></a>Typy svazku

Svazky zařízení StorSimple může být:

* **Místně vázaný**: Data v těchto svazcích zůstává v poli hello za všech okolností a nebudou distribuována toohello cloudu.
* **Víceúrovňová**: Data v těchto svazků můžete distribuována toohello cloudu. Když vytvoříte vrstvený svazek, na místní úrovni hello je zřízený přibližně 10 % hello místa a 90 % prostoru hello se zřizuje v cloudu hello. Například pokud jste zřídili svazku 1 TB, 100 GB by nacházet v místním prostoru hello a 900 GB se použije v cloudu hello při hello datové vrstvy. Pak z toho vyplývá, že pokud spustíte mimo všechny hello volné místo na hello zařízení, nejde zřídit vrstvený svazek (protože hello 10 % požadované na hello místní vrstvy nebudete mít k dispozici).

### <a name="provisioned-capacity"></a>Zřízená kapacita
Naleznete toohello následující tabulka pro maximální zřízená kapacita pro každý typ svazku.

| **Identifikátor limit**                                       | **Limit**     |
|------------------------------------------------------------|---------------|
| Minimální velikost vrstvený svazek                            | 500 GB        |
| Maximální velikost vrstvený svazek                            | 5 TB          |
| Minimální velikost místně vázaný svazek                    | 50 GB         |
| Maximální velikost místně vázaný svazek                    | 500 GB        |

## <a name="hello-volumes-blade"></a>okno svazky Hello
Hello **svazky** nabídky na souhrnné okně vaší StorSimple služby zobrazí hello seznam svazků úložiště v daném poli StorSimple a toomanage, můžete je.

![Okno svazky](./media/storsimple-virtual-array-manage-volumes/volumes-blade.png)

Svazek se skládá z řady atributy:

* **Název svazku** – popisný název, který musí být jedinečný a pomáhá identifikovat hello svazku.
* **Stav** – může být online nebo offline. Pokud je svazek offline, není viditelné tooinitiators (servery) povolený přístup toouse hello svazku.
* **Typ** – Určuje, zda je svazek hello **nastavování** (hello výchozí) nebo **místně vázaný**.
* **Kapacita** – určuje hello množství dat použít jako porovnání toohello celkové množství dat, které můžou být uložené ve iniciátor hello (server).
* **Zálohování** – v případě, že z hello pole virtuální zařízení StorSimple, jsou všechny svazky automaticky povolené pro zálohování.
* **Připojení hostitele** – určuje hello iniciátorů (serverů), které jsou povoleny přístup toothis svazku.

![Podrobnosti o svazky](./media/storsimple-virtual-array-manage-volumes/volume-details.png)

Používejte hello pokyny v této kurz tooperform hello následující úlohy:

* Přidání svazku
* Upravit svazku
* Svazek převést do režimu offline
* Odstranění svazku

## <a name="add-a-volume"></a>Přidání svazku

1. Z hello StorSimple služby souhrnu okna, klikněte na tlačítko **+ přidat svazek** z příkazového řádku hello. Otevře se hello **Přidání svazku** okno.
   
    ![Přidání svazku](./media/storsimple-virtual-array-manage-volumes/add-volume.png)
2. V hello **Přidání svazku** okně hello následující:
   
   * V hello **název svazku** pole, zadejte jedinečný název pro svazek. Název Hello musí být řetězec, který obsahuje 3 too127 znaky.
   * V hello **typ** rozevíracího seznamu, zadejte zda toocreate **nastavování** nebo **místně vázaný** svazku. Pro úlohy, které vyžadují místní záruky, nízkou latenci a vyšší výkon, vyberte **místně vázaný svazek**. Všechna ostatní data, vyberte **nastavování** svazku.
   * V hello **kapacity** pole, zadejte hello velikost svazku hello. Vrstvený svazek musí být mezi 500 GB a 5 TB a místně vázaný svazek musí být mezi 50 GB a 500 GB.
   * * Klikněte na tlačítko **připojení hostitele**, vyberte přístup k řízení záznamu (ACR) odpovídající toohello iniciátor iSCSI má tooconnect toothis svazek a pak klikněte na tlačítko **vyberte**.
3. Klikněte na tlačítko tooadd na nového hostitele připojené **přidat nový**, zadejte název hostitele hello a jeho iSCSI kvalifikovaný název IQN () a pak klikněte na tlačítko **přidat**.
   
    ![Přidání svazku](./media/storsimple-virtual-array-manage-volumes/volume-add-acr.png)
4. Po dokončení konfigurace svazku, klikněte na tlačítko **vytvořit**. Vytvoří se svazek s hello zadaná nastavení a můžete se zobrazí oznámení o úspěšném vytvoření hello hello stejné. Ve výchozím nastavení zálohování budou povolené pro svazek hello.
5. tooconfirm, který hello svazek byl úspěšně vytvořil, přejděte toohello **svazky** okno. Měli byste vidět hello svazku uvedené.
   
    ![Úspěšné vytvoření svazku](./media/storsimple-virtual-array-manage-volumes/volume-success.png)

## <a name="modify-a-volume"></a>Upravit svazku

Upravte svazek, když potřebujete toochange hello hostitele, kteří přístup hello svazku. Hello ostatní atributy svazek nelze změnit po vytvoření svazku hello.

#### <a name="toomodify-a-volume"></a>toomodify svazku

1. Z hello **svazky** nastavit hello StorSimple služby souhrnu okna, vyberte hello virtuální pole, na které hello svazku, které chcete toomodify nachází.
2. **Vyberte** hello svazku a klikněte na tlačítko **připojení hostitele** tooview hello aktuálně připojené hostitele a upravit ho tooa jiný server.
   
    ![Upravit svazku](./media/storsimple-virtual-array-manage-volumes/volume-edit-acr.png)
3. Uložte změny kliknutím hello **Uložit** panelu příkazů. Zadané nastavení uplatní a zobrazí se oznámení.

## <a name="take-a-volume-offline"></a>Svazek převést do režimu offline

Můžete potřebovat tootake svazek offline při plánování toomodify ho nebo odstraňte ji. Pokud je svazek offline, není k dispozici pro přístup pro čtení a zápis. Budete potřebovat tootake hello svazku do offline režimu na hostiteli hello i hello zařízení.

#### <a name="tootake-a-volume-offline"></a>tootake svazek offline

1. Ujistěte se, že svazek hello dotyčném není používán před přepnutím do režimu offline.
2. Trvat hello svazku do offline režimu na hostiteli hello první. Tím se eliminuje všechny potenciální riziko poškození dat na svazku hello. Konkrétní kroky najdete v části toohello pokyny pro operační systém hostitele.
3. Po hello svazek na hostiteli hello je offline, trvat hello svazek v poli hello offline provedením hello následující kroky:
   
   * Z hello **svazky** nastavit hello StorSimple služby souhrnu okna, vyberte hello virtuální pole, na které hello nachází svazku můžete chcete tootake do režimu offline.
   * **Vyberte** hello svazku a klikněte na tlačítko **...**  (případně klikněte pravým tlačítkem na tento řádek) a z hello kontextové nabídky, vyberte **převést do režimu offline**.
     
        ![Offline svazku](./media/storsimple-virtual-array-manage-volumes/volume-offline.png)
   * Zkontrolujte informace hello v hello **převést do režimu offline** okno a potvrďte, že přijímáte hello operace. Klikněte na tlačítko **převést do režimu offline** tootake hello svazek do režimu offline. Zobrazí se oznámení hello operace probíhá.
   * tooconfirm, který hello svazek byl úspěšně do režimu offline, přejděte toohello **svazky** okno. Měli byste vidět hello stav svazku hello jako offline.
     
       ![Potvrzení offline svazku](./media/storsimple-virtual-array-manage-volumes/volume-offline-confirm.png)

## <a name="delete-a-volume"></a>Odstranění svazku

> [!IMPORTANT]
> Svazek můžete odstranit pouze v případě, že je offline.
> 
> 

Proveďte následující kroky toodelete svazek hello.

#### <a name="toodelete-a-volume"></a>toodelete svazku

1. Z hello **svazky** nastavit hello StorSimple služby souhrnu okna, vyberte hello virtuální pole, na které hello svazku, které chcete toodelete nachází.
2. **Vyberte** hello svazku a klikněte na tlačítko **...**  (případně klikněte pravým tlačítkem na tento řádek) a z hello kontextové nabídky, vyberte **odstranit**.
   
    ![Odstranění svazku](./media/storsimple-virtual-array-manage-volumes/volume-delete.png)
3. Zkontrolujte stav hello hello svazku chcete toodelete. Pokud chcete toodelete svazku hello není v režimu offline, proveďte ho offline hello první, následující kroky [do offline režimu svazku](#take-a-volume-offline).
4. Po zobrazení výzvy k potvrzení v hello **odstranit** přijměte potvrzení hello a klikněte na tlačítko **odstranit**. Hello svazek bude odstraněno a hello **svazky** okno se zobrazí hello aktualizovat seznam svazků v rámci pole virtuálním hello.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak příliš[klonovat svazek StorSimple](storsimple-virtual-array-clone.md).

