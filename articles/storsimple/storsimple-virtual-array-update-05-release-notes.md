---
title: "aaaStorSimple virtuální aktualizace pole 0,5 poznámky k verzi | Microsoft Docs"
description: "Popisuje důležité otevřené problémy a řešení pro spuštění pole virtuální zařízení StorSimple hello aktualizovat 0,5."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/08/2017
ms.author: alkohli
ms.openlocfilehash: a249e2e8f0105dcf6cc02d3136dfa3e37ea615d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-update-05-release-notes"></a>Poznámky k verzi 0,5 aktualizace pole virtuální zařízení StorSimple

## <a name="overview"></a>Přehled

Hello následující poznámky k verzi identifikovat hello kritické otevřené problémy a hello vyřešit problémy pro Microsoft Azure StorSimple virtuální pole aktualizace.

Poznámky k verzi Hello se průběžně aktualizuje a při zjištění zásadních problémů, které vyžadují řešení, se řešení přidá. Před nasazením pole virtuální zařízení StorSimple, pečlivě zkontrolujte hello informace obsažené v poznámkách k verzi hello.

Aktualizace 0,5 odpovídá verzi softwaru toohello **10.0.10290.0**.

> [!NOTE]
> Aktualizace jsou rušivý a zařízení restartovat. Pokud vstupně-výstupní operace probíhá, hello zařízení způsobuje výpadek. Podrobné pokyny, jak tooapply hello aktualizace, najdete příliš[nainstalujte aktualizaci 0,5](storsimple-virtual-array-install-update-05.md).


## <a name="whats-new-in-hello-update-05"></a>Co je nového v aktualizaci 0,5 hello
Aktualizace 0,5 je primárně oprava chyby sestavení. Hello hlavní vylepšení a opravy chyb jsou následující:

- **Zálohování odolnosti vylepšení** – tato verze obsahuje opravy, které zlepšují odolnost zálohování hello. V hello starších verzích, zálohování se zopakuje pouze pro určité výjimky. Tato verze opakování všechny výjimky zálohování hello a díky hello pružnější zálohy.

- **Aktualizace pro sledování využití úložiště** – od 30. června 2017 se hello sledování využití úložiště pro virtuální zařízení řady StorSimple vyřadí. To platí toohello monitorování grafy na všechny virtuální pole hello verzi Update 0,4 nebo nižší. Tato aktualizace obsahuje změny hello požadované pro toocontinue hello používání úložiště využití monitorování v hello portálu Azure. Nainstalujte tuto aktualizaci před 30. června 2017 toocontinue pomocí hello funkci monitorování.


## <a name="issues-fixed-in-hello-update-05"></a>Chyby v hello aktualizace 0,5

Hello následující tabulka obsahuje souhrn chyby v této verzi.

| Ne. | Funkce | Problém |
| --- | --- | --- |
| 1 |Odolnost proti chybám zálohování| V hello starších verzích, zálohování se zopakuje pouze pro určité výjimky. Tato verze obsahuje zálohy toomake opravu, která je pružnější opakováním všechny výjimky zálohování hello.|
| 2 |Monitorování| Hello sledování využití úložiště pro virtuální zařízení řady StorSimple bude zastaralé od 30. června 2017. Tato akce ovlivní hello monitorování grafy na službu StorSimple Manager zařízení hello systémem pole virtuální zařízení StorSimple (1200 modelu). Tato verze obsahuje aktualizace, které umožňují hello uživatele toocontinue hello použití sledování využití úložiště na virtuální pole hello nad rámec 30. června 2017.|
| 3 |Souborový server| V hello starších verzích, uživatel může omylem zkopírovat virtuální pole toohello šifrované soubory. Tato verze obsahuje opravu, která nepovolí kopírování pole toovirtual šifrované soubory. Pokud vaše zařízení má existující toohello předchozí šifrované soubory, které se aktualizace, zálohování budou pokračovat toofail, dokud všechny hello šifrované soubory se odstraní ze systému hello. |


## <a name="known-issues-in-hello-update-05"></a>Známé problémy v hello aktualizace 0,5

Hello následující tabulka obsahuje souhrn známé problémy pro hello pole virtuální zařízení StorSimple a zahrnuje hello problémy uvedené verze z předchozích verzí hello.

