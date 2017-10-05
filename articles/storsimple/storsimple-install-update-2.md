---
title: "Nainstalujte na zařízení StorSimple Update 2 | Microsoft Docs"
description: "Popisuje postup instalace zařízení StorSimple 8000 řady Update 2 na zařízení řady StorSimple 8000."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 8c8981df-75d9-4d19-b137-d6c6ba39dcfb
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: e788439608b7122f2bca6b99b832baa5258e472d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-update-2-on-your-storsimple-device"></a>Nainstalujte na zařízení StorSimple Update 2
## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak nainstalovat aktualizace 2 na zařízení StorSimple se starší verzí softwaru prostřednictvím portálu Azure classic. Tento kurz také popisuje kroky potřebné k aktualizaci, když brána je nakonfigurovaná na síťovém rozhraní než DATA 0 zařízení StorSimple a se pokoušíte aktualizovat z verze 1 před aktualizací softwaru.

Aktualizace 2 zahrnuje zařízení aktualizací softwaru, aktualizací ovladače LSI a aktualizace firmwaru disku. Software zařízení a LSI aktualizace jsou omezovaly aktualizace a provádět prostřednictvím portálu Azure classic. Aktualizace firmwaru disku rušivý aktualizace a lze použít pouze prostřednictvím rozhraní Windows PowerShell zařízení.

> [!IMPORTANT]
> * Update 2 nemusí zobrazit okamžitě, protože provedeme postupné zavádění aktualizací. Kontrola aktualizací za pár dní znovu jako tato aktualizace brzy bude dostupná.
> * Sadu ruční a Automatická předběžné kontroly se provádějí před instalací, který měl zjistit stav zařízení z hlediska hardwaru stavu a připojení k síti. Tyto předběžné kontroly se provádí pouze v případě, že aktualizace použít z portálu Azure classic.
> * Doporučujeme instalovat aktualizace softwaru a ovladačů prostřednictvím portálu Azure classic. Má jenom přejděte na rozhraní prostředí Windows PowerShell na zařízení (instalovat aktualizace) Pokud selže kontrola před aktualizací brány na portálu. Aktualizace může trvat 4-7 hodin k instalaci (včetně aktualizací Windows). Aktualizace režimu údržby musí být nainstalován prostřednictvím rozhraní Windows PowerShell zařízení. Jako rušivý aktualizace jsou aktualizace režimu údržby, tyto povede k výpadkům pro vaše zařízení.
> * Pokud běží volitelné Snapshot Manager zařízení StorSimple, zajistěte, aby upgradu vaší verzí Snapshot Manager na Update 2 před aktualizací zařízení.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-2-via-the-azure-classic-portal"></a>Instalaci aktualizace 2 prostřednictvím portálu Azure classic
Proveďte následující kroky k aktualizaci zařízení [Update 2](storsimple-update2-release-notes.md).

> [!NOTE]
> Aktualizace 2 umožňuje Microsoftu jiné diagnostické informace z tohoto zařízení pro vyžádání obsahu. Výsledkem je když náš tým operations identifikuje zařízení, která došlo k potížím, jsme jsou lépe vybaveny shromažďovat informace ze zařízení a diagnostikovat problémy. Přijetím Update 2, můžete nám umožňují poskytovat tento proaktivní podporu.
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. Ověřte, zda je spuštěna vaše zařízení **StorSimple 8000 řady Update 2 (6.3.9600.17673)**. **Datum poslední aktualizace** také by měl být upraven. Dozvíte se taky, že jsou k dispozici aktualizace režim údržby (Tato zpráva může nadále zobrazovat až 24 hodin po instalaci aktualizace).
   
   Aktualizace režimu údržby jsou rušivý aktualizace, které vést k výpadkům zařízení a dají se použít jenom přes rozhraní Windows PowerShell vašeho zařízení. V některých případech při spuštění aktualizace 1.2 firmware disku mohou být již aktuální, v takovém případě nemusíte instalovat všechny aktualizace režimu údržby.
2. Stáhnout aktualizace režimu údržby pomocí kroků uvedených v [ke stažení opravy hotfix](#to-download-hotfixes) k hledání a stahování KB3121899, který nainstaluje aktualizace firmwaru disku (s jinými aktualizacemi musí už být nainstalovaný nyní).
3. Postupujte podle kroků uvedených v [instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes) do režimu údržby instalace aktualizací.

## <a name="install-update-2-as-a-hotfix"></a>Instalaci aktualizace 2 jako oprava hotfix
Tento postup použijte, pokud selže kontrola brány při pokusu o instalaci aktualizací prostřednictvím portálu Azure classic. Kontrola selže, jak máte bránu přiřazené 0 síťové rozhraní bez dat a vaše zařízení používá verzi softwaru před Update 1.

Verze softwaru, které lze upgradovat pomocí metody opravy hotfix jsou Update 0.1, 0.2, aktualizace a Update 0.3, Update 1, aktualizace 1.1 a 1.2 aktualizace. Metoda opravy hotfix zahrnuje následující tři kroky:

* Stažení opravy hotfix z katalogu služby Microsoft Update.
* Instalace a ověřte regulární režimu opravy hotfix.
* Instalace a ověřte opravu hotfix režimu údržby.

K instalaci aktualizace 2 jako oprava hotfix, musíte stáhnout a nainstalovat následující opravy hotfix:

| Pořadí | kB | Popis | Typ aktualizace |
| --- | --- | --- | --- |
| 1 |KB3121901 |Aktualizace softwaru |Regulární |
| 2 |KB3121900 |LSI ovladačů |Regulární |
| 3 |KB3080728 |Oprava Storport </br> Windows Server 2012 R2 |Regulární |
| 4 |KB3090322 |Oprava spaceport </br> Windows Server 2012 R2 |Regulární |
| 5 |KB3121899 |Firmware disku |Údržby |

> [!IMPORTANT]
> * Pokud zařízení používá verzi (GA) verze, kontaktujte prosím [Microsoft Support](storsimple-contact-microsoft-support.md) a pomůže vám při aktualizaci.
> * Tento postup je nutné provést pouze jednou použít Update 2. Portál Azure classic můžete použít následující aktualizace.
> * Každou instalaci oprav hotfix může trvat asi 20 minut. Čas celkový instalace je blízko 2 hodiny.
> * Před použitím tohoto postupu použít aktualizaci, ujistěte se, že oba řadiče zařízení jsou online a jsou hardwarové součásti v pořádku.
> 
> 

Proveďte následující kroky k instalaci této aktualizace jako oprava hotfix.

[!INCLUDE [storsimple-install-update2-hotfix](../../includes/storsimple-install-update2-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Další kroky
Další informace o [verzi Update 2](storsimple-update2-release-notes.md).

