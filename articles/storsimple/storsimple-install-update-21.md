---
title: "Nainstalujte aktualizace 2.2 na zařízení StorSimple | Microsoft Docs"
description: "Popisuje postup instalace zařízení StorSimple 8000 řady aktualizace 2.2 na vašem zařízení řady StorSimple 8000."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 047c7a4b-73d0-45ea-8d51-c54d71871392
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/02/2016
ms.author: alkohli
ms.openlocfilehash: c60b4de88d60f0373d69fe2f3cee5ccf888b8d8c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-update-22-on-your-storsimple-device"></a>Nainstalujte aktualizace 2.2 na zařízení StorSimple
## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak nainstalovat aktualizace 2.2 na zařízení StorSimple se starší verzí softwaru prostřednictvím portálu Azure classic a metodou opravu hotfix. Metoda opravy hotfix se používá při brána je nakonfigurovaná na síťovém rozhraní než DATA 0 zařízení StorSimple a pokoušíte se z verze 1 před aktualizací softwaru aktualizovat.

Aktualizace 2.2 zahrnuje software zařízení, rozhraní WMI a aktualizace iSCSI. Pokud aktualizace z verze 2.1, pouze aktualizace softwaru zařízení budou muset použít. Pokud aktualizace z verze před aktualizací 2, bude také se budete muset použít pro ovladače LSI, Spaceport, Storport a aktualizace firmwaru disku. Zařízení software, rozhraní WMI, iSCSI, LSI ovladače, Spaceport a Storport opravy se omezovaly aktualizace a dají se použít prostřednictvím portálu Azure classic. Aktualizace firmwaru disku rušivý aktualizace a lze použít pouze prostřednictvím rozhraní Windows PowerShell zařízení. 

> [!IMPORTANT]
> * Sadu ruční a Automatická předběžné kontroly se provádějí před instalací, který měl zjistit stav zařízení z hlediska hardwaru stavu a připojení k síti. Tyto předběžné kontroly se provádí pouze v případě, že aktualizace použít z portálu Azure classic.
> * Doporučujeme instalovat aktualizace softwaru a ovladačů prostřednictvím portálu Azure classic. Má jenom přejděte na rozhraní prostředí Windows PowerShell na zařízení (instalovat aktualizace) Pokud selže kontrola před aktualizací brány na portálu. V závislosti na verzi, který chcete aktualizovat z se aktualizace může trvat hodiny 1.5 – 2.5 k instalaci. Aktualizace režimu údržby musí být nainstalován prostřednictvím rozhraní Windows PowerShell zařízení. Jako rušivý aktualizace jsou aktualizace režimu údržby, tyto povede k výpadkům pro vaše zařízení.
> * Pokud běží volitelné Snapshot Manager zařízení StorSimple, zajistěte, aby upgradu vaší verzí Snapshot Manager na aktualizace 2.2 před aktualizací zařízení.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-22-via-the-azure-classic-portal"></a>Nainstalujte aktualizace 2.2 prostřednictvím portálu Azure classic
Proveďte následující kroky k aktualizaci zařízení [aktualizovat 2.2](storsimple-update21-release-notes.md).

> [!NOTE]
> Chcete-li použít Update 2 nebo novější (včetně Update 2.1), Microsoft bude moci vyžádat jiné diagnostické informace ze zařízení. Výsledkem je když náš tým operations identifikuje zařízení, která došlo k potížím, jsme jsou lépe vybaveny shromažďovat informace ze zařízení a diagnostikovat problémy. Přijetím Update 2 nebo novější, můžete nám umožňují poskytovat tento proaktivní podporu.
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. Ověřte, zda je spuštěna vaše zařízení **StorSimple 8000 řady aktualizace 2.2 (6.3.9600.17708)**. **Datum poslední aktualizace** také by měl být upraven. 
   
   Při aktualizaci z verze před Update 2, zobrazí se také, že jsou aktualizace režim údržby k dispozici (Tato zpráva může nadále zobrazovat až 24 hodin po instalaci aktualizace).
   
   Aktualizace režimu údržby jsou rušivý aktualizace, které vést k výpadkům zařízení a dají se použít jenom přes rozhraní Windows PowerShell vašeho zařízení. V některých případech při spuštění aktualizace 1.2 firmware disku mohou být již aktuální, v takovém případě nemusíte instalovat všechny aktualizace režimu údržby.
   
   Při aktualizaci z Update 2, zařízení by teď měly být aktuální. Můžete přeskočit zbývající kroky.
