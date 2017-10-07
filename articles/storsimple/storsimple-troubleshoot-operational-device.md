---
title: "aaaTroubleshoot nasazené zařízení StorSimple | Microsoft Docs"
description: "Popisuje, jak toodiagnose a opravte chyby, ke kterým došlo v zařízení StorSimple, který je aktuálně nasazená a funkční."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ea5d89ae-e379-423f-b68b-53785941d9d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/16/2016
ms.author: v-sharos
ms.openlocfilehash: b48433055e05e3fb27575b88dca9f6b23c649ca2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-an-operational-storsimple-device"></a>Řešení potíží provozní zařízení StorSimple
## <a name="overview"></a>Přehled
Tento článek obsahuje pokyny k odstraňování problémů užitečná pro řešení problémů s konfigurací, můžete setkat po zařízení StorSimple je nasazen a provozní. Popisuje běžné problémy, možné příčiny a doporučené kroky toohelp vyřešit problémy, které se můžete setkat při spuštění Microsoft Azure StorSimple. Tyto informace platí fyzického zařízení tooboth hello StorSimple místní a virtuální zařízení StorSimple hello.

Na konci hello tohoto článku, které můžete najít seznam kódy chyb, které se můžou vyskytnout během operace Microsoft Azure StorSimple a také kroky můžete využít tooresolve hello chyby. 

## <a name="setup-wizard-process-for-operational-devices"></a>Průvodce instalací pro provozní zařízení
Pomocí Průvodce instalací hello ([Invoke-HcsSetupWizard][1]) toocheck hello konfigurace zařízení a v případě potřeby proveďte opravné akce.

Když spustíte Průvodce instalací hello na zařízení dříve nakonfigurované a funkční, tok procesu hello se liší. Můžete změnit pouze hello následující položky:

* IP adresa, maska podsítě a bránu
* Primární server DNS.
* Primární server NTP
* Konfigurace volitelné webového proxy serveru

Průvodce instalací Hello neprovede hello operace související toopassword kolekce a registraci zařízení.

## <a name="errors-that-occur-during-subsequent-runs-of-hello-setup-wizard"></a>Chyb vzniklých při dalším spuštění Průvodce instalací hello
Hello následující tabulka popisuje hello chyby, které se mohou vyskytnout při spuštění Průvodce instalací hello na provozní zařízení, možné příčiny chyb hello a doporučené akce tooresolve je. 

| Ne. | Chybová zpráva nebo podmínku | Možné příčiny | Doporučená akce |
|:--- |:--- |:--- |:--- |
| 1 |Chyba 350032: Toto zařízení již bylo deaktivováno. |Tato chyba se zobrazí, pokud spustíte Průvodce instalací hello na zařízení, které je deaktivována. |[Kontaktujte Microsoft Support](storsimple-contact-microsoft-support.md) pro další kroky. Deaktivované zařízení nelze vložit do služby. Předtím, než je možné hello zařízení znovu aktivovat, může být vyžadováno obnovení továrního nastavení. |
| 2 |Invoke-HcsSetupWizard: ERROR_INVALID_FUNCTION (výjimky z HRESULT: 0x80070001) |aktualizace serveru DNS Hello selhává. Nastavení DNS jsou globální nastavení a jsou použity napříč všech síťových rozhraní hello povolena. |Povolte rozhraní hello a znovu použít nastavení DNS hello. To může narušit hello sítě pro ostatní povoleno rozhraní, protože tato nastavení jsou globální. |
| 3 |Hello zařízení se zobrazí na portálu služby StorSimple Manager hello toobe online, ale při spusťte toocomplete hello minimální instalaci a konfiguraci hello uložit, hello operace selže. |Během počáteční instalace nebyla nakonfigurována hello webový proxy server, i když došlo k skutečné proxy serveru na místě. |Použití hello [rutiny Test-HcsmConnection] [ 2] toolocate hello chyby. [Kontaktujte Microsoft Support](storsimple-contact-microsoft-support.md) Pokud problém nelze toocorrect hello. |
| 4 |Invoke-HcsSetupWizard: Hodnota nespadá do rozsahu hello očekává. |Maska podsítě nesprávný vytvoří této chybě. Možné příčiny jsou: <ul><li> Maska podsítě Hello je chybí nebo je prázdný.</li><li>Hello formátu předpony protokolu Ipv6 je nesprávný.</li><li>rozhraní Hello je povolenou podporu cloudu, ale brána hello je chybí nebo není správný.</li></ul>Všimněte si, že rozhraní DATA 0 má automaticky povolenou podporu cloudu pokud nakonfigurovaný pomocí Průvodce instalací hello. |toodetermine hello problém, použijte podsíť 0.0.0.0 nebo 256.256.256.256 a podívejte se na výstup hello. Zadejte správné hodnoty pro masku podsítě hello, bránu a předponu Ipv6, podle potřeby. |

## <a name="error-codes"></a>Kódy chyb
Chyby jsou uvedeny v pořadí.

| Číslo chyby | Popis nebo text chyby | Akce doporučené uživatele |
|:--- |:--- |:--- |
| 10502 |Při přístupu k účtu úložiště došlo k chybě. |Počkejte několik minut a potom akci opakujte. Pokud hello chyba potrvá, požádejte prosím podporu Microsoftu pro další kroky. |
| 40017 |operace zálohování Hello má selhala, protože nebyl nalezen svazek, zadaný v hello zásady zálohování na hello zařízení. |Opakujte operaci zálohování hello, pokud hello chyba přetrvává, obraťte se na Microsoft Support. pro další kroky. |
| 40018 |operace zálohování Hello má selhala, protože na zařízení hello nebyl nalezen žádný hello svazků určená v zásadách zálohování hello. |Opakujte operaci zálohování hello, pokud hello chyba přetrvává, obraťte se na Microsoft Support. pro další kroky. |
| 390061 |Hello systém je zaneprázdněn nebo není k dispozici. |Počkejte několik minut a potom akci opakujte. Pokud hello chyba potrvá, požádejte prosím podporu Microsoftu pro další kroky. |
| 390143 |Došlo k chybě s kódem chyby 390143. (Neznámá chyba.) |Pokud hello chyba přetrvává, kontaktujte prosím Microsoft Support pro další kroky. |

## <a name="next-steps"></a>Další kroky
Pokud jste problém hello nelze tooresolve [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) o pomoc. 

[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx
