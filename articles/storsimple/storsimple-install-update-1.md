---
title: "Nainstalujte aktualizace 1.2 na zařízení StorSimple | Microsoft Docs"
description: "Popisuje postup instalace zařízení StorSimple 8000 řady aktualizace 1.2 na vašem zařízení řady StorSimple 8000."
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
ms.openlocfilehash: 80ff35cc47dfc38089f4c392ef4c90baf9ccc03e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="install-update-12-on-your-storsimple-8000-series-device"></a>Nainstalujte aktualizace 1.2 na vašem zařízení řady StorSimple 8000
## <a name="overview"></a>Přehled
Tento kurz vysvětluje, jak nainstalovat aktualizace 1.2 na zařízení StorSimple, která je spuštěná verze softwaru před Update 1. V kurzu platí i pro další kroky potřebné k aktualizaci, když brána je nakonfigurovaná na síťovém rozhraní než DATA 0 zařízení StorSimple.

Aktualizace 1.2 zahrnuje zařízení aktualizací softwaru, aktualizací ovladače LSI a aktualizace firmwaru disku. Software a aktualizace ovladačů LSI jsou omezovaly aktualizace a provádět prostřednictvím portálu Azure classic. Aktualizace firmwaru disku rušivý aktualizace a lze použít pouze prostřednictvím rozhraní Windows PowerShell zařízení.

V závislosti na verzi zařízení používá, můžete určit, pokud se použijí aktualizace 1.2. Verze softwaru vašeho zařízení, můžete zkontrolovat přechodem na **rychlého přehledu** části vašeho zařízení **řídicí panel**.

</br>

| Pokud spuštěná verze softwaru... | Co se stane portálu? |
| --- | --- |
| Vydání – GA |Pokud používáte verzi (GA), se nevztahují této aktualizace. Prosím [kontaktovat Microsoft Support](storsimple-contact-microsoft-support.md) aktualizace zařízení. |
| Aktualizace 0.1 |Portál použije aktualizace 1.2. |
| Aktualizace 0.2 |Portál použije aktualizace 1.2. |
| Aktualizace 0.3 |Portál použije aktualizace 1.2. |
| Update 1 |Tato aktualizace není k dispozici. |
| Aktualizace 1.1 |Tato aktualizace není k dispozici. |

</br>

> [!IMPORTANT]
> * 1.2 aktualizace nemusí zobrazit okamžitě, protože provedeme postupné zavádění aktualizací. Kontrola aktualizací za pár dní znovu jako tato aktualizace brzy bude dostupná.
> * Tato aktualizace zahrnuje sadu ruční a Automatická předběžné kontroly, který měl zjistit stav zařízení z hlediska hardwaru stavu a připojení k síti. Tyto předběžné kontroly se provádí pouze v případě, že aktualizace použít z portálu Azure classic.
> * Doporučujeme instalovat aktualizace softwaru a ovladačů prostřednictvím portálu Azure classic. Má jenom přejděte na rozhraní prostředí Windows PowerShell na zařízení (instalovat aktualizace) Pokud selže kontrola před aktualizací brány na portálu. Instalace aktualizací může trvat 5 až 10 hodin k instalaci (včetně aktualizací Windows). Aktualizace režimu údržby musí být nainstalován prostřednictvím rozhraní Windows PowerShell zařízení. Jako rušivý aktualizace jsou aktualizace režimu údržby, tyto povede k výpadkům pro vaše zařízení.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-the-azure-classic-portal"></a>Nainstalujte aktualizace 1.2 prostřednictvím portálu Azure classic
Proveďte následující kroky k aktualizaci zařízení [aktualizovat 1.2](storsimple-update1-release-notes.md). Tento postup použijte, pouze pokud máte bránu konfigurovanou v síťového rozhraní DATA 0 v zařízení.

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. Ověřte, zda je spuštěna vaše zařízení **StorSimple 8000 řady aktualizace 1.2 (6.3.9600.17584)**. **Datum poslední aktualizace** také by měl být upraven. Dozvíte se taky, že jsou k dispozici aktualizace režim údržby (Tato zpráva může nadále zobrazovat až 24 hodin po instalaci aktualizace).
   
   Aktualizace režimu údržby jsou rušivý aktualizace, které vést k výpadkům zařízení a dají se použít jenom přes rozhraní Windows PowerShell vašeho zařízení.
   
   ![Stránka údržby](./media/storsimple-install-update-1/InstallUpdate12_10M.png "údržby stránky")
