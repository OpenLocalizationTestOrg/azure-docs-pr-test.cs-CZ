---
title: "aaaManage šablony šířky pásma StorSimple | Microsoft Docs"
description: "Popisuje, jak toomanage StorSimple šířky pásma šablony, které umožňují toocontrol šířku pásma."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 0027b90e-91a5-437d-9bd0-06b05674aa5f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2016
ms.author: alkohli
ms.openlocfilehash: 3f767e985667121e977106e7a1f8e5a3ad25f022
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-storsimple-bandwidth-templates"></a>Použití hello StorSimple Manager service toomanage StorSimple šířky pásma šablon
## <a name="overview"></a>Přehled
Šablony šířky pásma povolit využití šířky pásma sítě tooconfigure napříč více času dne plány tootier hello daty z hello cloudu toohello zařízení StorSimple.

Plány omezení šířky pásma můžete:

* Zadejte vlastní šířky pásma plány v závislosti na použití sítě zatížení hello.
* Centralizovat správu a znovu použít plány hello mezi různými zařízeními snadné a plynulé způsobem.

> [!NOTE]
> Tato funkce je k dispozici pouze pro fyzického zařízení StorSimple a není pro virtuální zařízení.
> 
> 

Všechny šablony šířky pásma hello služby se zobrazí v tabulkovém formátu a obsahovat hello následující informace:

* **Název** – při vytváření šablony šířky pásma toohello přiřazen jedinečný název.
* **Plán** – hello počet plány, které jsou součástí šablony dané šířky pásma.
* **Používá** – hello počtu svazků pomocí šablon hello šířky pásma.

Použít službu StorSimple Manager hello **konfigurace** stránky v šablonách pásma Azure classic portálu toomanage hello.

Taky můžete najít další informace o toohelp nakonfigurovat šablony šířky pásma v:

* Otázky a odpovědi týkající se šířky pásma šablony
* Doporučené postupy pro šablony šířky pásma

## <a name="add-a-bandwidth-template"></a>Přidání šablony šířky pásma
Proveďte následující kroky toocreate novou šablonu šířky pásma hello.

#### <a name="tooadd-a-bandwidth-template"></a>tooadd šablony šířky pásma
1. Na hello služby StorSimple Manager **konfigurace** klikněte na tlačítko **šablony šířky pásma přidat či upravit**.
2. V hello **šablony šířky pásma, přidat či upravit** dialogové okno:
   
   1. Z hello **šablony** rozevíracího seznamu vyberte **vytvořit nový** tooadd novou šablonu šířky pásma.
   2. Zadejte jedinečný název pro šablonu šířky pásma.
