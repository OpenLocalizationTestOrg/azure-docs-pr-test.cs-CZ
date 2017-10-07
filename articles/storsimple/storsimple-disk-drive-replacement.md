---
title: "aaaReplace diskovou jednotku na zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak tooreplace disk jednotka v primární skříni StorSimple nebo EBOD skříň."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 98890d36-b613-40fd-994e-330dd907a8a1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: d2c78a6d951b0f00ac42e74a34cf1bc83952a3c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-disk-drive-on-your-storsimple-device"></a>Místo disku v zařízení StorSimple
## <a name="overview"></a>Přehled
V tomto kurzu vysvětluje, jak můžete odeberete a nahradíte nefunkční nebo selhání jednotky pevného disku na zařízení s Microsoft Azure StorSimple. tooreplace diskové jednotce, budete muset:

* Musí se vypnout hello antitamper zámku
* Odebrat hello diskovou jednotku
* Nainstalujte hello nahrazení diskovou jednotku

> [!IMPORTANT]
> Před odebírání a nahrazování diskovou jednotku, zkontrolujte informace o zabezpečení hello v [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="disengage-hello-antitamper-lock"></a>Musí se vypnout hello antitamper zámku
Tento postup vysvětluje, jak můžete hello antitamper zámky v zařízení StorSimple pověření nebo odpojí po nahrazení hello diskových jednotek. zámky antitamper Hello jsou umístěné v obslužných rutin nosného hello jednotky, a jsou přístupná prostřednictvím malou aperturou v části západky hello hello popisovač. Disky se dodává s hello zámky sadu toohello uzamčení pozici.

#### <a name="toounlock-hello-antitamper-lock"></a>toounlock hello antitamper zámku
1. Pečlivě vložte hello zámku klíč ("tamperproof" T10 šroubovák, které poskytuje Microsoft) do otvoru hello ve hello popisovač a do jeho soketu. 
   
   > [!NOTE]
   > Pokud je aktivovaná antitamper zámku hello, je zobrazen v hello otvoru hello red indikátoru.
   > 
   > 
   
    ![Uzamčení diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    **Obrázek 1** proti zfalšování zámku pověření
   
   | Štítek | Popis |
   |:--- |:--- |
   | 1 |Otvoru indikátoru |
   | 2 |Antitamper zámku |
2. Otočte hello klíč v proti směru hodinových ručiček směru, dokud indikátor hello red není zobrazená v otvoru hello výše hello klíč.
3. Klíč hello odeberte.
   
    ![Odemknout diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    **Obrázek 2** Odemknutý diskovou jednotku
4. Hello disku lze nyní odebrat.

Postupujte podle kroků hello v zpětné tooengage hello zámku.

## <a name="remove-hello-disk-drive"></a>Odebrat hello diskovou jednotku
Zařízení StorSimple podporuje konfiguraci RAID 10 jako úložiště prostorů. To znamená, že může fungovat normálně u jeden selhání disk SSD jednotky SSD (Solid-State Drive), nebo jednotku pevného disku (HDD). 

> [!IMPORTANT]
> * Pokud má váš systém více než jednoho disku, který selhal, neodebírejte více než jeden SSD a HDD z hello systému v libovolném bodě v čase. Díky tomu může dojít ke ztrátě dat.
> * Zajistěte, aby umístíte náhradní SSD ve slotu, která dříve obsahovala SSD. Podobně umístěte náhradní pevný disk ve slotu, která dříve obsahovala pevný disk.
> * V hello portál Azure classic, jsou sloty číslo v rozsahu 0 – 11. Proto pokud hello portál ukazuje, že na pozici 2 selhání disku, na zařízení hello hledat hello poškozeném disku ve slotu třetí hello z hello nahoře vlevo.
> 
> 

Jednotky můžete odebrat a nahradit, když je operační systém hello.

#### <a name="tooremove-a-drive"></a>tooremove na jednotku
1. tooidentify hello selhání disku, v portálu Azure classic hello naleznete příliš**zařízení** > **údržby** > **stavu hardwaru**. Vzhledem k tomu, že disk může selhat v primární skříň hello nebo EBOD skříň (Pokud používáte 8600 model), podívejte se na stav hello hello disků v části **sdílené součásti** a v části **EBOD skříň sdílené součásti**. Poškozený disk v buď skříň zobrazí červený stav.
2. Vyhledejte hello jednotky v popředí hello hello primární skříň nebo hello EBOD skříň. 
3. Pokud je odemčený hello disku, pokračujte dalším krokem toohello. Pokud je hello disk, odemknout pomocí následujícího postupu hello v [musí vypnout antitamper zámku hello](#disengage-the-antitamper-lock).
4. Stiskněte klávesu hello černé opatřit na hello jednotky poskytovatel modulu a načítat hello jednotky poskytovatel popisovač odhlašování a rychle z hello přední hello skříní. 
   
    ![Uvolnění popisovač diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    **Obrázek 3** uvolnění hello jednotky popisovač
5. Pokud hello jednotky poskytovatel popisovač je plně rozšířen, posuňte hello jednotky poskytovatel mimo hello skříň. 
   
    ![Klouzavé disku z disku](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    **Obrázek 4** klouzavé hello disku mimo poskytovatel hello

## <a name="install-hello-replacement-disk-drive"></a>Nainstalujte hello nahrazení diskovou jednotku
Po jednotku se nezdařila v zařízení StorSimple a odeberete ji, postupujte podle tohoto postupu tooreplace její na nový disk.

#### <a name="tooinsert-a-drive"></a>tooinsert na jednotku
1. Ujistěte se, že popisovač poskytovatel jednotky hello je plně rozšířené, jak ukazuje následující obrázek hello.
   
    ![Diskovou jednotku s popisovačem rozšířené](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    **Obrázek 5** jednotku s popisovačem rozšířené
2. Vysuňte hello jednotky poskytovatel všechny způsob hello do skříně hello. 
   
    ![Klouzavé disku do poskytovatel diskovou jednotku](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    **Obrázek 6** posuvné hello jednotky poskytovatel do skříně hello
3. S hello jednotky poskytovatel vložené, Zavřít hello jednotky poskytovatel popisovačem při zpracování pokračuje toopush hello jednotky poskytovatel do hello skříň, dokud hello jednotky poskytovatel popisovač přichytí k uzamčení umístění.
4. Použití hello zámku klíč, který byl poskytnut Microsoft (tamperproof Torx šroubovák) toosecure hello poskytovatel popisovač do místní vypnutím hello zámku šroubovacím čtvrtletí zapnout po směru hodinových ručiček.
5. Ověřte, zda hello nahrazení byla úspěšná a hello jednotka je provozní přístup k hello portál Azure classic a navigace příliš**údržby** > **stavu hardwaru**. V části **sdílené součásti** nebo **EBOD skříň sdílené součásti**, musí být ve stavu jednotky hello zeleně, která udává, že je v pořádku.
   
   > [!NOTE]
   > Ho může trvat několik hodin pro tooturn stav disku hello zelená po nahrazení hello.
   > 
   > 

## <a name="next-steps"></a>Další kroky
Další informace o [StorSimple hardwarové součásti nahrazení](storsimple-hardware-component-replacement.md).

