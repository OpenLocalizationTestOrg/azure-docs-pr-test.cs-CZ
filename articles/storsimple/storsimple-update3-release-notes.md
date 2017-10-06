---
title: "Poznámky k verzi Update 3 řady aaaStorSimple 8000 | Microsoft Docs"
description: "Popisuje nové funkce hello, problémy a řešení pro zařízení StorSimple 8000 řady Update 3."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 2158aa7a-4ac3-42ba-8796-610d1adb984d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5bfcba61f7f210531437f650eafaad4dbd8c7c45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="update-3-release-notes-for-your-storsimple-8000-series-device"></a>Aktualizace 3 poznámky k verzi pro vaše zařízení řady StorSimple 8000

## <a name="overview"></a>Přehled
Hello následující poznámky k verzi popisují hello nové funkce a identifikovat hello kritické otevřené problémy pro StorSimple 8000 řady Update 3. Také obsahují seznam aktualizací softwaru zařízení StorSimple hello součástí této verze. 

Aktualizace 3 může být zařízení StorSimple použité tooany systémem verze (GA) nebo Update 0.1 prostřednictvím aktualizace 2.2. Hello zařízení verzi spojenou s aktualizací 3 je 6.3.9600.17759.

Přečtěte si hello informace obsažené v uvolněte hello poznámky před nasazením hello aktualizovat v řešení StorSimple.

> [!IMPORTANT]
> * Aktualizace 3 má software zařízení, LSI ovladače a firmware a ovladače Storport a Spaceport aktualizace. Tato aktualizace trvá přibližně tooinstall 1.5 – 2 hodiny. 
> * Pro nové verze, nemusí zobrazit aktualizace okamžitě, protože provedeme postupné zavádění hello aktualizací. Počkejte několik dní, a pak vyhledejte aktualizace znovu jako tyto brzy bude dostupná.
> 
> 

## <a name="whats-new-in-update-3"></a>Co je nového ve verzi Update 3
Hello následující klíčových vylepšení a opravy chyb byly provedeny ve verzi Update 3.

