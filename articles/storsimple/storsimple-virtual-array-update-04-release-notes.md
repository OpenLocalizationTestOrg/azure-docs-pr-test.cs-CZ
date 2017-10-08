---
title: "aaaStorSimple virtuální aktualizace pole 0,4 poznámky k verzi | Microsoft Docs"
description: "Popisuje důležité otevřené problémy a řešení pro spuštění pole virtuální zařízení StorSimple hello aktualizovat 0.4."
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
ms.date: 04/05/2017
ms.author: alkohli
ms.openlocfilehash: 1fd174ff483f4f7b1b4a7853c9d9573d1e948cb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-virtual-array-update-04-release-notes"></a>Poznámky k verzi 0,4 aktualizace pole virtuální zařízení StorSimple

## <a name="overview"></a>Přehled

Hello následující poznámky k verzi identifikovat hello kritické otevřené problémy a hello vyřešit problémy pro Microsoft Azure StorSimple virtuální pole aktualizace.

Poznámky k verzi Hello se průběžně aktualizuje a při zjištění zásadních problémů, které vyžadují řešení, se řešení přidá. Před nasazením pole virtuální zařízení StorSimple, pečlivě zkontrolujte hello informace obsažené v poznámkách k verzi hello.

Aktualizace 0.4 odpovídá verzi softwaru toohello **10.0.10289.0**.

> [!NOTE]
> Aktualizace jsou rušivý a zařízení restartovat. Pokud vstupně-výstupní operace probíhá, hello zařízení způsobuje výpadek.


## <a name="whats-new-in-hello-update-04"></a>Co je nového v aktualizaci 0.4 hello
Aktualizace 0.4 je primárně oprava chyby sestavení kombinaci s několik vylepšení. V této verzi několik chyb, což vede k selhání zálohování v předchozí verzi hello vyřeší. Hello hlavní vylepšení a opravy chyb jsou následující:

- **Vylepšení výkonu zálohování** – tato verze udělal několik klíčových vylepšení výkonu zálohování tooimprove hello. V důsledku toho hello zálohování, které zahrnují velké množství souborů najdete v části k výraznému snížení času toocomplete hello, úplné a přírůstkové zálohování.

- **Vylepšený výkon obnovení** – tato verze obsahuje vylepšení, která výrazně zlepšit výkon obnovení hello při použití velkého počtu souborů. Pokud používáte 2 – 4 miliony souborů, doporučujeme zřízení virtuální pole s 16 GB paměti RAM toosee hello vylepšení. Když pomocí méně než 2 miliony souborů, hello minimální požadavek pro hello virtuální počítač dál toobe 8 GB RAM.

- **Vylepšení tooSupport balíček** -hello vylepšení zahrnují protokolování v hello statistiku pro disk, procesoru, paměti, sítě a cloudu do balíčku hello podpora a vylepšení hello proces diagnostikování nebo ladění problémů zařízení.

- **Limit místně připnutý iSCSI svazky too200 GB** -místně vázaných svazků, doporučujeme omezit tooa 200 GB iSCSI svazek na pole virtuální zařízení StorSimple. Hello místní rezervaci pro vrstvené svazky pokračuje toobe 10 % hello zřízený velikost svazku, ale je limitován 200 GB. 

- **Opravy chyb spojených s** – v předchozích verzích softwaru, byly toobackups související problémy, které způsobí selhání zálohování. V této verzi se odstranily tyto chyby.


## <a name="issues-fixed-in-hello-update-04"></a>Chyby v hello aktualizace 0.4

Hello následující tabulka obsahuje souhrn chyby v této verzi.

| Ne. | Funkce | Problém |
| --- | --- | --- |
| 1 |Výkon zálohování|V hello by starších verzích, zálohování hello zahrnující velké množství souborů trvat dlouhou dobu toocomplete (v pořadí hello dní). V této verzi najdete k výraznému snížení hello čas toocompletion hello úplné a přírůstkové zálohování. |
| 2 |Balíček pro podporu|Disk, procesoru, paměti, sítě a statistiky cloudu nyní přihlášeni protokoly podpory toohello vytváření balíčků podporu hello velmi efektivní při odstraňování problémů jakékoli zařízení.|
| 3 |Zálohování |V dřívějších verzích dlouho běžící zálohování může mít za následek zpracujte místo na hello zařízení, což vede k selhání zálohování. Tato chyba opravená v této verzi tím, že více než 5 zálohy tooqueue najednou.|
| 4 |iSCSI | V dřívějších verzích se hello místní rezervace pro vrstvené nebo místně vázaných svazků 10 % velikost svazku hello zřízený. V této verzi hello místní rezervace pro všechny svazky iSCSI (místně připnuté nebo víceúrovňová) je omezená too10 % s maximálně až 200 GB (pro vrstvené svazky větší než 2 TB) a tím uvolňování další místo na místní disk, hello. Doporučujeme vám, že hello místně vázaný svazky v této verzi být omezená too200 GB.|


## <a name="known-issues-in-hello-update-04"></a>Známé problémy v hello aktualizace 0.4

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
| **15.** |Souborový server |Symbolické odkazy nejsou podporovány. | |
| **16.** |Souborový server |Soubory chráněné pomocí Windows systém souboru (Encrypting File System) po zkopírování přes nebo uložené na hello výsledek serveru soubor pole virtuální zařízení StorSimple v nepodporované konfigurace.  | |

## <a name="next-step"></a>Další krok
[Nainstalujte aktualizaci 0.4](storsimple-virtual-array-install-update-04.md) na pole virtuální zařízení StorSimple.

## <a name="references"></a>Odkazy
Hledáte starší poznámku k verzi? Přejít na: 

* [K vydání 0,3 verze Update pole virtuální zařízení StorSimple](storsimple-ova-update-03-release-notes.md)
* [Poznámky k verzi zařízení StorSimple virtuální pole aktualizace 0,1 a 0,2](storsimple-ova-update-01-release-notes.md)
* [Poznámky k verzi zařízení StorSimple virtuální pole Obecné dostupnosti](storsimple-ova-pp-release-notes.md)

