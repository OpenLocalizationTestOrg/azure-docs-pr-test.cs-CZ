---
title: "aaaStorSimple 8000 aktualizovat 0,3 poznámky k verzi | Microsoft Docs"
description: "Popisuje hello nové funkce a opravy, otevřete problémy a k dispozici řešení pro hello únor 2015 verze Microsoft Azure StorSimple (Update 0.3)."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: b01bfd04-f9f8-45f4-ade8-95ac2b979e6a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: 53638f9d286b2085c6b45f9e3fae75c34fc73e7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-03-release-notes---february-2015"></a>Aktualizace řady StorSimple 8000 0,3 poznámky – únor 2015
## <a name="overview"></a>Přehled
Následující poznámky k verzi Hello identifikovat hello kritické otevřete problémy pro aktualizaci řady StorSimple 8000 0,3 vydané v únor 2015. Také obsahují seznam hello StorSimple softwaru a aktualizace firmwaru, které jsou součástí této verze. Po vydání řady StorSimple 8000 verze hello byl proveden všeobecně dostupná v červenec 2014 se hello třetí verzi.

Tato aktualizace se změní verze softwaru hello zařízení z hello leden aktualizace. Verze toobe 6.3.9600.17312, pokračuje. Můžete potvrdit, že hello aktualizace byla nainstalována kontrolou hello **poslední aktualizovat** datum. Pokud je datum hello 2/10/2015 nebo novější, pak hello aktualizace byl úspěšně nainstalován.  

Doporučujeme, abyste vyhledat a použít všechny dostupné aktualizace ihned po instalaci zařízení StorSimple. Můžete také zapnout automatické aktualizace toodownload a nainstalovat důležité aktualizace od společnosti Microsoft při jejich vydání. Další informace najdete v tématu [aktualizace zařízení StorSimple](storsimple-update-device.md).  

Přečtěte si hello informace obsažené v uvolněte hello poznámky před nasazením hello aktualizovat v řešení StorSimple.  

> [!IMPORTANT]
> * Pomocí služby StorSimple Manager hello a není prostředí Windows PowerShell pro aktualizaci února hello tooinstall StorSimple.   
> * Tato aktualizace trvá přibližně tooinstall hodinu. Ale pokud k instalaci kumulativní aktualizace, hello proces může trvat toocomplete o 3 hodiny.  
> * Února verzi zařízení StorSimple Hello neobsahuje žádné virtuální zařízení aktualizace toohello StorSimple. Můžete také použít všechny dostupné aktualizace toohello virtuální zařízení s Windows, včetně poslední zabezpečení opravy, ale neuvidíte změny ve verzi pro virtuální zařízení hello.  
> 
> 

Ujistěte se, že hello následující požadované součásti jsou nesplnění předchozí tooupdating zařízení StorSimple.  

