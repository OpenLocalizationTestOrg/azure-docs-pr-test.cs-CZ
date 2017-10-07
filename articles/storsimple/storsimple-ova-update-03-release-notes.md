---
title: "Poznámky k verzi aaaStorSimple virtuální pole aktualizace | Microsoft Docs"
description: "Popisuje důležité otevřené problémy a řešení pro spuštění hello pole virtuální zařízení StorSimple Update 0.3."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: b197651a-3c40-4185-b23d-4c8f22cfa8f4
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/15/2016
ms.author: alkohli
ms.openlocfilehash: 305e6419b248fde134abae65f3d2c241d72ffa18
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-update-03-release-notes"></a>Poznámky k verzi 0,3 aktualizace pole virtuální zařízení StorSimple
## <a name="overview"></a>Přehled
Hello následující poznámky k verzi identifikovat hello kritické otevřené problémy a hello vyřešit problémy pro Microsoft Azure StorSimple virtuální pole aktualizace.

Poznámky k verzi Hello se průběžně aktualizuje a při zjištění zásadních problémů, které vyžadují řešení, se řešení přidá. Před nasazením pole virtuální zařízení StorSimple, pečlivě zkontrolujte hello informace obsažené v poznámkách k verzi hello.

Update 0.3 odpovídá verzi softwaru toohello **10.0.10288.0**.

> [!NOTE]
> Aktualizace jsou rušivý a zařízení restartovat. Pokud vstupně-výstupní operace probíhá, hello zařízení způsobuje výpadek.
> 
> 

## <a name="whats-new-in-hello-update-03"></a>Co je nového v hello Update 0.3
Update 0.3 je primárně oprava chyby sestavení. V této verzi několik chyb, což vede k selhání zálohování v předchozí verzi hello vyřeší.

## <a name="issues-fixed-in-hello-update-03"></a>Chyby v hello Update 0.3
Hello následující tabulka obsahuje souhrn chyby v této verzi.

| Ne. | Funkce | Problém |
| --- | --- | --- |
| 1 |Zálohování |Problém zobrazila v hello starší verze, kde hello by dojít k nezdaru zálohování toocomplete pro sdílené složky. Pokud chcete tento problém došlo k chybě, dojde k selhání úlohy zálohování hello a Kritická výstraha objevila hello StorSimple Manager service toonotify hello uživatele. Tento problém nepřešly vliv hello data ve sdílených složkách hello nebo přístup k datům toohello. příčiny Hello byl zjištění a napravení v této verzi. <br></br> Oprava Hello zpětně neprojeví tooshares, který se zobrazuje již tento problém. Zákazníci, kteří se zobrazuje tento problém by měl nejprve použít Update 0.3, a poté požádejte Microsoft Support tooperform problémem úplnou zálohování toofix hello. Místo kontaktovat Microsoft Support, můžete zákazníkům také obnovit tooa novou sdílenou složku ze zálohy v pořádku pro sdílené složky hello vliv. |
| 2 |iSCSI |Problém zobrazila v hello starší verze, kde by při kopírování dat tooa svazek na hello pole virtuální zařízení StorSimple zmizí hello svazky. Tento problém byl opraven v této verzi. <br></br> Hello opravy se projeví pouze na nově vytvořený svazky. Hello opravy se zpětně neprojeví toovolumes, který se zobrazuje již tento problém. Zákazníci by měli toobring hello vliv na svazky online prostřednictvím hello portál Azure classic, proveďte zálohu pro tyto svazky a pak tyto svazky toonew svazky obnovit. |

## <a name="known-issues-in-hello-update-03"></a>Známé problémy v hello Update 0.3
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
| **8.** |Použít kapacity pro sdílené složky |Může se zobrazit sdílení spotřeby, pokud nejsou žádná data ve sdílené složce hello. Je to proto, že kapacita hello používá pro sdílené složky zahrnuje metadata. | |
| **9.** |Zotavení po havárii |Můžete provést pouze hello zotavení po havárii souborový server toohello stejné doméně jako hello zdrojového zařízení. V této verzi nepodporuje po havárii obnovení tooa cílové zařízení v jiné doméně. |Tato možnost je implementovaná v novější verzi. |
| **10.** |Azure PowerShell |virtuální zařízení StorSimple Hello nelze spravovat prostřednictvím hello prostředí Azure PowerShell v této verzi. |Veškerá Správa hello hello virtuální zařízení by mělo být provedeno prostřednictvím hello portál Azure classic a hello místního webového uživatelského rozhraní. |
| **11.** |Změna hesla |Hello virtuální pole zařízení konzoly přijímá pouze vstup ve formátu klávesnice en US. | |
| **12.** |CHAP |Po vytvoření pověření CHAP nelze odebrat. Pokud změníte přihlašovací údaje hello CHAP, navíc potřebovat tootake hello offline svazky a přiřaďte je online pro hello změnit tootake efekt. |Tomuto problému dochází v novější verzi. |
| **13.** |serveru se službou iSCSI |Hello 'použít úložiště, zobrazí pro svazek iSCSI se může lišit v hello služby StorSimple Manager a hello iSCSI hostitele. |zobrazení souborů hello má hostitel iSCSI Hello.<br></br>Hello zařízení uvidí hello bloky přidělené při hello svazku na maximální velikost hello. |
| **14.** |Souborový server |Pokud má soubor ve složce alternativní Data datového proudu (reklamy) s ním spojená, není hello reklamy zálohovat nebo obnovit prostřednictvím obnovení po havárii, klonování a obnovení na úrovni položek. | |

## <a name="next-step"></a>Další krok
[Nainstalujte aktualizaci 0,3](storsimple-ova-install-update-01.md) na pole virtuální zařízení StorSimple.

## <a name="references"></a>Odkazy
Hledáte starší poznámku k verzi? Přejít na: 

* [Poznámky k verzi zařízení StorSimple virtuální pole aktualizace 0,1 a 0,2](storsimple-ova-update-01-release-notes.md)
* [Poznámky k verzi zařízení StorSimple virtuální pole Obecné dostupnosti](storsimple-ova-pp-release-notes.md)

