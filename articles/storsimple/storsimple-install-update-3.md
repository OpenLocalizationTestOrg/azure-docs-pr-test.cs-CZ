---
title: "aaaInstall Update 3 zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak tooinstall StorSimple 8000 řady Update 3 na vašem zařízení řady StorSimple 8000."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: c6c4634d-4f3a-4bc4-b307-a22bf18664e1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a156b8919639f1c7afb0fdef3d882d40d48f1c48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-3-on-your-storsimple-8000-series-device"></a>Nainstalujte na zařízení řady StorSimple 8000 Update 3

## <a name="overview"></a>Přehled

Tento kurz popisuje, jak hello tooinstall Update 3 na zařízení StorSimple se starší verzí softwaru prostřednictvím portálu Azure classic a pomocí metody hello opravu hotfix. Hello opravu hotfix metoda se používá, když brána je nakonfigurovaná na síťovém rozhraní než DATA 0 hello zařízení StorSimple a pokoušíte tooupdate z verze 1 před aktualizací softwaru.

Aktualizace 3 zahrnuje zařízení software, LSI ovladače a firmware, Storport a Spaceport aktualizace. Pokud aktualizace Update 2 nebo dřívější verzi, bude také být požadované tooapply iSCSI, rozhraní WMI a v některých případech je na disku při aktualizacích firmwaru. Hello software zařízení, rozhraní WMI, iSCSI, ovladače LSI, Spaceport a Storport opravy se omezovaly aktualizace a dají se použít prostřednictvím hello portál Azure classic. aktualizace firmwaru disku Hello rušivý aktualizace a lze použít pouze prostřednictvím rozhraní Windows PowerShell hello hello zařízení. 

> [!IMPORTANT]
> * Sadu ruční a Automatická předběžné kontroly se provádějí předchozí toohello instalace toodetermine hello zařízení stav z hlediska hardwaru stavu a připojení k síti. Tyto předběžné kontroly se provádí pouze v případě, že použijete hello aktualizace z hello portál Azure classic.
> * Doporučujeme, abyste instalaci hello softwaru a aktualizací ovladače prostřednictvím hello portál Azure classic. Rozhraní Windows PowerShell toohello hello zařízení (aktualizace tooinstall) by měl jdou pouze pokud selže kontrola před aktualizací brány hello hello portálu. V závislosti na verzi hello, který chcete aktualizovat z se hello aktualizací může trvat hodiny 1.5 – 2.5 tooinstall. aktualizace režimu údržby Hello musí být nainstalován prostřednictvím rozhraní Windows PowerShell hello hello zařízení. Jako rušivý aktualizace jsou aktualizace režimu údržby, tyto povede k výpadkům pro vaše zařízení.
> * Pokud spuštěná hello volitelné Snapshot Manager zařízení StorSimple, ujistěte se, že jste upgradovali Snapshot Manager verze tooUpdate 2 předchozí tooupdating hello zařízení.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-3-via-hello-azure-classic-portal"></a>Nainstalujte aktualizace 3 prostřednictvím hello portál Azure classic
Proveďte následující kroky tooupdate hello zařízení příliš[Update 3](storsimple-update3-release-notes.md).

> [!NOTE]
> Chcete-li použít Update 2 nebo novější (včetně Update 2.1), Microsoft bude možné toopull jiné diagnostické informace z hello zařízení. Výsledkem je když náš tým operations identifikuje zařízení, která došlo k potížím, jsme jsou lepší vybavené toocollect informace z hello zařízení a diagnostikovat problémy. Přijetím Update 2 nebo novější, můžete nám umožňují tooprovide této proaktivní podpory.
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

Ověřte, zda je spuštěna vaše zařízení **StorSimple 8000 řady Update 3 (6.3.9600.17759)**. Hello **datum poslední aktualizace** také by měl být upraven. 
   - Při aktualizaci z předchozí tooUpdate verze 2, zobrazí se také, že jsou k dispozici aktualizace režimu údržby hello (Tato zpráva může pokračovat toobe zobrazený pro až too24 hello hodin po instalaci aktualizace).
     Aktualizace režimu údržby jsou rušivý aktualizace, které vést k výpadkům zařízení a dají se použít jenom přes rozhraní Windows PowerShell hello vašeho zařízení. V některých případech při spuštění aktualizace 1.2, možná již je aktuální firmware disku, v takovém případě nepotřebujete tooinstall aktualizuje všechny režimu údržby.
   - Pokud jste aktualizaci z Update 2 nebo novější, zařízení by teď měly být aktuální. Další krok text hello, můžete přeskočit.

