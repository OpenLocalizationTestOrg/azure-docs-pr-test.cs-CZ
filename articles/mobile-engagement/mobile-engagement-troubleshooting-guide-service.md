---
title: "aaaAzure Mobile Engagement Průvodce odstraňováním potíží – služby"
description: "Řešení potíží s příručky pro Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 8b4275da-c0b4-4690-824a-48e9d7a1fc6e
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: cf48db323a873ccef95946f7bb26e8d7473c002f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-service-issues"></a>Průvodci odstraňováním potíží problémů služby
Hello následují možných problémů se můžete setkat s jak spouští Azure Mobile Engagement.

## <a name="service-outages"></a>Výpadky
### <a name="issue"></a>Problém
* Problémy, které se zobrazují toobe způsobené Azure Mobile Engagement výpadky.

### <a name="causes"></a>Způsobí, že
* Problémy, které se zobrazují toobe způsobené Azure Mobile Engagement výpadky může být způsobeno několika různých problémy:
  * Izolované problémy, které původně zobrazit systémové tooall Azure Mobile Engagement
  * Známé problémy, kvůli výpadku serveru (ne vždy zobrazuje stav serveru):
  * Plánování zpoždění, cílení na chyby, oznámení "BADGE" update problémy, statistiky zastavit shromažďování nabízené přestane pracovat, rozhraní API zastavení práce, nové aplikace nebo uživatele nelze vytvořit, chyb DNS a vypršení časového limitu v hello uživatelského rozhraní, rozhraní API nebo aplikace na zařízení.
  * Cloud závislostí výpadků [stav služby Azure](http://status.azure.com/)
  * Nabízená oznámení služby (PNS) závislostí výpadků
  * Výpadky obchodu s aplikacemi

1) tootest toosee, pokud je systémová hello problém můžete otestovat hello stejné funkce z jiné

* Integrovaná aplikace Azure Mobile Engagement
* Testovací zařízení
* Zkušební verze operačního systému zařízení
* Kampaň
* Uživatelský účet správce
* Prohlížeč (IE, Firefox, Chrome, atd.)
* Počítač

2) tootest Pokud hello problém se týká pouze hello uživatelského rozhraní nebo hello rozhraní API:

* Otestujte hello stejné funkce z obou hello Azure Mobile Engagement uživatelského rozhraní a hello Azure Mobile Engagement rozhraní API.

3) tootest Pokud hello problém s vaší sítí mobilního telefonu:

* Test při připojené toohello Internetu přes Wi-Fi a při připojení prostřednictvím sítě 3G mobilního telefonu.
* Potvrďte, že brána firewall neblokuje hello Azure Mobile Engagement IP adresy nebo porty.

4) tootest Pokud hello problém s vaším zařízením:

* Otestujte, pokud vaše zařízení je možné tooconnect tooAzure Mobile Engagement s jinou integrované aplikace Azure Mobile Engagement.
* Test, který může generovat události, úlohy a dojde k chybě z váš telefon, který si můžete prohlédnout ve hello Azure Mobile Engagement uživatelského rozhraní. 
* Otestovat, pokud odesíláte nabízená oznámení z hello Azure Mobile Engagement uživatelského rozhraní tooyour zařízení podle jeho ID zařízení. 

5) tootest Pokud hello problém s vaší aplikací:

* Instalace a testování vaší aplikace z emulátoru místo z fyzického zařízení:

6) tootest Pokud hello problém s operačním systémem upgraduje tooend uživatele, zařízení, které vyžadují upgradu tooresolve SDK:

* Testování aplikace na různých zařízeních s různými verzemi nástroje hello operačního systému.
* Potvrďte, že používáte nejnovější verzi hello SDK hello.

## <a name="connectivity-and-incorrect-information-issues"></a>Připojení a nesprávné informace o problémech
### <a name="issue"></a>Problém
* Problémy protokolování do hello Azure Mobile Engagement uživatelského rozhraní.
* Chyby připojení s hello rozhraní API služby Azure Mobile Engagement je.
* Potíže s odesíláním značky informace o aplikaci prostřednictvím hello rozhraní API pro zařízení.
* Problémy stahování protokolů nebo exportovaná data z Azure Mobile Engagement.
* Nesprávné informace zobrazené v hello Azure Mobile Engagement uživatelského rozhraní.
* Nesprávné informace zobrazené v protokolech Azure Mobile Engagement.

### <a name="causes"></a>Způsobí, že
* Potvrďte, že váš uživatelský účet má dostatečná oprávnění tooperform hello úloh.
* Potvrďte, že tento problém hello není izolované tooone počítači nebo v místní síti.
* Potvrďte, že tento hello služby Azure Mobile Engagement žádné hlášené výpadky.
* Přesvědčte se, vaše aplikace informace o označení souborů podívejte se na všech těchto pravidel:
  * Použití pouze hello UTF8 znakovou sadu (znakovou sadu ANSI hello není podporována.).
  * Použít čárku "," jako hello oddělovací znak (služby požadavek toorequest toochange hello .csv oddělovací znak můžete otevřít z čárkou tooanother znak "," například středníkem ";").
  * Používejte všechny malá písmena pro logické hodnoty "true" a "false".
  * Použijte soubor, který je menší než hello maximální velikost souboru 35 MB.

