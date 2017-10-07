---
title: "aaaStorSimple 8000 aktualizovat 0,2 poznámky k verzi | Microsoft Docs"
description: "Popisuje hello nové funkce a opravy, otevřete problémy a k dispozici řešení pro hello leden 2015 verze Microsoft Azure StorSimple (Update 0,2)."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: d9684ae3-b38f-4678-9d70-e5dbc6b03350
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/16/2016
ms.author: v-sharos
ms.openlocfilehash: 1cee795df0b53d9b9276bc33074cf1a7aa188835
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-02-release-notes---january-2015"></a>Aktualizace řady StorSimple 8000 0,2 poznámky – leden 2015
## <a name="overview"></a>Přehled
Hello následující poznámky k verzi identifikovat hello kritické otevřené problémy hello leden 2015 verze služby Microsoft Azure StorSimple. Také obsahují seznam hello StorSimple softwaru a aktualizace firmwaru, které jsou součástí této verze. Toto je druhý verze hello po vydání řady StorSimple 8000 verze hello byl proveden všeobecně dostupná v červenec 2014.

Tato aktualizace se změní verze softwaru hello fyzického zařízení z hello říjen aktualizace. Verze toobe 6.3.9600.17312, pokračuje. Obrázek Hello používá image virtuálního zařízení hello došlo ke změně v této verzi. Proto všechny nové virtuální zařízení hello vytvořili po 20 1. 2015 se zobrazí verze hello jako 6.3.9600.17361.  

Přečtěte si následující informace obsažené v hello poznámky k verzi pro aktualizace z ledna 2015 hello hello.

> [!IMPORTANT]
> * Tato aktualizace není k dispozici prostřednictvím služby Windows Update a není možné nainstalovat jako jiné aktualizace. Vaše zařízení neobdrží tuto aktualizaci, i v případě, že jste nainstalovali aktualizace hello pomocí hello portál Azure classic. Tato aktualizace bude platit pouze zařízení toovirtual vytvořili po 20 leden 2015. 
> * Leden verzi zařízení StorSimple Hello neobsahuje žádné fyzické zařízení aktualizace toohello StorSimple. Můžete také použít všechny dostupné aktualizace toohello virtuální zařízení s Windows, včetně poslední zabezpečení opravy, ale neuvidíte změny ve verzi pro fyzického zařízení StorSimple hello.
> 
> 