Stáhnout aktualizace režimu údržby hello pomocí kroků uvedených v tomto seznamu hello [opravy hotfix toodownload](#to-download-hotfixes) toosearch pro a stáhnout KB3121899, který se nainstaluje aktualizace firmwaru disku (hello jiné aktualizace by měla již nainstalovat nyní). Postupujte podle kroků uvedených v tomto seznamu hello [instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello údržby režim aktualizace. 

## <a name="install-update-3-as-a-hotfix"></a>Instalaci aktualizace Update 3 jako oprava hotfix
Tento postup použijte, pokud selže kontrola brány hello při pokusu o tooinstall hello aktualizací prostřednictvím hello portál Azure classic. Kontrola Hello selže, jako byla přiřazena tooa jiný-síťového rozhraní DATA 0 a vaše zařízení používá předchozí tooUpdate software verze 1.

Hello verze softwaru, které lze upgradovat pomocí metody hello opravy hotfix jsou:

* Aktualizovat 0.1, 0.2 a 0.3
* Aktualizací 1, 1.1 a 1.2
* Aktualizace 2, 2.1, 2.2 

> [!IMPORTANT]
> * Pokud zařízení používá verzi (GA) verze, kontaktujte prosím [Microsoft Support](storsimple-contact-microsoft-support.md) tooassist s hello aktualizujete.
> 
> 

Metoda opravy hotfix Hello zahrnuje hello následující tři kroky:

1. Stažení opravy hotfix hello z hello katalogu služby Microsoft Update.
2. Instalace a ověřte hello regulární režimu opravy hotfix.
3. Instalace a ověřte opravu hotfix režimu údržby hello (jenom při aktualizaci z před aktualizací 2 softwaru).

#### <a name="download-updates-for-your-device"></a>Stažení aktualizací pro zařízení
**Pokud vaše zařízení se systémem Update 2.1 nebo 2.2**, musíte stáhnout a nainstalovat následující oprav hotfix v pořadí určeném hello hello:

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace |
| --- | --- | --- | --- | --- |
| 1. |KB3186843 |Aktualizace softwaru &#42; |Regulární <br></br>Bez přerušení |~ 45 minut |
| 2. |KB3186859 |LSI ovladače a firmware |Regulární <br></br>Bez přerušení |~ 20 minut |
| 3. |KB3121261 |Oprava Storport a Spaceport </br> Windows Server 2012 R2 |Regulární <br></br>Bez přerušení |~ 20 minut |

&#42;  *Všimněte si, že aktualizace softwaru hello se skládá ze dvou binární soubory: aktualizace softwaru zařízení uvedena `all-hcsmdssoftwareupdate` hello položek konfigurace a kterými Mds agenta `all-cismdsagentupdatebundle`. aktualizace softwaru hello zařízení musí být nainstalována před hello položek konfigurace a Mds Agent. Také je nutné restartovat řadič active hello prostřednictvím hello `Restart-HcsController` rutiny po použití hello položek konfigurace a aktualizace agenta služby Mds (a před použitím hello zbývající aktualizace).* 

**Pokud vaše zařízení používá Update 0.1, 0.2, 0.3, 1.0, 1.1, 1.2 nebo 2.0**, musíte stáhnout a nainstalovat hello následující opravy hotfix v přidání toohello softwaru, LSI ovladače a firmware aktualizace (uvedené v hello předcházející tabulce), v pořadí určeném hello:

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace |
| --- | --- | --- | --- | --- |
| 4. |KB3146621 |balíček iSCSI |Regulární <br></br>Bez přerušení |~ 20 minut |
| 5. |KB3103616 |Balíček služby WMI |Regulární <br></br>Bez přerušení |~ 12 minut |

<br></br>

**Pokud vaše zařízení používá verzi 0,2, 0,3, 1.0, 1.1 a 1.2**, můžete potřebovat při aktualizacích firmwaru tooinstall disku nad všechny aktualizace hello ukazuje hello předchozích tabulek. Můžete ověřit, zda potřebovat hello aktualizace firmwaru disku spuštěním hello `Get-HcsFirmwareVersion` rutiny. Pokud používáte tyto verze firmwaru: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, pak nepotřebujete tooinstall tyto aktualizace.

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace |
| --- | --- | --- | --- | --- |
| 6. |KB3121899 |Firmware disku |Údržby <br></br>Rušivý |~ 30 minut |

<br></br>

> [!IMPORTANT]
> * Tento postup musí toobe provést pouze jednou tooapply Update 3. Můžete použít hello Azure classic portálu tooapply následné aktualizace.
> * Pokud aktualizaci z 2.2 aktualizace, je čas celkový instalace hello zavřít too1.1 hodin.
> * Před použitím tohoto postupu tooapply hello aktualizace, ujistěte se, že oba řadiče zařízení hello jsou online a všechny hardwarové součásti hello jsou v pořádku.
> 
> 

Proveďte následující kroky toodownload hello a instalaci opravy hotfix hello.

[!INCLUDE [storsimple-install-update3-hotfix](../../includes/storsimple-install-update3-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Další kroky
Další informace o hello [verzi Update 3](storsimple-update3-release-notes.md).

