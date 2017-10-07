---
title: "Poznámky k verzi aaaStorSimple 8000 vydání verze | Microsoft Docs"
description: "Popisuje nové funkce hello, otevřete problémy a k dispozici řešení pro hello červenec 2014 Microsoft Azure StorSimple verze."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 12f1796e-37c3-42b4-b997-a84fc1950c20
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/18/2016
ms.author: v-sharos
ms.openlocfilehash: 74863a3e2811dc7be5e6f482a5be4bbc37e3cd71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-release-version-release-notes---july-2014"></a>Poznámky k verzi zařízení StorSimple 8000 řady vydání verze – červenec 2014
## <a name="overview"></a>Přehled
Poznámky k verzi Hello postupujte podle identifikovat hello kritické otevřené problémy hello StorSimple řady 8000 červenec 2014 obecné dostupnosti (GA) verzi Microsoft Azure StorSimple. Tato verze odpovídá verzi toosoftware 6.3.9600.17215.  

Pokud není uvedeno jinak, tyto poznámky k verzi platí tooall modely hello zařízení StorSimple. Poznámky k verzi Hello se průběžně aktualizuje; Při zjištění zásadních problémů, které vyžadují řešení, jsou přidány. Před nasazením řešení Microsoft Azure StorSimple, zvažte hello následující informace.  

## <a name="known-issues-in-this-release"></a>Známé problémy v této verzi
Hello následující tabulka obsahuje souhrn známých problémů v této verzi.  

| Ne. | Funkce | Problém | Komentáře nebo alternativní řešení | Platí toophysical zařízení | Platí toovirtual zařízení |
| --- | --- | --- | --- | --- | --- |
| 1 |Obnovení továrního nastavení |V některých případech při provádění obnovení továrního nastavení, hello zařízení StorSimple může být pomalé a zobrazit tato zpráva: **toofactory resetování probíhá (fáze 8)**. To se stane, když stisknutím CTRL + C, když probíhá hello rutiny. |Není stiskněte kombinaci kláves CTRL + C po inicializaci obnovení továrního nastavení. Pokud jste již v tomto stavu, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 2 |Disk kvora |Ve výjimečných případech odpojení hello většinu disků ve skříni EBOD hello zařízení 8600 jsou výsledkem žádný disk kvora, pak hello fondu úložiště bude offline. Zůstane offline, i když jsou znovu připojeny disky hello. |Budete potřebovat tooreboot hello zařízení. Pokud hello potíže potrvají, kontaktujte prosím Microsoft Support pro další kroky. |Ano |Ne |
| 3 |Selhání snímku cloudu |Ve výjimečných případech může cloudový snímek nezdaří s chybou hello **zálohování dosažen limit maximální**. K tomu dojde, pokud být delší než 255 online klony na hello stejného zařízení, z hello stejné původního svazku, který byl odstraněn. | |Ano |Ano |
| 4 |Nesprávný řadiče ID |Když se provádí nahrazení řadiče, může řadič 0 zobrazí jako řadič 1. Při nahrazení řadiče při načtení hello bitovou kopii z uzlu sdílené hello, hello řadiče ID můžete zobrazují původně jako ID řadiče sdílené hello. Ve výjimečných případech může být toto chování vidět také po restartu systému. |Není třeba žádné akce uživatele. Tato situace bude automaticky vyřešen po dokončení nahrazení řadiče hello. |Ano |Ne |
| 5 |Monitorování grafy zařízení |V hello služby StorSimple Manager monitorování grafy hello zařízení nefungují, pokud základní nebo v konfiguraci proxy serveru pro zařízení hello hello je povolené ověřování NTLM. |Úprava konfigurace webového proxy serveru pro hello zařízení zaregistrovali služby StorSimple Manager tak, že ověřování nastavíte tooNONE hello. toodo se hello hello spuštění prostředí Windows PowerShell pro rutinu StorSimple Set-HcsWebProxy. |Ano |Ano |
| 6 |Účty úložiště |Použití účtu úložiště hello úložiště služby toodelete hello se o nepodporovaný scénář. To povede tooa situace, ve kterém nelze načíst data uživatele. | |Ano |Ano |
| 7 |Navrácení služeb po obnovení |Navrácení služeb po obnovení do 24 hodin zotavení po havárii (DR) není podporována. | |Ano |Ne |
| 8 |Převzetí služeb při selhání zařízení |Více převzetí služeb kontejner svazků z hello stejný zdroj zařízení toodifferent cílové zařízení není podporován. Převzetí služeb při selhání jednom zařízení neaktivní toomultiple zařízení bude hello kontejnery svazků na hello nejprve převzít služby při selhání zařízení dojít ke ztrátě dat vlastnictví. Po takové převzetí služeb při selhání, budou tyto kontejnery svazků zobrazí nebo chovat jinak při zobrazení v hello portál Azure classic. | |Ano |Ne |
| 9 |Instalace |Během StorSimple adaptéru pro instalaci služby SharePoint je třeba tooprovide IP zařízení pro toofinish hello instalace úspěšně. | |Ano |Ne |
| 10 |Síťová rozhraní |Síťová rozhraní DATA 2 a DATA 3 měla vzájemně zaměněny v softwaru hello. |Pokud potřebujete tooconfigure tato rozhraní, obraťte se na Microsoft Support. |Ano |Ne |