## <a name="whats-new-in-hello-january-release"></a>Co je nového ve verzi leden hello
Tato aktualizace obsahuje opravy související toohello svazky na virtuálním zařízení hello přechod do stavu offline. (Viz [problémy opravit v této verzi](#issues-fixed-in-the-january-release).)  

aktualizace Hello neobsahuje nové funkce nebo funkce.  

## <a name="issues-fixed-in-hello-january-release"></a>Chyby v hello leden verze
Hello následující tabulka popisuje hello problému, který byl vyřešen v této aktualizaci.

| Ne. | Funkce | Problém | Platí toophysical zařízení | Platí toovirtual zařízení |
| --- | --- | --- | --- | --- |
| 1 |Svazky přechod do stavu offline |Při zachování cloudu vysoké latenci pro několik minut, přejděte na hostitelích hello offline svazky virtuálního zařízení StorSimple hello. Tato oprava zvyšuje hello prahová hodnota latence cloudu, a současně minimalizujete její hello situacích, které by způsobily hello svazky offline toogo na hostitelích. |Ne |Ano |

## <a name="known-issues-in-hello-january-release"></a>Známé problémy v lednu verzi hello
Hello následující tabulka obsahuje souhrn známých problémů v této verzi.

| Ne. | Funkce | Problém | Komentáře nebo alternativní řešení | Platí toophysical zařízení | Platí toovirtual zařízení |
| --- | --- | --- | --- | --- | --- |
| 1 |Obnovení továrního nastavení |V některých případech při provádění obnovení továrního nastavení, hello zařízení StorSimple může být pomalé a zobrazit tato zpráva: **toofactory resetování probíhá (fáze 8).** To se stane, když stisknutím CTRL + C, když probíhá hello rutiny. |Není stiskněte kombinaci kláves CTRL + C po inicializaci obnovení továrního nastavení. Pokud jste již v tomto stavu, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 2 |Disk kvora |Ve výjimečných případech odpojení hello většinu disků ve skříni EBOD hello zařízení 8600 jsou výsledkem žádný disk kvora, pak hello fondu úložiště bude offline. Zůstane offline, i když jsou znovu připojeny disky hello. |Budete potřebovat tooreboot hello zařízení. Pokud hello potíže potrvají, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 3 |Selhání snímku cloudu |Ve výjimečných případech může cloudový snímek nezdaří s chybou hello **zálohování dosažen limit maximální**. K tomu dojde, pokud být delší než 255 online klony na hello stejného zařízení, z hello stejné původního svazku, který byl odstraněn. | |Ano |Ano |
| 4 |Nesprávný řadiče ID |Když se provádí nahrazení řadiče, může řadič 0 zobrazí jako řadič 1. Při nahrazení řadiče při načtení hello bitovou kopii z uzlu sdílené hello, hello řadiče ID můžete zobrazují původně jako ID řadiče sdílené hello.  Ve výjimečných případech může být toto chování vidět také po restartu systému. |Není třeba žádné akce uživatele. Tato situace bude automaticky vyřešen po dokončení nahrazení řadiče hello. |Ano |Ne |
| 5 |Monitorování grafy zařízení |V hello služby StorSimple Manager monitorování grafy hello zařízení nefungují, pokud základní nebo v konfiguraci proxy serveru pro zařízení hello hello je povolené ověřování NTLM. |Úprava konfigurace webového proxy serveru pro hello zařízení zaregistrovali služby StorSimple Manager tak, že ověřování nastavíte tooNONE hello. toodo se hello hello spuštění prostředí Windows PowerShell pro rutinu StorSimple Set-HcsWebProxy. |Ano |Ano |
| 6 |Účty úložiště |Použití účtu úložiště hello úložiště služby toodelete hello se o nepodporovaný scénář. To povede tooa situace, ve kterém nelze načíst data uživatele. | |Ano |Ano |
| 7 |Převzetí služeb při selhání zařízení |Více převzetí služeb kontejner svazků z hello stejný zdroj zařízení toodifferent cílové zařízení není podporován. |Převzetí služeb při selhání jednom zařízení neaktivní toomultiple zařízení bude hello kontejnery svazků na hello nejprve převzít služby při selhání zařízení dojít ke ztrátě dat vlastnictví. Po takové převzetí služeb při selhání, budou tyto kontejnery svazků zobrazí nebo chovat jinak při zobrazení v hello portál Azure classic. |Ano |Ne |
| 8 |Instalace |Během StorSimple adaptéru pro instalaci služby SharePoint je nutné tooprovide zařízení IP v pořadí pro toofinish hello instalace úspěšně. | |Ano |Ne |
| 9 |Webový proxy server |Pokud má vaše konfigurace webového proxy serveru HTTPS jako hello zadaný protokol, pak bude mít vliv komunikaci služby zařízení a hello zařízení přejde do režimu offline. Podpora balíčky se budou generovat také v procesu hello spotřebovávat značné množství prostředků vašeho zařízení. |Ujistěte se, že adresa URL proxy serveru webové hello má HTTP jako hello zadaný protokol. Další informace o postupu naleznete na příliš[konfigurace webového proxy serveru pro vaše zařízení](storsimple-configure-web-proxy.md). |Ano |Ne |
| 10 |Webový proxy server |Pokud nakonfigurujete a povolíte webový proxy server na registrované zařízení, bude potřebovat toorestart hello aktivního řadiče na vašem zařízení. | |Ano |Ne |
| 11 |Cloud vysokou latencí a vysokou vstupně-výstupní úlohy |Když v zařízení StorSimple dojde kombinaci cloudu velmi vysoké latenci (pořadí sekund) a vysoké vstupně-výstupní úlohy, svazky hello zařízení přejděte do sníženou stavu a hello vstupně-výstupních operací může selhat s chybou "zařízení není připraveno". |Budete potřebovat řadiče zařízení hello toomanually restartování nebo provést převzetí služeb při selhání toorecover zařízení z této situaci. |Ano |Ne |

## <a name="physical-device-updates-in-hello-january-release"></a>Aktualizace fyzického zařízení v lednu verzi hello
Tato aktualizace neobsahuje žádné jiné zařízení StorSimple změny toohello.

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-hello-january-release"></a>Serial-attached SCSI (SAS) řadiče a aktualizace firmwaru v lednu verzi hello
Tato verze neobsahuje žádné aktualizace toohello serial-attached SCSI (SAS) řadiče nebo hello firmware. aktualizace ovladačů Hello byl v hello říjnu verze 2014. 

## <a name="virtual-device-updates-in-hello-january-release"></a>Aktualizace virtuální zařízení v lednu verzi hello
Tato verze obsahuje aktualizovanou bitovou kopii pro virtuální zařízení hello. Všechny virtuální zařízení hello vytvořený po 20 leden 2015 se zobrazí jako 6.3.9600.17361 hello verze softwaru.