* **Automatizované změny recyklace místa** – od aktualizace 3, algoritmy recyklace místa hello se spouští na hello pohotovostní řadiči systému hello výsledkem je rychlejší spouštění. Další informace o hello porty, které jsou požadované toowork recyklací místa, najdete v části toohello [StorSimple sítě požadavky](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
* **Vylepšení výkonu** – Update 3 je vylepšený výkon čtení a zápis toohello cloudu.
* **Vylepšení související s migrací** – v této verzi několik oprav chyb a vylepšení měla provést pro funkci migrace hello z řad 5000/7000 zařízení too8000 řady zařízení. Další informace o tom, jak toouse hello funkce migrace, přejděte příliš[migrace z řad 5000/7000 zařízení too8000 řady zařízení](https://gallery.technet.microsoft.com/Azure-StorSimple-50007000-c1a0460b). 
* **Monitorování související opravy** – v této verzi chyby související s toomonitoring grafy, řídicí panel služby a řídicí panel zařízení jsme vyřešili.

## <a name="issues-fixed-in-update-3"></a>Chyby v Update 3
Následující tabulky Hello poskytuje shrnutí problémů, které jsme vyřešili ve verzi Update 3.    

| Ne | Funkce | Problém | Platí toophysical zařízení | Platí toovirtual zařízení |
| --- | --- | --- | --- | --- |
| 1 |Migrace dat na straně hostitele |V hello starší verze, hello zařízení StorSimple cloudu byl přechod do stavu offline při migraci dat na straně hostitele. Tento problém vyřešen v této verzi. |Ne |Ano |
| 2 |Místně vázaných svazků |V předchozí verzi hello byly problémy související tooI/O selhání, selhání převodu svazku a selhání datapath místně vázaných svazků. Tyto problémy se kvůli kořenové a pevné v této verzi. |Ano |Ne |
| 3 |Monitorování |Bylo víc problémy související tooreporting jednotek a monitorování a také zařízení řídicího panelu grafů, kde nesprávné informace o zobrazila pro místně připojené svazky. V této verzi opravě těchto problémů. |Ano |Ne |
| 4 |Zápisy náročnými vstupně-výstupních operací |Při použití StorSimple pro úlohy zahrnující velkou zápisy, uživatel hello by spustit do nepravidelným chyb, kde se právě hello pracovní sada vrstvené do cloudu hello. Tato chyba je opravena v této verzi. |Ano |Ano |
| 5 |Zálohování |V určitých výjimečných případech v předchozích verzích hello softwaru když uživatel provedl zálohování vzdálené klon, by spustit do cloudu chyby a operaci hello by chybu. V této verzi hello problému a po úspěšném dokončení operace hello. |Ano |Ano |
| 6 |Zásady zálohování |V určitých výjimečných případech v hello starší verze softwaru, byl chyby související s toohello odstranění zásady zálohování. Tento problém vyřešen v této verzi. |Ano |Ano |

## <a name="known-issues-in-update-3"></a>Známé problémy ve verzi Update 3
Hello následující tabulka obsahuje souhrn známých problémů v této verzi.

| Ne. | Funkce | Problém | Komentáře nebo alternativní řešení | Platí toophysical zařízení | Platí toovirtual zařízení |
| --- | --- | --- | --- | --- | --- |
| 1 |Disk kvora |Ve výjimečných případech odpojení hello většinu disků ve skříni EBOD hello zařízení 8600 jsou výsledkem žádný disk kvora, pak fondu úložiště hello přejde do režimu offline. Zůstane offline, i když jsou znovu připojeny disky hello. |Budete potřebovat tooreboot hello zařízení. Pokud hello potíže potrvají, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 2 |Nesprávný řadiče ID |Když se provádí nahrazení řadiče, může řadič 0 zobrazí jako řadič 1. Při nahrazení řadiče při načtení hello bitovou kopii z uzlu sdílené hello, hello řadiče ID můžete zobrazují původně jako ID řadiče sdílené hello. Ve výjimečných případech může být toto chování vidět také po restartu systému. |Není třeba žádné akce uživatele. Tato situace bude automaticky vyřešen po dokončení nahrazení řadiče hello. |Ano |Ne |
| 3 |Účty úložiště |Použití účtu úložiště hello úložiště služby toodelete hello se o nepodporovaný scénář. To povede tooa situace, ve kterém nelze načíst data uživatele. | |Ano |Ano |
| 4 |Převzetí služeb při selhání zařízení |Více převzetí služeb kontejner svazků z hello stejný zdroj zařízení toodifferent cílové zařízení není podporován. Převzetí služeb při selhání jednom zařízení neaktivní toomultiple zařízení bude hello kontejnery svazků na hello nejprve převzít služby při selhání zařízení dojít ke ztrátě dat vlastnictví. Po takové převzetí služeb při selhání, budou tyto kontejnery svazků zobrazí nebo chovat jinak při zobrazení v hello portál Azure classic. | |Ano |Ne |
| 5 |Instalace |Během StorSimple adaptéru pro instalaci služby SharePoint je nutné tooprovide zařízení IP v pořadí pro toofinish hello instalace úspěšně. | |Ano |Ne |
| 6 |Webový proxy server |Pokud má vaše konfigurace webového proxy serveru HTTPS jako hello zadaný protokol, pak bude mít vliv komunikaci služby zařízení a hello zařízení přejde do režimu offline. Podpora balíčky se budou generovat také v procesu hello spotřebovávat značné množství prostředků vašeho zařízení. |Ujistěte se, že adresa URL proxy serveru webové hello má HTTP jako hello zadaný protokol. Další informace, přejděte příliš[konfigurace webového proxy serveru pro vaše zařízení](storsimple-configure-web-proxy.md). |Ano |Ne |
| 7 |Webový proxy server |Pokud nakonfigurujete a povolíte webový proxy server na registrované zařízení, bude potřebovat toorestart hello aktivního řadiče na vašem zařízení. | |Ano |Ne |
| 8 |Cloud vysokou latencí a vysokou vstupně-výstupní úlohy |Když v zařízení StorSimple dojde kombinaci cloudu velmi vysoké latenci (pořadí sekund) a vysoké vstupně-výstupní úlohy, svazky hello zařízení přejděte do sníženou stavu a hello vstupně-výstupních operací může selhat s chybou "zařízení není připraveno". |Budete potřebovat řadiče zařízení hello toomanually restartování nebo provést převzetí služeb při selhání toorecover zařízení z této situaci. |Ano |Ne |
| 9 |Azure PowerShell |Při použití rutiny StorSimple hello **Get-AzureStorSimpleStorageAccountCredential &#124; Select-Object - nejprve 1 - čekání** tooselect hello první objekt, takže můžete vytvořit nový **VolumeContainer** objektu hello rutina vrátí všechny objekty hello. |Zabalení hello rutiny v závorkách následujícím způsobem: **(Get-Azure-StorSimpleStorageAccountCredential) &#124; Select-Object - nejprve 1 - čekání** |Ano |Ano |
| 10 |Migrace |Při předávání více kontejnery svazků pro migraci, je přesné jenom pro první kontejneru svazků hello hello ÉTA pro poslední zálohu. Kromě toho paralelní migrace se spustí po hello první 4 záloh v kontejneru svazků první hello se migrují. |Doporučujeme, abyste současně migrovat jeden kontejner svazků. |Ano |Ne |
| 11 |Migrace |Po obnovení hello nejsou přidány svazky toohello zálohování zásady nebo hello skupinu virtuálního disku. |Je nutné tooadd tyto svazky tooa zásady zálohování v pořadí toocreate zálohy. |Ano |Ano |
| 12 |Migrace |Po dokončení migrace hello hello 5000/7000 řady zařízení nesmí mít přístup k hello migrovat data kontejnery. |Doporučujeme vám odstranit hello migrovat data kontejnery po hello migrace je kompletní a potvrzené. |Ano |Ne |
| 13 |Klon a zotavení po Havárii |Zařízení StorSimple softwarem Update 1 nelze klonovat nebo provést zařízení tooa obnovení po havárii 1 softwarem před aktualizací. |Budete potřebovat tooupdate hello cílové zařízení tooUpdate 1 tooallow těchto operací |Ano |Ano |
| 14 |Migrace |Zálohu konfigurace pro migraci na zařízení řady 5000 7000 může selhat, pokud existují skupiny svazek s žádné přidružené svazky. |Odstranit všechny skupiny hello prázdný svazek s žádné přidružené svazky a poté opakujte hello zálohu konfigurace. |Ano |Ne |
| 15 |Rutiny Azure PowerShell a místně vázaných svazků |Nelze vytvořit místně vázaný svazek prostřednictvím rutin prostředí Azure PowerShell. (Jakýkoli svazek, který vytvoříte pomocí prostředí Azure PowerShell bude víceúrovňová.) |Vždy používejte svazky tooconfigure místně vázaný služby StorSimple Manager hello. |Ano |Ne |
| 16 |K dispozici místa pro místně vázaných svazků |Pokud odstraníte místně vázaný svazek, hello místa na disku pro nové svazky nemusí být okamžitě aktualizován. aktualizace služby StorSimple Manager Hello hello volné místo dostupné přibližně za hodinu. |Počkejte, než jednu hodinu, než se pokusíte toocreate hello nový svazek. |Ano |Ne |
| 17 |Místně vázaných svazků |Úlohu obnovení zpřístupní hello dočasné snímku zálohy v hello zálohování katalogu, ale jenom pro dobu trvání hello hello obnovení úlohy. Kromě toho zpřístupňuje skupinu virtuální disk s předponou **tmpCollection** na hello **zásady zálohování** stránky, ale jenom pro dobu trvání hello hello obnovit úlohu. |Toto chování může dojít, pokud vaše úlohy obnovení má pouze místně vázaný svazky nebo kombinaci místně vázaný a vrstvené svazky. Pokud úloha obnovení hello obsahuje pouze vrstvené svazky, toto chování nedojde. Není vyžadován žádný zásah uživatele. |Ano |Ne |
| 18 |Místně vázaných svazků |Pokud zrušení úlohy obnovení a dojde k selhání řadiče okamžitě později, se zobrazí úloha obnovení hello **se nezdařilo** místo **zrušeno**. Pokud úloha obnovení selže a dojde k selhání řadiče okamžitě později, se zobrazí úloha obnovení hello **zrušeno** místo **se nezdařilo**. |Toto chování může dojít, pokud vaše úlohy obnovení má pouze místně vázaný svazky nebo kombinaci místně vázaný a vrstvené svazky. Pokud úloha obnovení hello obsahuje pouze vrstvené svazky, toto chování nedojde. Není vyžadován žádný zásah uživatele. |Ano |Ne |
| 19 |Místně vázaných svazků |Pokud zrušení úlohy obnovení, nebo pokud se nezdaří obnovení a pak dojde k selhání řadiče, úlohu další obnovení se zobrazí na hello **úlohy** stránky. |Toto chování může dojít, pokud vaše úlohy obnovení má pouze místně vázaný svazky nebo kombinaci místně vázaný a vrstvené svazky. Pokud úloha obnovení hello obsahuje pouze vrstvené svazky, toto chování nedojde. Není vyžadován žádný zásah uživatele. |Ano |Ne |
| 20 |Místně vázaných svazků |Pokud se pokusíte tooconvert vrstvený svazek (vytvořený a klonovaný s aktualizace 1.2 nebo starší) tooa místně připnutý svazku a zařízení není dostatek místa nebo je výpadku cloudu a potom hello clone(s) může být poškozený. |K tomuto problému dochází pouze pro svazky, které byly vytvořené a klonovaný s 2.1 před aktualizací softwaru. To by měl být nepravidelným scénář. | | |
| 21 |Převod svazku |Neaktualizovat hello ACRs připojené tooa svazku při převodu svazku probíhá (vrstvené toolocally připnuté nebo naopak). Aktualizace hello ACRs může mít za následek poškození dat. |V případě potřeby aktualizujte převodu svazku předchozí toohello ACRs hello a neprovádějte žádné další aktualizace ACR během převodu hello. | | |
| 22 |Aktualizace |Při použití Update 3, hello **údržby** stránku hello Azure classic portálu se následující hello zobrazení zprávy související tooUpdate 2 – "řady StorSimple 8000 Update 2 obsahuje schopnost shromažďovat tooproactively Microsoft hello protokolování informací z vašeho zařízení když jsme potenciální problémy". To je zavádějící, protože naznačuje, že toto zařízení hello se aktualizované tooUpdate 2. Když zařízení hello succeesfully aktualizovat tooUpdate 3, tato zpráva zmizí. |Toto chování bude opraven v budoucí verzi. |Ano |Ne |

## <a name="controller-and-firmware-updates-in-update-3"></a>Řadiče a aktualizace firmwaru. ve verzi Update 3
Tato verze obsahuje LSI ovladače a firmware aktualizace. Další informace o tom, jak tooinstall hello LSI ovladače a firmware aktualizace, najdete v části [instalaci aktualizace Update 3](storsimple-install-update-3.md) zařízení StorSimple.

## <a name="virtual-device-updates-in-update-3"></a>Aktualizace virtuální zařízení ve verzi Update 3
Tato aktualizace nemůže být použité toohello cloudu zařízení StorSimple (také označované jako hello virtuální zařízení). Nové virtuální zařízení bude nutné toobe vytvořili. 

## <a name="next-step"></a>Další krok
Zjistěte, jak příliš[instalaci aktualizace Update 3](storsimple-install-update-3.md) zařízení StorSimple.

