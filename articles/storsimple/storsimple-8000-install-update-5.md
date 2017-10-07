---
title: "aaaInstall aktualizací 5 na zařízení řady StorSimple 8000 | Microsoft Docs"
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
ms.date: 08/22/2017
ms.author: alkohli
ms.openlocfilehash: a832f9953e8e39408efeeed375e3afe8eee8d0e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-5-on-your-storsimple-device"></a>Instalace aktualizací 5 zařízení StorSimple

## <a name="overview"></a>Přehled

Tento kurz popisuje, jak hello tooinstall aktualizací 5 na zařízení StorSimple se starší verzí softwaru prostřednictvím portálu Azure a pomocí metody hello opravu hotfix. Hello opravu hotfix metoda se používá, když brána je nakonfigurovaná na síťovém rozhraní než DATA 0 hello zařízení StorSimple a pokoušíte tooupdate z verze 1 před aktualizací softwaru.

Aktualizace 5 zahrnuje zařízení software, Storport a Spaceport, aktualizacemi zabezpečení operačního systému a aktualizace operačního systému a aktualizace firmwaru disku.  software zařízení Hello, Spaceport, Storport, zabezpečení a jiné aktualizace operačního systému jsou omezovaly aktualizace. Hello omezovaly nebo pravidelné aktualizace lze použít prostřednictvím hello portál Azure nebo prostřednictvím metody hello opravu hotfix. aktualizace firmwaru disku Hello rušivý aktualizace a se použijí v případě, že hello zařízení je v režimu údržby prostřednictvím metody opravu hotfix hello pomocí rozhraní Windows PowerShell hello hello zařízení.

> [!IMPORTANT]
> * Sadu ruční a Automatická předběžné kontroly se provádějí předchozí toohello instalace toodetermine hello zařízení stav z hlediska hardwaru stavu a připojení k síti. Tyto předběžné kontroly se provádí pouze v případě, že použijete hello aktualizace z hello portálu Azure.
> * Doporučujeme nainstalovat hello softwaru a další pravidelné aktualizace prostřednictvím hello portálu Azure. Rozhraní Windows PowerShell toohello hello zařízení (aktualizace tooinstall) by měl jdou pouze pokud selže kontrola před aktualizací brány hello hello portálu. V závislosti na hello verze, ze kterého je aktualizován, hello aktualizací může trvat 4 hodiny (nebo vyšší) tooinstall. aktualizace režimu údržby Hello musí být nainstalován prostřednictvím rozhraní Windows PowerShell hello hello zařízení. Jako rušivý aktualizace jsou aktualizace režimu údržby, tyto vést k výpadkům pro vaše zařízení.
> * Pokud spuštěná hello volitelné Snapshot Manager zařízení StorSimple, ujistěte se, že jste upgradovali Snapshot Manager verze tooUpdate 5 předchozí tooupdating hello zařízení.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-5-via-hello-azure-portal"></a>Instalace aktualizací 5 prostřednictvím hello portálu Azure
Proveďte následující kroky tooupdate hello zařízení příliš[aktualizací 5](storsimple-update5-release-notes.md).

> [!NOTE]
> Microsoft vrátí jiné diagnostické informace z hello zařízení. Výsledkem je když náš tým operations identifikuje zařízení, která došlo k potížím, jsme jsou lepší vybavené toocollect informace z hello zařízení a diagnostikovat problémy.

[!INCLUDE [storsimple-8000-install-update4-via-portal](../../includes/storsimple-8000-install-update5-via-portal.md)]

Ověřte, zda je spuštěna vaše zařízení **StorSimple 8000 řady aktualizací 5 (6.3.9600.17845)**. Hello **datum poslední aktualizace** by měl být upraven.

* Nyní uvidíte, že jsou k dispozici aktualizace režimu údržby hello (Tato zpráva může pokračovat toobe zobrazený pro až too24 hello hodin po instalaci aktualizace). Aktualizace režimu údržby jsou rušivý aktualizace, které vést k výpadkům zařízení a dají se použít jenom přes rozhraní Windows PowerShell hello vašeho zařízení.

