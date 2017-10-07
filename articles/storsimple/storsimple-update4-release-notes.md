---
title: "Poznámky k verzi aaaStorSimple 8000 Update 4 řady | Microsoft Docs"
description: "Popisuje nové funkce hello, problémy a řešení pro zařízení StorSimple 8000 řady aktualizací 4."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/04/2017
ms.author: alkohli
ms.openlocfilehash: 4bca8ca222e6706fd6eaf56b702b0d34b6ffd1c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-4-release-notes"></a>Poznámky k verzi zařízení StorSimple 8000 řady aktualizací 4

## <a name="overview"></a>Přehled

Hello následující poznámky k verzi popisují hello nové funkce a identifikovat hello kritické otevřené problémy pro StorSimple 8000 řady aktualizací 4. Také obsahují seznam aktualizací softwaru zařízení StorSimple hello součástí této verze. 

Aktualizace 4 může být zařízení StorSimple použité tooany systémem verze (GA) nebo Update 0.1 prostřednictvím aktualizace 3.1. Hello zařízení verzi spojenou s aktualizací 4 je 6.3.9600.17820.

Přečtěte si hello informace obsažené v uvolněte hello poznámky před nasazením hello aktualizovat v řešení StorSimple.

> [!IMPORTANT]
> * Aktualizace 4 obsahuje software zařízení, Seznam USM firmwaru, LSI ovladače a firmware, firmwaru disku, Storport a Spaceport, zabezpečení a jiné aktualizace operačního systému. Tato aktualizace trvá přibližně tooinstall 4 hodiny. Aktualizace firmwaru disku je rušivý aktualizace a výsledkem výpadku pro vaše zařízení. Doporučujeme použít aktualizaci 4 tookeep aktuální zařízení. 
> * Pro nové verze, nemusí zobrazit aktualizace okamžitě, protože provedeme postupné zavádění hello aktualizací. Počkejte několik dní, a pak vyhledejte aktualizace znovu jako tyto brzy bude dostupná.

## <a name="whats-new-in-update-4"></a>Co je nového v aktualizaci 4

Hello následující klíčových vylepšení a opravy chyb byly provedeny v aktualizaci 4.

* **Chytřejší automatizované algoritmy recyklace místa** – v aktualizaci 4 hello automatizované místo recyklaci algoritmy jsou recyklace místa hello rozšířené tooadjust cykly podle očekávání hello uvolnit místo v cloudu hello. 

* **Vylepšení výkonu pro místně vázaných svazků** – aktualizace 4 je vylepšený výkon hello místně vázaných svazků ve scénářích, které mají přijímat vysoké data (velikost dat porovnatelný z hlediska toovolume).