| Ne. | Funkce | Problém | Alternativní řešení a komentář |
| --- | --- | --- | --- |
| **1.** |Aktualizace |virtuální zařízení Hello vytvořen ve verzi preview hello nemůže být aktualizovaný tooa podporované verze obecné dostupnosti. |Tato virtuální zařízení musí být převzetí služeb při selhání pro hello verze obecné dostupnosti pomocí pracovního postupu zotavení po havárii. |
| **2.** |Zřízené datový disk |Jednou, když máte zřízenou datový disk určité zadané velikosti a vytvoření virtuálního zařízení odpovídající StorSimple hello nesmí zvětšení nebo zmenšení hello datový disk. Pokus toodo výsledkem ke ztrátě všech dat hello v místních vrstvách hello hello zařízení. | |
| **3.** |Zásady skupiny |Když je zařízení připojené k doméně, použití zásad skupiny může nepříznivě ovlivnit hello zařízení operaci. |Zkontrolujte, zda je vaše virtuální pole ve vlastní organizační jednotku (OU) pro službu Active Directory a žádné objekty zásad skupiny (GPO) jsou použité tooit. |
| **4.** |Místní webového uživatelského rozhraní |Pokud funkce Rozšířené zabezpečení jsou povolené v aplikaci Internet Explorer (IE ESC), některé z uživatelského rozhraní místní webové stránky, jako je například údržby nebo Poradce při potížích s nemusí fungovat správně. Tlačítka na těchto stránkách nemusí fungovat. |Vypněte funkce rozšířeného zabezpečení aplikace Internet Explorer. |
| **5.** |Místní webového uživatelského rozhraní |Ve virtuálním počítači technologie Hyper-V hello síťová rozhraní hello webového uživatelského rozhraní se zobrazují jako rozhraní 10 GB/s. |Toto chování je odraz technologie Hyper-V. Technologie Hyper-V vždy zobrazuje 10 GB/s pro virtuální síťové adaptéry. |
| **6.** |Vrstvené svazky nebo sdílené složky |Zámek pro aplikace, které pracují s hello StorSimple vrstvené svazky rozsah bajtů není podporována. Pokud je povolený zámek rozsah bajtů, StorSimple vrstvení není funkční. |Doporučené míry zahrnují: <br></br>Vypněte rozsah bajtů zamykání logiky aplikace.<br></br>Zvolte tooput data pro tuto aplikaci místně vázaných svazků názvem na rozdíl od tootiered svazky.<br></br>*Přímý přístup paměti*: při použití místně připnutý svazky a je povolený zámek rozsah bajtů, hello místně vázaný svazek může být online, ještě před hello obnovení je dokončeno. V takových případech Pokud obnovení probíhá, pak je nutné počkat toocomplete obnovení hello. |
| **7.** |Vrstvený sdílené složky |Práce s velkými soubory může mít za následek pomalé vrstvy. |Při práci s velkých souborů, doporučujeme, že soubor největší hello je menší než 3 % hello velikost sdílené složky. |
| **8.** |Použít kapacity pro sdílené složky |Může se zobrazit sdílení spotřeby, pokud nejsou žádná data ve sdílené složce hello. Tato spotřeba totiž kapacita hello používá pro sdílené složky zahrnuje metadata. | |
| **9.** |Zotavení po havárii |Můžete provést pouze hello zotavení po havárii souborový server toohello stejné doméně jako hello zdrojového zařízení. V této verzi nepodporuje po havárii obnovení tooa cílové zařízení v jiné doméně. |Tato možnost je implementovaná v novější verzi. Další informace, přejděte příliš[převzetí služeb při selhání a zotavení po havárii pro pole virtuální zařízení StorSimple](storsimple-virtual-array-failover-dr.md) |
| **10.** |Azure PowerShell |virtuální zařízení StorSimple Hello nelze spravovat prostřednictvím hello prostředí Azure PowerShell v této verzi. |Veškerá Správa hello hello virtuální zařízení by mělo být provedeno prostřednictvím hello Azure portal a hello místního webového uživatelského rozhraní. |
| **11.** |Změna hesla |Hello konzoly virtuální pole zařízení přijímá pouze vstup v en-us klávesové formátu. | |
| **12.** |CHAP |Po vytvoření pověření CHAP nelze odebrat. Pokud změníte přihlašovací údaje hello CHAP, navíc potřebovat tootake hello offline svazky a přiřaďte je online pro hello změnit tootake efekt. |Tomuto problému dochází v novější verzi. |
| **13.** |serveru se službou iSCSI |Hello 'použít úložiště, zobrazí pro svazek iSCSI se může lišit v služby StorSimple Manager zařízení hello a hello iSCSI hostitele. |zobrazení souborů hello má hostitel iSCSI Hello.<br></br>Hello zařízení uvidí hello bloky přidělené při hello svazku na maximální velikost hello. |
| **14.** |Souborový server |Pokud má soubor ve složce alternativní Data datového proudu (reklamy) s ním spojená, není hello reklamy zálohovat nebo obnovit prostřednictvím obnovení po havárii, klonování a obnovení na úrovni položek. | |
| **15.** |Souborový server |Symbolické odkazy nejsou podporovány. | |
| **16.** |Souborový server |Soubory chráněné pomocí Windows systém souboru (Encrypting File System) po zkopírování přes nebo uložené na hello výsledek serveru soubor pole virtuální zařízení StorSimple v nepodporované konfigurace.  | |

## <a name="next-step"></a>Další krok
[Nainstalujte aktualizaci 0,5](storsimple-virtual-array-install-update-05.md) na pole virtuální zařízení StorSimple.

## <a name="references"></a>Odkazy
Hledáte starší poznámku k verzi? Přejít na:

* [K vydání 0,4 verze Update pole virtuální zařízení StorSimple](storsimple-virtual-array-update-04-release-notes.md)
* [K vydání 0,3 verze Update pole virtuální zařízení StorSimple](storsimple-ova-update-03-release-notes.md)
* [Poznámky k verzi zařízení StorSimple virtuální pole aktualizace 0,1 a 0,2](storsimple-ova-update-01-release-notes.md)
* [Poznámky k verzi zařízení StorSimple virtuální pole Obecné dostupnosti](storsimple-ova-pp-release-notes.md)

