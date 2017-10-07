---
title: "Poznámky k verzi aaaStorSimple 8000 Update 2 řady | Microsoft Docs"
description: "Popisuje nové funkce hello, problémy a řešení pro zařízení StorSimple 8000 řady Update 2."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: e2c8bffd-7fc5-4b77-b632-a4f59edacc3a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/24/2016
ms.author: v-sharos
ms.openlocfilehash: 36c75aad900c7b1286a924732967b8ee519a3d4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-2-release-notes"></a>Poznámky k verzi zařízení StorSimple 8000 řady Update 2
## <a name="overview"></a>Přehled
Hello následující poznámky k verzi popisují hello nové funkce a identifikovat hello kritické otevřené problémy pro StorSimple 8000 řady Update 2. Také obsahují seznam hello softwaru zařízení StorSimple, ovladačů a aktualizace firmwaru disku součástí této verze. 

Aktualizace 2 může být zařízení StorSimple použité tooany systémem verze (GA) nebo Update 0.1 prostřednictvím aktualizace 1.2. Hello zařízení verzi spojenou s aktualizací 2 je 6.3.9600.17673.

Přečtěte si hello informace obsažené v uvolněte hello poznámky před nasazením hello aktualizovat v řešení StorSimple.

> [!IMPORTANT]
> * Tato aktualizace (včetně aktualizací Windows hello) trvá přibližně 4-7 hodin tooinstall. 
> * Aktualizace 2 má softwaru, LSI ovladače a aktualizace firmwaru SSD.
> * Pro nové verze, nemusí zobrazit aktualizace okamžitě, protože provedeme postupné zavádění hello aktualizací. Počkejte několik dní, a pak vyhledejte aktualizace znovu jako tyto brzy bude dostupná.
> 
> 

## <a name="whats-new-in-update-2"></a>Co je nového v aktualizaci Update 2
Aktualizace 2 zavádí následující nové funkce hello.

* **Místně vázaný svazky** – v předchozích verzích řady StorSimple 8000 hello bloků dat byly vrstvené toohello cloudu na základě využití. Došlo k dispozici žádné tooguarantee způsobem, který by bloky Zůstaňte na místní. V aktualizaci Update 2 Když vytváříte svazek, můžete určit svazek místně vázaný a primární data ze svazku nebudou vrstvené toohello cloudu. Snímky místně vázaných svazků bude stále zkopírují toohello cloudu pro zálohování, takže hello cloudu lze použít pro účely obnovení dat mobility a po havárii. Kromě toho můžete změnit typ svazku hello (tedy převést svazky toolocally připnutý vrstvené svazky a převést místně připnutý tootiered svazky). 
* **Vylepšení virtuálního zařízení StorSimple** – dřív hello řady StorSimple 8000 umístěný hello virtuálního zařízení jako řešení pro obnovení nebo vývoj/testování po havárii. Došlo jenom jeden model virtuálního zařízení (model 1100). Aktualizace 2 uvádí dva modely virtuální zařízení: 
  
  * 8010 (dříve nazývané hello 1100) – žádná změna; má kapacitou 30 TB a používá standardní úložiště Azure.
  * 8020 – má kapacitou 64 TB a pro zlepšení výkonu používá Azure Premium storage.
    
    Neexistuje jeden virtuální pevný disk pro oba modely virtuální zařízení (8010/8020). Při prvním spuštění virtuálního zařízení hello, zjistí hello platformy parametry a použije hello modelu správnou verzi.
* **Vylepšení sítě** – Update 2 obsahuje následující vylepšení sítí hello:
  
  * Několik síťových adaptérů se dá nastavit pro hello cloud tak, aby převzetí služeb při selhání může dojít, pokud síťový adaptér se nezdaří.
  * Směrování vylepšení s pevnou metriky pro cloud povolené bloky.
  * Online opakování se nezdařilo prostředky převzetí služeb při selhání.
  * Nové výstrahy pro selhání služby.
