---
title: "aaaInstall aktualizace 4 na zařízení řady StorSimple 8000 | Microsoft Docs"
description: "Vysvětluje, jak tooinstall StorSimple 8000 řady aktualizace 4 na vašem zařízení řady StorSimple 8000."
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
ms.date: 08/02/2017
ms.author: alkohli
ms.openlocfilehash: 3507edbde5e6e43b6c450bfea19494d47b5a5ae7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-4-on-your-storsimple-device"></a>Nainstalujte aktualizace 4 na zařízení StorSimple

## <a name="overview"></a>Přehled

Tento kurz popisuje, jak hello tooinstall aktualizace 4 na zařízení StorSimple se starší verzí softwaru prostřednictvím portálu Azure a pomocí metody hello opravu hotfix. Hello opravu hotfix metoda se používá, když brána je nakonfigurovaná na síťovém rozhraní než DATA 0 hello zařízení StorSimple a pokoušíte tooupdate z verze 1 před aktualizací softwaru.

Aktualizace 4 zahrnuje software zařízení, Seznam USM firmware, aktualizacemi zabezpečení operačního systému LSI pro ovladače a firmware, Storport a Spaceport a hostitel jiné aktualizace operačního systému.  software zařízení Hello, Seznam USM firmwaru, Spaceport, Storport a dalších aktualizací operačního systému jsou omezovaly aktualizace. Hello omezovaly nebo pravidelné aktualizace lze použít prostřednictvím hello portál Azure nebo prostřednictvím metody hello opravu hotfix. aktualizace firmwaru disku Hello rušivý aktualizace a lze použít pouze prostřednictvím metody opravu hotfix hello pomocí rozhraní Windows PowerShell hello hello zařízení.

> [!IMPORTANT]
> * Sadu ruční a Automatická předběžné kontroly se provádějí předchozí toohello instalace toodetermine hello zařízení stav z hlediska hardwaru stavu a připojení k síti. Tyto předběžné kontroly se provádí pouze v případě, že použijete hello aktualizace z hello portálu Azure.
> * Doporučujeme nainstalovat hello softwaru a další pravidelné aktualizace prostřednictvím hello portálu Azure. Rozhraní Windows PowerShell toohello hello zařízení (aktualizace tooinstall) by měl jdou pouze pokud selže kontrola před aktualizací brány hello hello portálu. V závislosti na hello verze, ze kterého je aktualizován, hello aktualizací může trvat 4 hodiny (nebo vyšší) tooinstall. aktualizace režimu údržby Hello musí být nainstalován také prostřednictvím rozhraní Windows PowerShell hello hello zařízení. Jako rušivý aktualizace jsou aktualizace režimu údržby, tyto povede k výpadkům pro vaše zařízení.
> * Pokud spuštěná hello volitelné Snapshot Manager zařízení StorSimple, ujistěte se, že jste upgradovali Snapshot Manager verze tooUpdate 4 předchozí tooupdating hello zařízení.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-4-via-hello-azure-portal"></a>Nainstalujte aktualizace 4 prostřednictvím hello portálu Azure
Proveďte následující kroky tooupdate hello zařízení příliš[aktualizace 4](storsimple-update4-release-notes.md).

> [!NOTE]
> Microsoft vrátí jiné diagnostické informace z hello zařízení. Výsledkem je když náš tým operations identifikuje zařízení, která došlo k potížím, jsme jsou lepší vybavené toocollect informace z hello zařízení a diagnostikovat problémy. 

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update4-via-portal.md)]

Ověřte, zda je spuštěna vaše zařízení **StorSimple 8000 řady aktualizace 4 (6.3.9600.17820)**. Hello **datum poslední aktualizace** také by měl být upraven.

* Nyní uvidíte, že jsou k dispozici aktualizace režimu údržby hello (Tato zpráva může pokračovat toobe zobrazený pro až too24 hello hodin po instalaci aktualizace). Aktualizace režimu údržby jsou rušivý aktualizace, které vést k výpadkům zařízení a dají se použít jenom přes rozhraní Windows PowerShell hello vašeho zařízení.