* Ujistěte se, že oba řadiče zařízení běží před aktualizací. Pokud buď řadiče neběží, se nezdaří kontrola hello. tooverify, které hello řadiče jsou v dobrém stavu, přejděte příliš**stavu hardwaru** pod hello **údržby** stránky. Pokud jsou komponenty, **vyžadují pozornost**, obraťte se na Microsoft Support soubory protokolů.
* Zajistěte, aby pevné IP adresy pro řadič 0 a řadič 1 jsou směrovatelné a můžete připojit toohello Internetu, které se používají pro obsluhu hello aktualizace toohello zařízení. Můžete použít hello [rutiny Test-Connection](https://technet.microsoft.com/library/hh849808.aspx) tooping známou adresu mimo hello sítě, jako je outlook.com, tooverify, který hello řadiče má toohello připojení mimo síť.
* Ujistěte se, že porty 80 a 443 jsou k dispozici v zařízení StorSimple pro odchozí komunikaci. Další informace najdete v tématu hello [požadavcích sítě pro zařízení StorSimple](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device).
* Pokud verze softwaru zařízení hello je starší než 6.3.9600.17312 (října 2014 aktualizace), zakažte hello Data 2 a Data 3 porty, pokud je povoleno, před spuštěním aktualizace hello. Pak hello Data 2 nebo Data 3 porty povolené při instalaci aktualizace hello může způsobit vaše toogo řadiče zařízení do režimu obnovení. Upozorňujeme, že když zakážete hello síťových rozhraní, všechny svazky hello přidružené bude převedeno do režimu offline a hello vstupně-výstupních operací se přeruší hello dobu aktualizace hello.  

## <a name="whats-new-in-hello-february-release"></a>Co je nového ve verzi února hello
Tato aktualizace obsahuje oprava hello objekt pro vytváření resetování problému, ke kterým došlo na zařízení, kteří upgradovali z verze toohello hello GA října 2014 verze. Další informace najdete v tématu [problémy opravit v této verzi](#issues-fixed-in-the-february-release).   

Tato aktualizace neobsahuje nové funkce nebo funkce.  

## <a name="issues-fixed-in-hello-february-release"></a>Chyby v hello února verze
Hello následující tabulka popisuje hello problému, který byl vyřešen v této aktualizaci.  

| Ne. | Funkce | Problém | Platí toophysical zařízení | Platí toovirtual zařízení |
| --- | --- | --- | --- | --- |
| 1 |Obnovení továrního nastavení |Pokusíte tooperform tovární nastavení zařízení, který měl původně hello GA verze (verze 6.3.9600.17215) nainstalován, ale byl aktualizovaný toohello říjen verze (verze 6.3.9600.17312). Obnovit Hello tovární nastavení selže a nestabilní hello zařízení. |Ano |Ne |

## <a name="known-issues-in-hello-february-release"></a>Známé problémy ve verzi února hello
Hello následující tabulka obsahuje souhrn známých problémů v této verzi.

| Ne. | Funkce | Problém | Komentáře nebo alternativní řešení | Platí toophysical zařízení | Platí toovirtual zařízení |
| --- | --- | --- | --- | --- | --- |
| 1 |Obnovení továrního nastavení |V některých případech při provádění obnovení továrního nastavení, hello zařízení StorSimple může být pomalé a zobrazit tato zpráva: **toofactory resetování probíhá (fáze 8)**. To se stane, když stisknutím CTRL + C, když probíhá hello rutiny. |Není stiskněte kombinaci kláves CTRL + C po inicializaci obnovení továrního nastavení. Pokud jste již v tomto stavu, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 2 |Disk kvora |Ve výjimečných případech odpojení hello většinu disků ve skříni EBOD hello z 8600device jsou výsledkem žádný disk kvora, pak hello fondu úložiště bude offline. Zůstane offline, i když jsou znovu připojeny disky hello. |Budete potřebovat tooreboot hello zařízení. Pokud hello potíže potrvají, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 3 |Selhání snímku cloudu |Ve výjimečných případech může cloudový snímek nezdaří s chybou hello **zálohování dosažen limit maximální**. K tomu dojde, pokud být delší než 255 online klony na hello stejného zařízení, z hello stejné původního svazku, který byl odstraněn. | |Ano |Ano |
| 4 |Nesprávný řadiče ID |Když se provádí nahrazení řadiče, může řadič 0 zobrazí jako řadič 1. Při nahrazení řadiče při načtení hello bitovou kopii z uzlu sdílené hello, hello řadiče ID můžete zobrazují původně jako ID řadiče sdílené hello. Ve výjimečných případech může být toto chování vidět také po restartu systému. |Není třeba žádné akce uživatele. Tato situace bude automaticky vyřešen po dokončení nahrazení řadiče hello. |Ano |Ne |
| 5 |Monitorování grafy zařízení |V hello služby StorSimple Manager monitorování grafy hello zařízení nefungují, pokud základní nebo v konfiguraci proxy serveru pro zařízení hello hello je povolené ověřování NTLM. |Úprava konfigurace webového proxy serveru pro hello zařízení zaregistrovali služby StorSimple Manager tak, že ověřování nastavíte tooNONE hello. toodo se hello hello spuštění prostředí Windows PowerShell pro rutinu StorSimple Set-HcsWebProxy. |Ano |Ano |
| 6 |Účty úložiště |Použití účtu úložiště hello úložiště služby toodelete hello se o nepodporovaný scénář. To povede tooa situace, ve kterém nelze načíst data uživatele. | |Ano |Ano |
| 7 |Převzetí služeb při selhání zařízení |Více převzetí služeb kontejner svazků z hello stejný zdroj zařízení toodifferent cílové zařízení není podporován.    Převzetí služeb při selhání jednom zařízení neaktivní toomultiple zařízení bude hello kontejnery svazků na hello nejprve převzít služby při selhání zařízení dojít ke ztrátě dat vlastnictví. Po takové převzetí služeb při selhání, budou tyto kontejnery svazků zobrazí nebo chovat jinak při zobrazení v hello portál Azure classic. | |Ano |Ne |
| 8 |Instalace |Během StorSimple adaptéru pro instalaci služby SharePoint je nutné tooprovide zařízení IP v pořadí pro toofinish hello instalace úspěšně. | |Ano |Ne |
| 9 |Webový proxy server |Pokud má vaše konfigurace webového proxy serveru HTTPS jako hello zadaný protokol, pak bude mít vliv komunikaci služby zařízení a hello zařízení přejde do režimu offline. Podpora balíčky se budou generovat také v procesu hello spotřebovávat značné množství prostředků vašeho zařízení. |Ujistěte se, že adresa URL proxy serveru webové hello má HTTP jako hello zadaný protokol. Další informace o příliš[konfigurace webového proxy serveru pro vaše zařízení](storsimple-configure-web-proxy.md). |Ano |Ne |
| 10 |Webový proxy server |Pokud nakonfigurujete a povolíte webový proxy server na registrované zařízení, bude potřebovat toorestart hello aktivního řadiče na vašem zařízení. | |Ano |Ne |
| 11 |Cloud vysokou latencí a vysokou vstupně-výstupní úlohy |Když v zařízení StorSimple dojde kombinaci cloudu velmi vysoké latenci (pořadí sekund) a vysoké vstupně-výstupní úlohy, svazky hello zařízení přejděte do sníženou stavu a hello vstupně-výstupních operací může selhat s chybou "zařízení není připraveno". |Budete potřebovat řadiče zařízení hello toomanually restartování nebo provést převzetí služeb při selhání toorecover zařízení z této situaci. |Ano |Ne |

## <a name="physical-device-updates-in-hello-february-release"></a>Aktualizace fyzického zařízení ve verzi února hello
Obnovit tento aktualizace opravy hello tovární nastavení problém, který došlo k chybě v zařízení, kteří upgradovali z verze toohello hello GA října 2014 verze. Neobsahuje žádné jiné zařízení StorSimple aktualizace toohello.  

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-hello-february-release"></a>Serial-attached SCSI (SAS) řadiče a aktualizace firmwaru v únoru verzi hello
Tato verze neobsahuje žádné aktualizace toohello serial-attached SCSI (SAS) řadiče nebo hello firmware. aktualizace ovladačů Hello byl v hello říjnu verze 2014.  

## <a name="virtual-device-updates-in-hello-february-release"></a>Aktualizace virtuální zařízení ve verzi února hello
Tato verze neobsahuje žádné aktualizace pro virtuální zařízení hello. Použití této aktualizace se nezmění hello softwaru verzi virtuálního zařízení.