2. Stáhnout aktualizace režimu údržby pomocí kroků uvedených v [ke stažení opravy hotfix](#to-download-hotfixes) k hledání a stahování KB3121899, který nainstaluje aktualizace firmwaru disku (s jinými aktualizacemi musí už být nainstalovaný nyní).
3. Postupujte podle kroků uvedených v [instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes) do režimu údržby instalace aktualizací. 

## <a name="install-update-22-as-a-hotfix"></a>Nainstalujte aktualizace 2.2 jako oprava hotfix
Tento postup použijte, pokud selže kontrola brány při pokusu o instalaci aktualizací prostřednictvím portálu Azure classic. Kontrola selže, jak máte bránu přiřazené 0 síťové rozhraní bez dat a vaše zařízení používá verzi softwaru před Update 1.

Verze softwaru, které lze upgradovat pomocí metody opravy hotfix jsou:

* Aktualizovat 0.1, 0.2 a 0.3
* Aktualizací 1, 1.1 a 1.2
* Aktualizace 2, 2.1 

> [!IMPORTANT]
> * Pokud zařízení používá verzi (GA) verze, kontaktujte prosím [Microsoft Support](storsimple-contact-microsoft-support.md) a pomůže vám při aktualizaci.
> 
> 

Metoda opravy hotfix zahrnuje následující tři kroky:

* Stažení opravy hotfix z katalogu služby Microsoft Update.
* Instalace a ověřte regulární režimu opravy hotfix.
* Instalace a ověřte opravu hotfix režim údržby (jenom při aktualizaci z před aktualizací 2 softwaru).

#### <a name="download-updates-for-a-device-running-update-21-software"></a>Stažení aktualizací pro zařízení se softwarem Update 2.1
**Pokud vaše zařízení používá aktualizovat 2.1**, je nutné stáhnout pouze aktualizace softwaru zařízení KB3179904. Instalovat pouze binární soubor, kterými 'všechna hcsmdssoftwareudpate'. Neinstalujte Cis a kterými aktualizace agenta MDS `all-cismdsagentupdatebundle`. Uděláte to tak bude dojít k chybě. Toto je omezovaly aktualizaci, vstupně-výstupní operace nesmí být přerušeny a zařízení nebude mít žádné výpadky.

#### <a name="download-updates-for-a-device-running-update-2-software"></a>Stažení aktualizací pro zařízení se softwarem Update 2
**Pokud vaše zařízení se softwarem Update 2**, musíte stáhnout a nainstalovat následující opravy hotfix v předepsaných pořadí:

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace |
| --- | --- | --- | --- | --- |
| 1. |KB3179904 |Aktualizace softwaru &#42; |Regulární <br></br>Bez přerušení |~ 45 minut |
| 2. |KB3146621 |balíček iSCSI |Regulární <br></br>Bez přerušení |~ 20 minut |
| 3. |KB3103616 |Balíček služby WMI |Regulární <br></br>Bez přerušení |~ 12 minut |

 &#42;  *Všimněte si, že se skládá ze dvou binární soubory aktualizace softwaru: aktualizace softwaru zařízení uvedena `all-hcsmdssoftwareupdate` a agent služby Mds a položek konfigurace, kterými `all-cismdsagentupdatebundle`. Aktualizace softwaru zařízení musí být nainstalován před Mds a položek konfigurace agenta. Také je nutné restartovat řadič prostřednictvím služby active `Restart-HcsController` aktualizovat rutiny po instalaci agenta položek konfigurace a služby MDS (a před použitím zbývající aktualizace).* 

#### <a name="download-updates-for-a-device-running-pre-update-2-software"></a>Stažení aktualizací pro zařízení se systémem před aktualizací 2 softwaru
**Pokud vaše zařízení používá verze 0,2, 0,3, 1.0 a 1.1**, musíte stáhnout a instalovat LSI ovladače a firmware aktualizaci kromě softwaru, iSCSI a WMI aktualizace. Tato aktualizace je již nainstalována, pokud používáte aktualizace 1.2 nebo 2. 

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace |
| --- | --- | --- | --- | --- |
| 4. |KB3121900 |LSI ovladače a firmware |Regulární <br></br>Bez přerušení |~ 20 minut |

<br></br>
**Pokud vaše zařízení používá verzi 0,2, 0,3, 1.0, 1.1 a 1.2**, musíte stáhnout a nainstalovat Spaceport a opravte Storport. Tyto jsou již nainstalovány, pokud používáte Update 2.

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace |
| --- | --- | --- | --- | --- |
| 5. |KB3090322 |Oprava spaceport </br> Windows Server 2012 R2 |Regulární <br></br>Bez přerušení |~ 20 minut |
| 6. |KB3080728 |Oprava Storport </br> Windows Server 2012 R2 |Regulární <br></br>Bez přerušení |~ 20 minut |

<br></br>
Můžete také nainstalovat aktualizace firmwaru disku. Můžete ověřit, zda je nutné aktualizace firmwaru disku spuštěním `Get-HcsFirmwareVersion` rutiny. Pokud používáte tyto verze firmwaru: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, pak nemusíte tyto aktualizace nainstalujete.

| Pořadí | kB | Popis | Typ aktualizace | Čas instalace |
| --- | --- | --- | --- | --- |
| 7. |KB3121899 |Firmware disku |Údržby <br></br>Rušivý |~ 30 minut |

<br></br>

> [!IMPORTANT]
> * Tento postup je nutné provést pouze jednou pro použití aktualizace 2.2. Portál Azure classic můžete použít následující aktualizace.
> * Pokud aktualizaci z Update 2, je čas celkový instalace blízké 1,5 hodiny.
> * Před použitím tohoto postupu k použití aktualizace, ujistěte se, že jak řadiče zařízení jsou online a jsou hardwarové součásti v pořádku.
> 
> 

Proveďte následující kroky ke stažení a instalaci opravy hotfix.

[!INCLUDE [storsimple-install-update21-hotfix](../../includes/storsimple-install-update21-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Další kroky
Další informace o [verzi Update 2.1](storsimple-update21-release-notes.md).

