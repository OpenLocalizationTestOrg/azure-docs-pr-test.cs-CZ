---
title: "aaaManage formátujte svazky zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak tooadd, upravit, monitorování a odstranit svazky zařízení StorSimple a tootake je v režimu offline v případě potřeby."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ccabd859-590c-4568-a81d-6ff38f125be3
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/11/2016
ms.author: v-sharos
ms.openlocfilehash: 8dc646261ee2ad8f2b80291518716f4de55b63f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-volumes"></a>Použít svazky toomanage služby StorSimple Manager hello
[!INCLUDE [storsimple-version-selector-manage-volumes](../../includes/storsimple-version-selector-manage-volumes.md)]

## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak toouse hello toocreate služby StorSimple Manager a spravovat svazky na zařízení StorSimple hello a virtuální zařízení StorSimple.

Hello služby StorSimple Manager je rozšířením hello portál Azure classic, která umožňuje spravovat z jedné webové rozhraní vašeho řešení StorSimple. Ve svazcích toomanaging přidání můžete použít toocreate služby StorSimple Manager hello a spravovat služby StorSimple, zobrazit a spravovat zařízení, zobrazovat výstrahy a zobrazit a spravovat zásady zálohování a zálohování katalog hello.

> [!NOTE]
> Azure StorSimple lze vytvářet pouze dynamicky zřizované svazky. Nelze vytvářet zcela či částečně zřizované svazky v systému Azure StorSimple.
> 
> Dynamické zajišťování je technologie virtualizace, ve kterém úložiště k dispozici, zobrazí se tooexceed fyzické prostředky. Místo předem rezervování dostatečné úložiště, Azure StorSimple používá dynamického zřizování tooallocate akorát toomeet aktuální požadavky na volné místo. Hello elastické povaha cloudového úložiště usnadňuje tento přístup, protože Azure StorSimple můžete zvýšit nebo snížit cloudové úložiště toomeet splněné měnící se požadavky.
> 
> 

## <a name="hello-volumes-page"></a>Svazky stránku Hello
Hello **svazky** stránka vám umožní toomanage hello úložné svazky, které jsou zřízené v hello Microsoft Azure StorSimple zařízení pro vaše iniciátorů (serverů). Zobrazuje hello seznam svazků zařízení StorSimple.

 ![Stránka svazky](./media/storsimple-manage-volumes/HCS_VolumesPage.png)

Svazek se skládá z řady atributy:

* **Název** – popisný název, který musí být jedinečný a pomáhá identifikovat hello svazku. Tento název se také používá v monitorování sestavy při filtrování na konkrétním svazku.
* **Stav** – může být online nebo offline. Pokud svazek, pokud je offline, není viditelné tooinitiators (servery) povolený přístup toouse hello svazku.
* **Kapacita** – Určuje, jak velkého objemu hello je, kterou posuzuje iniciátor hello (server). Kapacita určuje hello celkové množství dat, které můžou být uložené ve iniciátor hello (server). Svazky jsou tence zřízený a data se odstraňují. To znamená, že zařízení není předem přidělit kapacita fyzické úložiště interně, nebo v cloudu hello podle tooconfigured kapacita svazku. kapacita svazku Hello je přidělené a využívat na vyžádání.
* **Typ** – typ svazku hello může být vrstvené nebo archivace (dílčí typ víceúrovňová)
* **Přístup k** – určuje hello iniciátorů (serverů), které jsou povoleny přístup toothis svazku. Iniciátory, které nejsou členy záznam řízení přístupu (ACR), který je přidružený svazek hello neuvidí hello svazku.
* **Monitorování** – Určuje, jestli je monitorována svazku. Svazek bude mít povoleno ve výchozím nastavení při vytvoření monitorování. Monitorování bude, ale, bude zakázán pro klonování svazku. tooenable monitorování pro svazek, postupujte podle pokynů hello monitorování svazku.

Hello nejčastější úkoly spojené se svazkem jsou:

* Přidání svazku
* Upravit svazku
* Odstranění svazku
* Svazek převést do režimu offline
* Monitorování svazku

