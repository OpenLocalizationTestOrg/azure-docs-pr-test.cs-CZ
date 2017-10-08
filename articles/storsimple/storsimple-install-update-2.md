---
title: "aaaInstall Update 2 v zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak tooinstall StorSimple 8000 řady Update 2 na zařízení řady StorSimple 8000."
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
ms.openlocfilehash: 33a0bea4358c944644563192f686af332d2ad7bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-2-on-your-storsimple-device"></a>Nainstalujte na zařízení StorSimple Update 2
## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak aktualizovat tooinstall 2 na zařízení StorSimple se starší verzí softwaru prostřednictvím hello portál Azure classic. Hello kurzu platí i pro hello kroky potřebné k hello aktualizace, když brána je nakonfigurovaná na síťovém rozhraní než DATA 0 hello zařízení StorSimple a pokoušíte tooupdate z verze 1 před aktualizací softwaru.

Aktualizace 2 zahrnuje zařízení aktualizací softwaru, aktualizací ovladače LSI a aktualizace firmwaru disku. Hello zařízení softwaru a aktualizací LSI jsou omezovaly aktualizace a provádět prostřednictvím hello portál Azure classic. aktualizace firmwaru disku Hello rušivý aktualizace a lze použít pouze prostřednictvím rozhraní Windows PowerShell hello hello zařízení.

> [!IMPORTANT]
> * Update 2 nemusí zobrazit okamžitě, protože provedeme postupné zavádění hello aktualizací. Kontrola aktualizací za pár dní znovu jako tato aktualizace brzy bude dostupná.
> * Sadu ruční a Automatická předběžné kontroly se provádějí předchozí toohello instalace toodetermine hello zařízení stav z hlediska hardwaru stavu a připojení k síti. Tyto předběžné kontroly se provádí pouze v případě, že použijete hello aktualizace z hello portál Azure classic.
> * Doporučujeme, abyste instalaci hello softwaru a aktualizací ovladače prostřednictvím hello portál Azure classic. Rozhraní Windows PowerShell toohello hello zařízení (aktualizace tooinstall) by měl jdou pouze pokud selže kontrola před aktualizací brány hello hello portálu. Hello aktualizací může trvat hodiny 4-7 tooinstall (včetně aktualizací Windows hello). aktualizace režimu údržby Hello musí být nainstalován prostřednictvím rozhraní Windows PowerShell hello hello zařízení. Jako rušivý aktualizace jsou aktualizace režimu údržby, tyto povede k výpadkům pro vaše zařízení.
> * Pokud spuštěná hello volitelné Snapshot Manager zařízení StorSimple, ujistěte se, že jste upgradovali Snapshot Manager verze tooUpdate 2 předchozí tooupdating hello zařízení.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-2-via-hello-azure-classic-portal"></a>Instalaci aktualizace 2 prostřednictvím hello portál Azure classic
Proveďte následující kroky tooupdate hello zařízení příliš[Update 2](storsimple-update2-release-notes.md).

> [!NOTE]
> Aktualizace 2 umožňuje Microsoft toopull jiné diagnostické informace z hello zařízení. Výsledkem je když náš tým operations identifikuje zařízení, která došlo k potížím, jsme jsou lepší vybavené toocollect informace z hello zařízení a diagnostikovat problémy. Přijetím Update 2, můžete nám umožňují tooprovide této proaktivní podpory.
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. Ověřte, zda je spuštěna vaše zařízení **StorSimple 8000 řady Update 2 (6.3.9600.17673)**. Hello **datum poslední aktualizace** také by měl být upraven. Dozvíte se taky, že jsou k dispozici aktualizace režim údržby (Tato zpráva může pokračovat toobe zobrazený pro až too24 hello hodin po instalaci aktualizace).
   
   Aktualizace režimu údržby jsou rušivý aktualizace, které vést k výpadkům zařízení a dají se použít jenom přes rozhraní Windows PowerShell hello vašeho zařízení. V některých případech při spuštění aktualizace 1.2, možná již je aktuální firmware disku, v takovém případě nepotřebujete tooinstall aktualizuje všechny režimu údržby.
2. Stáhnout aktualizace režimu údržby hello pomocí kroků uvedených v tomto seznamu hello [opravy hotfix toodownload](#to-download-hotfixes) toosearch pro a stáhnout KB3121899, který se nainstaluje aktualizace firmwaru disku (hello jiné aktualizace by měla již nainstalovat nyní).
3. Postupujte podle kroků uvedených v tomto seznamu hello [instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello údržby režim aktualizace.

## <a name="install-update-2-as-a-hotfix"></a>Instalaci aktualizace 2 jako oprava hotfix
Tento postup použijte, pokud selže kontrola brány hello při pokusu o tooinstall hello aktualizací prostřednictvím hello portál Azure classic. Kontrola Hello selže, jako byla přiřazena tooa jiný-síťového rozhraní DATA 0 a vaše zařízení používá předchozí tooUpdate software verze 1.

verze softwaru Hello, které lze upgradovat pomocí metody hello opravy hotfix jsou Update 0.1, 0.2, aktualizace a Update 0.3, Update 1, aktualizace 1.1 a 1.2 aktualizace. Metoda opravy hotfix Hello zahrnuje hello následující tři kroky:

* Stažení opravy hotfix hello z hello katalogu služby Microsoft Update.
* Instalace a ověřte hello regulární režimu opravy hotfix.
* Instalace a ověřte opravu hotfix režimu údržby hello.

tooinstall Update 2 jako oprava hotfix, musíte stáhnout a nainstalovat hello následující opravy hotfix:

| Pořadí | kB | Popis | Typ aktualizace |
| --- | --- | --- | --- |
| 1 |KB3121901 |Aktualizace softwaru |Regulární |
| 2 |KB3121900 |LSI ovladačů |Regulární |
| 3 |KB3080728 |Oprava Storport </br> Windows Server 2012 R2 |Regulární |
| 4 |KB3090322 |Oprava spaceport </br> Windows Server 2012 R2 |Regulární |
| 5 |KB3121899 |Firmware disku |Údržby |

> [!IMPORTANT]
> * Pokud zařízení používá verzi (GA) verze, kontaktujte prosím [Microsoft Support](storsimple-contact-microsoft-support.md) tooassist s hello aktualizujete.
> * Tento postup musí toobe provést pouze jednou tooapply Update 2. Můžete použít hello Azure classic portálu tooapply následné aktualizace.
> * Každou instalaci oprav hotfix může trvat asi 20 minut toocomplete. Čas celkový instalace je zavřít too2 hodin.
> * Před použitím tohoto postupu tooapply hello aktualizace, ujistěte se, že oba řadiče zařízení jsou online a všechny hardwarové součásti hello jsou v pořádku.
> 
> 

Proveďte následující kroky tooapply hello tuto aktualizaci opravu hotfix.

[!INCLUDE [storsimple-install-update2-hotfix](../../includes/storsimple-install-update2-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Další kroky
Další informace o hello [verzi Update 2](storsimple-update2-release-notes.md).

