---
title: "Poznámky k verzi pole virtuální zařízení StorSimple | Microsoft Docs"
description: "Popisuje důležité otevřené problémy a řešení pro pole virtuální zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 84908160-2b8b-4f4f-a674-f39aaa0bd4de
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/13/2016
ms.author: alkohli
ms.openlocfilehash: f3ea83e32af4de2637d12766ee8c51adfbf0d3a3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="storsimple-virtual-array-release-notes"></a>Poznámky k verzi pole virtuální zařízení StorSimple
## <a name="overview"></a>Přehled
Následující poznámky k verzi identifikovat kritická otevřené problémy pro tuto verzi obecné dostupnosti (GA). března 2016 společnost Microsoft Azure StorSimple virtuální pole (také označované jako místní virtuální zařízení StorSimple nebo virtuální zařízení StorSimple). Tato verze odpovídá verzi softwaru 10.0.10271.0.

Poznámky k verzi se průběžně aktualizuje a při zjištění zásadních problémů, které vyžadují řešení, se řešení přidá. Před nasazením virtuálního zařízení StorSimple, pečlivě zkontrolujte informace obsažené v poznámkách k verzi. 

Následující tabulka obsahuje souhrn známých problémů v této verzi.

| Ne. | Funkce | Problém | Alternativní řešení a komentář |
| --- | --- | --- | --- |
| **1.** |Aktualizace |Virtuální zařízení vytvořené ve verzi preview nelze aktualizovat na podporovanou verzi obecné dostupnosti. |Tato virtuální zařízení musí převzetí služeb při selhání pro verzi obecné dostupnosti pomocí pracovního postupu zotavení po havárii. |
| **2.** |Zřízené datový disk |Jakmile máte zřízen datový disk určité zadané velikosti a vytvořit odpovídající virtuální zařízení StorSimple, nesmí zvětšení nebo zmenšení datový disk. Pokusili, dojde ke ztrátě všech dat v místních vrstvách zařízení. | |
| **3.** |Zásady skupiny |Když je zařízení připojené k doméně, použití zásad skupiny může nepříznivě ovlivnit činnost zařízení. |Zkontrolujte, zda je vaše virtuální pole ve vlastní organizační jednotku (OU) pro službu Active Directory a žádné objekty zásad skupiny (GPO) se použijí k němu. |
| **4.** |Místní webového uživatelského rozhraní |Pokud funkce Rozšířené zabezpečení jsou povolené v aplikaci Internet Explorer (IE ESC), některé z uživatelského rozhraní místní webové stránky, jako je například údržby nebo Poradce při potížích s nemusí fungovat správně. Tlačítka na těchto stránkách nemusí fungovat. |Vypněte funkce rozšířeného zabezpečení aplikace Internet Explorer. |
| **5.** |Místní webového uživatelského rozhraní |Ve virtuálním počítači technologie Hyper-V rozhraní sítě v webového uživatelského rozhraní se zobrazí jako 10 GB/s rozhraní. |Toto chování je odraz technologie Hyper-V. Technologie Hyper-V vždy zobrazuje 10 GB/s pro virtuální síťové adaptéry. |
| **6.** |Vrstvené svazky nebo sdílené složky |Rozsah bajtů uzamčení pro aplikace, které pracují s StorSimple vrstvené svazky se nepodporuje. Pokud je povolený zámek rozsah bajtů, StorSimple vrstvení nebude fungovat. |Doporučené míry zahrnují: <br></br>Vypněte rozsah bajtů zamykání logiky aplikace.<br></br>Zvolte dávat data pro tuto aplikaci v místně vázaných svazků a vrstvené svazky.<br></br>*Přímý přístup paměti*: Pokud místně pomocí připnutý svazky a je povolený zámek rozsah bajtů, uvědomte si, že místně vázaný svazek může být online i před dokončením obnovení. V takových případech Pokud obnovení probíhá, pak je nutné počkat na dokončení obnovení. |
| **7.** |Vrstvený sdílené složky |Práce s velkými soubory může mít za následek pomalé vrstvy. |Při práci s velkých souborů, doporučujeme vám, že na největší soubor je menší než velikost sdílené složky % 3. |
| **8.** |Použít kapacity pro sdílené složky |Může se zobrazit sdílet spotřeba neexistují žádná data ve sdílené složce. To je proto využité kapacitě pro sdílené složky obsahuje metadata. | |
| **9.** |Zotavení po havárii |Můžete provádět pouze obnovení po havárii souborového serveru ke stejné doméně jako u zdrojového zařízení. V této verzi nepodporuje zotavení po havárii na cílovém zařízení v jiné doméně. |To se provádí v novější verzi. |
| **10.** |Azure PowerShell |Virtuální zařízení StorSimple nelze spravovat pomocí prostředí Azure PowerShell v této verzi. |Všechny správu virtuálních zařízení by mělo být provedeno prostřednictvím portálu Azure classic a místní webového uživatelského rozhraní. |
| **11.** |Změna hesla |Konzole virtuální pole zařízení přijímá pouze vstup ve formátu klávesnice en US. | |
| **12.** |CHAP |Po vytvoření pověření CHAP nelze odebrat. Kromě toho pokud změníte přihlašovací údaje CHAP, musíte převést svazky do režimu offline a pak uvést je online změna projevila. |Tyto budou řešeny v novější verzi. |
| **13.** |serveru se službou iSCSI |'Použít úložiště, zobrazí pro svazek iSCSI může být jiný služby StorSimple Manager a iSCSI hostitele. |Zobrazení souborů má hostitel iSCSI.<br></br>Zařízení se zobrazí na bloky přidělené při při maximální velikosti svazku. |