2. Stáhnout aktualizace režimu údržby pomocí kroků uvedených v [ke stažení opravy hotfix](#to-download-hotfixes) k hledání a stahování KB3063416, který nainstaluje aktualizace firmwaru disku (s jinými aktualizacemi musí už být nainstalovaný nyní).
3. Postupujte podle kroků uvedených v [instalaci a ověření opravy hotfix režimu údržby](#to-install-and-verify-maintenance-mode-hotfixes) do režimu údržby instalace aktualizací.
4. V portálu Azure classic, přejděte na **údržby** a v dolní části stránky klikněte na tlačítko **kontrolovat aktualizace** k vyhledání všech aktualizací Windows a pak klikněte na **instalovat aktualizace**. Budete hotovi, po všechny aktualizace jsou úspěšně nainstalováni.

## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>Nainstalujte aktualizace 1.2 na zařízení, které má brána nakonfigurovaná bez dat 0 síťového rozhraní
Tento postup byste měli použít pouze v případě nezdaří Kontrola brány při pokusu o instalaci aktualizací prostřednictvím portálu Azure classic. Kontrola selže, jak máte bránu přiřazené 0 síťové rozhraní bez dat a vaše zařízení používá verzi softwaru před Update 1. Pokud zařízení nemá brány na 0 síťové rozhraní bez dat, můžete je aktualizovat zařízení přímo z portálu Azure classic. V tématu [nainstalujte aktualizaci 1.2 prostřednictvím portálu Azure classic](#install-update-1.2-via-the-azure-classic-portal).

Verze softwaru, které lze upgradovat pomocí této metody jsou Update 0.1, aktualizace 0,2 a Update 0.3.

> [!IMPORTANT]
> * Pokud zařízení používá verzi (GA) verze, kontaktujte prosím [Microsoft Support](storsimple-contact-microsoft-support.md) a pomůže vám při aktualizaci.
> * Tento postup je nutné provést pouze jednou pro použití aktualizace 1.2. Portál Azure classic můžete použít následující aktualizace.
> 
> 

Pokud se vaše zařízení používá 1 softwaru před aktualizací a má bránu nastavení pro síťové rozhraní než DATA 0, můžete použít aktualizace 1.2 v následujících dvou způsobů:

* **Možnost 1**: stáhnout aktualizaci a použijte ho pomocí `Start-HcsHotfix` rutiny z rozhraní Windows PowerShell zařízení. Toto je doporučená metoda. **Nepoužívejte tuto metodu použít aktualizace 1.2, pokud vaše zařízení se systémem Update 1.0 nebo aktualizace 1.1.**
* **Možnost 2**: Odeberte konfigurace brány a instalovat aktualizace přímo z portálu Azure classic.

Podrobné pokyny pro každou z nich jsou uvedené v následujících částech.

## <a name="option-1-use-windows-powershell-for-storsimple-to-apply-update-12-as-a-hotfix"></a>Možnost 1: Použití Windows Powershellu pro StorSimple pro použití aktualizace 1.2 jako oprava hotfix
Tento postup byste měli použít pouze v případě, že používáte Update 0.1, 0.2, 0.3 a pokud kontrola vaší brány se nezdařila při pokusu o instalaci aktualizace z portálu Azure classic. Pokud používáte software verze (GA), [Microsoft Support](storsimple-contact-microsoft-support.md) aktualizace zařízení.

K instalaci aktualizace 1.2 jako oprava hotfix, musíte stáhnout a nainstalovat následující opravy hotfix:

| Pořadí | kB | Popis | Typ aktualizace |
| --- | --- | --- | --- |
| 1 |KB3063418 |Aktualizace softwaru |Regulární |
| 2 |KB3043005 |Aktualizace řadiče LSI SAS |Regulární |
| 3 |KB3063416 |Firmware disku |Údržby |

Před použitím tohoto postupu použít aktualizaci, ujistěte se, že:

* Oba řadiče zařízení jsou online.

Proveďte následující kroky pro použití aktualizace 1.2. **Aktualizace může trvat přibližně 2 hodiny (přibližně 30 minut pro software, 30 minut pro ovladače, firmware disku 45 minut).**

[!INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]

## <a name="option-2-use-the-azure-classic-portal-to-apply-update-12-after-removing-the-gateway-configuration"></a>Možnost 2: Použít aktualizace 1.2 po odebrání konfigurace brány pomocí portálu Azure classic
Tento postup platí jenom pro zařízení StorSimple, které jsou spuštěné verze softwaru před Update 1 a mít bránu nastavená v síťovém rozhraní než DATA 0. Musíte vymazat nastavení brány před instalací aktualizace.

Aktualizace může trvat několik hodin. Pokud váš hostitel se nacházejí v různých podsítích, odebrání konfigurace brány na rozhraní iSCSI může způsobit výpadky. Doporučujeme nakonfigurovat DATA 0 pro přenosy iSCSI, abyste snížili výpadek.

Proveďte následující kroky k deaktivaci síťové rozhraní s bránou a pak použít aktualizaci.

[!INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Další kroky
Další informace o [verze 1.2 aktualizace](storsimple-update1-release-notes.md).