3. Definování **šířky pásma plán**. toocreate plánu:
   
   1. Hello rozevíracím seznamu vyberte dny hello hello týden hello plánu je nakonfigurován pro. Zaškrtnutím políček hello nachází před hello příslušných dnů v hello seznamu můžete vybrat více dní.
   2. Vyberte hello **celý den** možnost, pokud je pro celý den hello vynucený plán hello. Pokud zaškrtnete toto políčko, je už zadat **čas spuštění** nebo **koncový čas**. plán Hello spouští z too11 12:00:00: 59 PM.
   3. Hello rozevíracím seznamu vyberte **čas spuštění**. To je, když bude zahájena hello plán.
   4. Hello rozevíracím seznamu vyberte **koncový čas**. To je, když se zastaví hello plán.
      
      > [!NOTE]
      > Překrývající se plány nejsou povoleny. Pokud hello počáteční a koncový čas způsobí překrývající se plán, zobrazí se chybová zpráva toothat vliv.
      > 
      > 
   5. Zadejte hello **šířky pásma**. Toto je hello šířky pásma v MB za sekundu (Mbps) používané zařízení StorSimple v operací zahrnujících hello cloudu (nahrávání a stahování). Zadejte číslo mezi 1 a 1000 u tohoto pole.
   6. Klikněte na ikonu zaškrtnutí hello ![ikona zaškrtnutí](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Hello šablonu, kterou jste vytvořili se přidá toohello seznam šablon šířky pásma na hello služby **konfigurace** stránky.
      
      ![Vytvořte novou šablonu šířky pásma](./media/storsimple-manage-bandwidth-templates/HCS_CreateNewBT1.png)
4. Klikněte na tlačítko **Uložit** v dolní části hello hello stránky a pak klikněte na tlačítko **Ano** po zobrazení výzvy k potvrzení. Tato akce uloží změny hello konfigurace, které jste provedli.

## <a name="edit-a-bandwidth-template"></a>Upravit šablonu šířky pásma
Proveďte následující kroky tooedit šablony šířky pásma hello.

### <a name="tooedit-a-bandwidth-template"></a>tooedit šablony šířky pásma
1. Klikněte na tlačítko **šablony šířky pásma přidat či upravit**.
2. V hello **šablony šířky pásma, přidat či upravit** dialogové okno:
   
   1. Z hello **šablony** rozevíracího seznamu vyberte existující šířky pásma, které chcete toomodify šablony.
   2. Proveďte změny. (Můžete upravit stávající nastavení hello.)
   3. Klikněte na ikonu zaškrtnutí hello ![Ikona zaškrtnutí](./media/storsimple-manage-bandwidth-templates/HCS_CheckIcon.png). Zobrazí se změněné šablony hello v hello seznam šablon šířky pásma na stránku konfigurace služby hello.
3. Klikněte na změny, toosave **Uložit** v hello dolní části stránky hello. Klikněte na tlačítko **Ano** po zobrazení výzvy k potvrzení.

> [!NOTE]
> Nebude možné toosave, které změny pokud hello upravená plánu překrývá s existující naplánovat v šabloně hello šířky pásma se změnit.
> 
> 

## <a name="delete-a-bandwidth-template"></a>Odstranit šablonu šířky pásma
Proveďte následující kroky toodelete šablony šířky pásma hello.

#### <a name="toodelete-a-bandwidth-template"></a>toodelete šablony šířky pásma
1. V hello tabulkového seznamu šablon hello šířky pásma pro vaši službu vyberte šablonu hello chcete toodelete. Ikony odstranění (**x**) se zobrazí toohello extrémně napravo od hello vybranou šablonu. Klikněte na tlačítko hello **x** ikonu toodelete hello šablony.
2. Zobrazí se výzva k potvrzení. Klikněte na tlačítko **OK** tooproceed.

Když je šablona hello používá všechny svazky, nebude možné toodelete ho. Zobrazí se chybová zpráva oznamující, že tato šablona hello používá. Dialogové okno chyby zpráva se zobrazí radí, že má být odebrána všechny šablony toohello odkazy hello.

Všechny šablony toohello hello odkazy můžete odstranit pomocí přístupu k hello **kontejnery svazků** stránky a úprava hello kontejnery svazků, které tuto šablonu použít tak, aby použít jinou šablonu nebo použít vlastní nebo neomezená šířky pásma nastavení. Pokud byly odebrány všechny odkazy hello, můžete odstranit šablonu hello.

## <a name="use-a-default-bandwidth-template"></a>Použít výchozí šablonu šířky pásma
Výchozí šablony šířky pásma je k dispozici a je používána kontejnery svazků ve výchozím nastavení řízení šířky pásma tooenforce při přístupu k hello cloudu. výchozí šablony Hello slouží také jako připravené odkaz pro uživatele, kteří vytvořit své vlastní šablony. Podrobnosti o Hello této výchozí šablony jsou:

* **Název** – neomezená noci a víkendů
* **Plán** – jeden plán z tooFriday pondělí, která se vztahuje šířky pásma 1 MB/s mezi 8: 00 a 17: 00 času. tooUnlimited zbývající hello týdnu hello je nastavená šířka pásma Hello.

výchozí šablony Hello se dá upravit. je sledovat využití Hello této šablony (včetně upravená verze).

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Vytvořit šablonu celodenní šířky pásma, která se spouští v zadanou dobu
Použijte tento postup toocreate plánu, který se spustí v zadaný čas a spustí celý den. V příkladu hello hello plán začíná na 9: 00 v hello ráno a spouští až 9: 00 hello další ráno. Je důležité toonote, který hello počáteční a koncový čas pro dané plán musí oba být obsaženy na hello stejné 24 hodin naplánovat a nemůžou zahrnovat víc dnů. Pokud potřebujete tooset šablon šířky pásma, které jsou rozmístěny v několika dní, budete potřebovat toouse více plánů (jak je znázorněno v příkladu hello).

#### <a name="toocreate-an-all-day-bandwidth-template"></a>toocreate šablonu celodenní šířky pásma
1. Vytvořte plán, který začíná v 9: 00 v hello ráno a spouští dokud půlnoci.
2. Přidejte jiný plán. Nakonfigurujte druhý toorun plán hello od půlnoci až 9: 00 v hello ráno.
3. Uložte šablonu šířky pásma hello.

složený plán Hello bude poté otevřete současně dle vašeho výběru a spusťte celodenní.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Otázky a odpovědi týkající se šířky pásma šablony
**Q**. Ovládací prvky toobandwidth následovat, když jsou mezi hello plány? (Skončila plánu a jiný se ještě nespustilo.)

**A**. V takových případech bude těmto nekompatibilitám bez řízení šířky pásma. To znamená, že hello zařízení můžete použít neomezenou šířku pásma při vrstvení toohello dat v cloudu.

**Q**. Můžete upravit šablony šířky pásma na zařízení s offline?

**A**. Nebudete moct toomodify šablony šířky pásma na kontejnery svazků hello odpovídající zařízení je offline.

**Q**. Můžete upravit šablonu šířky pásma přidružené kontejner svazků, když jsou offline hello přidružené svazky?

**A**. Můžete upravit šablonu šířky pásma přidružené kontejner svazků, jejichž svazky jsou offline. Všimněte si, že po offline svazky se žádná data vrstvené z cloudu toohello hello zařízení.

**Q**. Můžete odstranit výchozí šablonu?

**A**. I když můžete odstranit výchozí šablonu, není vhodné toodo tak. Hello použití výchozí šablony, včetně upravená verze sledován. Hello sledování dat se analyzují a průběhu hello času je použité tooimprove hello výchozí šablony.

**Q**. Jak byste zjistit, jestli vaše šablony šířky pásma musí být toobe změnit?

**A**. Jeden z hello přihlásí, je nutné, aby toomodify hello šířky pásma šablony je při spuštění zobrazuje hello sítě zpomalit nebo vyseknutí několikrát za den. V takovém případě sledujte prohlížením hello vstupně-výstupní výkon a propustnost sítě grafy hello úložiště a využití sítě.

Z dat propustnosti sítě hello Identifikujte denní dobu hello a hello kontejnery svazků, ve které hello dojde k přetížení sítě. Pokud to se stane, když dat se vrstvené toohello cloudu (získat tyto informace z vstupně-výstupní výkon pro všechny kontejnery svazků pro toocloud zařízení), bude nutné toomodify hello šířky pásma šablon přidružených k vaší kontejnery svazků.

Po úpravě hello šablony jsou používány, budete potřebovat síť hello toomonitor znovu pro významné latenci. Pokud tyto stále existují, budete potřebovat toorevisit šablony šířky pásma.

**Q**. Co se stane, pokud máte více kontejnery svazků na zařízení plány této překrývají, ale jiné limity platí tooeach?

**A**. Předpokládejme, že máte zařízení s 3 kontejnery svazků. Hello plány, které jsou přidružené k těmto kontejnerům úplně překrývat. Pro každou z těchto kontejnerů hello šířky pásma použít je mez 5, 10 až 15 MB/s. Když se vyskytnou vstupně-výstupních operací na všech těchto kontejnerů v hello mohou být použity současně minimální hello limitů šířky pásma hello 3: v tomto případě 5 MB/s jako tyto odchozí sdílení vstupně-výstupní požadavky hello stejné fronty.

## <a name="best-practices-for-bandwidth-templates"></a>Doporučené postupy pro šablony šířky pásma
Použijte tyto osvědčené postupy pro zařízení StorSimple:

* Konfigurace šablon šířky pásma na vaše zařízení tooenable proměnné omezení propustnost sítě hello hello zařízení v různých časech hello den. Tyto šablony šířky pásma při použití s plánů zálohování můžete efektivně využívat další šířku pásma sítě pro operace cloudu mimo špičku.
* Vypočítejte hello skutečné pásma potřebná pro konkrétní nasazení na základě velikosti hello hello nasazení a plánovanou dobu hello požadované obnovení (RTO).

## <a name="next-steps"></a>Další kroky
Další informace o [pomocí hello tooadminister služby StorSimple Manager zařízení StorSimple](storsimple-manager-service-administration.md).