## <a name="add-a-volume"></a>Přidání svazku
Můžete [vytvořili svazek](storsimple-deployment-walkthrough-u1.md#step-6-create-a-volume) během nasazování vašeho řešení StorSimple. Přidání svazku je podobným způsobem.

### <a name="tooadd-a-volume"></a>tooadd svazku
1. Na hello **zařízení** vyberte hello zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko hello **kontejnery svazků** kartě.
2. Vyberte kontejner svazků a klikněte na šipku hello v hello odpovídající řádek tooaccess hello svazky přidružené hello kontejneru.
3. Klikněte na tlačítko **přidat** na hello dolní části stránky hello. Spustí Hello Průvodci přidáním svazku.
   
     ![Průvodce přidáním svazku základní nastavení](./media/storsimple-manage-volumes/AddVolume1.png)
4. V hello Průvodce přidáním svazku, v části **základní nastavení**, hello následující:
   
   1. Zadejte **Název** svazku.
   2. Zadejte hello **zřízená kapacita** svazku v GB nebo TB. Hello kapacita musí být mezi 1 GB a 64 TB pro fyzické zařízení. Maximální kapacita Hello, který může být zřízen svazek na virtuální zařízení StorSimple je 30 TB.
   3. Vyberte hello **typ použití** svazku. Pokud používáte vrstvený svazek pro archivní data, výběr hello hello **použít tento svazek pro archivní data s méně často používaná** políčko změní velikost bloku odstranění duplicit hello svazku too512 KB. Pokud tuto možnost nevyberete, hello příslušný vrstvený svazek bude používat velikost bloku dat 64 KB. Větší velikost bloku odstranění duplicitních dat umožňuje hello zařízení tooexpedite hello přenos velkých objemů archivních dat toohello cloudu. (Vrstvené svazky se dřív označovaných jako primární svazky.)
   4. Klikněte na ikonu šipky hello ![ikonu šipky](./media/storsimple-manage-volumes/HCS_ArrowIcon.png)toogo toohello **další nastavení** stránky.
      
        ![Přidat další nastavení Průvodce svazku](./media/storsimple-manage-volumes/AddVolume2.png)
5. V části **další nastavení**, přidejte nový záznam řízení přístupu (ACR):
   
   1. Hello rozevíracím seznamu vyberte záznam řízení přístupu (ACR). Alternativně můžete přidat nové ACR. ACRs určit, které hostitele můžou pro přístup svých svazků odpovídající hello IQN hostitele s uvedena v záznamu hello.
   2. Doporučujeme povolit výchozí zálohování tak, že vyberete hello **povolit výchozí zálohování tohoto svazku** zaškrtávací políčko.
   3. Klikněte na ikonu zaškrtnutí hello ![Ikona zaškrtnutí](./media/storsimple-manage-volumes/HCS_CheckIcon.png) toocreate hello svazek s hello zadat nastavení.

Nový svazek je nyní připraven toouse.

## <a name="modify-a-volume"></a>Upravit svazku
Upravte svazek, když potřebujete tooexpand ho nebo změňte hello hostitele, kteří přístup hello svazku.

> [!IMPORTANT]
> * Pokud změníte velikost svazku hello na hello zařízení, musí velikost svazku hello toobe změnit také v hostiteli se hello.
> * Hello hostitelské kroků popsaných v tomto poli se pro Windows Server 2012 (2012 R2). Postupy pro Linux nebo jiných hostitelských operačních systémech se liší. Tooyour hostitele operačního systému pokyny najdete v případě úpravy hello svazek na hostitele s jiným operačním systémem.
> 
> 

### <a name="toomodify-a-volume"></a>toomodify svazku
1. Na hello **zařízení** vyberte hello zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko hello **kontejneru svazků** kartě. Tato stránka obsahuje seznam v tabulkovém formátu všechny hello kontejnery svazků, které jsou přidruženy hello zařízení.
2. Vyberte kontejner svazků a klikněte na něj toodisplay hello seznam všech svazků hello hello kontejneru.
3. Na hello **svazky** , vyberte svazek a klikněte na tlačítko **upravit**.
4. V hello upravte průvodce svazku, v části **základní nastavení**, můžete provést následující hello:
   
   * Upravit hello **název** a **typ** Pokud chcete toomodify svazek archivace tooan vrstvený svazek výběrem hello **použít tento svazek pro archivní data s méně často používaná** velikost bloku políčko toochange hello odstranění duplicitních dat pro svazek too512 KB.
   * Zvýšit hello **zřízená kapacita**. Hello **zřízená kapacita** může být pouze zvýšena. Po vytvoření, nelze zmenšit svazek.
     
     > [!NOTE]
     > Kontejner svazků hello nelze změnit po je přiřazen tooa svazku.
     > 
     > 
5. V části **další nastavení**, můžete provést následující hello:
   
   * Upravte hello ACRs za předpokladu, že svazek hello je offline. Pokud je svazek hello online, budete potřebovat tootake ho offline první. Najdete kroky toohello v [do offline režimu svazek](#take-a-volume-offline) předchozí toomodifying hello ACR.
   * Upravte hello seznam ACRs po hello svazek je offline.
     
     > [!NOTE]
     > Nelze změnit hello **povolit výchozí zálohování tohoto svazku** možnost pro svazek hello.
     > 
     > 
6. Uložte změny kliknutím na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-manage-volumes/HCS_CheckIcon.png). Hello portál Azure classic zobrazí zprávu aktualizací svazku. Zpráva o úspěšném provedení zobrazí při hello svazku byla úspěšně aktualizována.
7. Pokud se rozšířit svazek, proveďte následující kroky v hostitelském počítači Windows hello:
   
   1. Přejděte příliš**Správa počítače** ->**Správa disků**.
   2. Klikněte pravým tlačítkem na **Správa disků** a vyberte **Prohledat disky**.
   3. V seznamu hello disků vyberte hello svazek, který jste aktualizaci, klikněte pravým tlačítkem a pak vyberte **rozšířit svazek**. Spustí se Průvodce rozšířit svazek Hello. Klikněte na **Další**.
   4. Dokončete Průvodce hello, přijetí hello výchozí hodnoty. Po dokončení Průvodce hello hello svazku by měl zobrazit hello zvětšit velikost.

![Dostupné video](./media/storsimple-manage-volumes/Video_icon.png)**Dostupné video**

toowatch video, které ukazuje, jak tooexpand svazek, klikněte na tlačítko [zde](https://azure.microsoft.com/documentation/videos/expand-a-storsimple-volume/).

## <a name="take-a-volume-offline"></a>Svazek převést do režimu offline
Můžete potřebovat tootake svazek offline při plánování toomodify ho nebo odstraňte ji. Pokud je svazek offline, není k dispozici pro přístup pro čtení a zápis. Budete potřebovat tootake hello svazku do offline režimu na hostiteli hello i hello zařízení. Proveďte následující kroky tootake svazek offline hello.

### <a name="tootake-a-volume-offline"></a>tootake svazek offline
1. Ujistěte se, že svazek hello dotyčném není používán před přepnutím do režimu offline.
2. Trvat hello svazku do offline režimu na hostiteli hello první. Tím se eliminuje všechny potenciální riziko poškození dat na svazku hello. Konkrétní kroky najdete v části toohello pokyny pro operační systém hostitele.
3. Po hello hostitel je v režimu offline, trvat hello svazku na hello zařízení do offline režimu provedením hello následující kroky:
   
   1. Na hello **zařízení** vyberte hello zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko hello **kontejnery svazků** kartě hello **kontejnery svazků** kartě seznamy v tabulkovém formátu všechny Hello kontejnery svazků, které jsou přidruženy hello zařízení.
   2. Vyberte kontejner svazků a klikněte na něj toodisplay hello seznam všech svazků hello hello kontejneru.
   3. Vyberte svazek a klikněte na tlačítko **převést do režimu offline**.
   4. Po zobrazení výzvy k potvrzení klikněte na **Ano**. Hello svazku by teď měly být offline.
      
      Jakmile je svazek offline, hello **přepnout do režimu Online** bude k dispozici.

> [!NOTE]
> Hello **přepnout do režimu Offline** příkaz odešle žádost toohello zařízení tootake hello svazek offline. Pokud hostitele stále používají hello svazku, to vede k přerušení připojení, ale nebudou hello svazku přepnutím do režimu offline.
> 
> 

## <a name="delete-a-volume"></a>Odstranění svazku
> [!IMPORTANT]
> Svazek můžete odstranit pouze v případě, že je offline.
> 
> 

Proveďte následující kroky toodelete svazek hello.

### <a name="toodelete-a-volume"></a>toodelete svazku
1. Na hello **zařízení** vyberte hello zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko hello **kontejnery svazků** kartě.
2. Vyberte kontejner hello svazek, který má svazek hello chcete toodelete. Klikněte na tlačítko hello svazku kontejneru tooaccess hello **svazky** stránky.
3. Všechny svazky hello přidružený tento kontejner se zobrazí v tabulkovém formátu. Zkontrolujte stav hello hello svazku chcete toodelete. Pokud chcete toodelete svazku hello není v režimu offline, proveďte ho offline hello první, následující kroky [do offline režimu svazku](#take-a-volume-offline).
4. Po hello svazek je offline, klikněte na tlačítko **odstranit** v hello dolní části stránky hello.
5. Po zobrazení výzvy k potvrzení klikněte na **Ano**. Hello svazek bude odstraněno a hello **svazky** stránky se zobrazí hello aktualizovat seznam svazků hello kontejneru.

## <a name="monitor-a-volume"></a>Monitorování svazku
Monitorování svazku můžete toocollect I/E-související statistiky pro svazek. Monitorování je povoleno ve výchozím nastavení pro hello nejprve 32 svazky, které vytvoříte. Ve výchozím nastavení vypnutá monitorování další svazky. Monitorování klonovaný svazků také ve výchozím nastavení vypnutá.

Proveďte následující kroky tooenable hello nebo zakázat monitorování pro svazek.

### <a name="tooenable-or-disable-volume-monitoring"></a>tooenable nebo zakažte monitorování svazku
1. Na hello **zařízení** vyberte hello zařízení, na ni dvakrát kliknete a pak klikněte na tlačítko hello **kontejnery svazků** kartě.
2. Vyberte kontejner svazků hello, ve které hello svazek nachází a potom klikněte na hello svazku kontejneru tooaccess hello **svazky** stránky.
3. Všechny svazky hello spojené s tímto kontejnerem jsou uvedeny v tabulkovém zobrazení hello. Klikněte a vyberte hello svazek nebo klonování svazku.
4. V dolní části hello hello stránky, klikněte na tlačítko **upravit**.
5. V Průvodci upravit svazek hello pod **základní nastavení**, vyberte **povolit** nebo **zakázat** z hello **monitorování** rozevíracího seznamu.
   
    ![Upravit nastavení základního svazku](./media/storsimple-manage-volumes/HCS_MonitorVolumeM.png)

## <a name="next-steps"></a>Další kroky
* Zjistěte, jak příliš[klonovat svazek StorSimple](storsimple-clone-volume.md).
* Zjistěte, jak příliš[použití hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