* **Aktualizace vylepšení** – v aktualizaci 1.2 a starší, byla aktualizována řady StorSimple 8000 hello prostřednictvím dvou kanálů: Windows Update pro clustering, iSCSI a tak dále a Microsoft Update pro binární soubory a firmwaru.
    Aktualizace 2 používá službu Microsoft Update všech balíčků aktualizace. To by měla vést tooless čas opravy nebo provádění převzetí služeb při selhání. 
* **Aktualizace firmwaru** – hello následující firmware aktualizace jsou součástí:
  
  * LSI: lsi_sas2.sys verze produktu 2.00.72.10
  * Pouze SSD (žádné aktualizace HDD): XMGG, XGEG, KZ50, F6C2 a VR08
* **Proaktivní podporu** – Microsoft toopull jiné diagnostické informace z hello zařízení umožňuje Update 2. Když náš tým operations identifikuje zařízení, která došlo k potížím, jsme jsou lepší vybavené toocollect informace z hello zařízení a diagnostikovat problémy. **Přijetím Update 2, můžete nám umožňují tooprovide tato proaktivní podpora**.    

## <a name="issues-fixed-in-update-2"></a>Chyby v Update 2
Následující tabulky Hello poskytuje shrnutí problémů, které byly odstraněny v aktualizace 2.    

| Ne. | Funkce | Problém | Platí toophysical zařízení | Platí toovirtual zařízení |
| --- | --- | --- | --- | --- |
| 1 |Síťová rozhraní |Po upgradu tooUpdate 1 hello služby StorSimple Manager oznámila, že porty Data2 a Data3 hello se nezdařilo u jednoho řadiče. Tento problém byl opraven. |Ano |Ne |
| 2 |Aktualizace |Po upgradu tooUpdate 1 zvukové poplašné výstrahy došlo k chybě v hello portál Azure classic na několika zařízeních. Tento problém byl opraven. |Ano |Ne |
| 3 |Openstack ověřování |Při použití Openstack jako poskytovatele cloudových služeb, obdržíte chybu, že řetězec vašeho cloudu ověřování byla příliš dlouhá. To byl opraven. |Ano |Ne |

## <a name="known-issues-in-update-2"></a>Známé problémy v aktualizaci Update 2
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
| 20 |Místně vázaných svazků |Pokud se pokusíte tooconvert vrstvený svazek (vytvořený a klonovaný s aktualizace 1.2 nebo starší) tooa místně připnutý svazku a zařízení není dostatek místa nebo je výpadku cloudu a potom hello clone(s) může být poškozený. |K tomuto problému dochází pouze pro svazky, které byly vytvořené a klonovaný s před aktualizací 2 softwaru. To by měl být nepravidelným scénář. | | |
| 21 |Převod svazku |Neaktualizovat hello ACRs připojené tooa svazku při převodu svazku probíhá (vrstvené toolocally připnuté nebo naopak). Aktualizace hello ACRs může mít za následek poškození dat. |V případě potřeby aktualizujte převodu svazku předchozí toohello ACRs hello a neprovádějte žádné další aktualizace ACR během převodu hello. | | |

## <a name="controller-and-firmware-updates-in-update-2"></a>Řadiče a aktualizace firmwaru v aktualizaci Update 2
Tato verze aktualizuje hello ovladače a firmware hello disku na svém zařízení.

* Další informace o hello LSI firmware aktualizace, najdete v článku znalostní báze Microsoft Knowledge base 3121900. 
* Další informace o disku firmware hello aktualizace, najdete v článku znalostní báze Microsoft Knowledge base 3121899.

## <a name="virtual-device-updates-in-update-2"></a>Aktualizace virtuální zařízení v aktualizaci Update 2
Tato aktualizace nemůže být použité toohello virtuální zařízení. Nové virtuální zařízení bude nutné toobe vytvořili. 

## <a name="next-step"></a>Další krok
Zjistěte, jak příliš[instalaci aktualizace 2](storsimple-install-update-2.md) zařízení StorSimple.

