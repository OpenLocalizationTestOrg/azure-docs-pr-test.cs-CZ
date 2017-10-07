---
title: "aaaInstall 1.2 aktualizace zařízení StorSimple | Microsoft Docs"
description: "Vysvětluje, jak tooinstall StorSimple 8000 řady aktualizace 1.2 na vašem zařízení řady StorSimple 8000."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 7a513923-eb77-4078-b0ab-f8e90183796a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0a7601dc0b1ce60eb854227243ecb02d6fb2c678
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-12-on-your-storsimple-8000-series-device"></a>Nainstalujte aktualizace 1.2 na vašem zařízení řady StorSimple 8000
## <a name="overview"></a>Přehled
Tento kurz popisuje, jak aktualizovat tooinstall 1.2 v zařízení StorSimple, které běží předchozí tooUpdate software verze 1. Hello kurzu platí i pro hello další kroky potřebné pro aktualizaci hello, pokud brána je nakonfigurovaná na síťovém rozhraní než DATA 0 zařízení StorSimple hello.

Aktualizace 1.2 zahrnuje zařízení aktualizací softwaru, aktualizací ovladače LSI a aktualizace firmwaru disku. Hello softwaru a aktualizací ovladače LSI jsou omezovaly aktualizace a provádět prostřednictvím hello portál Azure classic. aktualizace firmwaru disku Hello rušivý aktualizace a lze použít pouze prostřednictvím rozhraní Windows PowerShell hello hello zařízení.

V závislosti na verzi zařízení používá, můžete určit, pokud se použijí aktualizace 1.2. Verze softwaru hello vašeho zařízení, můžete zkontrolovat tak, že přejdete toohello **rychlého přehledu** části vašeho zařízení **řídicí panel**.

</br>

| Pokud spuštěná verze softwaru... | Co se stane portálu hello? |
| --- | --- |
| Vydání – GA |Pokud používáte verzi (GA), se nevztahují této aktualizace. Prosím [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) tooupdate zařízení. |
| Aktualizace 0.1 |Portál použije aktualizace 1.2. |
| Aktualizace 0.2 |Portál použije aktualizace 1.2. |
| Aktualizace 0.3 |Portál použije aktualizace 1.2. |
| Update 1 |Tato aktualizace není k dispozici. |
| Aktualizace 1.1 |Tato aktualizace není k dispozici. |

</br>

> [!IMPORTANT]
> * 1.2 aktualizace nemusí zobrazit okamžitě, protože provedeme postupné zavádění hello aktualizací. Kontrola aktualizací za pár dní znovu jako tato aktualizace brzy bude dostupná.
> * Tato aktualizace zahrnuje sadu ruční a Automatická předběžné kontroly toodetermine hello zařízení stav z hlediska hardwaru stavu a připojení k síti. Tyto předběžné kontroly se provádí pouze v případě, že použijete hello aktualizace z hello portál Azure classic.
> * Doporučujeme, abyste instalaci hello softwaru a aktualizací ovladače prostřednictvím hello portál Azure classic. Rozhraní Windows PowerShell toohello hello zařízení (aktualizace tooinstall) by měl jdou pouze pokud selže kontrola před aktualizací brány hello hello portálu. Hello aktualizací může trvat tooinstall 5 až 10 hodin (včetně aktualizací Windows hello). aktualizace režimu údržby Hello musí být nainstalován prostřednictvím rozhraní Windows PowerShell hello hello zařízení. Jako rušivý aktualizace jsou aktualizace režimu údržby, tyto povede k výpadkům pro vaše zařízení.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-hello-azure-classic-portal"></a>Nainstalujte aktualizace 1.2 prostřednictvím hello portál Azure classic
Proveďte následující kroky tooupdate hello zařízení příliš[aktualizace 1.2](storsimple-update1-release-notes.md). Tento postup použijte, pouze pokud máte bránu konfigurovanou v síťového rozhraní DATA 0 v zařízení.

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. Ověřte, zda je spuštěna vaše zařízení **StorSimple 8000 řady aktualizace 1.2 (6.3.9600.17584)**. Hello **datum poslední aktualizace** také by měl být upraven. Dozvíte se taky, že jsou k dispozici aktualizace režim údržby (Tato zpráva může pokračovat toobe zobrazený pro až too24 hello hodin po instalaci aktualizace).
   
   Aktualizace režimu údržby jsou rušivý aktualizace, které vést k výpadkům zařízení a dají se použít jenom přes rozhraní Windows PowerShell hello vašeho zařízení.
   
   ![Stránka údržby](./media/storsimple-install-update-1/InstallUpdate12_10M.png "údržby stránky")
