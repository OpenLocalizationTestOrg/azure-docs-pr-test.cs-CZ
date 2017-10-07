---
title: "Poznámky k verzi aaaStorSimple virtuální pole aktualizace | Microsoft Docs"
description: "Popisuje důležité otevřené problémy a řešení hello pole virtuální zařízení StorSimple spuštění Update 0,2 a 0,1."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 3993864d-2ddd-4302-a2f1-8d737fba6eab
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/16/2016
ms.author: alkohli
ms.openlocfilehash: dfd38890feeb667c95134f2adbb35ce2df165620
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-update-02-and-01-release-notes"></a>Poznámky k verzi virtuální pole aktualizace 0,2 a 0,1 StorSimple
## <a name="overview"></a>Přehled
Hello následující poznámky k verzi identifikovat hello kritické otevřené problémy a hello vyřešit problémy pro Microsoft Azure StorSimple virtuální pole aktualizace. (Microsoft Azure StorSimple virtuální pole je také označované jako hello virtuálního zařízení StorSimple místní nebo virtuální zařízení StorSimple hello.) 

Poznámky k verzi Hello se průběžně aktualizuje a při zjištění zásadních problémů, které vyžadují řešení, se řešení přidá. Před nasazením virtuálního zařízení StorSimple, pečlivě zkontrolujte hello informace obsažené v poznámkách k verzi hello.

Aktualizace 0,2 odpovídá verzi softwaru toohello **10.0.10280.0**; Update 0.1 je verze **10.0.10279.0**. v níže uvedených částech Hello seznam hello změn pro jednotlivé aktualizace. 

> [!NOTE]
> Aktualizace jsou rušivý a restartuje vaše zařízení. Pokud vstupně-výstupní operace probíhá, je možné zařízení hello za výpadku.
> 
> 

## <a name="issues-fixed-in-hello-update-02"></a>Chyby v hello aktualizace 0,2
Přidání toohello oprava popsané v hello následující tabulka obsahuje všechny změny z Update 0.1 aktualizace 0,2:

| Funkce | Problém |
| --- | --- |
| Aktualizace |V poslední verzi hello nebyly vyhledávat aktualizace automaticky v hello portál Azure classic, takže jste měli toouse hello místní aktualizace tooinstall webového uživatelského rozhraní. Tento problém vyřešen v této verzi. Po instalaci aktualizace 0,2, můžete nainstalovat pomocí portálu Azure classic hello budoucí aktualizace. |

## <a name="whats-new-in-hello-update-01"></a>Co je nového v hello Update 0.1
Aktualizace 0,1 obsahuje následující hello oprav chyb a vylepšení. 

* **Odolnost pro cloud výpadků**: Tato verze obsahuje několik oprav chyb kolem zotavení po havárii, zálohování, obnovení a vrstvení v události hello přerušení připojení k cloudu. 
* **Zvýšení výkonu obnovení**: Tato verze obsahuje opravy chyb, které výrazně zkrátit čas dokončení hello hello obnovení úloh.
* **Automatizované optimalizace recyklace místa**: Při odstranění dat na dynamicky zřizované svazky hello nepoužívané úložiště bloků potřebovat toobe uvolnit. Tato verze má lepší hello místo recyklaci proces z hello cloudu, což vede k hello nepoužívané místo stává stále k dispozici rychlejší jako porovnání toohello předchozí verze.
* **Nové bitové kopie virtuálního disku**: nový virtuální pevný disk, VHDX a VMDK jsou nyní k dispozici prostřednictvím hello portál Azure classic. Tyto bitové kopie tooprovision nová Update 0.1 zařízení si můžete stáhnout.
* **Zlepšení hello přesnosti stav úlohy portálu hello**: V hello nebyl granulární dřívější verzi softwaru, stav úlohy vytváření sestav v portálu hello. Tento problém se vyřeší v této verzi.
* **Prostředí připojení k doméně**: opravy chyb související toodomain připojení a přejmenování hello zařízení.

## <a name="issues-fixed-in-hello-update-01"></a>Chyby v hello Update 0.1
Hello následující tabulka obsahuje souhrn chyby v této verzi.

| Ne. | Funkce | Problém |
| --- | --- | --- |
| 1 |VMDK |V některé verze VMware disk s operačním systémem hello zobrazila jako zhuštěné příčinou výstrah a přerušení normální provozní podmínky. To byla opravena v této verzi. |
| 2 |serveru se službou iSCSI |V poslední verzi hello hello uživatel byl požadované toospecify bránu pro každé povolené síťové rozhraní virtuálního zařízení StorSimple. Toto chování je v této verzi změnilo, tak, že hello má uživatel tooconfigure nejméně jedna brána pro všechna síťová rozhraní hello povolena. |
| 3 |Balíček pro podporu |V hello dřívější verzi softwaru, podporu kolekce balíčků selhalo při velikosti balíčku hello byly větší než 1 GB. Tento problém vyřešen v této verzi. |
| 4 |Přístup do cloudu |V poslední verzi hello, pokud hello pole virtuální zařízení StorSimple nemá připojení k síti a byl restartován, hello místního uživatelského rozhraní by měla mít problémy s připojením. Tento problém vyřešen v této verzi. |
| 5 |Monitorování grafy |V předchozí verzi hello následující selhání zařízení zobrazí grafy využití kapacity cloudu hello nesprávné hodnoty v hello portál Azure classic. Tento problém je vyřešený v aktuální verzi hello. |