* **Obnovení na základě Heatmap** – v dříve hello verzích následující obnovení po havárii (DR), hello data byla obnovena z cloudu hello podle vzorů přístupu hello výsledkem je nízký výkon. 

    Nová funkce je implementována v aktualizaci 4, sleduje často používaná data toocreate heatmap, když je zařízení hello v předchozí tooDR použití (nejčastěji používaných bloky dat mají vysokou heat, zatímco menší bloky dat používá mít nízkou heat). Po zotavení po Havárii používá StorSimple hello heatmap tooautomatically obnovení a rehydrataci při spotřebě hello data z cloudu hello. 

    Obnovení všech hello jsou nyní heatmap na základě obnovení. Další informace o tom, jak tooquery a zrušit heatmap na základě úlohy obnovení a rehydrataci, přejděte příliš[prostředí Windows PowerShell pro referenční informace o rutinách StorSimple](https://technet.microsoft.com/library/dn688168.aspx).

* **Nástroj diagnostiky StorSimple** – v aktualizaci 4 StorSimple diagnostiky, se nástroj vydané tooallow pro snadné diagnostiku a řešení potíží s problémy související s stav součásti toosystem, sítě, výkonu a hardwaru. Tento nástroj je spuštěn prostřednictvím hello Windows Powershellu pro StorSimple. Další informace, přejděte příliš[řešení problémů pomocí diagnostiky StorSimple nástroj](storsimple-8000-diagnostics.md).

* **Na základě uživatelského rozhraní nástroj pro migraci StorSimple** -předchozí toothis vydání, požadované migraci dat z řady 5000 7000 hello uživatelé tooexecute součást pracovní postup migrace hello pomocí rozhraní Azure PowerShell hello. V této verzi snadno použitelné StorSimple migrace na základě uživatelského rozhraní nástroje je k dispozici pro podporu toofacilitate hello stejný pracovní postup migrace. Tento nástroj by také umožňují hello konsolidace intervalů obnovení. 

* **Změny související s FIPS** – toto a vyšší verzi, ve výchozím nastavení všechna zařízení řady StorSimple 8000 hello zapnutá FIPS pro obě hello účty veřejného cloudu Azure a Microsoft Azure Government.

* **Aktualizovat změny** – v této verzi chyby související tooupdate bylo opraveno selhání.

* **Výstrahy pro selhání disku** – v této verzi se přidá nová výstraha s upozorněním, uživatel hello brzké selhání disku. Pokud narazíte na tuto výstrahu, obraťte se na Microsoft Support tooship náhradní disk. Další informace, přejděte příliš[hardwaru výstrahy na zařízení StorSimple](storsimple-manage-alerts.md#hardware-alerts).

* **Změny nahrazení řadiče** -rutinu, která umožňuje hello uživatele tooquery hello stav procesu nahrazení řadiče hello je přidaný do této verze. Další informace, přejděte toohello [rutiny tooquery řadiče nahrazení stav](https://technet.microsoft.com/library/dn688168.aspx).


## <a name="issues-fixed-in-update-4"></a>Chyby v aktualizaci 4

Hello následující tabulka obsahuje souhrn problémy, které jsme vyřešili v aktualizaci 4.    

| Ne | Funkce | Problém | Platí toophysical zařízení | Platí toovirtual zařízení |
| --- | --- | --- | --- | --- |
| 1 |Převzetí služeb při selhání |V hello starší verzi, po hello převzetí služeb při selhání, že byl problém související s toocleanup zjištěnými na hello zákazníka. Tento problém vyřešen v této verzi. |Ano |Ano |
| 2 |Místně vázaných svazků |V předchozí verzi hello došlo k problému toorelated svazku vytváření místně vázaných svazků, které by způsobilo chyby při vytváření svazku. Tento problém byl způsobeno kořenové a pevné v této verzi. |Ano |Ne |
| 3 |Balíček pro podporu |V předchozí verzi byly problémy související tooSupport balíček, který by způsobilo System.OutOfMemory výjimky nebo jiné chyby, které vedly k nedodržení vytvoření balíčku podpory. Tyto chyby budou opraveny v této verzi. |Ano |Ano |
| 4 |Monitorování |V předchozí verzi existuje problém související s toomonitoring grafy pro místně vázaných svazků, kde byla spotřeba ukazuje EB. Tato chyba se vyřeší v této verzi. |Ano |Ano |
| 5 |Migrace |V předchozí verzi nebyly k dispozici několik spolehlivost problémy související toohello migrace ze zařízení řady too8000 řady 5000 7000. Mít byly tyto problémy řešeny v této verzi. |Ano |Ano |
| 6 |Aktualizace |V předchozích verzích, pokud došlo k chybě aktualizace, hello řadiče přejde do režimu obnovení a proto hello uživatele nebylo možné zpracovat hello aktualizace a potřebovat toocontact Microsoft Support. <br> Toto chování se změnilo v této verzi. Pokud uživatel hello k selhání aktualizace po oba řadiče hello hello spuštěna stejná verze (aktualizace 4), hello řadiče nepřecházejí do režimu obnovení. Pokud uživatel hello zaznamená této chyby, doporučujeme, abyste chvilku počkejte a pak zkuste znovu aktualizovat hello. může Hello opakování úspěšné. Pokud hello opakování selže, budou měli požádat Microsoft Support. |Ano |Ano |


## <a name="known-issues-in-update-4-from-previous-releases"></a>Známé problémy v aktualizaci 4 z předchozích verzí

Nejsou žádné nové známé problémy v aktualizaci 4. Pro seznam problémů, které přenášejí tooUpdate 4 z předchozích verzí, přejděte příliš[poznámky k verzi Update 3](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-4"></a>Serial-attached SCSI (SAS) řadiče a aktualizace firmwaru v aktualizaci 4

Tato verze obsahuje řadič SAS a LSI ovladače a firmware aktualizace. Další informace o tom, jak tooinstall tyto aktualizace, najdete v části [nainstalovat aktualizace 4](storsimple-install-update-4.md) zařízení StorSimple.

## <a name="virtual-device-updates-in-update-4"></a>Aktualizace virtuální zařízení v aktualizaci 4

Tato aktualizace nemůže být použité toohello cloudu zařízení StorSimple (také označované jako hello virtuální zařízení). Nové virtuální zařízení bude nutné toobe vytvořili. 

## <a name="next-step"></a>Další krok

Zjistěte, jak příliš[nainstalovat aktualizace 4](storsimple-install-update-4.md) zařízení StorSimple.