* Stáhnout aktualizace režimu údržby hello pomocí kroků uvedených v tomto seznamu hello [opravy hotfix toodownload](#to-download-hotfixes) toosearch pro a stáhnout KB4011837, který se nainstaluje aktualizace firmwaru disku (hello jiné aktualizace by měla již nainstalovat nyní). Postupujte podle kroků uvedených v tomto seznamu hello [instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello údržby režim aktualizace.

## <a name="install-update-4-as-a-hotfix"></a>Nainstalujte aktualizace 4 jako oprava hotfix
Hello doporučená metoda tooinstall, aktualizace 4 je prostřednictvím hello portálu Azure.

Tento postup použijte, pokud selže kontrola brány hello při pokusu o tooinstall hello aktualizací prostřednictvím hello portálu Azure. Kontrola Hello selže, jako byla přiřazena tooa jiný-síťového rozhraní DATA 0 a vaše zařízení používá předchozí tooUpdate software verze 1.

Hello verze softwaru, které lze upgradovat pomocí metody hello opravy hotfix jsou:

* Aktualizovat 0.1, 0.2 a 0.3
* Aktualizací 1, 1.1 a 1.2
* Aktualizace 2, 2.1, 2.2
* Aktualizace 3, verze 3.1


Metoda opravy hotfix Hello zahrnuje hello následující tři kroky:

1. Stažení opravy hotfix hello z hello katalogu služby Microsoft Update.
2. Instalace a ověřte hello regulární režimu opravy hotfix.
3. Instalace a ověřte opravu hotfix režimu údržby hello.

#### <a name="download-updates-for-your-device"></a>Stažení aktualizací pro zařízení

Musíte stáhnout a nainstalovat následující hello oprav hotfix v určeném hello pořadí a hello navrhované složky:

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace |Instalace ve složce|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4011839 |Aktualizace softwaru |Regulární <br></br>Bez přerušení |~ 25 minut |FirstOrderUpdate|
| 2A. |KB4011841 <br> KB4011842 |LSI ovladače a firmware aktualizace <br> Aktualizace firmwaru Seznam USM (verze 3.38) |Regulární <br></br>Bez přerušení |~ 3 hodiny <br> (včetně 2A. + 2B. + 2 C.)|SecondOrderUpdate|
| 2B. |KB3139398 KB3108381 <br> KB3205400 KB3142030 <br> KB3197873 KB3197873 <br> KB3192392 KB3153704 <br> KB3174644 KB3139914  |Balíček aktualizace zabezpečení operačního systému <br> Stáhněte si Windows serveru 2012 R2 |Regulární <br></br>Bez přerušení |- |SecondOrderUpdate|
| 2C. |KB3210083 KB3103616 <br> KB3146621 KB3121261 <br> KB3123538 |Balíček aktualizace operačního systému <br> Stáhněte si Windows serveru 2012 R2 |Regulární <br></br>Bez přerušení |- |SecondOrderUpdate|

Také můžete potřebovat při aktualizacích firmwaru tooinstall disku nad všechny aktualizace hello ukazuje hello předchozích tabulek. Můžete ověřit, zda potřebovat hello aktualizace firmwaru disku spuštěním hello `Get-HcsFirmwareVersion` rutiny. Pokud používáte tyto verze firmwaru: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N002`, `0106`, pak nepotřebujete tooinstall tyto aktualizace.

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace | Instalace ve složce|
| --- | --- | --- | --- | --- | --- |
| 3. |KB3121899 |Firmware disku |Údržby <br></br>Rušivý |~ 30 minut | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Tento postup musí toobe provést pouze jednou tooapply aktualizací 4. Můžete použít hello Azure portálu tooapply následné aktualizace.
> * Pokud aktualizace Update 3 nebo 3.1, je čas celkový instalace hello zavřít too4 hodin.
> * Před použitím tohoto postupu tooapply hello aktualizace, ujistěte se, že oba řadiče zařízení hello jsou online a všechny hardwarové součásti hello jsou v pořádku.

Proveďte následující kroky toodownload hello a instalaci opravy hotfix hello.

[!INCLUDE [storsimple-install-update4-hotfix](../../includes/storsimple-install-update4-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Další kroky
Další informace o hello [verze aktualizací 4](storsimple-update4-release-notes.md).