* Stáhnout aktualizace režimu údržby hello pomocí kroků uvedených v tomto seznamu hello [opravy hotfix toodownload](#to-download-hotfixes) toosearch pro a stáhnout KB4011837, který se nainstaluje aktualizace firmwaru disku (hello jiné aktualizace by měla již nainstalovat nyní). Postupujte podle kroků uvedených v tomto seznamu hello [instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello údržby režim aktualizace.

## <a name="install-update-5-as-a-hotfix"></a>Instalace aktualizací 5 jako oprava hotfix


Hello verze softwaru, které lze upgradovat pomocí metody hello opravy hotfix jsou:

* Aktualizovat 0.1, 0.2 a 0.3
* Aktualizací 1, 1.1 a 1.2
* Aktualizace 2, 2.1, 2.2
* Aktualizace 3, verze 3.1
* Aktualizace 4

> [!NOTE] 
> Hello doporučená metoda tooinstall, aktualizací 5 je prostřednictvím hello portálu Azure. Tento postup použijte, pokud selže kontrola brány hello při pokusu o tooinstall hello aktualizací prostřednictvím hello portálu Azure. Kontrola Hello selže, pokud máte bránu přiřazené tooa jiný-síťového rozhraní DATA 0 a vaše zařízení používá verzi softwaru starší než Update 1.

Metoda opravy hotfix Hello zahrnuje hello následující tři kroky:

1. Stažení opravy hotfix hello z hello katalogu služby Microsoft Update.
2. Instalace a ověřte hello regulární režimu opravy hotfix.
3. Instalace a ověřte opravu hotfix režimu údržby hello.

#### <a name="download-updates-for-your-device"></a>Stažení aktualizací pro zařízení

Musíte stáhnout a nainstalovat následující hello oprav hotfix v určeném hello pořadí a hello navrhované složky:

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace |Instalace ve složce|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4037264 |Aktualizace softwaru<br> Stáhněte si oba _HcsSfotwareUpdate.exe_ a _CisMSDAgent.exe_ |Regulární <br></br>Bez přerušení |~ 25 minut |FirstOrderUpdate|

Pokud aktualizace ze zařízení se systémem aktualizace 4, potřebujete jenom tooinstall hello OS kumulativní aktualizace jako druhý pořadí aktualizace.

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace |Instalace ve složce|
| --- | --- | --- | --- | --- | --- |
| 2A. |KB4025336 |Balíčku kumulativní aktualizace operačního systému <br> Stáhnout verzi Windows Server 2012 R2 |Regulární <br></br>Bez přerušení |- |SecondOrderUpdate|

Pokud instalaci ze zařízení se systémem Update 3 nebo starší, nainstalujte hello následující kromě toohello kumulativní aktualizace.

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace |Instalace ve složce|
| --- | --- | --- | --- | --- | --- |
| 2B. |KB4011841 <br> KB4011842 |LSI ovladače a firmware aktualizace <br> Aktualizace firmwaru Seznam USM (verze 3.38) |Regulární <br></br>Bez přerušení |~ 3 hodiny <br> (včetně 2A. + 2B. + 2 C.)|SecondOrderUpdate|
| 2C. |KB3139398 <br> KB3142030 <br> KB3108381 <br> KB3153704 <br> KB3174644 <br> KB3139914   |Balíček aktualizace zabezpečení operačního systému <br> Stáhnout verzi Windows Server 2012 R2 |Regulární <br></br>Bez přerušení |- |SecondOrderUpdate|
| 2D. |KB3146621 <br> KB3103616 <br> KB3121261 <br> KB3123538 |Balíček aktualizace operačního systému <br> Stáhnout verzi Windows Server 2012 R2 |Regulární <br></br>Bez přerušení |- |SecondOrderUpdate|


Také můžete potřebovat při aktualizacích firmwaru tooinstall disku nad všechny aktualizace hello ukazuje hello předchozích tabulek. Můžete ověřit, zda potřebovat hello aktualizace firmwaru disku spuštěním hello `Get-HcsFirmwareVersion` rutiny. Pokud používáte tyto verze firmwaru: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N003`, `0107`, pak nepotřebujete tooinstall tyto aktualizace.

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace | Instalace ve složce|
| --- | --- | --- | --- | --- | --- |
| 3. |KB4037263 |Firmware disku |Údržby <br></br>Rušivý |~ 30 minut | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Pokud aktualizaci z aktualizace 4, je čas celkový instalace hello zavřít too4 hodin.
> * Před použitím tohoto postupu tooapply hello aktualizace, ujistěte se, že oba řadiče zařízení hello jsou online a všechny hardwarové součásti hello jsou v pořádku.

Proveďte následující kroky toodownload hello a instalaci opravy hotfix hello.

[!INCLUDE [storsimple-install-update5-hotfix](../../includes/storsimple-install-update5-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Další kroky
Další informace o hello [verze aktualizací 5](storsimple-update5-release-notes.md).