2. Stáhnout aktualizace režimu údržby hello pomocí kroků uvedených v tomto seznamu hello [opravy hotfix toodownload](#to-download-hotfixes) toosearch pro a stáhnout KB3063416, který se nainstaluje aktualizace firmwaru disku (hello jiné aktualizace by měla již nainstalovat nyní).
3. Postupujte podle kroků uvedených v tomto seznamu hello [instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello údržby režim aktualizace.
4. V hello portál Azure classic, přejděte toohello **údržby** a v dolní části hello hello stránky, klikněte na tlačítko **kontrolovat aktualizace** toocheck pro všechny aktualizace systému Windows a pak klikněte na tlačítko **instalovat aktualizace** . Budete hotovi, po všech z hello aktualizace se úspěšně nainstaloval.

## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>Nainstalujte aktualizace 1.2 na zařízení, které má brána nakonfigurovaná bez dat 0 síťového rozhraní
Tento postup byste měli použít pouze v případě nezdaří Kontrola brány hello při pokusu o tooinstall hello aktualizací prostřednictvím hello portál Azure classic. Kontrola Hello selže, jako byla přiřazena tooa jiný-síťového rozhraní DATA 0 a vaše zařízení používá předchozí tooUpdate software verze 1. Pokud zařízení nemá brány na 0 síťové rozhraní bez dat, můžete je aktualizovat zařízení přímo z hello portál Azure classic. V tématu [nainstalujte aktualizaci 1.2 prostřednictvím portálu Azure classic hello](#install-update-1.2-via-the-azure-classic-portal).

Hello verze softwaru, které lze upgradovat pomocí této metody jsou Update 0.1, aktualizace 0,2 a Update 0.3.

> [!IMPORTANT]
> * Pokud zařízení používá verzi (GA) verze, kontaktujte prosím [Microsoft Support](storsimple-contact-microsoft-support.md) tooassist s hello aktualizujete.
> * Tento postup musí toobe provést pouze jednou tooapply aktualizace 1.2. Můžete použít hello Azure classic portálu tooapply následné aktualizace.
> 
> 

Pokud vaše zařízení používá 1 softwaru před aktualizací a má bránu nastavení pro síťové rozhraní než DATA 0, můžete použít aktualizace 1.2 na hello následujících dvou způsobů:

* **Možnost 1**: stažení aktualizace hello a použít pomocí hello `Start-HcsHotfix` rutiny z rozhraní Windows PowerShell hello hello zařízení. Toto je doporučená metoda hello. **Tato metoda tooapply aktualizace 1.2 nepoužívejte, pokud vaše zařízení se systémem Update 1.0 nebo aktualizace 1.1.**
* **Možnost 2**: Konfigurace brány hello odebrat a hello instalace aktualizace přímo z hello portál Azure classic.

Podrobné pokyny pro každou z nich jsou uvedené v následující části hello.

## <a name="option-1-use-windows-powershell-for-storsimple-tooapply-update-12-as-a-hotfix"></a>Možnost 1: Použití Windows Powershellu pro StorSimple tooapply aktualizace 1.2 jako oprava hotfix
Tento postup byste měli použít pouze v případě, že používáte Update 0.1, 0.2, 0.3 a pokud kontrola vaší brány se nezdařila při pokusu o tooinstall aktualizace z hello portál Azure classic. Pokud používáte software verze (GA), [Microsoft Support](storsimple-contact-microsoft-support.md) tooupdate zařízení.

tooinstall aktualizace 1.2 jako oprava hotfix, musíte stáhnout a nainstalovat hello následující opravy hotfix:

| Pořadí | kB | Popis | Typ aktualizace |
| --- | --- | --- | --- |
| 1 |KB3063418 |Aktualizace softwaru |Regulární |
| 2 |KB3043005 |Aktualizace řadiče LSI SAS |Regulární |
| 3 |KB3063416 |Firmware disku |Údržby |

Před použitím tohoto postupu tooapply hello aktualizace, ujistěte se, že:

* Oba řadiče zařízení jsou online.

Proveďte následující kroky tooapply aktualizace 1.2 hello. **Hello aktualizací může trvat přibližně 2 hodiny toocomplete (přibližně 30 minut pro software, 30 minut pro ovladače, firmware disku 45 minut).**

[!INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]

## <a name="option-2-use-hello-azure-classic-portal-tooapply-update-12-after-removing-hello-gateway-configuration"></a>Možnost 2: Použijte hello Azure classic portálu tooapply 1.2 aktualizace po odebrání konfigurace brány hello
Tento postup platí jenom tooStorSimple zařízení, která běží software verze předchozí tooUpdate 1 a mít bránu nastavená v síťovém rozhraní než DATA 0. Budete potřebovat tooclear hello brány nastavení předchozí tooapplying hello aktualizace.

Hello aktualizace může trvat několik hodin toocomplete. Pokud váš hostitel se nacházejí v různých podsítích, odebrání konfigurace brány hello na rozhraní iSCSI hello může způsobit výpadky. Doporučujeme nakonfigurovat DATA 0 výpadek hello tooreduce přenosy iSCSI.

Proveďte následující kroky toodisable hello síťové rozhraní s bránou hello hello a potom použijte aktualizaci hello.

[!INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Další kroky
Další informace o hello [verze 1.2 aktualizace](storsimple-update1-release-notes.md).