## <a name="known-issues-in-hello-update-01"></a>Známé problémy v hello Update 0.1
Hello následující tabulka obsahuje souhrn známé problémy pro hello pole virtuální zařízení StorSimple a zahrnuje hello problémy uvedené verze z předchozích verzí hello. **Hello verze problémy uvedené v této verzi jsou označeny hvězdičkou. Téměř všechny hello problémů v tomto seznamu mají přenášejí z verze hello GA pole virtuální zařízení StorSimple.**

| Ne. | Funkce | Problém | Alternativní řešení a komentář |
| --- | --- | --- | --- |
| **1.** |Aktualizace |virtuální zařízení Hello vytvořen ve verzi preview hello nemůže být aktualizovaný tooa podporované verze obecné dostupnosti. |Tato virtuální zařízení musí být převzetí služeb při selhání pro hello verze obecné dostupnosti pomocí pracovního postupu zotavení po havárii. |
| **2.** |Zřízené datový disk |Jednou, když máte zřízenou datový disk určité zadané velikosti a vytvoření virtuálního zařízení odpovídající StorSimple hello nesmí zvětšení nebo zmenšení hello datový disk. Pokus toodo tak bude mít za následek ke ztrátě všech dat hello v místních vrstvách hello hello zařízení. | |
| **3.** |Zásady skupiny |Když je zařízení připojené k doméně, použití zásad skupiny může nepříznivě ovlivnit hello zařízení operaci. |Zkontrolujte, zda je vaše virtuální pole ve vlastní organizační jednotku (OU) pro službu Active Directory a žádné objekty zásad skupiny (GPO) jsou použité tooit. |
| **4.** |Místní webového uživatelského rozhraní |Pokud funkce Rozšířené zabezpečení jsou povolené v aplikaci Internet Explorer (IE ESC), některé z uživatelského rozhraní místní webové stránky, jako je například údržby nebo Poradce při potížích s nemusí fungovat správně. Tlačítka na těchto stránkách nemusí fungovat. |Vypněte funkce rozšířeného zabezpečení aplikace Internet Explorer. |
| **5.** |Místní webového uživatelského rozhraní |Ve virtuálním počítači technologie Hyper-V hello síťová rozhraní hello webového uživatelského rozhraní se zobrazují jako rozhraní 10 GB/s. |Toto chování je odraz technologie Hyper-V. Technologie Hyper-V vždy zobrazuje 10 GB/s pro virtuální síťové adaptéry. |
| **6.** |Vrstvené svazky nebo sdílené složky |Zámek pro aplikace, které pracují s hello StorSimple vrstvené svazky rozsah bajtů není podporována. Pokud je povolený zámek rozsah bajtů, StorSimple vrstvení nebude fungovat. |Doporučené míry zahrnují: <br></br>Vypněte rozsah bajtů zamykání logiky aplikace.<br></br>Zvolte tooput data pro tuto aplikaci místně vázaných svazků názvem na rozdíl od tootiered svazky.<br></br>*Přímý přístup paměti*: Pokud místně pomocí připnutý svazky a je povolený zámek rozsah bajtů, uvědomte si, že hello místně vázaný svazek může být online, ještě před hello obnovení je dokončeno. V takových případech Pokud obnovení probíhá, pak je nutné počkat toocomplete obnovení hello. |
| **7.** |Vrstvený sdílené složky |Práce s velkými soubory může mít za následek pomalé vrstvy. |Při práci s velkých souborů, doporučujeme, že soubor největší hello je menší než 3 % hello velikost sdílené složky. |
| **8.** |Použít kapacity pro sdílené složky |Může se zobrazit sdílet spotřeba hello neexistence všechna data ve sdílené složce hello. Je to proto, že kapacita hello používá pro sdílené složky zahrnuje metadata. | |
| **9.** |Zotavení po havárii |Můžete provést pouze hello zotavení po havárii souborový server toohello stejné doméně jako hello zdrojového zařízení. V této verzi nepodporuje po havárii obnovení tooa cílové zařízení v jiné doméně. |To se provádí v novější verzi. |
| **10.** |Azure PowerShell |virtuální zařízení StorSimple Hello nelze spravovat prostřednictvím hello prostředí Azure PowerShell v této verzi. |Veškerá Správa hello hello virtuální zařízení by mělo být provedeno prostřednictvím hello portál Azure classic a hello místního webového uživatelského rozhraní. |
| **11.** |Změna hesla |Hello virtuální pole zařízení konzoly přijímá pouze vstup ve formátu klávesnice en US. | |
| **12.** |CHAP |Po vytvoření pověření CHAP nelze odebrat. Dále pokud změníte přihlašovací údaje hello CHAP, bude nutné tootake hello offline svazky a přizpůsobit je online pro hello změnit tootake efekt. |Tyto budou řešeny v novější verzi. |
| **13.** |serveru se službou iSCSI |Hello 'použít úložiště, zobrazí pro svazek iSCSI se může lišit v hello služby StorSimple Manager a hello iSCSI hostitele. |zobrazení souborů hello má hostitel iSCSI Hello.<br></br>Hello zařízení uvidí hello bloky přidělené při hello svazku na maximální velikost hello. |
| **14.** |Souborový server * |Pokud má soubor ve složce alternativní Data datového proudu (reklamy) s ním spojená, není hello reklamy zálohovat nebo obnovit prostřednictvím obnovení po havárii, klonování a obnovení na úrovni položek. | |

## <a name="next-step"></a>Další krok
[Nainstalujte aktualizace](storsimple-ova-install-update-01.md) na pole virtuální zařízení StorSimple.

