---
title: "aaaModify hello DATA 0 nastavení v zařízení StorSimple | Microsoft Docs"
description: "Zjistěte, jak toouse Windows Powershellu pro StorSimple tooreconfigure hello síťového rozhraní DATA 0 v zařízení StorSimple."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 58e3d509-f425-4a7f-b650-d496a7c85193
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: caec51c3344d953299253301c2a0d7577d553c6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="modify-hello-data-0-network-interface-settings-on-your-storsimple-device"></a>Upravit nastavení rozhraní 0 sítě hello DATA v zařízení StorSimple
## <a name="overview"></a>Přehled
Zařízení s Microsoft Azure StorSimple má šest síťových rozhraní, z dat 0 tooDATA 5. Hello DATA 0 rozhraní vždycky nakonfigurovaný pomocí rozhraní Windows PowerShell hello nebo hello konzoly sériového portu a je automaticky povolenou podporu cloudu. Všimněte si, že nemůžete nakonfigurovat síťového rozhraní DATA 0 prostřednictvím hello portál Azure classic. 

Hello rozhraní DATA 0 je nejdřív nakonfigurovali prostřednictvím Průvodce instalací hello během počátečního nasazení zařízení StorSimple hello. Když je zařízení hello v provozní režim, musíte tooreconfigure DATA 0 nastavení. V tomto kurzu nabízí dvě metody toomodify DATA 0 síťová nastavení, jak pomocí prostředí Windows PowerShell pro StorSimple.

Po přečtení tohoto kurzu, budete moci:

* Upravit DATA 0 síťová nastavení prostřednictvím Průvodce instalací hello
* Upravit nastavení DATA 0 sítě prostřednictvím hello `Set-HcsNetInterface` rutiny

## <a name="modify-data-0-network-settings-through-setup-wizard"></a>Upravit nastavení DATA 0 sítě prostřednictvím Průvodce instalací
Nastavení DATA 0 sítě můžete znovu nakonfigurovat pomocí rozhraní Windows PowerShell toohello zařízení StorSimple připojení a spuštění relace Průvodce instalací. Proveďte následující kroky toomodify DATA 0 hello nastavení:

#### <a name="toomodify-data-0-network-settings-through-setup-wizard"></a>nastavení 0 sítě toomodify DATA prostřednictvím Průvodce instalací
1. V nabídce konzoly sériového portu hello, zvolte možnost 1, **přihlásit úplný přístup**. Po zobrazení výzvy zadejte hello **hesla správce zařízení**. výchozí heslo Hello je `Password1`.
2. Hello příkazového řádku zadejte:
   
    `Invoke-HcsSetupWizard`
3. Zobrazí se Průvodce instalací toohelp nakonfigurujete hello DATA 0 rozhraní vašeho zařízení. Zadejte nové hodnoty pro hello IP adresu, brány a síťovou masku.

> [!NOTE]
> pevné IP adresy, bude nutné překonfigurovat prostřednictvím hello toobe řadiče Hello **konfigurace** stránku hello zařízení StorSimple v hello portál Azure classic. Další informace, přejděte příliš[upravit síťových rozhraní](storsimple-modify-device-config.md#modify-network-interfaces).
> 
> 

## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a>Úprava nastavení DATA 0 sítě pomocí rutiny Set-HcsNetInterface
Jiný způsob, jak tooreconfigure DATA 0 síťové rozhraní je prostřednictvím použití hello hello `Set-HcsNetInterface` rutiny. Hello rutina se spustí z rozhraní Windows PowerShell hello zařízení StorSimple. Při použití tohoto postupu, pevné IP adresy řadiče hello také zde mohou být konfigurovány. Proveďte následující kroky toomodify hello DATA 0 hello nastavení: 

#### <a name="toomodify-data-0-network-settings-through-hello-set-hcsnetinterface-cmdlet"></a>nastavení 0 sítě toomodify dat pomocí rutiny Set-HcsNetInterface hello
1. V nabídce konzoly sériového portu hello, zvolte možnost 1, **přihlásit úplný přístup**. Po zobrazení výzvy zadejte heslo správce zařízení hello. výchozí heslo Hello je `Password1`.
2. Hello příkazového řádku zadejte:
   
    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
   
    V závorkách hello šikmo zadejte následující hodnoty pro DATA 0 hello:
   
   * Adresa IPv4
   * Brána IPv4
   * Maska podsítě IPv4
   * Opravené adresu IPv4 pro řadič 0
   * Opravené adresu IPv4 pro řadič 1
     
     Další informace o hello použití této rutiny naleznete příliš[prostředí Windows PowerShell pro referenční informace o rutinách StorSimple](https://technet.microsoft.com/library/dn688161.aspx).

## <a name="next-steps"></a>Další kroky
* tooconfigure síťová rozhraní kromě rozhraní DATA 0, můžete použít hello [konfigurovat stránku hello portál Azure classic](storsimple-modify-device-config.md). 
* Pokud máte problémy při konfiguraci síťových rozhraní, podívejte se příliš[potíží s nasazením](storsimple-troubleshoot-deployment.md).

